# 05-01 — .NET Avanzado: Los Internals que Distinguen a un Staff

> **Prerequisito:** [03-04-clean-architecture.md](../modulo-03-software-design/03-04-clean-architecture.md) y [03-06-cqrs-event-sourcing.md](../modulo-03-software-design/03-06-cqrs-event-sourcing.md) — Este archivo implementa esos patrones en .NET. Si aún no tienes el modelo mental de la Dependency Rule y por qué CQRS separa modelos de lectura/escritura, regresa a esos archivos primero.
>
> **Lo que este archivo NO es:** Una introducción a ASP.NET Core. Si necesitas aprender a registrar una ruta o usar EF Core por primera vez, la documentación oficial te sirve mejor. Este archivo asume que ya lo usas — y te explica qué ocurre por debajo.
>
> **Principio de este archivo:** "Ya sabes cómo. Ahora entiendes por qué."
>
> **🎯 Recurso Pluralsight:** El path **"ASP.NET Core 8 Fundamentals"** de Gill Cleeren y el path **"C# Advanced Topics"** de Deborah Kurata. Ábrete específicamente el módulo de *Middleware* y el módulo de *async/await* de estos paths **después** de leer las secciones correspondientes aquí. Este archivo construye el modelo mental — Pluralsight añade el detalle adicional y los ejercicios.

---

## Sección 1 — ASP.NET Core: el middleware pipeline real

### La intuición

La mayoría de developers sabe que el middleware "intercepta" los requests. Pero eso es una metáfora incompleta. El pipeline de middleware de ASP.NET Core no es un bus de eventos ni un sistema de plugins — es una **cadena de delegados** construida en tiempo de startup y ejecutada secuencialmente en cada request.

La diferencia importa porque determina exactamente por qué el orden de registro afecta el comportamiento, y por qué un middleware puede cortocircuitar el pipeline sin ningún mecanismo especial.

### El pipeline como cadena de delegados

```csharp
// Lo que escribe el developer en Program.cs:
var app = builder.Build();

app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

```csharp
// Lo que .NET construye internamente durante el startup:
// Una cadena de RequestDelegate donde cada middleware envuelve al siguiente.

// RequestDelegate es simplemente: Func<HttpContext, Task>

// ASP.NET Core construye la cadena "de adentro hacia afuera":
// El último en registrarse es el primero en ser envuelto.

// Resultado conceptual (simplificado):
RequestDelegate pipeline = context =>
    routingMiddleware(context, ctx =>
        authenticationMiddleware(ctx, c =>
            authorizationMiddleware(c, innerContext =>
                endpointMiddleware(innerContext)
            )
        )
    );
```

**Por qué esto es importante:** Cuando el primer middleware llama a `_next(context)`, está invocando directamente ese delegado — el siguiente middleware en la cadena. No hay dispatch, no hay bus, no hay overhead de reflection. Es una llamada de función directa. Esto es parte de por qué ASP.NET Core tiene la performance que tiene.

### El orden importa — y exactamente por qué

```csharp
// ❌ INCORRECTO — Authorization antes de Authentication
app.UseAuthorization();   // 1. Intenta verificar permisos
app.UseAuthentication();  // 2. Establece quién es el usuario — pero ya es tarde

// El problema: UseAuthorization necesita saber quién es el usuario (HttpContext.User)
// para verificar si tiene permiso. Si llega antes de Authentication,
// HttpContext.User es un ClaimsPrincipal anónimo vacío.
// Authorization falla silenciosamente o lanza 403 a todo el mundo.

// ✅ CORRECTO — orden que refleja la lógica de negocio
app.UseRouting();            // 1. ¿A qué endpoint va este request? (determina el endpoint seleccionado)
app.UseAuthentication();     // 2. ¿Quién hace el request? (popula HttpContext.User)
app.UseAuthorization();      // 3. ¿Tiene permiso? (lee HttpContext.User, verifica policies)
app.UseRateLimiting();       // 4. ¿No está excediendo el límite de requests?
app.MapControllers();        // 5. Ejecutar el endpoint seleccionado
```

El orden correcto refleja la lógica: primero saber a dónde va el request, luego quién lo hace, luego si puede hacerlo. Es el mismo principio que en un edificio: primero la recepción te dirige a la sala correcta, luego verifican tu ID, luego verifican si tienes acceso a esa sala.

### Implementar un middleware personalizado — la forma correcta

```csharp
// ✅ Clase middleware — la forma recomendada para middleware no trivial
public class CorrelationIdMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<CorrelationIdMiddleware> _logger;

    // Los middlewares se instancian UNA SOLA VEZ en startup (son Singleton por naturaleza).
    // Por eso se inyectan aquí: ILogger es Singleton y es seguro.
    // ⚠️ NUNCA inyectes Scoped services en el constructor del middleware.
    // Para servicios Scoped → úsalos como parámetros en InvokeAsync.
    public CorrelationIdMiddleware(
        RequestDelegate next,
        ILogger<CorrelationIdMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    // InvokeAsync puede tener parámetros adicionales — DI los resuelve por request.
    // Esto es el mecanismo que permite inyectar Scoped services correctamente.
    public async Task InvokeAsync(HttpContext context)
    {
        var correlationId = context.Request.Headers["X-Correlation-Id"]
            .FirstOrDefault()
            ?? Guid.NewGuid().ToString("N");

        // Agregar al Items dictionary del HttpContext — disponible en todo el pipeline
        context.Items["CorrelationId"] = correlationId;

        // OnStarting se ejecuta justo antes de que se envíen los headers al cliente.
        // No podemos agregar headers después de que el body empiece a escribirse.
        context.Response.OnStarting(() =>
        {
            context.Response.Headers["X-Correlation-Id"] = correlationId;
            return Task.CompletedTask;
        });

        // BeginScope: todos los logs dentro de este request incluirán el CorrelationId
        // automáticamente — sin pasarlo manualmente a cada logger.
        using (_logger.BeginScope(new Dictionary<string, object>
        {
            ["CorrelationId"] = correlationId,
            ["RequestPath"] = context.Request.Path.Value ?? string.Empty
        }))
        {
            await _next(context); // Pasar al siguiente middleware en la cadena
        }
    }
}

// Extensión para registro limpio — no expone el tipo directamente en Program.cs
public static class CorrelationIdMiddlewareExtensions
{
    public static IApplicationBuilder UseCorrelationId(
        this IApplicationBuilder builder)
        => builder.UseMiddleware<CorrelationIdMiddleware>();
}

// En Program.cs:
app.UseCorrelationId(); // Siempre antes de los demás — necesitamos el ID en todos los logs
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
```

### Short-circuit: cuándo el middleware decide no continuar

Un middleware puede retornar sin llamar a `_next` — esto es un short-circuit. No hay nada especial en el mecanismo: simplemente no invocas el siguiente delegado.

```csharp
public async Task InvokeAsync(HttpContext context)
{
    // Ejemplo: verificar un API key header antes de Authentication
    if (!context.Request.Headers.TryGetValue("X-Api-Key", out var apiKey)
        || !IsValidApiKey(apiKey))
    {
        // Short-circuit: responder directamente sin llamar a _next
        context.Response.StatusCode = StatusCodes.Status401Unauthorized;
        await context.Response.WriteAsJsonAsync(new { error = "Invalid API key" });
        return; // No llamamos a _next — el pipeline se detiene aquí
    }

    await _next(context); // Solo llegamos aquí si la validación pasó
}
```

⚠️ **Gotcha de producción:** Si haces short-circuit después de que el body de la respuesta ya empezó a escribirse (`context.Response.HasStarted == true`), no puedes cambiar el status code. Siempre verifica antes de escribir.

---

## Sección 2 — Dependency Injection: los internals que generan bugs en producción

### Los tres lifetimes y su razonamiento

La elección de lifetime no es cosmética — define el contrato de concurrencia y estado de tu servicio:

```csharp
// SINGLETON — una instancia para toda la vida del proceso
// Contrato: el servicio DEBE ser thread-safe. Nunca tiene estado mutable
// que dependa de un request individual.
builder.Services.AddSingleton<IMemoryCache, MemoryCache>(); // ✅
builder.Services.AddSingleton<HttpClient>(); // ✅ con HttpClientFactory
builder.Services.AddSingleton<DbContext>(); // ❌ DbContext no es thread-safe

// SCOPED — una instancia por request HTTP (o por scope creado manualmente)
// Contrato: puede tener estado que dura el tiempo de un request.
// Es el lifetime más frecuente para repositorios, Unit of Work, DbContext.
builder.Services.AddScoped<AppDbContext>(); // ✅ una instancia por request
builder.Services.AddScoped<IOrderRepository, SqlOrderRepository>(); // ✅

// TRANSIENT — nueva instancia cada vez que se resuelve
// Contrato: el servicio es ligero de crear y stateless.
// Si el servicio maneja recursos costosos (conexiones, streams), evitar Transient.
builder.Services.AddTransient<IEmailSender, SmtpEmailSender>(); // ✅ si SmtpEmailSender es ligero
builder.Services.AddTransient<AppDbContext>(); // ❌ si hay múltiples resoluciones por request
```

### Captive Dependency — el bug silencioso más común

Este es el bug que más frecuentemente aparece en bases de código medianas que nadie ha diseñado con cuidado:

```csharp
// ❌ CAPTIVE DEPENDENCY
// OrderProcessor es Singleton → se crea UNA VEZ y vive para siempre.
// IOrderRepository es Scoped → debería crearse UNA VEZ POR REQUEST.
//
// Problema: cuando OrderProcessor se construye, captura la instancia de
// IOrderRepository que existía en ese momento (la primera, al inicio).
// Esa instancia de repository es Scoped — debería haberse descartado
// al terminar el primer request, pero OrderProcessor la mantiene viva.
// Todos los requests futuros usarán el MISMO repository Scoped de siempre.

public class OrderProcessor // Registrado como Singleton
{
    private readonly IOrderRepository _repository; // Scoped — PROBLEMA

    public OrderProcessor(IOrderRepository repository) // Captura en construcción
    {
        _repository = repository; // Esta instancia nunca se renovará
    }

    public async Task ProcessAsync(OrderId orderId)
    {
        // Este repository puede tener un DbContext desconectado,
        // transacciones abiertas de requests anteriores, o datos stale en cache.
        await _repository.GetByIdAsync(orderId);
    }
}
```

```csharp
// .NET detecta este error en Development con ValidateScopes = true
// (habilitado por defecto en Development en ASP.NET Core):
// System.InvalidOperationException: Cannot consume scoped service 'IOrderRepository'
// from singleton 'OrderProcessor'.

// Pero en Production, ValidateScopes es false por defecto — el error pasa silencioso.
// Siempre activar en ambientes de staging:
builder.Host.UseDefaultServiceProvider(options =>
{
    options.ValidateScopes = true;
    options.ValidateOnBuild = true; // Detecta en startup, no en primer request
});
```

```csharp
// ✅ CORRECTO — IServiceScopeFactory para crear scopes manualmente
// El Singleton recibe la factory (que sí es Singleton),
// y crea un Scope nuevo cuando necesita el Scoped service.

public class OrderProcessor // Singleton
{
    private readonly IServiceScopeFactory _scopeFactory;
    private readonly ILogger<OrderProcessor> _logger;

    public OrderProcessor(IServiceScopeFactory scopeFactory, ILogger<OrderProcessor> logger)
    {
        _scopeFactory = scopeFactory;
        _logger = logger;
    }

    public async Task ProcessAsync(OrderId orderId)
    {
        // Crear un scope nuevo — el repository dentro vivirá solo hasta que el scope se descarte
        await using var scope = _scopeFactory.CreateAsyncScope();
        var repository = scope.ServiceProvider.GetRequiredService<IOrderRepository>();

        var order = await repository.GetByIdAsync(orderId);
        _logger.LogInformation("Processing order {OrderId}", orderId);
        // Al salir del using → scope se descarta → repository se descarta
    }
}
```

### Keyed Services (.NET 8+) — múltiples implementaciones del mismo tipo

```csharp
// Problema clásico antes de .NET 8: ¿cómo registrar dos implementaciones
// de IPaymentGateway y elegir cuál inyectar?
// Solución pre-.NET 8: factory pattern manual o named registrations con tricks.

// ✅ .NET 8+ — Keyed Services: nativo, limpio, sin workarounds
builder.Services.AddKeyedScoped<IPaymentGateway, StripeGateway>("stripe");
builder.Services.AddKeyedScoped<IPaymentGateway, PayPalGateway>("paypal");
builder.Services.AddKeyedScoped<IPaymentGateway, MercadoPagoGateway>("mercadopago");

// Resolución en el constructor
public class CheckoutService
{
    private readonly IPaymentGateway _primaryGateway;
    private readonly IPaymentGateway _backupGateway;

    public CheckoutService(
        [FromKeyedServices("stripe")] IPaymentGateway primaryGateway,
        [FromKeyedServices("mercadopago")] IPaymentGateway backupGateway)
    {
        _primaryGateway = primaryGateway;
        _backupGateway = backupGateway;
    }
}

// Resolución dinámica — cuando la key se conoce en runtime
public class PaymentRouter
{
    private readonly IServiceProvider _provider;

    public PaymentRouter(IServiceProvider provider) => _provider = provider;

    public async Task<PaymentResult> RoutePaymentAsync(string gatewayKey, decimal amount)
    {
        var gateway = _provider.GetRequiredKeyedService<IPaymentGateway>(gatewayKey);
        return await gateway.ChargeAsync(amount);
    }
}
```

---

## Sección 3 — EF Core internals: cómo funciona por debajo

### El Change Tracker — el motor invisible de EF Core

El Change Tracker es el mecanismo central de EF Core. Cuando cargas una entidad, EF Core guarda un snapshot del estado en ese momento. En `SaveChanges`, compara el estado actual con el snapshot y genera el SQL necesario.

```csharp
// Lo que ocurre internamente cuando cargas una entidad
var order = await _context.Orders
    .Include(o => o.Items)
    .FirstAsync(o => o.Id == orderId);

// EF Core ha hecho:
// 1. Ejecutado el SQL SELECT con JOIN
// 2. Materializarlo como objeto Order en memoria
// 3. Guardado un snapshot del estado inicial (internamente: una copia de los valores)
// 4. Registrado la entidad en el ChangeTracker con estado "Unchanged"

// Modificas la entidad
order.Status = OrderStatus.Confirmed;
order.UpdatedAt = DateTime.UtcNow;

// EF Core detecta el cambio inmediatamente (porque usa proxies o porque
// el ChangeTracker monitorea las propiedades)
Console.WriteLine(_context.Entry(order).State); // Modified

// SaveChanges: EF Core compara el snapshot con el estado actual
// Genera únicamente: UPDATE Orders SET Status = @p1, UpdatedAt = @p2 WHERE Id = @p3
// Solo las columnas que cambiaron — no un UPDATE de toda la fila.
await _context.SaveChangesAsync();
```

### El N+1 Problem — el bug de performance más frecuente

Si solo lees una sección de este archivo para entrevistas de arquitectura, que sea esta.

```csharp
// ❌ N+1 — código inocente que destruye el performance en producción
var customers = await _context.Customers
    .Where(c => c.IsActive)
    .ToListAsync(); // Query 1: SELECT * FROM Customers WHERE IsActive = 1

// EF Core tiene LazyLoading habilitado (si configuraste los navigation properties
// con virtual y el paquete Microsoft.EntityFrameworkCore.Proxies)
foreach (var customer in customers)
{
    // Cada acceso a Orders dispara una query separada
    // Si tienes 100 clientes → 101 queries en total (1 + 100)
    var orderCount = customer.Orders.Count; // Query 2, 3, 4... hasta N+1
    Console.WriteLine($"{customer.Name}: {orderCount} orders");
}

// En producción: con 1000 clientes = 1001 queries a la BD
// Síntoma en logs: el endpoint /api/customers tarda 3-5 segundos en cargar
```

```csharp
// ✅ Eager Loading — todo en una query con JOIN
var customers = await _context.Customers
    .Where(c => c.IsActive)
    .Include(c => c.Orders)           // JOIN a Orders
    .ThenInclude(o => o.Items)        // JOIN a OrderItems (nested)
    .ToListAsync();
// Una sola query: SELECT c.*, o.*, i.*
//                 FROM Customers c
//                 LEFT JOIN Orders o ON o.CustomerId = c.Id
//                 LEFT JOIN OrderItems i ON i.OrderId = o.Id
//                 WHERE c.IsActive = 1

// Pero ⚠️ cuidado: Include carga TODO. Si solo necesitas conteos,
// esto puede ser peor — cargas miles de rows de Items solo para contar.

// ✅ Projection — la mejor solución cuando solo necesitas datos específicos
var customerSummaries = await _context.Customers
    .Where(c => c.IsActive)
    .Select(c => new CustomerSummaryDto(
        c.Id,
        c.Name,
        c.Orders.Count,                          // COUNT en SQL — no carga las Orders
        c.Orders.Sum(o => (decimal?)o.Total) ?? 0 // SUM en SQL — no carga los detalles
    ))
    .ToListAsync();
// Una query con COUNT y SUM en SQL — la más eficiente
```

```csharp
// Cómo detectar N+1 en desarrollo — logging de EF Core
builder.Services.AddDbContext<AppDbContext>(options =>
    options
        .UseSqlServer(connectionString)
        // En Development: ver TODAS las queries con sus parámetros
        .LogTo(
            message => Debug.WriteLine(message),
            LogLevel.Information,
            DbContextLoggerOptions.DefaultWithLocalTime)
        .EnableSensitiveDataLogging()  // Ver los valores reales (no en Production)
        .EnableDetailedErrors());

// En la consola verás:
// info: Executed DbCommand (2ms) [Parameters=[@p0='?' (DbType = Guid)], ...]
//       SELECT * FROM Orders WHERE CustomerId = @p0
// ... repetido N veces → N+1 detectado
```

### Bulk operations con EF Core 7+ — cuando necesitas escala real

```csharp
// ❌ LENTO — el patrón "clásico" que destruye el performance en bulk operations
var expiredOrders = await _context.Orders
    .Where(o => o.Status == OrderStatus.Pending
             && o.CreatedAt < DateTime.UtcNow.AddDays(-7))
    .ToListAsync(); // Carga TODOS los objetos en memoria

foreach (var order in expiredOrders)
    order.Status = OrderStatus.Expired; // Modifica cada objeto en memoria

// SaveChanges genera N UPDATE statements — una por cada Order en el foreach
await _context.SaveChangesAsync();
// Con 10,000 órdenes expiradas → 10,001 queries (1 SELECT + 10,000 UPDATEs)

// ✅ RÁPIDO — EF Core 7+ ExecuteUpdateAsync
// Una sola query UPDATE — sin cargar objetos en memoria
var affected = await _context.Orders
    .Where(o => o.Status == OrderStatus.Pending
             && o.CreatedAt < DateTime.UtcNow.AddDays(-7))
    .ExecuteUpdateAsync(setters => setters
        .SetProperty(o => o.Status, OrderStatus.Expired)
        .SetProperty(o => o.UpdatedAt, DateTime.UtcNow)
        .SetProperty(o => o.ExpiredReason, "Timeout after 7 days"));
// SQL generado: UPDATE Orders
//               SET Status = 'Expired', UpdatedAt = NOW(), ExpiredReason = 'Timeout...'
//               WHERE Status = 'Pending' AND CreatedAt < DATEADD(day, -7, NOW())
// Una query. Milliseconds. Independiente del número de filas afectadas.

// ✅ RÁPIDO — ExecuteDeleteAsync para eliminaciones masivas
await _context.AuditLogs
    .Where(log => log.CreatedAt < DateTime.UtcNow.AddYears(-2))
    .ExecuteDeleteAsync();
// SQL: DELETE FROM AuditLogs WHERE CreatedAt < '2024-01-01'
```

⚠️ **Trade-off crítico de ExecuteUpdate/ExecuteDelete:** Estas operaciones **bypass el Change Tracker** completamente. EF Core no sabe que esos registros cambiaron — si tienes entidades cargadas en memoria de la misma sesión, quedarán en un estado inconsistente. Úsalas para operaciones batch desconectadas, no mezcladas con operaciones del Change Tracker en la misma request.

---

## Sección 4 — Async/Await internals: la state machine

Este es el tema que más diferencia a un Senior de un Staff en .NET. No porque el código cambie — sino porque entender qué genera el compilador te permite diagnosticar bugs de threading, entender el impacto en performance, y responder preguntas de entrevista que el 95% de developers no puede responder correctamente.

### Lo que el compilador genera

```csharp
// Lo que escribes — aparentemente simple:
public async Task<Order> GetOrderAsync(Guid orderId, CancellationToken ct = default)
{
    var order = await _repository.GetByIdAsync(orderId, ct);
    await _cache.SetAsync($"order:{orderId}", order, ct);
    return order;
}
```

```csharp
// Lo que el compilador C# genera — la state machine:
// (Simplificado para claridad — el código real tiene más optimizaciones)

// El método original se convierte en este stub:
public Task<Order> GetOrderAsync(Guid orderId, CancellationToken ct = default)
{
    var stateMachine = new GetOrderAsync__StateMachine
    {
        __this = this,
        orderId = orderId,
        ct = ct,
        __state = -1  // Estado inicial: no empezado
    };
    stateMachine.__builder = AsyncTaskMethodBuilder<Order>.Create();
    stateMachine.__builder.Start(ref stateMachine);
    return stateMachine.__builder.Task; // Retorna el Task ANTES de que termine
}

// La state machine real:
private struct GetOrderAsync__StateMachine : IAsyncStateMachine
{
    // Campos para capturar el contexto del método original
    public int __state;
    public AsyncTaskMethodBuilder<Order> __builder;
    public OrderService __this;
    public Guid orderId;
    public CancellationToken ct;

    // Variables locales del método original — ahora son campos de la struct
    private Order __order;
    private TaskAwaiter<Order> __awaiter1;
    private TaskAwaiter __awaiter2;

    public void MoveNext()
    {
        switch (__state)
        {
            case -1: // Primera ejecución
                __awaiter1 = __this._repository
                    .GetByIdAsync(orderId, ct)
                    .GetAwaiter();

                if (!__awaiter1.IsCompleted)
                {
                    // La operación es asíncrona — suspender y devolver control al caller
                    __state = 0;
                    __builder.AwaitUnsafeOnCompleted(ref __awaiter1, ref this);
                    return; // ← Aquí el thread se libera para procesar otros requests
                }
                goto case 0; // Si ya completó síncronamente, continuar sin suspender

            case 0: // Reanuda después del primer await
                __order = __awaiter1.GetResult(); // Obtener el resultado o relanzar excepción

                __awaiter2 = __this._cache
                    .SetAsync($"order:{orderId}", __order, ct)
                    .GetAwaiter();

                if (!__awaiter2.IsCompleted)
                {
                    __state = 1;
                    __builder.AwaitUnsafeOnCompleted(ref __awaiter2, ref this);
                    return;
                }
                goto case 1;

            case 1: // Reanuda después del segundo await
                __awaiter2.GetResult();
                __builder.SetResult(__order); // Completar el Task con el resultado
                break;
        }
    }
}
```

**Por qué esto importa en producción:**

1. **Las variables locales del método async se convierten en campos de la struct.** Si tu método async tiene muchas variables locales, la struct es más grande. En hot paths de alta concurrencia, esto puede importar para la presión sobre el GC.

2. **La struct se puede boxear en el heap si el método suspende realmente.** Si el awaitable está completo (`IsCompleted == true`) cuando haces `await`, la ejecución continúa síncronamente sin asignación en el heap. Si se suspende, la struct se mueve al heap — esto es una asignación.

3. **`ValueTask<T>` existe exactamente por esta razón:** Para hot paths donde la mayoría de las llamadas completan síncronamente, `ValueTask<T>` evita la asignación del heap cuando no hay suspensión real.

### ConfigureAwait(false) — cuándo importa y cuándo es ruido

```csharp
// ¿Qué es SynchronizationContext?
// En aplicaciones UI (WPF, WinForms), el SynchronizationContext captura el thread de UI.
// Después de un await, el código reanuda en el thread de UI (necesario para modificar controles).
// En ASP.NET Core moderno (desde ASP.NET Core 1.0), NO hay SynchronizationContext.
// El scheduler de ASP.NET Core no usa este mecanismo.

// ConfigureAwait(false): "No necesito volver al mismo contexto después del await.
// Puedes reanudar en cualquier thread disponible del ThreadPool."

// ✅ OBLIGATORIO en código de librería/NuGet:
// Tu librería puede ser consumida desde WPF, WinForms, o ASP.NET Core.
// Si no pones ConfigureAwait(false) en código de librería y alguien la usa desde WPF,
// el await intentará capturar el SynchronizationContext de UI — overhead o deadlock.
public async Task<string> GetDataAsync(string url)
{
    var response = await _httpClient
        .GetAsync(url)
        .ConfigureAwait(false); // No capturar contexto — librería agnóstica

    return await response.Content
        .ReadAsStringAsync()
        .ConfigureAwait(false);
}

// ⚠️ EN ASP.NET Core: técnicamente innecesario, pero no es error.
// ASP.NET Core no tiene SynchronizationContext, así que ConfigureAwait(false)
// no cambia el comportamiento — simplemente no hace nada diferente.
// La mayoría de developers lo omiten en código de aplicación ASP.NET Core.
// Si tu equipo tiene una convención, síguelo. Si no, puedes omitirlo.

// ❌ INCORRECTO — el caso que sí genera deadlock (en frameworks con SC):
public string GetDataSync() // Método síncrono
{
    // .Result en un contexto con SynchronizationContext puede deadloquear:
    // El thread principal espera el Task, pero el Task necesita el thread principal para reanudar.
    return GetDataAsync("https://api.example.com").Result; // 🔒 Deadlock en WPF/WinForms
}
```

### async void — el anti-patrón más peligroso en .NET

```csharp
// ❌ async void — NUNCA en código de aplicación
public async void ProcessOrderAsync(Guid orderId)
{
    try
    {
        var order = await _repository.GetByIdAsync(orderId);
        await _notificationService.SendAsync(order.CustomerEmail);
    }
    catch (Exception ex)
    {
        // Esta excepción NO puede ser capturada con try/catch por el caller.
        // async void lanza excepciones en el SynchronizationContext actual.
        // En aplicaciones de consola o ASP.NET Core: crash del proceso.
        _logger.LogError(ex, "Failed to process order");
        // El log puede no escribirse si el proceso ya crashó.
    }
}

// El problema: el caller no puede hacer await en un método void
void SomeMethod()
{
    ProcessOrderAsync(orderId); // No puedes hacer await aquí
    // No sabes si completó, si falló, ni cuándo termina.
    // Estás en fire-and-forget sin control ni visibilidad.
}
```

```csharp
// ✅ SIEMPRE retornar Task — el contrato correcto
public async Task ProcessOrderAsync(Guid orderId, CancellationToken ct = default)
{
    var order = await _repository.GetByIdAsync(orderId, ct);
    await _notificationService.SendAsync(order.CustomerEmail, ct);
}

// El caller puede hacer await y manejar excepciones correctamente
await ProcessOrderAsync(orderId, ct);

// ✅ Para fire-and-forget real — con manejo explícito de errores
// Si realmente necesitas lanzar trabajo en background sin await:
_ = Task.Run(async () =>
{
    try
    {
        await ProcessOrderAsync(orderId);
    }
    catch (Exception ex)
    {
        // Logging explícito — no dejamos las excepciones en el vacío
        _logger.LogError(ex, "Background processing failed for order {OrderId}", orderId);
    }
}, CancellationToken.None);
// O mejor: usar IHostedService / BackgroundService para trabajo de background

// La única excepción válida para async void: event handlers en UI
private async void Button_Click(object sender, EventArgs e)
{
    // Los event handlers tienen firma void — aquí no puedes evitarlo
    await DoSomethingAsync();
    // Pero siempre pon try/catch dentro si el método puede lanzar
}
```

---

## Sección 5 — Minimal APIs vs Controllers: la decisión con criterio

### Lo que cada uno es realmente

Minimal APIs y Controllers no son dos formas de hacer lo mismo — son dos modelos de programación con diferentes trade-offs:

- **Controllers** usan reflection para descubrir actions, filtros, y model binding. Son más expresivos para APIs complejas, soportan attribute-based configuration, y tienen el mayor ecosistema de extensiones.
- **Minimal APIs** son delegados registrados directamente en el pipeline de routing. Tienen un startup más rápido (menos reflection), menos capas de abstracción, y son ideales para microservicios con pocos endpoints.

```csharp
// Minimal APIs — para endpoints simples y microservicios
// Nota: se pueden organizar en grupos para mantener el código limpio

var ordersGroup = app.MapGroup("/api/orders")
    .RequireAuthorization()
    .WithTags("Orders")
    .WithOpenApi();

ordersGroup.MapGet("/{id:guid}", async (
    Guid id,
    IMediator mediator,           // Inyección directa por parámetro
    CancellationToken ct) =>
{
    var result = await mediator.Send(new GetOrderByIdQuery(id), ct);
    return result is null
        ? Results.NotFound()
        : Results.Ok(result);
})
.WithName("GetOrderById")
.Produces<OrderDto>(StatusCodes.Status200OK)
.Produces(StatusCodes.Status404NotFound);

ordersGroup.MapPost("/", async (
    CreateOrderRequest request,
    IMediator mediator,
    CancellationToken ct) =>
{
    var result = await mediator.Send(new CreateOrderCommand(request), ct);
    return Results.CreatedAtRoute("GetOrderById", new { id = result.OrderId }, result);
})
.WithName("CreateOrder")
.Produces<CreateOrderResponse>(StatusCodes.Status201Created)
.ProducesValidationProblem();
```

```csharp
// Controllers — para APIs complejas con muchos endpoints y filtros
[ApiController]
[Route("api/[controller]")]
[Authorize]
[ServiceFilter(typeof(AuditFilter))]         // Filtros globales para el controller
[ProducesResponseType(StatusCodes.Status401Unauthorized)]
[ProducesResponseType(StatusCodes.Status403Forbidden)]
public class OrdersController : ControllerBase
{
    private readonly IMediator _mediator;

    public OrdersController(IMediator mediator) => _mediator = mediator;

    [HttpGet("{id:guid}")]
    [ProducesResponseType<OrderDto>(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public async Task<IActionResult> GetById(
        Guid id,
        CancellationToken ct)
    {
        var result = await _mediator.Send(new GetOrderByIdQuery(id), ct);
        return result is null ? NotFound() : Ok(result);
    }

    [HttpPost]
    [Authorize(Policy = "OrderWrite")]
    [ValidateAntiForgeryToken]
    [ProducesResponseType<CreateOrderResponse>(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status422UnprocessableEntity)]
    public async Task<IActionResult> Create(
        [FromBody] CreateOrderRequest request,
        CancellationToken ct)
    {
        var result = await _mediator.Send(new CreateOrderCommand(request), ct);
        return CreatedAtAction(nameof(GetById), new { id = result.OrderId }, result);
    }
}
```

### La tabla de decisión con criterios reales

| Criterio | Minimal APIs | Controllers |
|---|---|---|
| Microservicio con 1-5 endpoints | ✅ Ideal — menos overhead | ❌ Overhead de reflection innecesario |
| API con 20+ endpoints | ⚠️ Posible con grupos, pero más verboso | ✅ Organización natural por controllers |
| Filtros globales (AuditFilter, ExceptionFilter) | ⚠️ Soportado pero más explícito | ✅ Attribute-based — más expresivo |
| API versioning complejo | ⚠️ Soporte disponible, menos maduro | ✅ Más natural y más ejemplos |
| Model binding avanzado (custom binders) | ❌ Soporte limitado | ✅ Soporte completo |
| Testing de endpoints | ✅ Con WebApplicationFactory | ✅ Igual de testeable |
| Tiempo de startup | ✅ Más rápido (menos reflection) | ❌ Más reflection en startup |
| Adoptado por equipos grandes | ⚠️ Menos familiar para muchos devs | ✅ Paradigma familiar para todos |

**La regla práctica:** Si estás construyendo un microservicio dedicado con un dominio acotado — Minimal APIs. Si estás construyendo una API de plataforma con muchos endpoints, filtros globales complejos, y un equipo grande — Controllers. No es ideología — son trade-offs de complejidad vs flexibilidad.

---

## Sección 6 — CQRS con MediatR: la conexión con el módulo 3

> **Conexión directa:** En [03-06-cqrs-event-sourcing.md](../modulo-03-software-design/03-06-cqrs-event-sourcing.md) entendiste el problema que CQRS resuelve: la asimetría entre modelos de escritura (ricos, con invariantes) y modelos de lectura (proyecciones planas optimizadas para UI). Aquí ves la implementación concreta con MediatR.

```csharp
// Command — modifica estado, retorna minimal info
public record CreateOrderCommand(
    Guid CustomerId,
    List<OrderItemRequest> Items) : IRequest<CreateOrderResult>;

// Command Handler — encapsula la lógica de escritura
public class CreateOrderCommandHandler
    : IRequestHandler<CreateOrderCommand, CreateOrderResult>
{
    private readonly AppDbContext _context;
    private readonly IPublisher _publisher; // Para Domain Events

    public CreateOrderCommandHandler(AppDbContext context, IPublisher publisher)
    {
        _context = context;
        _publisher = publisher;
    }

    public async Task<CreateOrderResult> Handle(
        CreateOrderCommand request,
        CancellationToken ct)
    {
        // Validar reglas de negocio
        var activeOrders = await _context.Orders
            .CountAsync(o => o.CustomerId == request.CustomerId
                          && o.Status == OrderStatus.Active, ct);

        if (activeOrders >= 5)
            throw new BusinessRuleException("Customer cannot have more than 5 active orders");

        // Crear el Aggregate
        var order = Order.Create(request.CustomerId, request.Items);

        _context.Orders.Add(order);
        await _context.SaveChangesAsync(ct);

        // Publicar Domain Event — desacoplado del Command
        await _publisher.Publish(new OrderCreatedEvent(order.Id, order.CustomerId), ct);

        return new CreateOrderResult(order.Id);
    }
}

// Query — no modifica estado, retorna proyección optimizada
public record GetOrdersByCustomerQuery(Guid CustomerId, int Page, int PageSize)
    : IRequest<PagedResult<OrderSummaryDto>>;

// Query Handler — lee directamente de la BD con proyección
// Nota: NO usa el Aggregate Order — usa una proyección plana directamente en SQL
public class GetOrdersByCustomerQueryHandler
    : IRequestHandler<GetOrdersByCustomerQuery, PagedResult<OrderSummaryDto>>
{
    private readonly AppDbContext _context;

    public GetOrdersByCustomerQueryHandler(AppDbContext context) => _context = context;

    public async Task<PagedResult<OrderSummaryDto>> Handle(
        GetOrdersByCustomerQuery request,
        CancellationToken ct)
    {
        // Proyección directa — EF Core genera el SELECT exacto que necesitas
        // Sin cargar el Aggregate completo, sin Change Tracking overhead
        var query = _context.Orders
            .AsNoTracking() // ← Sin Change Tracker para queries — siempre en reads
            .Where(o => o.CustomerId == request.CustomerId)
            .Select(o => new OrderSummaryDto(
                o.Id,
                o.CreatedAt,
                o.Status,
                o.Items.Count,
                o.Items.Sum(i => i.Price * i.Quantity)))
            .OrderByDescending(o => o.CreatedAt);

        var total = await query.CountAsync(ct);
        var items = await query
            .Skip((request.Page - 1) * request.PageSize)
            .Take(request.PageSize)
            .ToListAsync(ct);

        return new PagedResult<OrderSummaryDto>(items, total, request.Page, request.PageSize);
    }
}
```

**La conexión crítica:** Nota el `AsNoTracking()` en el Query Handler. En el módulo 3 entendiste por qué los modelos de lectura son independientes de los de escritura. En código .NET, eso se traduce en: los handlers de Query nunca deberían modificar estado, y `AsNoTracking()` no solo mejora el performance — fuerza ese contrato al nivel del framework.

---

## Recursos y siguiente paso

**📚 Pluralsight — consumir en este orden después de leer este archivo:**
1. **"ASP.NET Core 8 Fundamentals"** — módulo de *Middleware Pipeline* → valida la sección 1
2. **"C# Advanced Topics"** — módulos de *async/await* y *delegates* → valida la sección 4
3. **"Entity Framework Core in Depth"** — módulo de *Change Tracker* y *Performance* → valida sección 3

**Herramientas de diagnóstico para llevar a producción:**
- `EF Core Query Logging` — para detectar N+1 en desarrollo
- `MiniProfiler.AspNetCore` — profiling de queries SQL en el browser, integrado con EF Core
- `dotnet-trace` + `dotnet-counters` — diagnóstico de async contention y ThreadPool starvation

---

## Checklist de salida — 05-01

Antes de considerar este archivo completo:

- [ ] Puedo explicar qué es un `RequestDelegate` y cómo el middleware pipeline lo usa
- [ ] Puedo identificar un Captive Dependency en code review y explicar exactamente por qué es un bug
- [ ] Puedo describir qué campos genera el compilador en una state machine de `async/await`
- [ ] Puedo detectar un N+1 en EF Core usando los logs y proponer la solución correcta según el caso
- [ ] Puedo argumentar cuándo Minimal APIs vs Controllers con criterios técnicos concretos
- [ ] Puedo explicar `ConfigureAwait(false)` — cuándo importa y cuándo es irrelevante

---

**Siguiente archivo:** [05-02-azure-primario.md](./05-02-azure-primario.md)
