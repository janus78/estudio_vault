# 05-02 — Azure Primario: El Framework de Decisión de un Arquitecto

> **Prerequisito:** [04-04-message-queues.md](../modulo-04-system-design/04-04-message-queues.md), [04-07-security-en-system-design.md](../modulo-04-system-design/04-07-security-en-system-design.md), [04-09-deployment-y-cicd.md](../modulo-04-system-design/04-09-deployment-y-cicd.md) — Este archivo implementa los conceptos de mensajería, seguridad y deployment de esos módulos en servicios concretos de Azure.
>
> **Lo que este archivo NO es:** Un tutorial de Azure Portal o una guía de configuración paso a paso. Para eso existe la documentación oficial y Microsoft Learn. Este archivo es el framework de decisión — el criterio de cuándo usar qué servicio y cómo articularlo en una entrevista de arquitecto.
>
> **La pregunta que este archivo te permite responder:** *"Diseña el deployment de esta aplicación .NET en Azure."* — No con una respuesta genérica, sino con trade-offs reales y criterios técnicos defensibles.
>
> **🎯 Recursos:**
> - **Pluralsight — Azure Architecture path** → para profundidad en Container Apps y AKS
> - **Microsoft Learn** → [learn.microsoft.com/azure](https://learn.microsoft.com/azure) — gratuito, con sandboxes reales. Especialmente los paths de AZ-204 (Developer) y AZ-305 (Architecture)
> - **Azure Architecture Center** → [learn.microsoft.com/azure/architecture](https://learn.microsoft.com/azure/architecture) — patrones reales con diagramas de referencia

---

## Sección 1 — El framework de decisión de compute en Azure

Esta es la decisión más frecuente para un arquitecto .NET que trabaja en Azure: ¿dónde corro este workload?

La respuesta incorrecta: "uso App Service porque es lo que conozco."
La respuesta Staff: un árbol de decisión con criterios técnicos.

```mermaid
flowchart TD
    Start([Nuevo workload]) --> Q1{¿Es un proceso\ncontinuo o API\nsiempre activa?}

    Q1 -->|Sí| Q2{¿Necesitas control\ngranular de infra,\nnode pools, o\nmulti-cloud K8s?}
    Q1 -->|No — event-driven\no batch| Q3{¿El workload\ndura menos de\n10 minutos?}

    Q2 -->|Sí — equipo K8s\nexperimentado| AKS[🔵 AKS\nControl total\nMayor complejidad operacional]
    Q2 -->|No — quiero\nmanaged| Q4{¿Es un servicio\nHTTP o un\nworker de queue?}

    Q4 -->|HTTP — API,\nwebhook, backend| ACA_HTTP[🟢 Azure Container Apps\nManaged, KEDA, HTTPS nativo]
    Q4 -->|Worker —\nprocesa mensajes\nen background| ACA_WORKER[🟢 Container Apps\ncon Service Bus trigger\nEscala a cero]

    Q3 -->|Sí — < 10 min| Q5{¿Necesitas\norquestación\nde pasos?}
    Q3 -->|No — proceso largo\no larga duración| Q6{¿Puede\ncontainerizarse?}

    Q5 -->|No — acción\nindependiente| AF[🟣 Azure Functions\nConsumption plan]
    Q5 -->|Sí — múltiples\npasos y estado| DF[🟣 Durable Functions\nOrchestrator pattern]

    Q6 -->|Sí| ACA_JOB[🟢 Container Apps Jobs\nRun to completion]
    Q6 -->|No o desconocido| DF

    style AKS fill:#1e40af,color:#fff
    style ACA_HTTP fill:#059669,color:#fff
    style ACA_WORKER fill:#059669,color:#fff
    style ACA_JOB fill:#059669,color:#fff
    style AF fill:#7c3aed,color:#fff
    style DF fill:#7c3aed,color:#fff
```

**Cómo leer este árbol en una entrevista:**

Cuando el entrevistador pregunta *"¿dónde deployarías esta API?"*, no respondas directamente con un servicio. Muestra el razonamiento:

> *"Primero: ¿necesita estar siempre activa o puede escalar a cero? Si es una API de cara al usuario con SLA de latencia, necesita estar siempre activa — descartamos Consumption plan de Functions. ¿Necesito control de K8s o quiero un managed platform? Si el equipo no tiene experiencia operando K8s, Container Apps es el punto medio correcto — tengo KEDA, HTTPS automático, y escalo por métricas sin operar un cluster. Si en el futuro necesito customizar el scheduling o mezclar workloads de GPU, migro a AKS. Pero no over-engineering desde el día uno."*

Eso es pensar a nivel Staff. El servicio importa menos que el razonamiento que lo sustenta.

---

## Sección 2 — Azure Functions vs Container Apps vs AKS: los trade-offs reales

### Azure Functions — cuándo es la elección correcta

Azure Functions brilla cuando tienes workloads verdaderamente event-driven y puedes vivir con sus limitaciones:

```csharp
// Ejemplo completo: Function triggered por Azure Service Bus
// Nota: el manejo correcto de Complete/Abandon/DeadLetter es lo que
// diferencia un senior que "usa" Functions de un arquitecto que las "diseña"

[FunctionName("ProcessOrderCreated")]
public async Task Run(
    [ServiceBusTrigger(
        topicName: "domain-events",
        subscriptionName: "notification-subscription",
        Connection = "ServiceBusConnection")]
    ServiceBusReceivedMessage message,
    ServiceBusMessageActions messageActions,
    ILogger log,
    CancellationToken ct)
{
    var deliveryCount = message.DeliveryCount;
    log.LogInformation(
        "Processing message {MessageId}, attempt {Attempt}",
        message.MessageId, deliveryCount);

    OrderCreatedEvent? orderEvent = null;
    try
    {
        // Deserialización puede fallar — manejar por separado del procesamiento
        orderEvent = JsonSerializer.Deserialize<OrderCreatedEvent>(
            message.Body.ToString(),
            JsonSerializerOptions.Default)
            ?? throw new InvalidOperationException("Null deserialization result");

        await _notificationService.SendOrderConfirmationAsync(
            orderEvent.OrderId,
            orderEvent.CustomerEmail,
            ct);

        // ✅ Complete: el mensaje fue procesado exitosamente — se elimina de la queue
        await messageActions.CompleteMessageAsync(message, ct);

        log.LogInformation("Processed order {OrderId}", orderEvent.OrderId);
    }
    catch (JsonException ex)
    {
        // Error de deserialización → el mensaje está malformado, no tiene sentido reintentarlo
        // Dead Letter directamente, con razón explícita para debugging
        log.LogError(ex, "Invalid message format for {MessageId}", message.MessageId);
        await messageActions.DeadLetterMessageAsync(
            message,
            deadLetterReason: "DeserializationFailure",
            deadLetterErrorDescription: ex.Message,
            ct);
    }
    catch (Exception ex) when (deliveryCount < 3)
    {
        // Error transitorio y hay intentos disponibles → Abandon para retry automático
        // El mensaje vuelve a la queue con visibilidad delay (backoff)
        log.LogWarning(ex, "Transient error processing {MessageId}, will retry", message.MessageId);
        await messageActions.AbandonMessageAsync(message, null, ct);
    }
    catch (Exception ex)
    {
        // Agotamos los reintentos → Dead Letter con contexto completo
        log.LogError(ex, "Max retries exhausted for {MessageId}", message.MessageId);
        await messageActions.DeadLetterMessageAsync(
            message,
            deadLetterReason: "MaxRetriesExhausted",
            deadLetterErrorDescription: ex.ToString(),
            ct);
    }
}
```

**Limitaciones reales de Functions que debes conocer:**

| Limitación | Consumption Plan | Premium Plan | Impacto arquitectónico |
|---|---|---|---|
| Cold start | 1-5 segundos de latencia | No cold start | Inaceptable para APIs de usuario — usar Premium o Container Apps |
| Duración máxima | 10 minutos | Sin límite (config) | Procesos largos → Durable Functions o Container Apps Jobs |
| Estado | Stateless — cada ejecución es independiente | Stateless | Estado → Durable Functions o External storage |
| Concurrencia | Controlada por runtime, configurable | Controlada | Cargas de alta concurrencia → revisar maxConcurrentCalls |
| Dependencias en startup | Tiempo de startup visible en cold start | Warm | DI container pesado → visible en primer request |

### Durable Functions — para workflows de larga duración

```csharp
// Orchestrator — define el flujo de pasos con estado persistido
// ⚠️ Los orchestrators son deterministas: no pueden tener I/O directo,
// random, DateTime.Now, ni operaciones no reproducibles.
// Toda operación con efecto externo → Activity.
[FunctionName("OrderFulfillmentOrchestrator")]
public async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext context,
    ILogger log)
{
    var order = context.GetInput<OrderFulfillmentInput>();

    // Retry policy automática por Activity
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3)
    {
        BackoffCoefficient = 2.0,    // Backoff exponencial
        MaxRetryInterval = TimeSpan.FromMinutes(1)
    };

    // Paso 1: Validar inventario
    bool inventoryAvailable = await context.CallActivityWithRetryAsync<bool>(
        "ValidateInventory", retryOptions, order);

    if (!inventoryAvailable)
    {
        await context.CallActivityAsync("NotifyInventoryShortage", order);
        return; // El estado del orchestrator queda en "Completed"
    }

    // Paso 2: Reservar inventario y procesar pago en paralelo
    var reserveTask = context.CallActivityWithRetryAsync("ReserveInventory", retryOptions, order);
    var paymentTask = context.CallActivityWithRetryAsync<PaymentResult>(
        "ProcessPayment", retryOptions, order);

    await Task.WhenAll(reserveTask, paymentTask);
    var paymentResult = paymentTask.Result;

    // Paso 3: Esperar aprobación manual si el monto supera el threshold
    if (paymentResult.Amount > 10_000)
    {
        // Esperar evento externo — puede demorar horas o días
        // El orchestrator "duerme" sin consumir recursos mientras espera
        var approval = await context.WaitForExternalEvent<ApprovalEvent>(
            "OrderApproved",
            timeout: TimeSpan.FromHours(24));

        if (!approval.Approved)
        {
            await context.CallActivityAsync("CancelOrder", order);
            return;
        }
    }

    await context.CallActivityAsync("ShipOrder", order);
    await context.CallActivityAsync("SendConfirmationEmail", order);
}

// Activity — la unidad real de trabajo con efectos externos
[FunctionName("ProcessPayment")]
public async Task<PaymentResult> ProcessPayment(
    [ActivityTrigger] OrderFulfillmentInput order,
    ILogger log)
{
    // Aquí sí puede haber I/O, random, DateTime.Now — es una Activity normal
    return await _paymentService.ChargeAsync(order.CustomerId, order.Amount);
}
```

**Cuándo Durable Functions es la elección correcta:**
- Proceso con múltiples pasos que pueden fallar independientemente
- Workflow que necesita esperar eventos externos (aprobaciones, webhooks)
- Fan-out/fan-in: procesar N items en paralelo y agregar resultados
- Proceso que puede durar horas o días (con estado persistido automáticamente)

### Azure Container Apps — el punto medio que más escala

Container Apps resuelve el dilema "Functions tiene demasiado overhead pero AKS es demasiado complejo":

```yaml
# containerapp.yaml — configuración completa de una Container App
name: order-processor
location: eastus
resourceGroup: my-rg

properties:
  configuration:
    ingress:
      external: false          # Solo accesible dentro del entorno (no HTTP público)
      targetPort: 8080
    
    secrets:
      - name: servicebus-connection
        value: "Endpoint=sb://..."
    
    registries:
      - server: myregistry.azurecr.io
        username: myregistry
        passwordSecretRef: registry-password
  
  template:
    containers:
      - name: order-processor
        image: myregistry.azurecr.io/order-processor:1.2.0
        resources:
          cpu: 0.5
          memory: 1.0Gi
        env:
          - name: ASPNETCORE_ENVIRONMENT
            value: Production
          - name: ServiceBus__ConnectionString
            secretRef: servicebus-connection
    
    # Scale rules — esto es KEDA integrado
    scale:
      minReplicas: 0          # Escalar a cero cuando no hay mensajes — costo zero
      maxReplicas: 20
      rules:
        - name: service-bus-scale
          custom:
            type: azure-servicebus
            metadata:
              topicName: domain-events
              subscriptionName: order-processor-subscription
              messageCount: "10"   # Agregar 1 replica por cada 10 mensajes en queue
            auth:
              - secretRef: servicebus-connection
                triggerParameter: connection
```

**Cuándo Container Apps es mejor que Functions:**
- La app ya está containerizada — no necesitas reescribirla como Function
- El proceso dura más de 10 minutos regularmente
- Necesitas una API HTTP siempre activa sin cold start (configura `minReplicas: 1`)
- Necesitas sidecars (Dapr, logging agents) junto al container principal
- Quieres escalar por métricas de negocio, no solo por request count

### AKS — cuándo justifica la complejidad operacional

```csharp
// No hay código de "cómo usar AKS" aquí — eso es documentación.
// La pregunta relevante para una entrevista Staff es:
// ¿Cuándo es AKS la elección correcta sobre Container Apps?
```

**AKS es correcto cuando:**
1. **Node pools heterogéneos**: necesitas GPU nodes para ML inference mezclados con CPU nodes para APIs
2. **Scheduling granular**: necesitas afinidad/anti-afinidad de pods, taints y tolerations para workloads especializados
3. **Workloads mixtos que no caben en Container Apps**: jobs, stateful sets, daemonsets para logging agents
4. **Multi-cloud strategy**: Kubernetes es el denominador común — el mismo manifiesto puede deployarse en GKE o EKS
5. **El equipo ya opera Kubernetes**: si el equipo no tiene expertise en K8s, la complejidad operacional consume más valor del que AKS entrega

**AKS es overengineering cuando:**
- Tu equipo tiene < 3 personas con experiencia en K8s
- Todos tus workloads son HTTP stateless o workers de queue — Container Apps los maneja perfectamente
- El principal argumento para AKS es "escala mejor" — Container Apps con KEDA escala a miles de replicas

---

## Sección 3 — Azure Service Bus vs Event Hubs vs Event Grid

> **Conexión con módulo 4:** En [04-04-message-queues.md](../modulo-04-system-design/04-04-message-queues.md) entendiste los conceptos de queues, pub/sub, y streaming. Esta sección es la implementación de esos conceptos en Azure. Si los conceptos no están claros, regresa al módulo 4 primero.

### La tabla de decisión primaria

| Necesidad | Servicio correcto | Por qué NO el otro |
|---|---|---|
| Task queue con DLQ y retry automático | **Service Bus Queues** | Event Hubs no tiene DLQ ni retry |
| Pub/Sub con múltiples suscriptores del mismo evento | **Service Bus Topics** | Event Grid es para eventos de Azure resources |
| Ordering garantizado por entidad (por OrderId) | **Service Bus + Sessions** | Solo Sessions garantiza ordering por grupo |
| Telemetría de alta velocidad (IoT, logs, métricas) | **Event Hubs** | Service Bus no escala a millones de eventos/segundo |
| Replay de eventos históricos | **Event Hubs** | Service Bus no tiene retention configurable de días/semanas |
| Reaccionar a cambios en recursos de Azure (Blob creado, etc.) | **Event Grid** | Service Bus no integra nativamente con Azure resources |
| Webhook delivery a múltiples endpoints | **Event Grid** | Service Bus no tiene fan-out a HTTP endpoints |

### Azure Service Bus — implementación completa

```csharp
// Producer — publicar mensajes con metadatos correctos
public class OrderEventPublisher
{
    private readonly ServiceBusSender _sender;
    private readonly ILogger<OrderEventPublisher> _logger;

    public OrderEventPublisher(ServiceBusClient client, ILogger<OrderEventPublisher> logger)
    {
        _sender = client.CreateSender("domain-events");
        _logger = logger;
    }

    public async Task PublishOrderCreatedAsync(
        OrderCreatedEvent orderEvent,
        CancellationToken ct = default)
    {
        var message = new ServiceBusMessage(
            BinaryData.FromObjectAsJson(orderEvent))
        {
            // MessageId único → habilita deduplicación automática en el topic
            // Service Bus descarta mensajes duplicados en la ventana de deduplicación (default 10 min)
            MessageId = orderEvent.OrderId.ToString(),

            // CorrelationId → propagado en la cadena de mensajes para tracing distribuido
            CorrelationId = Activity.Current?.TraceId.ToString()
                            ?? Guid.NewGuid().ToString("N"),

            // ContentType → autodescriptivo — útil para consumers que manejan múltiples tipos
            ContentType = "application/json",

            // Subject → tipo del evento — permite filtrar en subscriptions sin leer el body
            Subject = "OrderCreated",

            // SessionId → garantiza ordering para todos los mensajes de esta Order
            // Todos los eventos de OrderId X se procesarán en orden por el mismo consumer
            SessionId = orderEvent.OrderId.ToString(),

            // Para mensajes que deben procesarse con delay (ej: recordatorio en 24h)
            // ScheduledEnqueueTime = DateTimeOffset.UtcNow.AddHours(24),
        };

        await _sender.SendMessageAsync(message, ct);
        _logger.LogInformation("Published OrderCreated for {OrderId}", orderEvent.OrderId);
    }

    // Batch sending — más eficiente para múltiples mensajes
    public async Task PublishBatchAsync(
        IEnumerable<OrderCreatedEvent> events,
        CancellationToken ct = default)
    {
        await using var batch = await _sender.CreateMessageBatchAsync(ct);

        foreach (var evt in events)
        {
            var message = new ServiceBusMessage(BinaryData.FromObjectAsJson(evt))
            {
                MessageId = evt.OrderId.ToString(),
                Subject = "OrderCreated"
            };

            // TryAddMessage retorna false si el batch está lleno (límite de 256KB)
            if (!batch.TryAddMessage(message))
            {
                // Enviar el batch actual y crear uno nuevo
                await _sender.SendMessagesAsync(batch, ct);
                // En producción real: crear nuevo batch aquí y agregar el mensaje
            }
        }

        await _sender.SendMessagesAsync(batch, ct);
    }
}
```

```csharp
// Consumer con Sessions — ordering garantizado por OrderId
public class OrderEventProcessor : IHostedService
{
    private readonly ServiceBusClient _client;
    private ServiceBusSessionProcessor? _processor;
    private readonly ILogger<OrderEventProcessor> _logger;

    public OrderEventProcessor(ServiceBusClient client, ILogger<OrderEventProcessor> logger)
    {
        _client = client;
        _logger = logger;
    }

    public Task StartAsync(CancellationToken ct)
    {
        _processor = _client.CreateSessionProcessor(
            topicName: "domain-events",
            subscriptionName: "order-processor-subscription",
            new ServiceBusSessionProcessorOptions
            {
                MaxConcurrentSessions = 8,           // 8 OrderIds procesándose en paralelo
                MaxConcurrentCallsPerSession = 1,    // Un mensaje por OrderId a la vez — ordering garantizado
                AutoCompleteMessages = false,        // Siempre manejar Complete/Abandon manualmente
                SessionIdleTimeout = TimeSpan.FromSeconds(30)
            });

        _processor.ProcessMessageAsync += ProcessMessageAsync;
        _processor.ProcessErrorAsync += ProcessErrorAsync;

        return _processor.StartProcessingAsync(ct);
    }

    private async Task ProcessMessageAsync(ProcessSessionMessageEventArgs args)
    {
        var message = args.Message;

        try
        {
            var orderEvent = message.Body.ToObjectFromJson<OrderCreatedEvent>();

            _logger.LogInformation(
                "Processing {Subject} for OrderId {OrderId}, session {SessionId}",
                message.Subject, orderEvent.OrderId, args.SessionId);

            // Tu lógica de negocio aquí
            await ProcessOrderEventAsync(orderEvent, args.CancellationToken);

            await args.CompleteMessageAsync(message, args.CancellationToken);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed processing message {MessageId}", message.MessageId);
            // No llamar Complete → el mensaje vuelve a la queue tras el lock timeout
            // Después de maxDeliveryCount (default: 10) → Dead Letter Queue automáticamente
        }
    }

    private Task ProcessErrorAsync(ProcessErrorEventArgs args)
    {
        _logger.LogError(args.Exception, "Session processor error: {ErrorSource}", args.ErrorSource);
        return Task.CompletedTask;
    }

    public Task StopAsync(CancellationToken ct)
        => _processor?.StopProcessingAsync(ct) ?? Task.CompletedTask;
}
```

### Event Hubs — para streaming de alta escala

Event Hubs no es una queue — es un **log distribuido** (piensa en Kafka managed). Los consumers leen el log desde un offset, pueden rewind, y múltiples consumer groups leen el mismo stream independientemente.

```csharp
// Producer — telemetría de alta velocidad
public class TelemetryProducer
{
    private readonly EventHubProducerClient _client;

    public TelemetryProducer(EventHubProducerClient client) => _client = client;

    public async Task SendTelemetryBatchAsync(
        IEnumerable<DeviceTelemetry> telemetryItems,
        CancellationToken ct)
    {
        // Partition key: todos los eventos del mismo dispositivo van a la misma partición
        // → ordering garantizado por dispositivo
        var options = new CreateBatchOptions
        {
            PartitionKey = telemetryItems.First().DeviceId
        };

        await using var batch = await _client.CreateBatchAsync(options, ct);

        foreach (var telemetry in telemetryItems)
        {
            var eventData = new EventData(BinaryData.FromObjectAsJson(telemetry));
            eventData.Properties["DeviceType"] = telemetry.DeviceType;
            eventData.Properties["Timestamp"] = telemetry.RecordedAt.ToString("O");

            if (!batch.TryAdd(eventData))
            {
                await _client.SendAsync(batch, ct);
                // Crear nuevo batch y reintentar
            }
        }

        await _client.SendAsync(batch, ct);
    }
}

// Consumer con EventProcessorClient — maneja checkpointing y partition balancing
// Para producción, usar EventProcessorClient en lugar de EventHubConsumerClient directamente
public class TelemetryProcessor : IHostedService
{
    private readonly EventProcessorClient _processor;

    public TelemetryProcessor(
        BlobContainerClient checkpointStore, // Azure Blob para guardar checkpoints
        string connectionString,
        string eventHubName)
    {
        // EventProcessorClient distribuye las particiones entre múltiples instancias
        // y guarda el checkpoint (offset leído) en Blob Storage
        _processor = new EventProcessorClient(
            checkpointStore,
            EventHubConsumerClient.DefaultConsumerGroupName,
            connectionString,
            eventHubName);

        _processor.ProcessEventAsync += ProcessEventAsync;
        _processor.ProcessErrorAsync += ProcessErrorAsync;
    }

    private async Task ProcessEventAsync(ProcessEventArgs args)
    {
        if (args.HasEvent)
        {
            var telemetry = args.Data.EventBody.ToObjectFromJson<DeviceTelemetry>();
            await ProcessTelemetryAsync(telemetry, args.CancellationToken);

            // Checkpoint: guardar el offset procesado en Blob Storage
            // Si la app reinicia, continúa desde aquí — no reprocesa eventos anteriores
            await args.UpdateCheckpointAsync(args.CancellationToken);
        }
    }

    public Task StartAsync(CancellationToken ct) => _processor.StartProcessingAsync(ct);
    public Task StopAsync(CancellationToken ct) => _processor.StopProcessingAsync(ct);
}
```

---

## Sección 4 — Azure SQL vs Cosmos DB: la decisión más importante en datos

> **Conexión con módulo 4:** En [04-02-bases-de-datos-system-design.md](../modulo-04-system-design/04-02-bases-de-datos-system-design.md) entendiste los trade-offs fundamentales entre SQL y NoSQL. Esta sección aplica esos conceptos a los servicios concretos de Azure.

### El criterio de decisión honesto

La industria tiene un sesgo hacia Cosmos DB porque "suena más moderno". Un arquitecto toma la decisión en base a criterios reales:

```
¿Cuándo Azure SQL es la elección correcta?

- Datos relacionales con integridad referencial requerida
- Transacciones ACID cross-entity (ej: actualizar Account y Transaction atómicamente)
- Modelo de datos estable y conocido
- El equipo conoce SQL bien
- El sistema tiene < 10M usuarios activos y no necesita distribución global
- El presupuesto importa — Cosmos DB es significativamente más caro

Respuesta directa: la gran mayoría de aplicaciones de negocio deberían usar Azure SQL.
Cosmos DB es la excepción justificada, no el default moderno.
```

```csharp
// Azure SQL con EF Core — configuración para producción

// En el DbContext: configuración de retry para errores transitorios de Azure SQL
builder.Services.AddDbContext<AppDbContext>((sp, options) =>
    options
        .UseSqlServer(
            builder.Configuration.GetConnectionString("AzureSQL"),
            sqlServerOptions => sqlServerOptions
                // Azure SQL puede tener errores transitorios en failover o throttling
                // EnableRetryOnFailure los maneja automáticamente con backoff exponencial
                .EnableRetryOnFailure(
                    maxRetryCount: 5,
                    maxRetryDelay: TimeSpan.FromSeconds(30),
                    errorNumbersToAdd: null) // null = errores transitorios default de Azure SQL
                .CommandTimeout(30)          // 30 segundos — ajustar según queries esperadas
        )
        // Default NoTracking para reads — las queries de solo lectura son la mayoría
        // Los Command handlers activan tracking explícitamente cuando lo necesitan
        .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking)
        .AddInterceptors(sp.GetRequiredService<AuditInterceptor>())  // Interceptores para auditing
);
```

```csharp
// Azure SQL Elastic Pools — para multi-tenant con muchas BDs pequeñas
// Si tienes 100 tenants con bases de datos separadas pero uso bajo por tenant,
// un Elastic Pool comparte DTUs/vCores entre todas — significativamente más barato.
// No hay cambio en el código — es una decisión de configuración de infraestructura.
```

### Cosmos DB — cuándo es la elección correcta y el costo real

```csharp
// Cosmos DB con SDK v3 — configuración para producción
builder.Services.AddSingleton<CosmosClient>(sp =>
{
    return new CosmosClient(
        accountEndpoint: builder.Configuration["CosmosDB:Endpoint"],
        // ✅ Managed Identity — sin connection strings con credenciales
        tokenCredential: new DefaultAzureCredential(),
        new CosmosClientOptions
        {
            // Consistencia más débil = menor latencia, menor costo en RUs
            // Strong: garantiza que todas las réplicas tienen el valor más reciente
            // Bounded Staleness: permite lag configurable — buen balance
            // Eventual: la más barata y rápida — para datos de lectura frecuente no críticos
            ConsistencyLevel = ConsistencyLevel.BoundedStaleness,

            // Connection mode Direct = TCP directo a los backends de Cosmos
            // Gateway = HTTP a través del gateway — más lento pero atraviesa firewalls
            ConnectionMode = ConnectionMode.Direct,

            SerializerOptions = new CosmosSerializationOptions
            {
                PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
            }
        });
});
```

```csharp
// El partition key — la decisión más crítica en Cosmos DB
// Un partition key malo destruye el costo y la performance.

// ❌ MALA elección de partition key
public class Order
{
    public string Id { get; set; } = default!;

    // OrderStatus como partition key: solo 3-5 valores posibles
    // Todos los "Pending" van a la misma partición → hotspot
    // La partición "Pending" recibe todo el tráfico de escritura
    [JsonPropertyName("status")]
    public string Status { get; set; } = default!; // ❌ Low cardinality = hotspot
}

// ✅ BUENA elección de partition key
public class Order
{
    public string Id { get; set; } = default!;

    // CustomerId como partition key:
    // - Alta cardinalidad (muchos valores únicos) → distribución uniforme
    // - Las queries más frecuentes son "órdenes de este cliente" → in-partition query
    // - Un cliente rara vez tiene miles de órdenes → no hay hot partition
    [JsonPropertyName("customerId")]
    public string CustomerId { get; set; } = default!; // ✅ High cardinality, query-aligned

    // En Cosmos DB, el item completo tiene todos los datos que necesitas
    // para las queries principales — no hay JOINs cross-partition
    public List<OrderItemDocument> Items { get; set; } = new();
    public CustomerSnapshot Customer { get; set; } = default!; // Denormalización intencional
}

// Query eficiente — in-partition
var query = new QueryDefinition(
    "SELECT * FROM c WHERE c.customerId = @customerId AND c.status = @status")
    .WithParameter("@customerId", customerId.ToString())
    .WithParameter("@status", status.ToString());

var iterator = _container.GetItemQueryIterator<Order>(
    query,
    requestOptions: new QueryRequestOptions
    {
        PartitionKey = new PartitionKey(customerId.ToString()), // In-partition → 1 RU mínimo
        MaxItemCount = 20
    });
```

**El costo real de Cosmos DB — lo que nadie te dice en el marketing:**

Request Units (RUs) es el modelo de pricing. Cada operación tiene un costo en RUs:
- Leer un item de 1KB por ID: ~1 RU
- Query que escanea 1000 items: ~100-500 RUs (según índices)
- Escritura de un item de 1KB: ~5 RUs

Un sistema con 1M reads/día y 100K writes/día necesita:
- 1M × 1 RU = 1M RUs en reads
- 100K × 5 RU = 500K RUs en writes
- Total: 1.5M RUs/día ÷ 86,400 segundos = ~17 RU/s mínimo
- Precio: ~$0.008/hora por 100 RU/s = ~$12/mes para este baseline
- Pero con picos de tráfico, necesitas aprovisionar más RUs o usar autoscale

Con Azure SQL en el mismo escenario: una instancia de General Purpose 4 vCores a ~$370/mes maneja millones de operaciones. **Para la mayoría de aplicaciones, Azure SQL es 10-50x más barato que Cosmos DB.**

Cosmos DB se justifica cuando:
- Necesitas distribución multi-region activo-activo (write anywhere) — Azure SQL replica solo activo-pasivo
- Tu p99 de latencia debe ser < 10ms garantizado en cualquier región del mundo
- El volumen de datos es multi-TB con sharding horizontal automático requerido
- El schema evoluciona constantemente y no puedes hacer migrations coordinadas

---

## Sección 5 — Managed Identity: el patrón de seguridad central en Azure

> **Conexión con módulo 4:** En [04-07-security-en-system-design.md](../modulo-04-system-design/04-07-security-en-system-design.md) entendiste por qué las credenciales hardcodeadas son el vector de ataque más frecuente. Managed Identity es la implementación de "no credentials en código" en Azure.

### La intuición

Managed Identity es el principio de que una aplicación en Azure no necesita saber una contraseña para acceder a otros servicios de Azure. Azure gestiona la identidad de la aplicación automáticamente — como un empleado que usa su tarjeta de acceso corporativa en lugar de memorizar contraseñas.

**Sin Managed Identity** (el anti-patrón):
```
appsettings.json → "ConnectionString": "Server=...;Password=MyPass123!"
↓
Este secreto vive en el repo, en variables de entorno, en los logs si alguien lo logea por error,
en las herramientas de CI/CD, y en la mente de todos los desarrolladores que alguna vez lo vieron.
```

**Con Managed Identity** (el patrón correcto):
```
La App Service / Container App tiene una identidad en Azure AD.
El Resource (SQL, Service Bus, Key Vault) tiene un RBAC role asignado a esa identidad.
No hay password en ningún lado — Azure verifica la identidad a nivel de plataforma.
```

### Implementación completa con DefaultAzureCredential

```csharp
// DefaultAzureCredential es la abstracción correcta:
// En local development → usa az login credentials (az login en terminal)
// En Azure (App Service, Container Apps, AKS con Workload Identity) → usa Managed Identity automáticamente
// Sin cambio de código entre ambientes.

// ✅ Key Vault con Managed Identity
// La app no sabe el secreto de Key Vault — Azure lo resuelve
builder.Configuration.AddAzureKeyVault(
    vaultUri: new Uri($"https://{builder.Configuration["KeyVault:Name"]}.vault.azure.net/"),
    credential: new DefaultAzureCredential());

// ✅ Azure SQL con Managed Identity
// La connection string NO tiene User ID ni Password — Azure AD autentica la Managed Identity
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(
        "Server=tcp:myserver.database.windows.net,1433;Initial Catalog=mydb;Encrypt=True;",
        sqlOptions =>
        {
            sqlOptions.EnableRetryOnFailure(5, TimeSpan.FromSeconds(30), null);
            // La autenticación Entra ID (Azure AD) se configura a nivel de SqlConnection
        })
    // Para Azure AD auth con EF Core, necesitas el paquete
    // Microsoft.Data.SqlClient que soporta Active Directory Default Authentication
);

// ✅ Azure Service Bus con Managed Identity
builder.Services.AddSingleton(sp =>
    new ServiceBusClient(
        fullyQualifiedNamespace: "mynamespace.servicebus.windows.net",
        credential: new DefaultAzureCredential()));

// ✅ Azure Blob Storage con Managed Identity
builder.Services.AddSingleton(sp =>
    new BlobServiceClient(
        serviceUri: new Uri("https://mystorage.blob.core.windows.net"),
        credential: new DefaultAzureCredential()));

// ✅ Azure Event Hubs con Managed Identity
builder.Services.AddSingleton(sp =>
    new EventHubProducerClient(
        fullyQualifiedNamespace: "mynamespace.eventhubs.windows.net",
        eventHubName: "telemetry",
        credential: new DefaultAzureCredential()));
```

### Asignar RBAC roles — el paso que los developers olvidan

Managed Identity por sí sola no da acceso — necesitas asignar el Role correcto en el Resource:

```bash
# Dar acceso a Key Vault — usando Azure CLI
# Primero obtener el Object ID de la Managed Identity de la App Service
MANAGED_IDENTITY_OBJECT_ID=$(az webapp identity show \
    --name my-api \
    --resource-group my-rg \
    --query principalId \
    --output tsv)

# Asignar el rol "Key Vault Secrets User" (leer secretos) a la Managed Identity
az role assignment create \
    --assignee-object-id $MANAGED_IDENTITY_OBJECT_ID \
    --role "Key Vault Secrets User" \
    --scope "/subscriptions/{sub-id}/resourceGroups/my-rg/providers/Microsoft.KeyVault/vaults/my-vault"

# Para Service Bus: rol "Azure Service Bus Data Receiver" para leer mensajes
az role assignment create \
    --assignee-object-id $MANAGED_IDENTITY_OBJECT_ID \
    --role "Azure Service Bus Data Receiver" \
    --scope "/subscriptions/{sub-id}/resourceGroups/my-rg/providers/Microsoft.ServiceBus/namespaces/my-namespace"

# Para Azure SQL: la Managed Identity necesita ser creada como usuario en la BD
# Esto se hace en T-SQL — el RBAC de Azure no aplica dentro de SQL a nivel de tabla
```

```sql
-- En Azure SQL: crear el usuario para la Managed Identity
-- Primero configurar el Entra ID Admin en el SQL Server (desde Azure Portal)
-- Luego en la base de datos:

CREATE USER [my-api] FROM EXTERNAL PROVIDER; -- 'my-api' es el nombre de la App Service
ALTER ROLE db_datareader ADD MEMBER [my-api];
ALTER ROLE db_datawriter ADD MEMBER [my-api];
-- Para operaciones de schema: db_ddladmin (solo en pipelines de migration)
```

### El patrón de secretos híbrido para configuración

En producción real, la configuración tiene dos capas:

```json
// appsettings.json — valores no sensitivos (en el repo)
{
    "KeyVault": {
        "Name": "my-vault-prod"     // El nombre del vault es público — no es un secreto
    },
    "Database": {
        "Server": "myserver.database.windows.net",  // No sensitivo
        "Name": "mydb"
    },
    "ServiceBus": {
        "Namespace": "mynamespace"  // No sensitivo
    }
}
```

```
// Azure Key Vault — valores sensitivos (nunca en el repo)
Database--Password    → "No existe — usamos Managed Identity"
ExternalApi--ApiKey   → "sk-abc123..."  (único secreto real que necesita Key Vault)
Jwt--SigningKey       → "-----BEGIN EC PRIVATE KEY-----..."
```

La regla: si no tiene un secreto real (password, API key, private key) — va en `appsettings.json`. Si tiene un secreto — va en Key Vault, accedido con Managed Identity. La connection string de Azure SQL con Managed Identity no tiene password → va en `appsettings.json`.

---

## Sección 6 — Tabla maestra de servicios Azure para arquitectos .NET

| Servicio | Cuándo usarlo | Cuándo NO usarlo |
|---|---|---|
| **Azure App Service** | APIs sencillas sin containerización, equipos sin expertise Docker | Microservicios que necesitan escalar a cero, workloads containerizados |
| **Azure Container Apps** | APIs containerizadas, workers de queue, escala a cero, managed K8s | Cuando necesitas control granular de scheduling o node pools heterogéneos |
| **Azure Kubernetes Service** | Control total de infra, node pools especializados, multi-cloud | Equipos sin expertise K8s, workloads que Container Apps puede manejar |
| **Azure Functions** | Event-driven, workloads < 10 min, pago por ejecución | APIs con SLA de baja latencia (cold start), procesos de larga duración |
| **Durable Functions** | Workflows multi-paso con estado, fan-out/fan-in, aprobaciones manuales | Workloads simples sin estado — overhead innecesario |
| **Azure SQL** | Datos relacionales, transacciones ACID, la mayoría de aplicaciones de negocio | Distribución global write-anywhere, datos multi-TB con sharding horizontal |
| **Cosmos DB** | Distribución multi-region, baja latencia global garantizada, schema variable | Presupuesto ajustado, datos relacionales, equipos sin expertise en NoSQL |
| **Azure Cache (Redis)** | Cache distribuida, session state, pub/sub de baja latencia | Datos persistentes (Redis no es una BD), datos grandes (límite de memoria) |
| **Service Bus Queues** | Task queues con DLQ, retry, y transacciones | Streaming de alta velocidad (Event Hubs es mejor) |
| **Service Bus Topics** | Pub/Sub con múltiples suscriptores y filtros | Fan-out simple a webhooks HTTP (Event Grid es mejor) |
| **Event Hubs** | Streaming de telemetría, logs, IoT a alta velocidad con replay | Reliable messaging con DLQ (Service Bus es mejor) |
| **Event Grid** | Reaccionar a eventos de Azure resources, webhook delivery | Ordering garantizado, retry semántico complejo |
| **Azure Key Vault** | Secretos, certificados, claves criptográficas | Configuración no sensitiva — va en appsettings.json |
| **Managed Identity** | Todo acceso a servicios Azure desde código | Cuando el servicio no soporta Azure AD auth (casos raros) |
| **Azure Front Door** | CDN global + WAF + routing inteligente para APIs globales | Aplicaciones single-region sin requisitos de CDN |
| **API Management** | Gateway con throttling, transformación, portal de developers | Microservicios internos sin exposición pública |

---

## Recursos y siguiente paso

**📚 Microsoft Learn — paths gratuitos con sandboxes reales:**
- **AZ-204 (Developer)** → [learn.microsoft.com/certifications/azure-developer](https://learn.microsoft.com/certifications/azure-developer) — cubre Functions, Container Apps, Service Bus, Cosmos DB
- **AZ-305 (Solutions Architect)** → [learn.microsoft.com/certifications/azure-solutions-architect](https://learn.microsoft.com/certifications/azure-solutions-architect) — decisiones de arquitectura, cuándo usar qué

**📚 Pluralsight — Azure Architecture path:**
- Módulo de *Azure Container Apps* — configuración de KEDA y Dapr
- Módulo de *Azure Service Bus* — patterns de messaging avanzados

**Azure Architecture Center** — para entrevistas de diseño:
- [Azure Application Architecture Guide](https://learn.microsoft.com/azure/architecture/guide/) — el framework oficial de arquitectura en Azure
- [Patterns de referencia](https://learn.microsoft.com/azure/architecture/patterns/) — los patterns más usados con implementación en Azure

---

## Checklist de salida — 05-02

Antes de considerar este archivo completo:

- [ ] Dado cualquier workload nuevo, puedo trazar el árbol de decisión de compute con razonamiento explícito
- [ ] Puedo explicar cuándo Azure Service Bus Sessions es necesario y qué problema de ordering resuelve
- [ ] Puedo argumentar concretamente cuándo Cosmos DB no es la elección correcta (y la mayoría del tiempo no lo es)
- [ ] Puedo implementar Managed Identity con DefaultAzureCredential y asignar los RBAC roles correctos
- [ ] Puedo diferenciar Event Hubs de Service Bus con casos de uso concretos, no solo con definiciones
- [ ] Puedo estimar el costo relativo de Cosmos DB vs Azure SQL para un caso de uso dado

---

**Siguiente archivo:** [05-03-python-suficiente.md](./05-03-python-suficiente.md)
