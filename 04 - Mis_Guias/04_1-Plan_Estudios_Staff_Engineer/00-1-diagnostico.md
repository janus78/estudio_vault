# 00-diagnóstico — Autoevaluación Técnica Honesta

> Este archivo se completa UNA VEZ antes de entrar al curriculum.
> Si ya empezaste el curriculum sin completarlo, detente y complétalo ahora.
> La honestidad aquí vale más que el ego. Un nivel mal calibrado al inicio
> produce un plan de estudio mal calibrado durante 8-12 meses.

---

## Instrucciones de uso

**Cómo completar esta evaluación:**

1. Lee cada área y su escala de niveles completa antes de asignarte un número
2. Responde las preguntas de calibración de la Sección 4 ANTES de asignarte niveles —
   las respuestas te darán una medición más honesta que tu percepción
3. Registra tus niveles en la tabla de la Sección 3
4. Lee el punto de entrada recomendado en la Sección 5 y síguelo aunque contradiga tu instinto

**Por qué la honestidad aquí es más valiosa que sobreestimarte:**

Un gap de conocimiento que no identificas no desaparece — se convierte en una bomba de tiempo
que explota en la entrevista o en producción. Si te asignas nivel 3 en system design cuando
estás en nivel 1, simplemente saltarás el contenido que más necesitas, llegarás al módulo 4
sin fundamentos, y el sistema no funcionará.

No hay nadie mirando estos números. El único beneficiado de la honestidad eres tú.

**Qué hacer con los resultados:**

Completar la tabla de la Sección 3, identificar gaps críticos (niveles ≤ 2 en áreas que no son
tu stack principal), y seguir las instrucciones de punto de entrada de la Sección 5.
Guardar este archivo con tus niveles registrados — es la línea base de tu progreso.

---

## Sección 2 — Autoevaluación por área

### Escala universal de niveles

Aplica esta escala a todas las áreas:

- **Nivel 1** — No tengo conocimiento formal. Si me preguntan sobre esto en una entrevista, no puedo responder.
- **Nivel 2** — Conozco los conceptos básicos pero no los aplico con confianza. Puedo definir los términos pero no explicar el mecanismo interno.
- **Nivel 3** — Los aplico en trabajo diario pero no puedo explicar los internals. Sé cómo usarlo pero no por qué funciona así.
- **Nivel 4** — Los domino y puedo explicar trade-offs y casos de uso. Puedo tomar decisiones de diseño justificadas.
- **Nivel 5** — Puedo diseñar sistemas, tomar decisiones arquitectónicas y enseñarlo. Entiendo los límites de cada solución.

---

### Área 1 — Algoritmos y Estructuras de Datos

**Qué evalúa esta área:**
No si puedes resolver problemas de LeetCode de memoria, sino si tienes los modelos mentales
fundamentales: cómo funciona una estructura de datos desde adentro, por qué una operación
tiene cierta complejidad, cómo elegir la estructura correcta para un problema dado.

**Preguntas de autoevaluación:**

1. ¿Puedes explicar por qué acceder a un elemento en un array es O(1) y por qué buscar
   en un array desordenado es O(n)?
2. ¿Puedes describir cómo funciona internamente una tabla hash y qué ocurre cuando
   hay colisiones?
3. ¿Sabes cuándo usar un heap en lugar de una lista ordenada, y cuál es el costo
   de inserción en cada uno?
4. ¿Puedes implementar BFS y DFS sobre un grafo sin consultar código de referencia?

**Mi nivel actual en Algoritmos y DSA:** ___

---

### Área 2 — Complejidad Algorítmica (Big-O)

**Qué evalúa esta área:**
No memorizar la complejidad de algoritmos conocidos, sino entender cómo analizar la complejidad
de un algoritmo que no has visto antes. La diferencia entre O(n log n) y O(n²) en un problema
de producción puede ser la diferencia entre un sistema que escala y uno que colapsa.

**Preguntas de autoevaluación:**

1. ¿Puedes calcular la complejidad de un algoritmo con dos loops anidados que no son
   de tamaño n cada uno?
2. ¿Entiendes la diferencia entre complejidad de tiempo y complejidad de espacio,
   y cuándo el trade-off entre ambas importa?
3. ¿Puedes explicar qué significa "amortizado O(1)" y por qué un ArrayList/List dinámico
   tiene esa complejidad en inserciones?
4. ¿Sabes qué es la notación Theta (Θ) y en qué se diferencia de O y Omega?

**Mi nivel actual en Complejidad Algorítmica:** ___

---

### Área 3 — Bases de Datos (SQL y NoSQL)

**Qué evalúa esta área:**
No si sabes escribir queries SQL, sino si entiendes el mecanismo interno de las bases de datos:
cómo funcionan los índices, qué hace el motor bajo el capó cuando ejecutas una query,
cuándo una base de datos relacional no es la herramienta correcta y por qué.

**Preguntas de autoevaluación:**

1. ¿Puedes explicar qué es un B-Tree, por qué los motores de bases de datos lo usan
   para índices, y qué ocurre cuando haces un INSERT en una tabla con un índice?
2. ¿Sabes qué es el ACID, qué garantiza cada letra, y qué operación interna de la BD
   hace posible el "Durability"?
3. ¿Puedes explicar cuándo usarías Cassandra en lugar de PostgreSQL, con un argumento
   técnico real, no solo "Cassandra escala mejor"?
4. ¿Entiendes qué es el problema N+1 en ORMs, cómo lo detectas con herramientas de
   profiling, y cómo lo resuelves correctamente?

**Mi nivel actual en Bases de Datos:** ___

---

### Área 4 — Software Design (SOLID, Patrones, Clean Architecture)

**Qué evalúa esta área:**
No si conoces los nombres de los patrones, sino si puedes identificar cuándo un patrón
resuelve un problema real, cuándo es over-engineering, y cuándo el código que tienes
delante viola un principio de diseño de forma que te generará un problema en 6 meses.

**Preguntas de autoevaluación:**

1. ¿Puedes explicar el principio de inversión de dependencias (D en SOLID) con un ejemplo
   de código concreto y sin mencionar la palabra "interfaz" en la primera oración?
2. ¿Puedes describir cuándo NO usarías el patrón Repository, con un argumento técnico sólido?
3. ¿Entiendes qué problema resuelve Clean Architecture y cuál es el costo real de adoptarla
   en un proyecto pequeño o mediano?
4. ¿Sabes diferenciar entre Aggregate Root en DDD y una entidad normal, y qué consecuencia
   tiene esa distinción en el diseño del dominio?

**Mi nivel actual en Software Design:** ___

---

### Área 5 — System Design (Distributed Systems, Escalabilidad)

**Qué evalúa esta área:**
No si has visto los diagramas de "Diseña Twitter", sino si tienes un modelo mental funcional
de los componentes de un sistema distribuido: qué hace un load balancer realmente, por qué
existe un CDN, cuándo el cacheo resuelve el problema y cuándo lo agrava.

**Preguntas de autoevaluación:**

1. ¿Puedes explicar el CAP Theorem con un ejemplo de sistema real, sin solo definir
   Consistencia, Disponibilidad y Tolerancia a Particiones?
2. ¿Sabes cuál es la diferencia entre consistencia eventual y consistencia fuerte,
   y cuándo cada una es la decisión correcta de negocio?
3. ¿Puedes explicar cómo funciona un sistema de rate limiting distribuido
   (no en un solo servidor)?
4. ¿Entiendes qué problema específico resuelve un message queue en una arquitectura
   y qué trade-offs introduce al adoptarlo?

**Mi nivel actual en System Design:** ___

---

### Área 6 — .NET / C# (Profundidad Técnica Real)

**Qué evalúa esta área:**
No si puedes escribir código C# que funciona, sino si entiendes lo que está pasando
bajo el capó: el Garbage Collector, el modelo de memoria, async/await como máquina de estados,
cómo funciona EF Core realmente.

**Preguntas de autoevaluación:**

1. ¿Puedes explicar qué hace el GC de .NET cuando un objeto sobrevive a una colección
   de Gen0 y pasa a Gen1? ¿Y qué son los Large Object Heap y cuándo son un problema?
2. ¿Sabes qué genera el compilador cuando escribes `async/await`, y por qué
   `async void` es peligroso en contextos que no sean event handlers?
3. ¿Puedes explicar cuándo usarías `IAsyncEnumerable<T>` en lugar de `Task<List<T>>`
   y qué diferencia hace en términos de memoria y latencia?
4. ¿Entiendes cómo EF Core genera SQL, cuándo ese SQL es subóptimo, y cómo usar
   `.AsNoTracking()`, proyecciones y queries compiladas correctamente?

**Mi nivel actual en .NET / C#:** ___

---

### Área 7 — Cloud / Azure

**Qué evalúa esta área:**
No si sabes desplegar cosas en Azure, sino si puedes tomar decisiones arquitectónicas
en la nube: elegir entre servicios con trade-offs reales, diseñar para resiliencia,
entender los modelos de costos.

**Preguntas de autoevaluación:**

1. ¿Puedes explicar cuándo usarías Azure Service Bus vs Azure Event Hub,
   con diferencias técnicas concretas, no solo "Service Bus es para mensajes,
   Event Hub es para eventos"?
2. ¿Sabes qué es el patrón Outbox y por qué existe en arquitecturas con Azure Service Bus?
3. ¿Puedes describir cómo diseñarías la resiliencia de un servicio en Azure
   ante una falla de región?
4. ¿Entiendes los modelos de concurrencia de Azure Cosmos DB y cuándo su consistencia
   configurable es una ventaja real vs un riesgo?

**Mi nivel actual en Cloud / Azure:** ___

---

### Área 8 — IA Integrada (como herramienta y como componente de sistema)

**Qué evalúa esta área:**
Dos ángulos distintos: (1) si usas IA como herramienta de desarrollo con criterio técnico
real, no solo como autocomplete glorificado, y (2) si puedes diseñar sistemas donde
un LLM es un componente de producción.

**Preguntas de autoevaluación:**

1. ¿Puedes explicar qué es RAG, por qué existe (cuál es el problema que resuelve),
   y cuándo fine-tuning sería una mejor opción que RAG?
2. ¿Sabes qué es context engineering y en qué se diferencia de prompt engineering?
3. ¿Puedes describir al menos 3 riesgos reales de poner un LLM en producción
   como componente de un sistema crítico?
4. ¿Usas IA en tu flujo de desarrollo con un criterio deliberado de cuándo y para qué,
   o la usas de forma reactiva cuando te bloqueas?

**Mi nivel actual en IA Integrada:** ___

---

## Sección 3 — Mapa de gaps

### Tu tabla de niveles

Completa esta tabla después de responder las preguntas de las secciones anteriores:

| Área | Mi nivel (1-5) | Gap objetivo (5) | Prioridad |
|------|---------------|------------------|-----------|
| Algoritmos y DSA | | 4-5 | |
| Complejidad Algorítmica | | 4 | |
| Bases de Datos | | 4-5 | |
| Software Design | | 4-5 | |
| System Design | | 4-5 | |
| .NET / C# | | 5 | |
| Cloud / Azure | | 4-5 | |
| IA Integrada | | 3-4 | |

**Cómo asignar prioridad:**
- **Crítica** — Nivel ≤ 2 en cualquier área que no sea IA Integrada
- **Alta** — Nivel 2-3 en áreas de System Design o Software Design
- **Media** — Nivel 3 en áreas donde ya tienes experiencia
- **Baja** — Nivel 4 en cualquier área (refinamiento, no construcción)

### Qué significan las combinaciones de niveles

**El gap más común para perfiles como el tuyo (Senior .NET):**
Alta competencia en .NET (nivel 4-5), niveles 1-2 en algoritmos, nivel 1-2 en system design,
nivel 2-3 en software design. Este es exactamente el perfil para el que fue diseñado
este curriculum.

**Señales de alerta que requieren más tiempo del estimado:**

- Si tienes nivel 1 en más de 3 áreas críticas → estima el extremo superior del timeline (12 meses)
- Si tienes nivel 1 en bases de datos Y system design → el Módulo 4 tomará más de 12 semanas
- Si tienes nivel 1 en algoritmos → necesitarás práctica activa diaria mínima de 45 minutos
  durante toda la duración del Módulo 2 para alcanzar el nivel necesario
- Si tienes nivel 2 en .NET siendo tu stack principal → revisa los fundamentos antes de
  entrar al Módulo 5 (el self-assessment puede estar mal calibrado aquí)

### Tabla de punto de entrada según nivel

| Algoritmos (nivel) | System Design (nivel) | Software Design (nivel) | Punto de entrada |
|---|---|---|---|
| 1-2 | 1-2 | 1-2 | M1 completo, sin atajos |
| 1-2 | 1-2 | 3+ | M1 completo, M3 acelerado |
| 3+ | 1-2 | 1-2 | M1 acelerado (01-01 y 01-02 rápidos), M2 y M3 completos |
| 3+ | 3+ | 3+ | Validar con checkpoint de M1, entrar a M2/M3 en paralelo |

---

## Sección 4 — Preguntas técnicas de calibración

**Instrucciones:** Intenta responder cada pregunta en papel o en un editor de texto
ANTES de buscar la respuesta. El objetivo no es que las respondas correctamente —
es que el proceso de intentarlo te dé una medición más honesta de tu nivel real que
cualquier percepción que tengas de ti mismo.

Si puedes responder correctamente sin buscar → nivel 4+
Si puedes dar la dirección general pero sin precisión técnica → nivel 3
Si sabes que existe pero no puedes explicarlo → nivel 2
Si la pregunta misma es nueva para ti → nivel 1

---

### Algoritmos y Estructuras de Datos

**P1.** ¿Cuál es la diferencia entre un B-Tree y un B+Tree, y por qué los motores de
bases de datos prefieren B+Tree para almacenar índices en disco?

**P2.** Tienes que implementar un LRU Cache con operaciones O(1) para get y put.
¿Qué estructura de datos usas y por qué? Sin código — solo el razonamiento.

**P3.** ¿Por qué Dijkstra no funciona con pesos negativos en aristas, y qué algoritmo
usarías en ese caso?

**P4.** ¿Qué es un heap y cuándo lo usarías en lugar de mantener una lista ordenada?
¿Cuál es el costo real de cada operación comparado?

---

### Complejidad Algorítmica

**P5.** ¿Cuál es la complejidad de espacio de una solución recursiva de Fibonacci sin memoización?
¿Y con memoización? ¿Por qué cambia?

**P6.** Tienes un algoritmo que para n elementos hace n iteraciones en el loop externo
y log(n) iteraciones en el loop interno. ¿Cuál es la complejidad total?

**P7.** ¿Qué significa que el método `Add` de una `List<T>` en C# tiene complejidad
O(1) amortizado? ¿Cuándo exactamente no es O(1) y por qué sigue siendo amortizado O(1)?

---

### Bases de Datos

**P8.** Explica qué hace el motor de PostgreSQL internamente cuando ejecutas:
`SELECT * FROM orders WHERE customer_id = 123 AND status = 'pending'`
y existe un índice compuesto en `(customer_id, status)`. ¿Y si el índice solo
está en `customer_id`?

**P9.** ¿Cuándo usarías Kafka en lugar de Azure Service Bus para mensajería?
Da dos razones técnicas específicas, no solo "Kafka escala más".

**P10.** ¿Qué es el problema N+1 en EF Core, cómo lo detectas con las herramientas
disponibles en .NET, y cuál es la forma correcta de resolverlo (no solo "usa Include")?

**P11.** ¿Qué garantiza la letra "I" (Isolation) en ACID, y cuáles son los diferentes
niveles de aislamiento en SQL? ¿Cuándo usarías Read Uncommitted y qué riesgo aceptas?

---

### Software Design

**P12.** ¿Qué problema resuelve CQRS y cuándo NO lo usarías? Da un ejemplo concreto
de un proyecto donde CQRS sería over-engineering.

**P13.** Tienes una clase `OrderService` que depende directamente de `SqlOrderRepository`.
Explica qué principio SOLID viola esto, por qué es un problema real (no teórico),
y cómo lo resolverías sin introducir complejidad innecesaria.

**P14.** ¿Qué es un Aggregate Root en Domain-Driven Design y cómo afecta al diseño
de tu modelo de base de datos cuando usas EF Core?

---

### System Design

**P15.** Explica el CAP Theorem con un ejemplo de sistema real. No definas los tres términos —
muestra con un escenario concreto qué ocurre cuando hay una partición de red y tienes
que elegir entre C y A.

**P16.** Diseña mentalmente un sistema de rate limiting que funcione en un cluster
de múltiples servidores. ¿Cuántas aproximaciones posibles ves? ¿Qué trade-offs tiene cada una?

**P17.** ¿Cómo diseñarías el sistema de notificaciones de una aplicación tipo Uber
(notificaciones al driver cuando hay un viaje disponible) para manejar millones de usuarios?
¿Qué componentes son críticos?

---

### .NET / C#

**P18.** Explica qué hace el compilador de C# cuando transforma este código:

```csharp
public async Task<string> GetDataAsync()
{
    var result = await httpClient.GetStringAsync("https://api.example.com");
    return result.ToUpper();
}
```

¿Qué clase genera el compilador? ¿Cuántos estados tiene la máquina de estados?

**P19.** ¿Cuándo usarías `IAsyncEnumerable<T>` en lugar de `Task<List<T>>` en una
API de ASP.NET Core? ¿Qué diferencia hace en la latencia percibida por el cliente
y en el uso de memoria del servidor?

**P20.** Tienes este código en producción:

```csharp
var orders = await _context.Orders
    .Where(o => o.CustomerId == customerId)
    .ToListAsync();

foreach (var order in orders)
{
    var items = await _context.OrderItems
        .Where(i => i.OrderId == order.Id)
        .ToListAsync();
    // procesar items...
}
```

Identifica el problema, explica el impacto en producción con números reales
(¿cuántas queries genera si hay 100 órdenes?), y proporciona la solución correcta.

---

## Sección 5 — Punto de entrada recomendado

Una vez que hayas completado la autoevaluación y las preguntas de calibración,
identifica tu punto de entrada con estas reglas:

### Regla principal

**Si tu nivel promedio en Algoritmos + System Design + Software Design es ≤ 2:**
→ Empieza por el Módulo 1 completo, sin saltar ningún archivo.
→ No intentes acelerar M1 porque "ya sabes programar" — programar y entender
   complejidad algorítmica son habilidades distintas.

**Si tu nivel en .NET es ≥ 4 pero Algoritmos y System Design son ≤ 2:**
→ Empieza por Módulo 1. Puedes moverse rápido en `01-03-memoria-y-gestion.md`
   porque tienes contexto, pero no saltes `01-01-estructuras-de-datos.md`
   ni `01-02-complejidad-algoritmica.md`.

**Si tu nivel en Algoritmos es ≥ 3 pero System Design es ≤ 2:**
→ Módulo 1 acelerado (valida con los checkpoints, si los alcanzas pasa rápido).
→ Módulo 2 y Módulo 3 en paralelo desde la semana 5.

**Sobre IA Integrada (Módulo 6):**
→ Sin excepción: siempre al final. No antes de completar M4.
→ La razón es técnica: diseñar sistemas con LLMs requiere entender system design primero.
   Un RAG bien diseñado requiere conocimiento de caching, bases de datos vectoriales,
   y patrones de consistencia. Sin M4, el Módulo 6 es teoría sin fundamento.

---

### Registra tu diagnóstico aquí

Completa esto y guarda el archivo:

```
Fecha del diagnóstico: _______________

Niveles registrados:
- Algoritmos y DSA: ___
- Complejidad Algorítmica: ___
- Bases de Datos: ___
- Software Design: ___
- System Design: ___
- .NET / C#: ___
- Cloud / Azure: ___
- IA Integrada: ___

Gaps críticos identificados (nivel ≤ 2):
_______________________________________________

Punto de entrada elegido:
_______________________________________________

Timeline estimado (basado en gaps):
_______________________________________________

Notas adicionales:
_______________________________________________
```

---

> **Siguiente paso:** [Ver el roadmap visual completo →](./00-roadmap-visual.md)
> Luego: [Empezar por Módulo 1 →](./modulo-01-cs-fundamentals/01-00-overview.md)
