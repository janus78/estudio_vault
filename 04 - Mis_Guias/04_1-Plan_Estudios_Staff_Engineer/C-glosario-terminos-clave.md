# Apéndice C — Glosario de Términos Clave

> **Documento de referencia rápida.** No leer linealmente — consultar por término.
> Dos formas de navegar: (1) por módulo para repasar vocabulario de un área, (2) por búsqueda Ctrl+F para un término específico.
> Las definiciones están calibradas para entrevistas de nivel Staff: precisas, sin relleno, con contexto de uso real.

---

## Sección 1 — Cómo usar este glosario

**Formato de cada entrada:**

**Término** — Definición precisa en 1-3 oraciones que captura el mecanismo real, no solo la descripción superficial.
*Contexto de uso:* Cuándo aparece este término en entrevistas o en producción — qué pregunta lo gatilla.
*No confundir con:* Términos similares que se mezclan frecuentemente y cuál es la distinción que importa.

**Criterio de calidad aplicado:** Una definición útil para entrevista no es "técnica para optimizar código". Es "técnica que guarda resultados de subproblemas ya calculados para evitar recomputación, reduciendo complejidad de O(2ⁿ) a O(n) en el caso típico de DP con memoización". La especificidad es el valor.

---

## Sección 2 — Términos por módulo

---

### Módulo 1 — CS Fundamentals

---

**Amortized complexity (complejidad amortizada)** — El costo promedio de una operación a lo largo de una secuencia de operaciones, cuando algunas operaciones individuales son costosas pero poco frecuentes. Se calcula dividiendo el costo total de N operaciones entre N.
*Contexto de uso:* Explicar por qué `List<T>.Add()` en C# es O(1) amortizado aunque algunos adds son O(n) (cuando se dobla el array interno). Aparece en preguntas sobre estructuras de datos dinámicas.
*No confundir con:* Average case complexity, que es el costo promedio sobre distribuciones de inputs — no sobre secuencias de operaciones del mismo tipo.

---

**B-Tree** — Árbol de búsqueda balanceado auto-balanceable donde cada nodo puede tener múltiples keys y múltiples hijos. Los datos pueden estar en cualquier nodo, tanto internos como hoja.
*Contexto de uso:* Contexto histórico de índices de bases de datos — la versión que los motores modernos reemplazaron con B+Tree.
*No confundir con:* B+Tree — la variante que usan los motores de BD modernos, donde los datos solo están en las hojas.

---

**B+Tree** — Variante del B-Tree donde todos los datos (registros) están en los nodos hoja, y los nodos internos solo contienen keys de navegación. Los nodos hoja están conectados entre sí en una lista enlazada.
*Contexto de uso:* Por qué los motores de BD (PostgreSQL, MySQL InnoDB, SQL Server) lo prefieren para índices: el range scan es eficiente porque se puede recorrer la lista enlazada de hojas sin subir al árbol. Es la pregunta de "¿por qué B+Tree y no B-Tree?" en entrevistas de bases de datos.
*No confundir con:* B-Tree (datos en todos los nodos, range scan requiere traversal completo del árbol).

---

**Boxing / Unboxing** — Boxing: conversión de un value type (struct, int, bool) a reference type (object) en .NET, que requiere allocation en el heap managed. Unboxing: la operación inversa, que requiere un cast explícito y puede lanzar `InvalidCastException`.
*Contexto de uso:* Fuente de GC pressure no obvia cuando ocurre en loops internos o en colecciones no genéricas (`ArrayList`). Pre-.NET 2.0, era el problema de performance más común. Hoy aparece en código que mezcla `object` con value types o usa APIs antiguas sin generics.
*No confundir con:* Heap allocation en general — boxing es una forma específica de heap allocation causada por la conversión de tipos.

---

**Cache miss** — Cuando el dato buscado en la capa de caché no está presente y se debe ir a la fuente de datos original (disco, base de datos, red). El costo real es el costo del acceso a la fuente original más el overhead de la búsqueda en caché.
*Contexto de uso:* Evaluar si una estrategia de caching tiene ROI real — si la tasa de cache miss es alta, el caché puede agravar la latencia en lugar de reducirla.
*No confundir con:* CPU cache miss — un concepto diferente a nivel de hardware sobre acceso a la caché L1/L2/L3 del procesador.

---

**Context switching** — El proceso del OS de guardar el estado completo de un thread que se está ejecutando (registros de CPU, puntero de instrucción, stack pointer) y cargar el estado de otro thread para ejecutarlo. Costo típico: 1-10 microsegundos por switch dependiendo del hardware.
*Contexto de uso:* Por qué crear miles de threads es contraproducente incluso si el CPU tiene muchos cores: el tiempo de contexto switch se convierte en overhead dominante. Es la justificación técnica de por qué async/await en .NET usa un threadpool pequeño en lugar de un thread por request.
*No confundir con:* Process switch — más costoso porque requiere cambiar el espacio de memoria virtual además del estado del thread.

---

**GIL (Global Interpreter Lock)** — Mutex en la implementación CPython de Python que garantiza que solo un thread ejecuta bytecode Python a la vez, incluso en máquinas con múltiples cores. No existe en .NET, Java, ni en otras implementaciones de Python como Jython o PyPy.
*Contexto de uso:* Por qué el multithreading en Python CPython no da paralelismo real para workloads CPU-bound. Para paralelismo CPU-bound en Python se usa `multiprocessing` (múltiples procesos separados) o código nativo (C extensions). Para I/O-bound, asyncio o threading funcionan porque el GIL se libera durante I/O.
*No confundir con:* `asyncio` en Python — que es concurrencia single-threaded para I/O-bound, no paralelismo CPU-bound.

---

**Heap (memoria)** — Región de memoria dinámica gestionada por el runtime (GC en .NET) donde viven los objetos de referencia y objetos de larga duración. El acceso es más lento que el stack porque requiere indirección de puntero y puede tener fragmentación.
*No confundir con:* Heap (estructura de datos) — árbol binario casi completo con la propiedad heap (max-heap o min-heap). Son dos conceptos completamente distintos con el mismo nombre.

---

**Large Object Heap (LOH)** — Región especial del managed heap de .NET para objetos cuyo tamaño es ≥ 85KB. El GC no compacta el LOH por defecto (compactación opcional desde .NET 4.5.1), lo que causa fragmentación de memoria a lo largo del tiempo si se crean y descartan objetos grandes frecuentemente.
*Contexto de uso:* Fuente de memory pressure en servicios que procesan archivos, responses HTTP grandes, o buffers de I/O. El síntoma es memoria disponible que baja gradualmente sin que el GC la recupere. La solución es `ArrayPool<T>` para reusar buffers en lugar de allocar nuevos.
*No confundir con:* Gen2 heap — el LOH es separado de Gen0/Gen1/Gen2, aunque se recolecta junto con Gen2.

---

**Virtual memory** — Abstracción del OS que da a cada proceso la ilusión de tener acceso exclusivo a un espacio de direcciones contiguo (típicamente 4GB en 32-bit, terabytes en 64-bit). El OS mapea páginas virtuales a páginas físicas de RAM a través del page table, cargando páginas bajo demanda (demand paging).
*Contexto de uso:* Por qué un proceso puede "usar" 2GB de virtual memory pero solo tener 200MB en RAM física — el resto está en el page file o no ha sido accedido todavía. En .NET, importante para entender la diferencia entre virtual memory reservada y memoria física committed.
*No confundir con:* Swap / Page file — mecanismo relacionado, pero virtual memory es la abstracción; el swap es el mecanismo de persistencia cuando la RAM física se agota.

---

### Módulo 2 — Algoritmos y Patrones

---

**Backtracking** — Técnica algorítmica de exploración exhaustiva que construye la solución incrementalmente y abandona ("hace backtrack") una rama de exploración en cuanto detecta que no puede llevar a una solución válida (constraint violation). Garantiza encontrar todas las soluciones o demostrar que no existen.
*Contexto de uso:* Problemas que piden generar "todas las combinaciones", "todas las permutaciones", "todos los subsets válidos" bajo constraints — N-Queens, Sudoku Solver, Word Search, Combination Sum. La señal es: el problema pide enumeración exhaustiva con pruning.
*No confundir con:* Dynamic Programming — DP optimiza (encuentra el mejor) o cuenta soluciones, no las enumera todas. Backtracking puede tener complejidad exponencial sin pruning efectivo; DP lo evita porque no re-explora.

---

**Dynamic Programming (DP)** — Técnica para resolver problemas de optimización o conteo que tienen dos propiedades: overlapping subproblems (los mismos subproblemas se resuelven múltiples veces) y optimal substructure (la solución óptima del problema se construye a partir de soluciones óptimas de subproblemas). Se implementa con memoization (top-down) o tabulation (bottom-up).
*Contexto de uso:* Siempre que el problema pregunte "mínimo", "máximo", "cuántas formas", "¿es posible?". Las señales de DP en el enunciado: "cuenta el número de caminos", "minimiza el costo", "¿puede el jugador ganar?".
*No confundir con:* Divide and Conquer — los subproblemas son independientes (no se solapan), por eso no hay ganancia en cachear. Ejemplo: Merge Sort (subproblemas independientes) vs Fibonacci (subproblemas solapados).

---

**In-place algorithm** — Algoritmo que usa O(1) espacio extra (espacio auxiliar constante), modificando el input directamente en lugar de crear copias. Nota: "O(1) espacio extra" puede excluir el call stack de la recursión, lo que es ambiguo — aclarar en entrevista.
*Contexto de uso:* Pedirle permiso al entrevistador antes de modificar el array de input ("¿Puedo modificar el array en lugar de crear uno nuevo?"). Reverse a linked list, rotate a matrix, Dutch National Flag problem.
*No confundir con:* Algoritmo eficiente en memoria en general — un algoritmo puede ser O(log n) space y no ser "in-place" en el sentido estricto.

---

**Memoization** — Técnica de implementación de DP top-down: resolver el problema recursivamente y guardar el resultado de cada subproblema en un cache (diccionario o array) para evitar recomputarlo si se necesita de nuevo. El árbol de recursión se convierte en un DAG porque los nodos repetidos se recortan.
*Contexto de uso:* Primera implementación de DP para la mayoría de los problemas — más natural que tabulation porque sigue la estructura recursiva del problema. En C#: `Dictionary<(int,int), int>` o `int[,]` con valor centinela (-1 o int.MinValue) para marcar "no calculado todavía".
*No confundir con:* Tabulation (bottom-up DP) — que llena una tabla iterativamente desde los casos base sin recursión. Tabulation generalmente tiene mejor constante de tiempo y evita stack overflow en inputs grandes.

---

**Tabulation** — Técnica de implementación de DP bottom-up: construir iterativamente una tabla desde los casos base hacia el problema completo, llenando cada celda usando los valores ya calculados. No usa recursión.
*Contexto de uso:* Preferida sobre memoization cuando el input puede ser muy grande (evita stack overflow), cuando se conoce el orden de dependencias de los subproblemas, o cuando se necesita optimizar espacio (muchos problemas DP 2D se pueden reducir a O(n) con rolling array).
*No confundir con:* Memoization (top-down recursiva) — misma complejidad asintótica, diferente implementación con trade-offs distintos de stack vs heap memory.

---

**Topological Sort** — Ordenamiento lineal de los vértices de un DAG (Directed Acyclic Graph) tal que para cada arista dirigida u→v, el vértice u aparece antes que v en el ordenamiento. Solo existe si el grafo no tiene ciclos. Dos algoritmos: Kahn's (BFS, usa in-degree array) y DFS-based (usa stack).
*Contexto de uso:* Orden de compilación de módulos con dependencias, scheduling de tareas con prerequisitos, orden de ejecución de migrations de base de datos. La señal de entrevista: "dado un grafo de dependencias, determina el orden de ejecución válido".
*No confundir con:* Shortest path en DAGs — topological sort puede ser un paso previo para encontrar shortest/longest path en DAGs, pero son problemas distintos.

---

**Union-Find (Disjoint Set Union — DSU)** — Estructura de datos para gestionar una colección de conjuntos disjuntos con dos operaciones: Find(x) retorna el representante del conjunto que contiene a x, y Union(x, y) fusiona los conjuntos de x e y. Con path compression y union by rank, ambas operaciones son O(α(n)) amortizado (prácticamente O(1)).
*Contexto de uso:* Detectar ciclos en grafos no dirigidos (si Union de dos nodos con el mismo Find retorna false, hay ciclo), encontrar componentes conexas, Minimum Spanning Tree (algoritmo de Kruskal).
*No confundir con:* Graph traversal (BFS/DFS) para componentes conexas — Union-Find es más eficiente para preguntas dinámicas de conectividad donde se agregan aristas incrementalmente.

---

### Módulo 3 — Software Design

---

**Aggregate (DDD)** — Cluster de objetos de dominio (Entities y Value Objects) que se trata como una unidad de consistencia para cambios de datos. Tiene un Aggregate Root — la única entidad a través de la cual se accede y modifica el aggregate. Las invariantes del dominio del aggregate son protegidas por el Aggregate Root.
*Contexto de uso:* Delimitar las transacciones en el dominio — una transacción debería modificar un solo aggregate. Si una operación necesita modificar dos aggregates, es señal de que o los boundaries están mal o se necesita eventual consistency entre aggregates.
*No confundir con:* SQL AGGREGATE functions (SUM, COUNT, AVG) — completamente diferente, solo coincidencia de nombre.

---

**Bounded Context** — Frontera explícita dentro del dominio del negocio donde un modelo de dominio específico tiene significado preciso y consistente. La misma palabra del negocio puede tener significado distinto en contextos diferentes — "Customer" en el contexto de Ventas vs en el contexto de Soporte tiene atributos y comportamientos distintos.
*Contexto de uso:* Base para la separación en microservicios (aunque no son lo mismo — un microservicio puede implementar un Bounded Context). En entrevistas de system design sobre decomposición de sistemas monolíticos, es la unidad conceptual correcta para dividir.
*No confundir con:* Microservicio — un bounded context puede implementarse como un monolito, múltiples microservicios, o un solo microservicio. La separación conceptual (bounded context) precede a la decisión técnica (cómo desplegar).

---

**Captive Dependency (dependencia cautiva)** — Bug de DI donde un servicio registrado como Singleton captura una dependencia registrada como Scoped (o Transient), impidiendo que esa dependencia se renueve según su ciclo de vida esperado. El resultado es que un objeto que debería tener una vida útil corta (un DbContext, por ejemplo) es retenido indefinidamente por el Singleton.
*Contexto de uso:* Error silencioso y frecuente en ASP.NET Core. Síntoma: comportamiento intermitente, datos corruptos, o excepciones de "DbContext disposed" después de larga ejecución. ASP.NET Core puede detectarlo en desarrollo con `ValidateScopes = true` en el DI container.
*No confundir con:* Circular dependency — cuando A depende de B y B depende de A. Son errores distintos aunque ambos son problemas de DI.

---

**Cohesion (cohesión)** — Medida de cuán relacionadas y enfocadas están las responsabilidades dentro de un módulo, clase, o componente. Alta cohesión significa que todos los elementos de un módulo pertenecen juntos y trabajan hacia un propósito único. Es una métrica deseable — maximizar.
*Contexto de uso:* Evaluar si una clase tiene demasiadas responsabilidades (Single Responsibility Principle). Una clase con métodos que no comparten estado ni propósito tiene baja cohesión y es señal de que debe dividirse.
*No confundir con:* Coupling (acoplamiento) — que mide dependencias entre módulos, no cohesión interna. El objetivo de diseño es alta cohesión + bajo acoplamiento simultáneamente.

---

**Coupling (acoplamiento)** — Medida de cuánto depende un módulo de la implementación interna de otros módulos. Alto acoplamiento significa que un cambio en el módulo A probablemente requiere cambios en el módulo B. Es una métrica no deseable — minimizar.
*Contexto de uso:* Justificar por qué se usan interfaces en lugar de clases concretas, por qué se usa Dependency Injection, por qué se evita acceder directamente a la BD desde el controlador. Afferent coupling (cuántos módulos dependen de este) vs efferent coupling (cuántos módulos usa este).
*No confundir con:* Cohesion — que mide la unidad interna de un módulo, no sus dependencias externas.

---

**Domain Event** — Representación de algo significativo que ocurrió en el dominio del negocio, expresado en tiempo pasado. Son inmutables, nombrados con verbos en pasado ("OrderPlaced", "PaymentProcessed"), y capturan el estado en el momento del evento. Disparados por el Aggregate Root al completar una operación de negocio.
*Contexto de uso:* Comunicación entre partes del mismo bounded context o como trigger de side effects dentro del dominio. En implementación con EF Core + MediatR: publicar Domain Events después de `SaveChanges()`.
*No confundir con:* Integration Event — usado para comunicación entre bounded contexts separados (a través de un message broker). Los Domain Events son internos al bounded context; los Integration Events cruzan fronteras.

---

**Idempotency (idempotencia)** — Propiedad de una operación donde ejecutarla múltiples veces con los mismos parámetros produce exactamente el mismo resultado que ejecutarla una sola vez. La segunda ejecución (y las subsiguientes) no causa efectos secundarios adicionales.
*Contexto de uso:* Crítico en APIs públicas (HTTP PUT y DELETE deben ser idempotentes por definición del protocolo; POST generalmente no lo es). Crítico en sistemas de mensajería con at-least-once delivery: los consumers deben ser idempotentes porque pueden recibir el mismo mensaje más de una vez. Implementación: idempotency keys.
*No confundir con:* Idempotent (el adjetivo) vs deterministic — un método puede ser determinista (mismo input, mismo output) sin ser idempotente si tiene efectos secundarios; un método puede ser idempotente sin ser determinista en el sentido estricto.

---

**Outbox Pattern** — Patrón de integración que garantiza que los Domain Events o mensajes se publican al message broker exactamente una vez, incluso ante fallas del sistema. En lugar de publicar directo al broker (que rompería la atomicidad de la transacción), los eventos se guardan en una tabla "outbox" en la misma transacción del aggregate. Un proceso separado (relay) lee el outbox y publica al broker.
*Contexto de uso:* Cada vez que necesitas garantizar que "si el dato se guardó, el evento se publicó" — y viceversa. Sin el Outbox Pattern, hay una ventana de tiempo entre `SaveChanges()` y la publicación al broker donde un crash del proceso deja al sistema en estado inconsistente.
*No confundir con:* Inbox Pattern — el mecanismo dual para garantizar exactly-once processing en el consumer (deduplicar mensajes recibidos antes de procesarlos).

---

**Strangler Fig Pattern** — Patrón de migración de sistemas legacy donde el sistema nuevo se construye gradualmente alrededor del viejo, capturando funcionalidad de a fragmentos, hasta que el sistema original puede ser eliminado. El nombre viene del árbol que crece alrededor de otro hasta eventualmente reemplazarlo.
*Contexto de uso:* Alternativa al big-bang rewrite — que tiene alta tasa de fracaso porque requiere replicar todo el comportamiento del sistema legacy antes de poder hacer el switch. Con Strangler Fig, el sistema legacy sigue en producción mientras el nuevo se construye y se va validando incrementalmente.
*No confundir con:* Feature flags — herramienta táctica que puede usarse dentro de una estrategia Strangler Fig, pero no es lo mismo.

---

**Value Object (DDD)** — Objeto del dominio sin identidad propia — su igualdad se determina por el valor de sus atributos, no por una identidad única. Son inmutables: en lugar de modificarlos, se crea un nuevo instance. Ejemplos: Email, Money, Address, DateRange.
*Contexto de uso:* Reemplazar "primitive obsession" (usar `string` para email, `decimal` para dinero) con objetos que encapsulan validación y comportamiento del concepto del dominio. En EF Core: mapear con `OwnsOne()` para que persistan en la misma tabla que la Entity.
*No confundir con:* Entity (DDD) — que tiene identidad única (`Id`) y puede mutar su estado a lo largo del tiempo. La misma "dirección" puede ser un Value Object (si la dirección es solo datos) o una Entity (si necesita rastrear historial de cambios o tiene relaciones propias).

---

### Módulo 4 — System Design

---

**Back-of-the-envelope calculation** — Estimación de orden de magnitud para dimensionar un sistema, usando números redondos y asunciones explícitas. El objetivo no es exactitud — es tomar decisiones de diseño con un rango de números razonable. Precisión de ±1 orden de magnitud es suficiente.
*Contexto de uso:* Primeros 5-10 minutos de una entrevista de system design. Los entrevistadores evalúan si el candidato puede razonar sobre escala, no si recuerda benchmarks exactos. Números útiles a memorizar: un servidor típico maneja ~10K requests/seg; una BD bien indexada maneja ~10K QPS; un SSD lee ~500MB/s; la RAM es ~100GB/s.
*No confundir con:* Exactitud de capacidad — en producción se hace capacity planning con datos reales, no con back-of-the-envelope.

---

**CAP Theorem** — En un sistema distribuido que experimenta una partición de red (P), es imposible garantizar simultáneamente Consistency (C) — todos los nodos ven el mismo dato al mismo tiempo — y Availability (A) — el sistema siempre responde a las requests. Se debe elegir entre CP (priorizar consistencia, rechazar requests durante la partición) o AP (priorizar disponibilidad, servir datos potencialmente desactualizados).
*Contexto de uso:* Justificar la elección de base de datos en system design (PostgreSQL/MySQL = CP, Cassandra/DynamoDB = AP). En entrevistas, el entrevistador espera que el candidato articule qué significa la elección CP vs AP para el usuario final — no solo para el sistema.
*No confundir con:* ACID Consistency — que es una garantía de integridad de transacciones dentro de una base de datos individual, no sobre un sistema distribuido. "Consistency" en CAP y "Consistency" en ACID son conceptos diferentes que desafortunadamente comparten el nombre.

---

**Circuit Breaker** — Patrón de resiliencia que envuelve llamadas a un servicio externo y monitorea los fallos. Cuando la tasa de fallos supera un umbral, el circuito "se abre" y las requests subsiguientes fallan rápido (sin esperar el timeout del servicio caído), permitiendo que el servicio downstream se recupere. Tres estados: Closed (normal), Open (fallas rápidas), Half-Open (prueba si el servicio se recuperó).
*Contexto de uso:* Microservicios que dependen de otros servicios. Sin Circuit Breaker, cuando el servicio B se cae, el servicio A acumula threads bloqueados esperando timeouts, eventualmente causando cascading failure. Implementación en .NET: Polly library.
*No confundir con:* Retry pattern — que reintenta operaciones fallidas. Circuit Breaker y Retry se usan en combinación pero son patrones distintos. Retry sin Circuit Breaker puede agravar la situación si el servicio downstream está sobrecargado.

---

**Consistent Hashing** — Algoritmo de distribución que mapea tanto datos como nodos a un "ring" de hash space, asignando cada dato al nodo más cercano en el ring. Cuando se agrega o remueve un nodo, solo un fragmento O(K/N) de los datos necesita reasignarse (donde K es el número de datos y N el número de nodos), en lugar de O(K) con hashing modular simple.
*Contexto de uso:* Sharding en sistemas distribuidos donde los nodos se agregan/remueven dinámicamente (caches distribuidos como Memcached, bases de datos distribuidas como Cassandra). La ventaja es que minimiza el rehashing — operación costosa en sistemas con grandes volúmenes de datos.
*No confundir con:* Partitioning estático (sharding por rango o por módulo) — más simple pero requiere rebalanceo masivo cuando cambia el número de shards.

---

**Error Budget** — Cantidad de tiempo (o porcentaje de requests) que un servicio puede estar fuera de su SLO en un período dado (típicamente rolling 30 días). Si el SLO es 99.9% de disponibilidad, el error budget es 0.1% = ~43 minutos/mes. Es una herramienta de decisión: si hay error budget disponible, se pueden hacer deploys y cambios arriesgados; si se agotó, se congela el desarrollo y se prioriza estabilidad.
*Contexto de uso:* El mecanismo que hace que los SLOs sean accionables en lugar de solo aspiracionales. En entrevistas sobre observabilidad y reliability, es la distinción entre alguien que solo define métricas y alguien que diseña sistemas de decisión.
*No confundir con:* SLA (contrato externo con penalidades) o SLO (el objetivo interno que define el error budget). El error budget es el corolario operacional del SLO.

---

**Fan-out** — El proceso de distribuir una escritura (write) a múltiples destinos simultáneamente. En el contexto de timelines sociales: cuando un usuario con N seguidores publica un tweet, se copian N entradas en N timelines de seguidores (fan-out on write), o se lee de N seguidores al construir el timeline (fan-out on read).
*Contexto de uso:* Problema central en el diseño de sistemas de feeds sociales (Twitter, Instagram, LinkedIn). El trade-off: fan-out on write tiene reads baratas pero writes costosas y storage grande; fan-out on read tiene writes baratas pero reads costosas. La solución madura (Hybrid Fan-Out) diferencia entre usuarios con muchos seguidores (celebrities — fan-out on read) y usuarios normales (fan-out on write).
*No confundir con:* Broadcasting — que es distribuir el mismo mensaje a todos los suscriptores de un topic (como en pub/sub), no a los seguidores individuales de un usuario específico.

---

**Linearizability** — El modelo de consistency más fuerte disponible en sistemas distribuidos: cada operación parece ejecutarse de forma instantánea en algún punto entre su inicio y su fin, y todas las operaciones son consistentes con un único historial global. Una vez que una escritura completa, todas las lecturas subsiguientes devuelven ese valor o un valor más nuevo.
*Contexto de uso:* Es el modelo de consistency que garantiza que los clientes nunca ven lecturas "viejas" después de que una escritura confirmó. Ejemplo de sistema linearizable: etcd, ZooKeeper. Ejemplo de sistema no-linearizable: Cassandra con eventual consistency. En entrevistas de distributed systems, es la respuesta correcta a "¿qué quiere decir strong consistency en la práctica?".
*No confundir con:* CAP Consistency — son el mismo concepto (linearizability = C en CAP), pero mucha gente define CAP Consistency de forma más vaga. Cuando alguien dice "consistency" en CAP, significa linearizability.

---

**Quorum** — El número mínimo de nodos que deben confirmar (acknowledge) una operación para que sea considerada exitosa en un sistema distribuido con N nodos. El quorum clásico es ⌊N/2⌋ + 1, lo que garantiza que cualquier dos quorums tienen al menos un nodo en común — y por tanto, cualquier escritura confirmada es visible en cualquier lectura confirmada.
*Contexto de uso:* Lecturas y escrituras en sistemas distribuidos (DynamoDB: W + R > N garantiza consistencia fuerte), algoritmos de consensus (Raft requiere quorum para confirmar entradas del log). La configuración W=1, R=1 da máxima disponibilidad con eventual consistency; W=N, R=N da máxima consistency pero mínima disponibilidad.
*No confundir con:* Mayoría simple en votaciones — el concepto es análogo pero en sistemas distribuidos el "voto" es la confirmación de que un nodo recibió y persistió la operación.

---

**RPO (Recovery Point Objective)** — El máximo de pérdida de datos aceptable si ocurre un desastre, medido en tiempo. "¿Hasta cuándo atrás podemos recuperar datos si el sistema falla ahora?" RPO = 1 hora significa aceptar perder hasta 1 hora de transacciones.
*Contexto de uso:* Determina la estrategia de backup y replicación: RPO = 0 requiere replicación síncrona en tiempo real; RPO = 24h puede satisfacerse con backups diarios. En entrevistas de system design y disaster recovery.
*No confundir con:* RTO (Recovery Time Objective) — que mide cuánto tiempo puede estar el sistema no disponible durante la recuperación, no cuántos datos se pierden.

---

**RTO (Recovery Time Objective)** — El tiempo máximo aceptable que puede estar el sistema no disponible después de un desastre, medido desde el momento del fallo hasta que el servicio está restaurado. RTO = 4 horas significa aceptar hasta 4 horas de downtime.
*Contexto de uso:* Determina la arquitectura de disaster recovery: RTO bajo requiere hot standby (sistema de backup listo para servir tráfico instantáneamente); RTO alto puede satisfacerse con warm standby o backups que requieren tiempo de restauración.
*No confundir con:* RPO (Recovery Point Objective) — que mide pérdida de datos, no tiempo de recuperación. Un sistema puede tener RPO = 0 (cero pérdida de datos con replicación síncrona) pero RTO = 2 horas (recuperación de la infraestructura toma tiempo).

---

**Saga Pattern** — Patrón para gestionar transacciones distribuidas que abarcan múltiples servicios, donde cada paso es una transacción local con una compensating transaction para revertirla si un paso posterior falla. Dos variantes: Choreography (cada servicio publica eventos y reacciona a eventos de otros) y Orchestration (un saga orchestrator central coordina los pasos).
*Contexto de uso:* Alternativa a 2PC (Two-Phase Commit) en arquitecturas de microservicios. Aplicable cuando una operación de negocio (crear un pedido, procesar un pago, reservar un inventario) debe ocurrir en múltiples servicios y necesita ser "deshacer" si algún paso falla.
*No confundir con:* 2PC (Two-Phase Commit) — que es bloqueante (los recursos quedan bloqueados hasta que el coordinador confirma o aborta), tiene single point of failure, y no escala bien. Sagas es la alternativa que escala en microservicios a costa de eventual consistency.

---

**Sharding** — Técnica de particionamiento horizontal de datos donde diferentes filas (o documentos) se distribuyen entre múltiples nodos de base de datos, cada uno con su propio schema completo. El shard key determina en qué nodo vive cada dato.
*Contexto de uso:* Escalar una base de datos más allá de lo que un solo nodo puede manejar en términos de almacenamiento o QPS de escritura. Los desafíos: cross-shard queries son costosas, los joins entre shards son problemáticos, rebalancear shards ante cambio en el shard key es complejo.
*No confundir con:* Replication — que copia los mismos datos en múltiples nodos para disponibilidad y durabilidad, no para distribuir la carga de escritura. Sharding y replication se usan juntos: se shardea para escala de escritura, y cada shard se replica para durabilidad.

---

**SLI (Service Level Indicator)** — La métrica específica y medible que captura el comportamiento del servicio desde la perspectiva del usuario. Ejemplos: porcentaje de requests que respondieron en < 200ms, porcentaje de requests exitosas (no errores 5xx), disponibilidad de la API.
*Contexto de uso:* La medida base sobre la que se define el SLO. Un SLI bien definido mide lo que el usuario experimenta, no lo que el sistema hace internamente (CPU usage no es un buen SLI porque el usuario no lo experimenta directamente).
*No confundir con:* SLO (el objetivo sobre el SLI, ej: "el SLI de latencia debe ser ≤ 200ms el 99% del tiempo") o SLA (el contrato externo con penalidades si el SLO no se cumple).

---

**WAL (Write-Ahead Log)** — Mecanismo de los motores de bases de datos donde los cambios se escriben primero en un log secuencial (el WAL) antes de aplicarse a las páginas de datos en disco. La entrada del WAL es durable antes de que la transacción confirme.
*Contexto de uso:* Cómo los motores de BD garantizan Atomicity y Durability de ACID: si el sistema falla, el WAL permite hacer recovery (replay de transacciones confirmadas no aplicadas) o rollback (deshacer transacciones parciales). También es la base de la replicación en PostgreSQL (streaming replication) y la fuente de Change Data Capture (CDC).
*No confundir con:* Append-only log en event sourcing — conceptualmente similar (log como fuente de verdad) pero en contextos distintos. El WAL es un mecanismo interno de la BD; el event store en Event Sourcing es un modelo de datos de la aplicación.

---

### Módulo 6 — IA Integrada

---

**Context Engineering** — El diseño completo de la información que fluye hacia y desde un LLM en un sistema: qué incluir en el contexto, cómo estructurarlo para maximizar la calidad del output, cómo gestionar el tamaño del context window, qué información incluir en qué orden, y cómo evitar degradación del contexto a lo largo de una sesión.
*Contexto de uso:* El nivel de madurez técnica que diferencia un sistema LLM de juguete de uno de producción. Un Staff Engineer que trabaja con LLMs piensa en context engineering, no en prompt engineering. Aparece en entrevistas de AI System Design como señal de madurez.
*No confundir con:* Prompt Engineering — que es una subdisciplina de context engineering, enfocada específicamente en el texto de las instrucciones. Context engineering es más amplio: incluye gestión de memoria, inyección de estado, recuperación de documentos (RAG), y el diseño del flujo de información completo.

---

**Context Window** — El límite máximo de tokens que un LLM puede procesar en una llamada, combinando input (prompt + documentos + historial) y output (respuesta generada). Es un recurso escaso y costoso — tanto en latencia como en costo monetario.
*Contexto de uso:* Restricción de diseño fundamental en sistemas con LLMs. Un context window grande (200K tokens en Claude) no significa "incluir todo" — incluir información irrelevante degrada la calidad del output y aumenta el costo. En diseño de sistemas RAG, el context window es el recurso que hay que gestionar intencionalmente.
*No confundir con:* Long Context (como feature) — la capacidad técnica de tener un context window grande. El hecho de que el modelo soporte 200K tokens no significa que siempre sea la estrategia correcta usarlos todos.

---

**Context Zombie** — Conversación con un LLM que ha acumulado tanto contexto contradictorio, incorrecto, o irrelevante que el modelo produce outputs degradados de forma consistente — pero la degradación es sutil y difícil de detectar sin evaluar los outputs sistemáticamente.
*Contexto de uso:* Problema operacional en sistemas agénticos con sesiones largas, especialmente cuando el agente acumula resultados de tool calls incorrectos o instrucciones contradictorias. La solución es dump-and-reset: iniciar una sesión nueva con contexto limpio y cuidadosamente construido.
*No confundir con:* Alucinación — que es un problema de generación puntual. Un Context Zombie es una condición de la sesión completa donde el contexto acumulado corrompe todos los outputs subsiguientes.

---

**Embedding** — Representación vectorial de texto (o imágenes, código) en un espacio vectorial de alta dimensión (típicamente 512-3072 dimensiones) donde textos semánticamente similares tienen vectores cercanos (distancia coseno baja). Generados por modelos de embedding específicos (OpenAI text-embedding-3-large, etc.).
*Contexto de uso:* La base técnica del vector search en pipelines RAG. Para encontrar documentos relevantes a una query, se convierte la query a embedding y se buscan los embeddings de documentos con menor distancia coseno. La calidad del modelo de embedding es uno de los factores más importantes en la calidad del RAG.
*No confundir con:* El output de un LLM generativo — que es texto en lenguaje natural. Los embeddings son vectores numéricos, no texto.

---

**Fine-tuning** — Proceso de adaptar un modelo de LLM pre-entrenado a un dominio específico o tarea específica, continuando el entrenamiento con un dataset curado de ejemplos del dominio. Cambia los pesos del modelo. Costoso en compute y en preparación de datos, pero puede mejorar performance en tareas muy específicas.
*Contexto de uso:* Alternativa a RAG cuando el conocimiento es estático y la tarea es muy específica (clasificación de texto en un dominio propietario, generación con un estilo de escritura muy específico). En 2026, la tendencia es hacia RAG + long context en lugar de fine-tuning para la mayoría de los casos de uso empresariales.
*No confundir con:* RAG — que no modifica el modelo, solo inyecta contexto en runtime. RAG es más flexible y actualizable; fine-tuning graba el conocimiento en los pesos del modelo pero el conocimiento queda congelado.

---

**Guardrails** — Validaciones aplicadas al output de un LLM (y/o al input del usuario) para garantizar que el sistema cumple constraints de seguridad, formato, contenido, o comportamiento. Pueden ser síncronos (bloquean el output si falla la validación) o asíncronos (registran violaciones para revisión posterior).
*Contexto de uso:* Componente de producción en cualquier sistema LLM expuesto a usuarios no controlados. Implementaciones: Guardrails AI, Azure Content Safety, validación con un LLM secundario (LLM-as-a-Judge), expresiones regulares para datos estructurados.
*No confundir con:* System prompt instructions — que son instrucciones al modelo sobre cómo comportarse. Los guardrails son validaciones sobre el output, no instrucciones que el modelo puede ignorar.

---

**LLM-as-a-Judge** — Patrón de evaluación donde un LLM (el "judge") evalúa la calidad del output de otro LLM (el "system under test"). Escalable a gran volumen de evaluaciones sin intervención humana. Tiene sesgos propios: preferencia por respuestas verbosas, sesgo hacia sus propias respuestas si se usa el mismo modelo.
*Contexto de uso:* Evaluación continua de sistemas RAG en producción. Permiten medir métricas como faithfulness (¿el output está fundamentado en los documentos recuperados?), relevance (¿el output responde la pregunta?), coherence (¿el output es coherente internamente?).
*No confundir con:* Human evaluation — que tiene más precisión pero no escala. LLM-as-a-Judge es una aproximación al gold standard de evaluación humana.

---

**Prompt Injection** — Ataque donde el input del usuario (o contenido externo procesado por el LLM) contiene instrucciones que intentan override el system prompt, exfiltrar información, o manipular el comportamiento del modelo hacia fines no autorizados. Análogo conceptualmente a SQL injection — se mezclan datos con instrucciones.
*Contexto de uso:* Riesgo de seguridad en cualquier sistema donde el LLM procesa input no controlado (documentos de usuarios, emails, páginas web). En sistemas RAG, los documentos recuperados pueden contener prompt injection. Mitigaciones: sanitización de input, instrucciones explícitas en el system prompt sobre qué ignorar, modelos más robustos a injection.
*No confundir con:* Jailbreaking — que intenta hacer que el modelo viole sus políticas de seguridad. Prompt injection es más específico: manipular el comportamiento del sistema usando el canal de datos.

---

**RAG (Retrieval-Augmented Generation)** — Arquitectura donde un LLM recibe documentos relevantes recuperados de una base de conocimiento externa antes de generar su respuesta. Resuelve el problema de que los LLMs tienen conocimiento estático (cutoff de entrenamiento) y no tienen acceso a información propietaria del negocio. El pipeline básico: query → retrieve → augment context → generate.
*Contexto de uso:* La arquitectura de referencia para sistemas LLM empresariales: chatbots sobre documentación interna, búsqueda semántica, asistentes sobre bases de conocimiento propietarias. En entrevistas de AI System Design, es el primer patrón de integración que se espera que cualquier candidato Staff conozca en detalle.
*No confundir con:* Fine-tuning — que graba el conocimiento en los pesos del modelo. RAG es más flexible y actualizable pero tiene latencia adicional (retrieval step).

---

**Reranker** — Modelo que toma los K resultados iniciales del vector search (retrieval con bi-encoder) y los reordena con mayor precisión usando un cross-encoder, que evalúa la relevancia de cada documento considerando la query completa en contexto, no solo su embedding.
*Contexto de uso:* Segunda pasada de relevancia en pipelines RAG de producción cuando la precisión del retrieval inicial no es suficiente. El bi-encoder (embeddings) es rápido pero impreciso para relevancia; el cross-encoder es preciso pero lento. La combinación es el patrón estándar en RAG de producción: retrieve 50 con embeddings → rerank a top-5 con cross-encoder.
*No confundir con:* El modelo de embedding — que genera los vectores para el retrieval inicial. El reranker es un paso posterior de refinamiento.

---

**Tool Use / Function Calling** — Capacidad de los LLMs de invocar funciones externas definidas por el desarrollador durante la generación de la respuesta. El modelo decide cuándo y cómo llamar las herramientas disponibles, recibe los resultados, y los incorpora a su respuesta. Base técnica de los sistemas agénticos.
*Contexto de uso:* Implementación de agentes con capacidad de acción: LLMs que pueden buscar en internet, ejecutar código, leer/escribir bases de datos, llamar APIs externas. En entrevistas de AI System Design, es el mecanismo que convierte un LLM de "modelo que responde preguntas" a "agente que ejecuta tareas".
*No confundir con:* Structured Output — capacidad de los LLMs de generar JSON u otros formatos estructurados. Function calling involucra invocación externa; structured output es solo formato de respuesta.

---

## Sección 3 — Términos que se confunden frecuentemente

La distinción que importa para cada par — en 2-3 oraciones, sin relleno.

---

**Authentication vs Authorization**
Authentication es el proceso de verificar la identidad del caller ("¿quién eres?"). Authorization es el proceso de verificar si el caller autenticado tiene permiso para realizar la operación solicitada ("¿qué puedes hacer?"). Se confunden porque en muchos sistemas ocurren seguidos, pero son responsabilidades distintas que pueden fallar de formas distintas. JWT autentica; los roles en el token autorizan.

---

**Stack (memoria) vs Heap (memoria) vs Heap (estructura de datos)**
Stack (memoria): región de memoria de LIFO para variables locales y call frames — rápida, automáticamente gestionada, tamaño limitado.
Heap (memoria): región de memoria dinámica gestionada por el GC para objetos de referencia — flexible, gestionada por el garbage collector.
Heap (estructura de datos): árbol binario casi completo con la propiedad heap (max o min) — usado para implementar priority queues.
Solo el nombre es el mismo. Son tres conceptos sin relación directa entre sí.

---

**CAP Consistency vs ACID Consistency**
Son el mismo concepto descrito en contextos distintos. "Consistency" en CAP (= Linearizability) significa que todos los nodos de un sistema distribuido ven el mismo dato simultáneamente. "Consistency" en ACID significa que las transacciones llevan la base de datos de un estado válido a otro estado válido, respetando las constraints del schema. Lamentablemente comparten el nombre y se usan en conversaciones diferentes, lo que causa confusión sistemática.

---

**Sharding vs Replication vs Partitioning**
Replication: copiar los mismos datos en múltiples nodos para durabilidad y disponibilidad.
Sharding: dividir datos diferentes en múltiples nodos para escala de almacenamiento y escritura.
Partitioning: término genérico que puede referirse a sharding (particionamiento horizontal) o a cualquier forma de dividir datos. En conversaciones de system design, siempre aclarar si se habla de horizontal partitioning (= sharding) o vertical partitioning (dividir columnas en tablas separadas).

---

**Microservices vs SOA (Service-Oriented Architecture)**
SOA es la arquitectura de los años 2000 con servicios grandes, protocols estándar (SOAP, WS-*), y un bus de integración centralizado (ESB). Microservices es la evolución: servicios pequeños con una responsabilidad, comunicación ligera (HTTP/gRPC, eventos), sin bus centralizado, deployables y escalables de forma independiente. La diferencia práctica: SOA tiende a servicios de capa de negocio grandes (Order Service que maneja todo); microservices tiende a servicios muy granulares y autónomos.

---

**REST vs HTTP API vs RPC**
HTTP API: cualquier API que usa el protocolo HTTP como transporte — incluye REST y variantes no REST.
REST: estilo arquitectónico con constraints específicas (stateless, resource-based URLs, uso correcto de verbos HTTP, HATEOAS en la versión estricta). En la práctica, la mayoría de las APIs "REST" son HTTP APIs que usan convenciones de URL sin ser REST puro.
RPC (Remote Procedure Call): paradigma donde se llama a funciones remotas como si fueran locales — incluye gRPC, SOAP, JSON-RPC. En gRPC las llamadas se modelan como procedimientos, no como recursos.

---

**Availability vs Reliability vs Durability**
Availability: porcentaje del tiempo en que el sistema está operando y respondiendo requests. 99.9% availability = 8.7 horas de downtime/año.
Reliability: probabilidad de que el sistema funcione correctamente cuando se necesita — incluye correctness, no solo uptime. Un sistema puede tener alta availability pero dar resultados incorrectos (alta availability, baja reliability).
Durability: probabilidad de que los datos persistan sin pérdida una vez escritos. Amazon S3 garantiza 11 nines (99.999999999%) de durabilidad — los datos no se pierden aunque el sistema tenga downtime.

---

**RPO vs RTO**
RPO (Recovery Point Objective): cuánta pérdida de datos es aceptable, medido en tiempo — "¿hasta cuándo atrás podemos recuperar?". Determina la frecuencia de backup y la estrategia de replicación.
RTO (Recovery Time Objective): cuánto tiempo puede estar el sistema no disponible durante la recuperación — "¿cuánto tardam os en volver a servir?". Determina la arquitectura de disaster recovery (hot/warm/cold standby).
Ejemplo: RPO=0 (ninguna pérdida de datos) + RTO=4h (hasta 4 horas de downtime aceptable) es una combinación válida — replicación síncrona para datos, pero restauración manual de infraestructura.

---

**SLI vs SLO vs SLA**
SLI (Service Level Indicator): la métrica medida — "latencia del 99th percentil de las requests exitosas".
SLO (Service Level Objective): el objetivo sobre el SLI — "el SLI de latencia debe ser ≤ 200ms el 99.9% del tiempo en rolling 30 días".
SLA (Service Level Agreement): el contrato externo que define consecuencias (créditos, penalidades) si el SLO no se cumple.
La jerarquía: el SLI se mide, el SLO es el objetivo sobre el SLI, y el SLA es el compromiso contractual basado en el SLO. En muchas organizaciones el SLO es más estricto que el SLA para tener margen de buffer.

---

**Fine-tuning vs RAG vs Long Context**
Fine-tuning: modifica los pesos del modelo con datos propietarios — el conocimiento queda "grabado" en el modelo. Caro, el conocimiento queda estático.
RAG: inyecta documentos relevantes en el contexto en runtime — el conocimiento es dinámico y actualizable sin re-entrenar.
Long Context: usar un modelo con un context window muy grande y poner toda la información relevante directamente en el prompt — simple de implementar pero costoso en tokens.
La elección depende de: cuán dinámico es el conocimiento (RAG gana si cambia frecuentemente), cuán específica es la tarea (fine-tuning gana en tareas muy especializadas), y el presupuesto de latencia y costo.

---

**Prompt Engineering vs Context Engineering**
Prompt Engineering: el diseño del texto de instrucciones que recibe el LLM — técnicas de few-shot prompting, chain-of-thought, role assignment. Es una subdisciplina.
Context Engineering: el diseño del sistema completo de información — incluye prompt engineering pero también gestión del context window, diseño de memoria (qué recordar entre sesiones), estrategias de retrieval (RAG), inyección de estado de herramientas, y arquitectura del flujo de información completo.
Un prompt engineer escribe mejor prompts. Un context engineer diseña el sistema de información que alimenta al LLM.

---

**Domain Event vs Integration Event**
Domain Event: ocurrió algo significativo dentro de un Bounded Context — "OrderPlaced", "PaymentProcessed". Es interno al contexto, se procesa dentro de la misma transacción o mediante un Outbox Pattern. No cruza fronteras de microservicios directamente.
Integration Event: mensaje publicado a un message broker para comunicar un hecho a otros Bounded Contexts o microservicios. Generalmente derivado de un Domain Event, pero transformado al contrato público del emisor. Cruza fronteras de servicio y se diseña para backward compatibility.

---

**Entity (DDD) vs Value Object (DDD)**
Entity: objeto con identidad única que persiste a lo largo del tiempo. Dos Entities con los mismos atributos son distintas si tienen diferente Id. Ejemplo: dos instancias de User con el mismo nombre son personas diferentes.
Value Object: objeto sin identidad propia, definido completamente por sus atributos, inmutable. Dos Value Objects con los mismos atributos son idénticos e intercambiables. Ejemplo: Money(100, "USD") == Money(100, "USD") — no importa qué instancia uses.

---

**Memoization vs Tabulation (Dynamic Programming)**
Memoization: top-down DP con recursión + cache. El orden de resolución de subproblemas es implícito (se calcula lo que se necesita). Más natural para problemas con muchos estados posibles donde no todos se visitan.
Tabulation: bottom-up DP con iteración + tabla. El orden de resolución es explícito (de los casos base hacia el problema). Generalmente más eficiente en constante (evita overhead de recursión y call stack), y permite optimizaciones de memoria (rolling array).

---

**Backtracking vs Dynamic Programming**
Backtracking: exploración exhaustiva con pruning — genera todas las soluciones válidas o prueba que no existen. Complejidad exponencial en el peor caso, viable con buen pruning.
Dynamic Programming: optimización o conteo que evita re-explorar el mismo subproblema. No genera todas las soluciones — encuentra la óptima o cuenta cuántas existen.
La confusión viene de que ambos usan recursión. La distinción: si el problema pide "encuentra TODAS las soluciones" → backtracking. Si pide "encuentra la MEJOR solución" o "cuántas soluciones existen" → DP.

---

## Nota final del curriculum

Este documento, junto con `A-recursos-completos.md` y `B-tracking-autoevaluacion.md`, completa el curriculum zero-to-hero-v2.

Los 60 archivos del curriculum representan el mapa. Este glosario es el diccionario del territorio. Usados juntos, tienen un solo objetivo: que puedas sentarte frente a un entrevistador Staff de una empresa tech sólida — o de FAANG — y demostrar que piensas como un arquitecto, no como alguien que memorizó respuestas.

El curriculum no garantiza ese resultado. La práctica activa lo garantiza.

---

**Para reportar gaps, errores, o términos faltantes en este glosario:**
Abrir una sesión nueva en el proyecto Claude zero-to-hero-v2 con el mensaje:
*"Encontré un gap en el Apéndice C: [término o distinción faltante]. Agrégalo al glosario con el formato estándar del archivo."*
