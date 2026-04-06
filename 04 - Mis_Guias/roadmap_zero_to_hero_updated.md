# Zero to Hero: Roadmap de Maestría para Senior Software Engineer

> **Versión Expandida y Revisada** — DSA · Software Architecture · System Design  
> Diseñado para ingenieros Senior que apuntan a entrevistas de nivel Staff en FAANG/Big Tech  
> Duración estimada: **26–30 semanas** (dedicación seria, no casual)

---

## Índice

1. [Convenciones y Leyenda](#convenciones)
2. [Principios del Roadmap](#principios)
3. [Vista General del Plan](#vista-general)
4. [Fase 1 — Fundamentos Visuales y Lógica](#fase-1)
5. [Fase 2 — Dominio de Algoritmos y Patrones de Coding](#fase-2)
6. [Fase 3 — Arquitectura y Diseño de Bajo Nivel (LLD)](#fase-3)
7. [Fase 4 — System Design a Gran Escala (HLD)](#fase-4)
8. [Fase 5 — Temas Transversales Críticos](#fase-5) *(nueva)*
9. [Plan Semanal Detallado](#plan-semanal)
10. [Biblioteca de Referencia Consolidada](#biblioteca)
11. [Estrategia de Behavioral Interviews](#behavioral)
12. [Consejos de Oro para Entrevistas de Alto Nivel](#consejos-de-oro)
13. [Métricas de Progreso y Autoevaluación](#metricas)

---

## Convenciones y Leyenda {#convenciones}

| Símbolo | Significado |
|---------|-------------|
| 🔵 **Log2Base2** | Fundamentos visuales — Ver antes de implementar |
| 🟡 **AlgoExpert** | Práctica de algoritmos orientada a FAANG |
| 🟢 **Educative.io** | Patrones, LLD, HLD, Distributed Systems |
| 🔴 **Recurso gratuito** | Solo cuando agrega valor que las plataformas no cubren |
| ⚠️ **Crítico** | No omitir bajo ninguna circunstancia |
| 💡 **Insight** | Consejo táctico de alto valor |
| 🏁 **Hito** | Checkpoint de confianza antes de avanzar |

---

## Principios del Roadmap {#principios}

Este plan está diseñado sobre cuatro principios que lo distinguen de un simple listado de cursos:

**1. Visualización → Implementación → Aplicación**  
Nunca implementes lo que no has visualizado primero. Log2Base2 existe para grabar la intuición estructural en tu cerebro antes de escribir una sola línea de código. Este orden no es opcional.

**2. Patrones sobre Algoritmos**  
Las entrevistas FAANG no evalúan si memorizaste Dijkstra — evalúan si reconoces en 3 minutos que el problema es un BFS en grafo implícito. Invertir tiempo en reconocer patrones acelera tu velocidad de solución más que estudiar algoritmos individuales.

**3. Trade-offs sobre Respuestas**  
En System Design no existe "la arquitectura correcta". Existe "el mejor trade-off para estos constraints". Cada decisión que tomes debe ir acompañada de su justificación y sus limitaciones conocidas.

**4. Profundidad Secuencial**  
No saltes fases. La Fase 4 requiere comprensión real de la Fase 3. La Fase 3 requiere que los patrones de la Fase 2 sean automáticos. Saltarse fundamentos es el error más común de los ingenieros Senior que fallan entrevistas de Staff.

---

## Vista General del Plan {#vista-general}

| Fase | Nombre | Duración | Plataforma Principal | Enfoque |
|------|--------|----------|----------------------|---------|
| 1 | Fundamentos Visuales y Lógica | 5–6 semanas | Log2Base2 + AlgoExpert | Estructuras de datos, complejidad, bases |
| 2 | Algoritmos y Patrones de Coding | 7–8 semanas | Educative + AlgoExpert | 15 patrones, DP, Graphs, práctica masiva |
| 3 | Arquitectura LLD y OOD | 4–5 semanas | Educative | SOLID, GoF, Clean Architecture, OOD |
| 4 | System Design HLD | 6–7 semanas | Educative + Papers | Distributed systems, escalabilidad, HLD |
| 5 | Temas Transversales Críticos | 3–4 semanas | Mixto | Concurrencia, Networking, Security, Observability |

**Total estimado:** 25–30 semanas de trabajo enfocado (15–20 hrs/semana)

---

## Fase 1 — Fundamentos Visuales y Lógica {#fase-1}

> *"El cerebro retiene estructuras que vio antes de codificarlas. No te saltes la visualización."*

**Duración:** 5–6 semanas  
**Objetivo:** Cerrar lagunas en fundamentos que se vuelven evidentes bajo presión de entrevista. Todo ingeniero Senior tiene al menos una.

### Temas Clave

- Arrays, Strings y manipulación de memoria
- Linked Lists (simple, doble, circular)
- Stacks y Queues (implementación y aplicaciones)
- Hash Tables (colisiones, open addressing, chaining)
- Trees: Binary Tree, BST, AVL, Trie
- Heaps y Priority Queues
- Graphs: representaciones (matriz de adyacencia, lista de adyacencia)
- Recursión, Stack de llamadas y Backtracking básico
- Análisis de Complejidad: Big-O, Big-Θ, Big-Ω, complejidad espacial
- Algoritmos de ordenamiento clásico: QuickSort, MergeSort, HeapSort
- Binary Search y sus variantes
- Two Pointers como técnica base

### Ruta de Aprendizaje

| Sem. | Plataforma | Curso / Módulo | Por qué este recurso ahora |
|------|------------|---------------|---------------------------|
| 1–2 | 🔵 Log2Base2 | Data Structures Visual Series: Arrays, Linked Lists, Stacks, Queues, Trees | Las animaciones de Log2Base2 graban la intuición visual antes de cualquier código. Absoluta prioridad para quien aprendió DSA de forma abstracta o hace años. |
| 2–3 | 🔵 Log2Base2 | Graphs, Heaps y Tries — Serie avanzada de estructuras | Cierra el ecosistema de estructuras con animaciones de traversal BFS/DFS antes de implementar en AlgoExpert. Ver el recorrido en movimiento antes de escribirlo. |
| 3–4 | 🟡 AlgoExpert | Data Structures Crash Course (sección integrada) | Refuerza con implementación real en tu lenguaje. Las explicaciones de Clément son directas, densas y orientadas 100% a entrevistas. Sin relleno. |
| 4–5 | 🟡 AlgoExpert | Problemas Easy & Medium: Arrays, Strings, Linked Lists, Trees (40–50 problemas) | Consolidar cada estructura resolviendo problemas categorizados. Usa el modo "Watch Solution" SOLO tras intentar mínimo 20 minutos genuinos. |
| 5–6 | 🔴 MIT OCW | MIT 6.006 — Lectures 1–6 (Introduction to Algorithms) | Las plataformas no reemplazan el rigor académico en complejidad amortizada y análisis formal. Solo las primeras 6 clases para solidificar Big-O matemáticamente. |

### ⚠️ Gaps Críticos de la Versión Anterior (añadidos)

**Análisis de Complejidad Amortizada** — No solo Big-O en el peor caso. Entiende por qué `push` en un ArrayList dinámico es O(1) amortizado aunque ocasionalmente sea O(n). Esto aparece en entrevistas de nivel Senior.

**Hash Tables en profundidad** — La mayoría de los cursos enseñan el API. Debes saber implementar una Hash Table desde cero: función hash, manejo de colisiones con chaining vs open addressing, y el concepto de load factor y rehashing.

**Recursos complementarios:**
- 🔴 *Visualgo.net* — Visualizaciones interactivas de todos los algoritmos de sorting y estructuras. Complemento gratuito perfecto para Log2Base2.
- 🔴 *CS50 — Week 5: Data Structures* (Harvard OpenCourseWare, YouTube) — La explicación de David Malan sobre Hash Tables es la mejor introducción existente.

### Checklist de Progreso — Fase 1

- [ ] Puedo dibujar en papel cualquier estructura de datos y explicar sus operaciones en O(n)
- [ ] Implementé desde cero: Linked List, Stack, Queue, BST, Min-Heap, Graph (lista de adyacencia)
- [ ] Implementé una Hash Table desde cero con manejo de colisiones por chaining
- [ ] Resuelvo problemas Easy de Arrays/Strings en menos de 15 minutos sin ayuda
- [ ] Explico la diferencia entre O(log n), O(n log n) y O(n²) con ejemplos reales de código
- [ ] Entiendo complejidad amortizada y puedo dar al menos 2 ejemplos concretos
- [ ] Completé 40+ problemas en AlgoExpert en las categorías de Fase 1

### 🏁 Hito de Confianza — Fase 1

Resuelve en vivo, sin buscar, con un límite de 30 minutos, estos tres problemas de AlgoExpert: **Binary Search**, **Validate BST** y **Min Height BST**. Resueltos correctamente con análisis de complejidad espacial y temporal incluido, tienes el derecho de avanzar a Fase 2.

---

## Fase 2 — Dominio de Algoritmos y Patrones de Coding {#fase-2}

> *"No memorizas algoritmos. Reconoces patrones. Esa distinción es la diferencia entre prepararte 3 meses y prepararte 8."*

**Duración:** 7–8 semanas  
**Objetivo:** Transformar el conocimiento de estructuras en patrones reutilizables de resolución de problemas bajo presión temporal.

### Los 15 Patrones Maestros

| # | Patrón | Tipos de Problema | Estructura Clave |
|---|--------|------------------|-----------------|
| 1 | Sliding Window | Subarray/substring con condición | Array / String |
| 2 | Two Pointers | Pares, tripletes, palíndromos | Array ordenado |
| 3 | Fast & Slow Pointers | Ciclos, punto medio | Linked List |
| 4 | Merge Intervals | Solapamiento de rangos | Array de intervalos |
| 5 | Cyclic Sort | Números en rango [1, n] | Array |
| 6 | In-place Reversal | Invertir Linked List por grupos | Linked List |
| 7 | BFS en árbol/grafo | Nivel por nivel, camino más corto | Queue |
| 8 | DFS en árbol/grafo | Path sum, island problems | Stack / Recursión |
| 9 | Two Heaps | Mediana en stream | Min-Heap + Max-Heap |
| 10 | Subsets / Backtracking | Permutaciones, combinaciones | Recursión |
| 11 | Binary Search modificado | Rotated arrays, peak finding | Array |
| 12 | Top K Elements | K más frecuentes/grandes/cercanos | Heap |
| 13 | K-way Merge | Merge de K listas ordenadas | Heap |
| 14 | Topological Sort | Prerequisitos, dependencias | Graph + Indegree |
| 15 | Dynamic Programming | Optimización con subproblemas | Tabla / Memoización |

### Temas Adicionales (Fase 2)

- Greedy Algorithms y cuándo son válidos vs DP
- Divide & Conquer y su relación con recursión
- Union-Find / Disjoint Set Union (DSU)
- Bit Manipulation: máscaras, XOR tricks, popcount
- String algorithms: KMP, Rabin-Karp (conceptual para entrevistas Staff)
- Math para entrevistas: números primos, GCD/LCM, potenciación modular

### Ruta de Aprendizaje

| Sem. | Plataforma | Curso / Módulo | Por qué ahora |
|------|------------|---------------|---------------|
| 7–8 | 🟢 Educative | Grokking the Coding Interview: Patterns for Coding Questions | El curso definitivo de patrones. Aprende cada patrón conceptualmente antes de practicar en volumen. Educative supera a cualquier otro recurso para esto. Seguir el orden del curso estrictamente. |
| 8–9 | 🔵 Log2Base2 | Dynamic Programming — Serie visual (subproblemas, memoization, tabulation) | DP es el tema que más candidatos reprueba. Log2Base2 visualiza la construcción de tablas DP de forma que ningún texto logra. Ver esto ANTES de los problemas de DP en AlgoExpert. |
| 9–12 | 🟡 AlgoExpert | Problemas Medium & Hard por patrón: Sliding Window, Graphs, DP, Greedy, Backtracking | AlgoExpert tiene la mejor cobertura de problemas estilo FAANG. Mínimo 6 problemas por día. Usa su categorización por tema. Meta: 120+ problemas resueltos. |
| 12–13 | 🟢 Educative | Grokking Dynamic Programming Patterns for Coding Interviews | Complemento para cerrar DP: cubre 0/1 Knapsack, Fibonacci avanzado, Palindromic Subsequences y variantes que aparecen en entrevistas Staff. |
| 13–14 | 🟡 AlgoExpert | Mock Interviews (feature integrada) + revisión de Hard problems | Simula presión real. Timer activo, sin ayuda externa, grabarte si es posible para revisar tu comunicación. |

### ⚠️ Gaps Críticos Añadidos (no estaban en versión anterior)

**Union-Find (DSU)** — Aparece con alta frecuencia en problemas de grafos de nivel Medium-Hard: Number of Islands II, Accounts Merge, Redundant Connection. No estaba en la ruta original.

**Bit Manipulation** — Subestimado. Problemas como Single Number, Missing Number, Power of Two se resuelven en O(1) con XOR. Las entrevistas de empresas como Meta lo usan frecuentemente.

**NeetCode 150 como guía de práctica paralela:**
- 🔴 *NeetCode.io* — Lista curada de 150 problemas de LeetCode organizados por patrón, con video soluciones gratuitas. Úsala como referencia paralela a AlgoExpert para diversificar problemas.

**Recursos complementarios:**
- 🔴 *Back to Back SWE (YouTube)* — Explicaciones visuales de DP, Trees y Graphs de calidad excepcional
- 🔴 *William Fiset (YouTube)* — La mejor serie de Graphs en YouTube: DFS, BFS, Dijkstra, Bellman-Ford, Bridges, SCCs

### Checklist de Progreso — Fase 2

- [ ] Identifico el patrón correcto en los primeros 3 minutos de leer un problema
- [ ] Resuelvo problemas Medium de LeetCode en menos de 25 minutos consistentemente
- [ ] Implemento Dijkstra, BFS, DFS y Topological Sort de memoria en cualquier lenguaje
- [ ] Implemento soluciones DP en dos variantes: top-down (memoization) y bottom-up (tabulation)
- [ ] Implemento Union-Find con path compression y union by rank
- [ ] Resuelvo al menos 5 problemas de Bit Manipulation
- [ ] Completé 120+ problemas en AlgoExpert con soluciones propias documentadas
- [ ] Realicé al menos 5 mock interviews cronometradas

### 🏁 Hito de Confianza — Fase 2

Completa en 45 minutos sin ayuda: **Word Ladder II** (BFS en grafo implícito), **Coin Change** (DP bottom-up) y **Course Schedule II** (Topological Sort). Si los explicas en voz alta con análisis de complejidad espacial y temporal correcto, estás listo para Fase 3.

---

## Fase 3 — Arquitectura y Diseño de Bajo Nivel (LLD) {#fase-3}

> *"La mayoría de candidatos descuida LLD. Aquí demuestras que eres Senior real, no solo alguien que memorizó diagramas de AWS."*

**Duración:** 4–5 semanas  
**Objetivo:** Dominar el diseño de clases, objetos, módulos y APIs con principios SOLID, patrones GoF y Clean Architecture.

### Temas Clave

**SOLID Principles**
- Single Responsibility Principle (SRP)
- Open/Closed Principle (OCP)
- Liskov Substitution Principle (LSP)
- Interface Segregation Principle (ISP)
- Dependency Inversion Principle (DIP)

**Design Patterns — GoF (Gang of Four)**

| Categoría | Patrones Clave para Entrevistas |
|-----------|--------------------------------|
| Creacionales | Singleton, Factory Method, Abstract Factory, Builder, Prototype |
| Estructurales | Adapter, Decorator, Facade, Composite, Proxy |
| Comportamiento | Strategy, Observer, Command, Iterator, Template Method, State, Chain of Responsibility |

**Clean Architecture**
- Capas: Entities, Use Cases, Interface Adapters, Frameworks
- Dependency Rule: las dependencias apuntan hacia adentro
- Separation of Concerns y cohesión modular
- Ports & Adapters (Hexagonal Architecture)

**Object Oriented Design Avanzado**
- Composition vs Inheritance: cuándo usar cada uno
- Principio de Inversión de Dependencias en práctica
- Diseño de APIs públicas y privadas
- Manejo de errores y excepciones en el diseño
- Inmutabilidad y diseño funcional aplicado a OOP

### Ruta de Aprendizaje

| Sem. | Plataforma | Curso / Módulo | Por qué ahora |
|------|------------|---------------|---------------|
| 15–16 | 🟢 Educative | Grokking the Low Level Design Interview Using OOD Principles | El mejor curso de LLD en formato de entrevista. Cubre los patrones con problemas tipo entrevista: Parking Lot, ATM, Chess, Library Management. Seguir en orden estricto. |
| 16 | 🟢 Educative | SOLID Principles — sección dentro del mismo curso | SOLID no es trivia de entrevista. Es el framework mental con el que justificas cada decisión de diseño. Educative lo conecta directamente con los patrones GoF. |
| 16–17 | 🔴 Libro: Design Patterns (GoF) | Capítulos: Strategy, Observer, Factory Method, Decorator, Singleton, Command, Adapter | Solo estos 6+1 patrones más frecuentes en entrevistas. El libro original tiene la definición canónica. Lee los capítulos específicos para profundidad real. Las plataformas simplifican. |
| 17–18 | 🟢 Educative | Grokking the Object Oriented Design Interview (problemas prácticos) | Practica diseñando sistemas de complejidad media: Movie Booking, Food Delivery, Amazon Lockers. Cada problema debe incluir diagrama de clases y justificación de patrones elegidos. |
| 18–19 | 🔴 YouTube: Arjan Codes | Serie "Design Patterns" — implementaciones reales en Python/Java | Arjan muestra patrones en código de producción real, no pseudocódigo. Su canal cierra el gap entre teoría del libro y código que pasaría un code review. |

### ⚠️ Gaps Críticos Añadidos

**Clean Architecture vs Layered Architecture** — La versión anterior no cubría esto. En entrevistas de Staff se espera que puedas comparar Hexagonal Architecture, Clean Architecture y Layered Architecture, explicar sus trade-offs y justificar cuándo usarías cada una.

**API Design** — Un tema omitido en la versión anterior. Diseñar APIs REST claras, idempotentes, versionadas y con contratos bien definidos es habilidad esperada en Senior. Recursos:
- 🔴 *Google API Design Guide* (aip.dev) — Gratuito, canónico
- 🔴 *Stripe API Docs* — El estándar de la industria para diseño de APIs de pago

**Refactoring** — Identificar y corregir code smells es parte del LLD interview. Recurso:
- 🔴 *Refactoring Guru* (refactoring.guru) — Catálogo completo de smells y refactors, gratuito

**Domain Driven Design (DDD) Conceptual** — No necesitas dominar DDD completo, pero debes conocer: Aggregate, Entity, Value Object, Domain Service, Bounded Context. Estos conceptos aparecen en entrevistas de arquitectura.

### Recursos Complementarios

- 🔴 *Refactoring: Improving the Design of Existing Code* — Martin Fowler. El libro de referencia absoluto para identificar code smells y aplicar refactors estructurados.
- 🔴 *Clean Architecture* — Robert C. Martin (Uncle Bob). Leer los capítulos 1–5 y 15–22.

### Checklist de Progreso — Fase 3

- [ ] Diseño el sistema de un Parking Lot con diagrama UML en 30 minutos sin referencias
- [ ] Explico Composition vs Inheritance con un ejemplo real de código y sus trade-offs
- [ ] Identifico las 5 violaciones de SOLID más comunes en un fragmento de código y las corrijo
- [ ] Implemento 7 patrones GoF de memoria: Strategy, Observer, Factory, Decorator, Singleton, Command, Adapter
- [ ] Explico la diferencia entre Clean Architecture y Layered Architecture con ventajas y desventajas
- [ ] Diseñé al menos 3 sistemas LLD completos con diagrama de clases y justificación de patrones
- [ ] Conozco los conceptos core de DDD: Aggregate, Entity, Value Object, Bounded Context

### 🏁 Hito de Confianza — Fase 3

En 40 minutos, diseña en vivo el **sistema de Hotel Booking** (clases, relaciones, patrones aplicados, principios SOLID, API pública REST). Debe incluir: diagrama de clases completo, justificación de cada patrón GoF elegido, análisis de extensibilidad futura y al menos una violación de SOLID que identificaste y corregiste durante el diseño.

---

## Fase 4 — System Design a Gran Escala (HLD) {#fase-4}

> *"Esta ronda define si eres Senior o Staff. El entrevistador no busca la respuesta correcta — busca que pienses como alguien que ha operado sistemas de millones de usuarios."*

**Duración:** 6–7 semanas  
**Objetivo:** Diseñar sistemas distribuidos escalables con trade-offs justificados, estimaciones back-of-the-envelope y comprensión profunda de los componentes de infraestructura.

### Framework de System Design Interview (RESHADED)

Usa este framework en todas tus entrevistas HLD:

| Paso | Acción | Tiempo |
|------|--------|--------|
| **R** — Requirements | Clarifica functional y non-functional requirements | 5 min |
| **E** — Estimation | Back-of-the-envelope: usuarios, QPS, storage, bandwidth | 5 min |
| **S** — System Interface | Define los endpoints/APIs principales | 3 min |
| **H** — High-Level Design | Dibuja los componentes macro y su interacción | 10 min |
| **A** — Algorithm/Data | Elige estructuras de datos y algoritmos clave | 5 min |
| **D** — Deep Dive | Profundiza en los 1–2 componentes más críticos | 12 min |
| **E** — Evolution | Cómo evoluciona el sistema: fallas, escala 10x, nuevos features | 5 min |
| **D** — Discussion | Trade-offs, alternativas descartadas, open questions | 5 min |

### Temas Clave

**Fundamentos de Escalabilidad**
- Escalabilidad horizontal vs vertical
- Stateless vs Stateful services
- Load Balancing: Round Robin, Least Connections, IP Hash, Consistent Hashing
- CDN: edge caching, origin pull vs push

**Bases de Datos**
- SQL vs NoSQL: cuándo usar cada tipo con justificación técnica
- Sharding estrategias: horizontal, vertical, directory-based, range-based
- Replicación: master-slave, master-master
- Indexing: B-Tree, LSM-Tree, diferencias de trade-off
- NewSQL: CockroachDB, Spanner (ACID a escala)
- Column-family: Cassandra, HBase (write-heavy, time-series)
- Document: MongoDB (flexible schema, geospatial)
- Graph: Neo4j (relaciones complejas)

**Caching**
- Cache-aside, write-through, write-behind, read-through
- Redis: estructuras de datos avanzadas, pub/sub, Lua scripts
- Cache invalidation strategies
- Cache stampede y cómo prevenirlo

**Mensajería y Eventos**
- Message Queues vs Event Streaming: diferencias fundamentales
- Kafka: particiones, consumer groups, offsets, retention, log compaction
- RabbitMQ: exchanges, routing keys, DLQ
- Pub/Sub vs Point-to-Point
- Event Sourcing y CQRS

**Patrones de Disponibilidad**
- Circuit Breaker, Retry con exponential backoff
- Bulkhead pattern
- Graceful degradation
- Health checks y readiness/liveness probes
- Blue-Green Deployment, Canary Releases

**Distributed Systems Fundamentals**
- CAP Theorem: ejemplos reales por cuadrante (CP, AP, CA)
- PACELC Theorem (extensión del CAP)
- Eventual consistency patterns: CRDT, Vector Clocks
- Consensus: Raft vs Paxos (conceptual y diferencias)
- Distributed transactions: 2PC, Saga Pattern
- Idempotencia y exactly-once delivery

### Ruta de Aprendizaje

| Sem. | Plataforma | Curso / Módulo | Por qué ahora |
|------|------------|---------------|---------------|
| 20–21 | 🟢 Educative | Grokking Modern System Design Interview for Engineers & Managers | El curso más completo de HLD con framework estructurado (Requirements → API → DB → Scaling). Seguir el orden de Educative: cada lección construye sobre la anterior. |
| 21–22 | 🟢 Educative | Grokking the Principles and Practices of Advanced System Design | El nivel avanzado: Raft, Paxos, distributed transactions, eventual consistency patterns. Aquí se separa el Senior del Staff. |
| 22 | 🟡 AlgoExpert | SystemsExpert — Módulo completo | Clément tiene un enfoque 100% orientado a entrevistas. Sus videos de System Design tienen una claridad difícil de igualar. Complemento de Educative, no sustituto. |
| 22–23 | 🔴 Papers originales | Amazon Dynamo, Google Spanner, Kafka Design, Raft, Google Bigtable, MapReduce | Ninguna plataforma reemplaza los papers originales. Citar el paper de Dynamo cuando hablas de eventual consistency es la diferencia entre "Hire" y "Strong Hire". |
| 23–24 | 🔴 YouTube: ByteByteGo | System Design series: URL Shortener, Twitter, YouTube, WhatsApp, Uber, Netflix | Alex Xu tiene los mejores videos de síntesis visual. Después de Educative, usa ByteByteGo para revisar arquitecturas end-to-end en 10–15 minutos cada una. |
| 24–26 | 🟢 Educative + práctica propia | Mock System Design Interviews + diseño sin guía cronometrado | Diseña 8–10 sistemas completos desde cero en papel/whiteboard. 45 min por sistema. Sin referencias. |

### Papers Obligatorios (con URL de búsqueda)

| Paper | Autores | Por qué leerlo |
|-------|---------|----------------|
| *Dynamo: Amazon's Highly Available Key-value Store* | DeCandia et al., 2007 | Consistent hashing, vector clocks, eventual consistency. Fundamento de DynamoDB y Cassandra. |
| *Bigtable: A Distributed Storage System for Structured Data* | Chang et al., 2006 | Column-family databases. Base de HBase y Apache Cassandra. |
| *MapReduce: Simplified Data Processing on Large Clusters* | Dean & Ghemawat, 2004 | Distributed computation. Entiende por qué Spark lo reemplazó y sus limitaciones. |
| *Spanner: Google's Globally Distributed Database* | Corbett et al., 2012 | TrueTime API, ACID a escala global. El sistema de bases de datos más ambicioso de Google. |
| *In Search of an Understandable Consensus Algorithm (Raft)* | Ongaro & Ousterhout, 2014 | Alternativa comprensible a Paxos. Usado en etcd, CockroachDB, TiKV. |
| *Kafka: a Distributed Messaging System for Log Processing* | Kreps et al., 2011 | Log distribuido, offset commits, consumer groups. Esencial para arquitecturas event-driven. |
| *The Chubby Lock Service for Loosely-Coupled Distributed Systems* | Burrows, 2006 | Distributed locking, coordination services. Base de Zookeeper. |

### Sistemas de Práctica (diseña estos end-to-end)

| Sistema | Complejidad | Patrones Clave |
|---------|-------------|----------------|
| URL Shortener | Media | Hashing, DB choice, cache |
| Pastebin | Media | Storage, expiry, CDN |
| Rate Limiter distribuido | Alta | Token bucket, Redis, multi-region |
| Twitter Feed / Timeline | Alta | Fan-out on write vs read, cache, sharding |
| YouTube / Netflix | Alta | CDN, transcoding pipeline, adaptive streaming |
| Uber / Lyft | Alta | Geospatial indexing, real-time matching, surge |
| Slack / WhatsApp | Alta | WebSockets, message ordering, presence |
| Dropbox / Google Drive | Alta | Chunking, sync protocol, conflict resolution |
| Search Autocomplete | Media | Trie, top-K, cache |
| Distributed Job Scheduler | Alta | Idempotencia, at-least-once, partitioning |

### ⚠️ Gaps Críticos Añadidos

**PACELC Theorem** — El CAP theorem es incompleto. PACELC extiende el análisis considerando latencia vs consistencia incluso cuando no hay partición de red. Es el framework moderno para comparar sistemas como DynamoDB, Cassandra y CockroachDB.

**Saga Pattern para transacciones distribuidas** — La versión anterior mencionaba 2PC pero no Saga. En microservicios modernos, Saga (coreografía vs orquestación) es el patrón estándar para transacciones distribuidas. Aparece en entrevistas Staff de Meta, Uber y Airbnb.

**Event Sourcing y CQRS** — Dos patrones de arquitectura fundamentales en sistemas de alta escala que no estaban en la versión anterior. CQRS permite escalar lectura y escritura independientemente; Event Sourcing provee audit trail completo y permite reconstruir estado.

**Recursos adicionales:**
- 🔴 *Designing Data-Intensive Applications* — Martin Kleppmann. El libro definitivo. Leer Capítulos 1–3 (storage engines), 5–6 (replication, partitioning), 8–9 (distributed systems, consistency). No hay sustituto.
- 🔴 *System Design Interview Vol 1 & 2* — Alex Xu. Complemento práctico de DDIA para entrevistas.

### Checklist de Progreso — Fase 4

- [ ] Diseño un sistema para 10M de usuarios con trade-offs justificados en menos de 45 minutos
- [ ] Explico CAP y PACELC con ejemplos de sistemas reales (DynamoDB, Zookeeper, Cassandra, Spanner)
- [ ] Propongo el tipo de base de datos correcto (SQL/NoSQL/NewSQL) justificando la elección
- [ ] Explico Consistent Hashing, Virtual Nodes y por qué soluciona el problema del resharding
- [ ] Diseñé end-to-end al menos 5 sistemas de la lista de práctica
- [ ] Leí y puedo discutir al menos 4 de los papers listados
- [ ] Explico Saga Pattern vs 2PC con sus trade-offs en transacciones distribuidas
- [ ] Realicé al menos 5 mock System Design interviews cronometradas con evaluación externa

### 🏁 Hito de Confianza — Fase 4

Diseña en 45 minutos el **Distributed Rate Limiter** para una API que maneja 100,000 req/s con soporte multi-region, recuperación ante fallos y consistencia eventual. Debe incluir: architecture diagram, justificación del algoritmo elegido (token bucket vs sliding window vs leaky bucket), elección de base de datos con justificación, manejo de edge cases (thundering herd, clock skew entre regiones) y estrategia de degradación ante fallas del almacén de estado.

---

## Fase 5 — Temas Transversales Críticos {#fase-5}

> *Esta fase es nueva. No existía en la versión anterior. Son los temas que separan a los candidatos buenos de los Strong Hire.*

**Duración:** 3–4 semanas (puede solaparse con Fase 4)  
**Objetivo:** Cubrir los pilares que las plataformas no unifican bien: concurrencia, networking, seguridad y observabilidad.

---

### 5.1 Concurrencia y Multithreading

**Por qué es crítico:** Las entrevistas de nivel Senior en Amazon, Google y Meta incluyen preguntas de diseño concurrente. Un bounded blocking queue, un rate limiter thread-safe o un task scheduler son preguntas estándar.

**Temas a dominar:**

- Threads, procesos, y el modelo de memoria compartida
- Race conditions, deadlocks, livelocks, starvation
- Mecanismos de sincronización: mutex, semaphore, monitor, condition variable
- Lock-free data structures: CAS (Compare-And-Swap), atomic operations
- Thread pools: design, queue types, rejection policies
- Concurrency patterns: Producer-Consumer, Reader-Writer, Dining Philosophers
- Java: `synchronized`, `volatile`, `java.util.concurrent` (CountDownLatch, CyclicBarrier, ExecutorService)
- Python: GIL, `threading`, `asyncio`, `concurrent.futures`
- Async I/O: event loop, coroutines, non-blocking I/O

**Recursos:**

| Recurso | Tipo | Por qué |
|---------|------|---------|
| 🟢 Educative: *Java Multithreading for Senior Engineering Interviews* | Plataforma | El más directo al formato de entrevista |
| 🔴 *Java Concurrency in Practice* — Brian Goetz | Libro | La referencia absoluta para concurrencia en JVM |
| 🔴 *The Art of Multiprocessor Programming* — Herlihy & Shavit | Libro | Para nivel Staff: algoritmos lock-free y wait-free |

**Problemas de práctica obligatorios:**
- Implementa un Bounded Blocking Queue thread-safe
- Implementa un Read-Write Lock desde cero
- Implementa un Thread Pool con queue y worker threads
- Implementa el patrón Dining Philosophers sin deadlock
- Diseña un Rate Limiter thread-safe para un servidor HTTP

---

### 5.2 Networking Fundamentals para System Design

**Por qué es crítico:** No puedes diseñar sistemas distribuidos sin entender la red sobre la que corren. Los entrevistadores de Staff nivel detectan inmediatamente si no entiendes la diferencia entre TCP y UDP en el contexto de tu diseño.

**Temas a dominar:**

- Modelo OSI vs TCP/IP stack
- TCP: three-way handshake, congestion control, flow control, TIME_WAIT
- UDP: cuándo es la elección correcta (gaming, streaming, DNS)
- HTTP/1.1 vs HTTP/2 vs HTTP/3 (QUIC): diferencias prácticas para diseño
- WebSockets y Server-Sent Events: cuándo usar cada uno
- gRPC vs REST vs GraphQL: trade-offs por caso de uso
- DNS: resolución, TTL, CDN routing, GeoDNS
- TLS/SSL: handshake, certificate chains, mTLS
- Proxies: forward proxy, reverse proxy, API gateway
- Long polling vs WebSockets vs SSE

**Recursos:**

| Recurso | Tipo | Por qué |
|---------|------|---------|
| 🔴 *Computer Networks: A Top-Down Approach* — Kurose & Ross (Cap. 1–3, 6) | Libro | La mejor introducción académica a networking |
| 🔴 ByteByteGo: "How HTTPS Works", "TCP vs UDP", "HTTP/1 vs HTTP/2 vs HTTP/3" | YouTube | Síntesis visual perfecta para System Design |
| 🔴 Cloudflare Blog (blog.cloudflare.com) | Blog | Artículos técnicos sobre DNS, TLS, CDN desde la perspectiva de producción |

---

### 5.3 Security Fundamentals en System Design

**Por qué es crítico:** En entrevistas de Staff y Principal Engineer, se espera que incluyas consideraciones de seguridad en tu diseño sin que el entrevistador las pida explícitamente. Es un differentiator enorme.

**Temas a dominar:**

- Autenticación vs Autorización: diferencia fundamental
- OAuth 2.0 flows: Authorization Code, Client Credentials, PKCE
- JWT: estructura, firma, validación, refresh tokens, revocación
- Session management: cookies vs tokens, secure/httpOnly/SameSite
- API security: rate limiting, input validation, SQL injection, XSS, CSRF
- Secrets management: vault, environment variables, key rotation
- Encryption at rest vs in transit: AES, RSA, ECC
- Zero Trust Architecture: principio de menor privilegio
- RBAC vs ABAC: modelos de autorización
- DDoS protection: Anycast, rate limiting por IP/user/endpoint

**Recursos:**

| Recurso | Tipo | Por qué |
|---------|------|---------|
| 🔴 *OWASP Top 10* (owasp.org) | Referencia | El estándar de la industria para vulnerabilidades web |
| 🔴 *The Web Application Hacker's Handbook* (Cap. 1–5) | Libro | Para entender ataques desde el punto de vista del atacante |
| 🔴 Auth0 Blog: serie de OAuth 2.0 | Blog | La explicación más clara de OAuth flows con diagramas |

---

### 5.4 Observability y Monitoring en Sistemas Distribuidos

**Por qué es crítico:** Diseñar un sistema sin hablar de cómo lo vas a operar es una señal clara de inexperiencia en entrevistas de Staff. "¿Cómo sabrías si este sistema tiene un problema a las 3am?" es una pregunta frecuente.

**Los tres pilares de Observabilidad:**

| Pilar | Descripción | Herramientas |
|-------|-------------|--------------|
| **Logs** | Eventos discretos con contexto | ELK Stack, CloudWatch Logs, Loki |
| **Metrics** | Datos numéricos en series de tiempo | Prometheus, Grafana, Datadog |
| **Traces** | Flujo de una request a través de servicios | Jaeger, Zipkin, AWS X-Ray |

**Temas a dominar:**

- SLI, SLO, SLA: definiciones y cómo calcularlos
- Error budgets y cómo afectan las decisiones de deployment
- Golden signals: latency, traffic, errors, saturation
- Structured logging vs unstructured logging
- Distributed tracing: context propagation, sampling strategies
- Alerting: symptom-based vs cause-based, avoiding alert fatigue
- Runbooks y on-call culture
- Chaos Engineering: principios de Chaos Monkey, fault injection

**Recursos:**

| Recurso | Tipo | Por qué |
|---------|------|---------|
| 🔴 *Site Reliability Engineering* (SRE Book) — Google (free online) | Libro | El estándar de la industria para operar sistemas a escala. Capítulos 4–6, 13–14. |
| 🔴 *Observability Engineering* — Charity Majors | Libro | La evolución de monitoring hacia observability real |
| 🔴 Honeycomb Blog (honeycomb.io/blog) | Blog | Artículos técnicos de alta calidad sobre observabilidad en producción |

### Checklist de Progreso — Fase 5

- [ ] Implemento un Bounded Blocking Queue thread-safe con mutex y condition variables
- [ ] Explico el three-way handshake de TCP y por qué TIME_WAIT existe
- [ ] Diseño el flujo de autenticación de una API con OAuth 2.0 y JWT incluyendo refresh tokens
- [ ] Incluyo consideraciones de observabilidad (logs, metrics, traces) en cualquier diseño HLD
- [ ] Defino SLI/SLO para un sistema que diseño y calculo su error budget
- [ ] Explico la diferencia entre HTTP/1.1, HTTP/2 y HTTP/3 con implicaciones para el diseño

---

## Plan Semanal Detallado {#plan-semanal}

> *Basado en 15–20 horas semanales de trabajo enfocado. Ajusta según tu disponibilidad pero no comprimas las fases — extiéndelas.*

### Distribución Semanal Recomendada (template)

| Día | Actividad | Duración |
|-----|-----------|----------|
| Lunes | Nuevo contenido (curso/video/paper) | 2–3 hrs |
| Martes | Problemas de práctica del tema del lunes | 2–3 hrs |
| Miércoles | Repaso de problemas fallidos + nuevos conceptos | 2 hrs |
| Jueves | Problemas de práctica + implementación desde cero | 2–3 hrs |
| Viernes | Mock interview (coding o system design, alternando) | 1.5 hrs |
| Sábado | Revisión semanal + lectura de libro/paper | 2–3 hrs |
| Domingo | Descanso activo o resolución libre sin presión | 0–1 hr |

### Semanas 1–6 (Fase 1 — Fundamentos)

| Semana | Lunes–Martes | Miércoles–Jueves | Viernes | Sábado |
|--------|-------------|-----------------|---------|--------|
| 1 | Log2Base2: Arrays + Linked Lists | Implementa LL desde cero + 10 problemas Easy | Mock: 2 problemas con timer | MIT 6.006 Lecture 1 |
| 2 | Log2Base2: Stacks, Queues, Hash Tables | Implementa Stack y Hash Table + 10 problemas | Mock: 2 problemas con timer | MIT 6.006 Lecture 2–3 |
| 3 | Log2Base2: Trees + BST | Implementa BST con insert/delete/search + 10 problemas | Mock: 3 problemas con timer | AlgoExpert Crash Course |
| 4 | Log2Base2: Heaps + Graphs | Implementa Min-Heap y Graph + 15 problemas | Mock: 3 problemas con timer | MIT 6.006 Lecture 4–5 |
| 5 | AlgoExpert: Medium Arrays/Strings | 20 problemas Medium categorizados | Mock: 4 problemas con timer | MIT 6.006 Lecture 6 |
| 6 | AlgoExpert: Medium Trees/Graphs | 20 problemas Medium + revisión de fallidos | **Hito Fase 1** | Revisión general |

### Semanas 7–14 (Fase 2 — Algoritmos)

| Semana | Contenido Principal | Meta de Problemas |
|--------|--------------------|--------------------|
| 7 | Educative: Sliding Window + Two Pointers | 15 problemas del patrón |
| 8 | Educative: Fast/Slow + BFS/DFS en grafos | 15 problemas del patrón |
| 9 | Log2Base2 DP + Educative: Grokking DP Part 1 | 10 problemas DP básicos |
| 10 | Educative: Grokking DP Part 2 + AlgoExpert Hard | 15 problemas DP + 5 Hard |
| 11 | Educative: Top K + Topological Sort + Union-Find | 15 problemas mixtos |
| 12 | AlgoExpert: Sprint de Hard problems | 25 problemas Hard |
| 13 | Educative: Grokking DP Patterns | 15 problemas DP avanzados |
| 14 | Mock interviews intensivo + revisión de errores | **Hito Fase 2** |

### Semanas 15–19 (Fase 3 — LLD)

| Semana | Contenido Principal | Entregable de Práctica |
|--------|--------------------|-----------------------|
| 15 | Educative LLD: Patrones 1–10 + SOLID | Diseña Parking Lot completo |
| 16 | Educative LLD: Patrones 11–24 + libro GoF (6 patrones) | Diseña ATM System |
| 17 | Educative OOD + Refactoring Guru | Diseña Movie Booking + Code Review de tus diseños anteriores |
| 18 | Arjan Codes + implementación de 7 patrones GoF | Implementa los 7 patrones en código real |
| 19 | Diseño libre sin guía + Clean Architecture | **Hito Fase 3** + diseño Hotel Booking |

### Semanas 20–26 (Fase 4 — HLD)

| Semana | Contenido Principal | Sistema de Práctica |
|--------|--------------------|--------------------|
| 20 | Educative Modern SD: Requirements, Estimation, APIs | URL Shortener |
| 21 | Educative Modern SD: DB, Caching, Messaging | Twitter Timeline |
| 22 | Educative Advanced SD + SystemsExpert | YouTube/Netflix |
| 23 | Papers: Dynamo + Kafka + Raft | Rate Limiter Distribuido |
| 24 | ByteByteGo series + Papers: Spanner + Bigtable | Uber/Lyft + Dropbox |
| 25 | Mock SD Interviews cronometradas (3 mocks) | Slack/WhatsApp |
| 26 | Mock SD Interviews + revisión de gaps | **Hito Fase 4** |

---

## Biblioteca de Referencia Consolidada {#biblioteca}

### Libros — Por Prioridad

| Prioridad | Libro | Autor | Para qué fase |
|-----------|-------|-------|---------------|
| ⭐⭐⭐ OBLIGATORIO | *Designing Data-Intensive Applications* | Martin Kleppmann | Fase 4 |
| ⭐⭐⭐ OBLIGATORIO | *The Algorithm Design Manual* | Steven Skiena | Fase 1–2 |
| ⭐⭐⭐ OBLIGATORIO | *Clean Architecture* | Robert C. Martin | Fase 3 |
| ⭐⭐ ALTO VALOR | *Design Patterns: Elements of Reusable OO Software* | GoF | Fase 3 |
| ⭐⭐ ALTO VALOR | *System Design Interview Vol. 1 & 2* | Alex Xu | Fase 4 |
| ⭐⭐ ALTO VALOR | *Java Concurrency in Practice* | Brian Goetz | Fase 5 |
| ⭐⭐ ALTO VALOR | *Refactoring* | Martin Fowler | Fase 3 |
| ⭐ COMPLEMENTARIO | *Site Reliability Engineering* (Google SRE Book) | Beyer et al. | Fase 5 |
| ⭐ COMPLEMENTARIO | *Introduction to Algorithms (CLRS)* | Cormen et al. | Referencia permanente |

### Canales de YouTube — Curados

| Canal | Especialidad | Mejor para |
|-------|-------------|------------|
| **NeetCode** | DSA patrones, soluciones claras | Fases 1–2 |
| **Back to Back SWE** | DP visual, Trees, Graphs | Fase 2 |
| **William Fiset** | Graph algorithms en profundidad | Fase 2 |
| **ByteByteGo** | System Design visual, síntesis | Fase 4 |
| **Gaurav Sen** | System Design conceptual | Fase 4 |
| **Arjan Codes** | Design Patterns en código real | Fase 3 |
| **Fireship** | Conceptos técnicos en 100 segundos | Repasos rápidos |
| **Hussein Nasser** | Networking y bases de datos en profundidad | Fase 5 |

### Recursos Online Gratuitos

| Recurso | URL | Para qué |
|---------|-----|---------|
| NeetCode 150 | neetcode.io | Lista curada de problemas por patrón |
| Visualgo | visualgo.net | Visualización interactiva de algoritmos |
| Refactoring Guru | refactoring.guru | Catálogo de patrones y smells |
| Google API Design Guide | aip.dev | Diseño de APIs REST canónico |
| Google SRE Book | sre.google/sre-book | Operación de sistemas a escala |
| High Scalability Blog | highscalability.com | Casos reales de arquitecturas a escala |
| Uber Engineering Blog | eng.uber.com | Casos reales de decisiones de arquitectura |
| Netflix TechBlog | netflixtechblog.com | Sistemas de streaming a escala global |
| Cloudflare Blog | blog.cloudflare.com | Networking, CDN, seguridad |

---

## Estrategia de Behavioral Interviews {#behavioral}

> *Las rondas de behavioral son tan eliminatorias como las técnicas. Un "no hire" en behavioral puede invalidar un desempeño técnico perfecto.*

### El Framework STAR Expandido (STARL)

| Componente | Qué incluir | Duración |
|------------|-------------|----------|
| **S** — Situation | Contexto del proyecto, equipo, presión de negocio | 20–30 seg |
| **T** — Task | Tu rol específico y la responsabilidad que tenías | 15–20 seg |
| **A** — Action | Las decisiones técnicas y humanas que tomaste. Usa "yo", no "nosotros" | 60–90 seg |
| **R** — Result | Impacto medible: latencia reducida X%, usuarios aumentados Y%, costo ahorrado $Z | 20–30 seg |
| **L** — Lessons | Qué aprendiste y cómo cambió tu forma de trabajar (diferenciador para Staff) | 15–20 seg |

### 5 Preguntas que Debes Preparar con Historias Reales

1. **Liderazgo técnico:** "Cuéntame de un proyecto técnico complejo que lideraste. ¿Cómo tomaste las decisiones de arquitectura?"
2. **Conflicto técnico:** "¿Alguna vez no estuviste de acuerdo con una decisión técnica de tu equipo o manager? ¿Cómo lo manejaste?"
3. **Errores y recuperación:** "Cuéntame de un bug o incidente en producción que causaste. ¿Qué hiciste?"
4. **Influencia sin autoridad:** "¿Cómo convenciste a tu equipo de adoptar una nueva tecnología o práctica que no había sido solicitada?"
5. **Priorización bajo presión:** "Cuéntame de un momento en que tuviste múltiples prioridades críticas. ¿Cómo decidiste qué hacer primero?"

### Valores de Liderazgo por Empresa

| Empresa | Valores / Competencias Clave |
|---------|------------------------------|
| **Amazon** | 16 Leadership Principles — Customer Obsession, Bias for Action, Invent and Simplify, Dive Deep |
| **Google** | Googleyness, Leadership, Role-Related Knowledge, General Cognitive Ability |
| **Meta** | Impact, Move Fast, Be Direct, Build Awesome Things |
| **Microsoft** | Growth Mindset, Customer Empathy, One Microsoft |
| **Apple** | Accountability, Creativity, Collaboration, Deep Technical Excellence |

---

## Consejos de Oro para Entrevistas de Alto Nivel {#consejos-de-oro}

### Durante la Entrevista de Coding

**1. Piensa en voz alta siempre**  
El entrevistador evalúa tu proceso mental, no solo tu solución. Un candidato que llegó a una solución subóptima pero articuló su razonamiento supera a uno que resolvió perfecto en silencio. Narrar tu pensamiento es una habilidad — practícala explícitamente con un timer.

**2. Clarifica antes de escribir código**  
Dos minutos de preguntas correctas pueden ahorrarte 20 minutos de implementación incorrecta. Pregunta: tipo de input (¿enteros positivos? ¿strings vacíos?), restricciones de memoria, si debe ser in-place. Muestra que piensas como un ingeniero, no como un estudiante.

**3. El patrón importa más que el algoritmo**  
Di en voz alta el patrón antes de implementar: "Esto parece un Sliding Window porque buscamos un subarray continuo con una condición." Esa sola frase comunica más inteligencia que 50 líneas de código correcto en silencio.

**4. Maneja el tiempo con disciplina**  
- 5 min: entender y clarificar  
- 5 min: approach y complejidad estimada  
- 20 min: implementación  
- 5 min: testing, edge cases, optimización  

Si no terminas el código perfecto pero presentas un approach sólido con análisis de complejidad, casi siempre pasas.

**5. Siempre analiza complejidad espacial, no solo temporal**  
La mayoría de candidatos calcula O(n) en tiempo y olvida el espacio. Los entrevistadores de nivel Senior siempre preguntan ambas. Si usas recursión: el call stack es espacio O(n) o O(log n) dependiendo de la profundidad.

### Durante la Entrevista de System Design

**6. Clarifica requirements antes de dibujar**  
Nunca empieces a dibujar sin hacer al menos 4 preguntas: ¿cuántos usuarios activos diarios?, ¿es más read-heavy o write-heavy?, ¿necesitamos consistencia fuerte o eventual es aceptable?, ¿cuáles son los SLOs (latencia máxima, uptime mínimo)? Cada respuesta cambia tu arquitectura.

**7. Haz back-of-the-envelope antes del diseño detallado**  
Estima en voz alta: "100M usuarios activos diarios, 10 posts por usuario por día = 1,000 posts/seg en escritura. Con un replication factor de 3 en Kafka, necesito ~3,000 writes/seg sostenidos." Esta práctica señala madurez de ingeniería inmediatamente.

**8. Domina los trade-offs, no las respuestas**  
No existe "la arquitectura correcta" — existe "el mejor trade-off para estos constraints". Cuando eliges una tecnología, di explícitamente qué estás sacrificando: "Elegiría Cassandra sobre PostgreSQL porque priorizamos write throughput y disponibilidad, pero aceptamos que no tendremos transacciones ACID multi-row."

**9. Incluye seguridad y observabilidad sin que te lo pidan**  
Mencionar autenticación, autorización, rate limiting, logging estructurado y SLOs en tu diseño sin que el entrevistador lo solicite es una señal directa de experiencia en producción. Los candidatos Junior esperan que les pregunten. Los candidatos Staff los incluyen proactivamente.

**10. Practica en condiciones reales, no cómodas**  
Timer activo, sin referencias, hablando en voz alta, en una pizarra o papel blanco. Si practicas en silencio con acceso a Google, no estás entrenando para una entrevista. La ansiedad se entrena con exposición repetida, no con más teoría.

### Mentalidad y Proceso

**11. Lee código de producción, no solo tutoriales**  
El gap entre resolver LeetCode y ser contratado como Staff en FAANG incluye tu capacidad de diseñar código mantenible y operable. Lee el código fuente de Redis (C), Kafka (Scala/Java), etcd (Go). Ver cómo ingenieros de clase mundial nombran, estructuran y documentan transforma tu perspectiva permanentemente.

**12. El error más común: estudiar más, practicar menos**  
En el último mes antes de entrevistas, el ratio debe ser 20% aprender / 80% practicar. La mayoría de candidatos lo invierte. Conocer todos los patrones teóricamente pero no tenerlos automatizados bajo presión no te llevará a ningún lado.

**13. Un no no es el fin — es datos**  
Pide feedback siempre que puedas después de una entrevista fallida. Los rechazos de FAANG no son juicios de tu valor como ingeniero — son snapshots de desempeño en condiciones específicas. Los mejores ingenieros que conozco fallaron múltiples rondas antes de pasar. La diferencia es que iteraron con los datos que tenían.

---

## Métricas de Progreso y Autoevaluación {#metricas}

### Indicadores de Nivel por Fase

| Métrica | Nivel Básico | Nivel Objetivo | Nivel Staff |
|---------|-------------|----------------|-------------|
| Problemas Easy AlgoExpert | < 20 min | < 10 min | < 7 min |
| Problemas Medium AlgoExpert | < 45 min | < 25 min | < 15 min |
| Problemas Hard AlgoExpert | No resuelto | < 45 min | < 30 min |
| System Design (45 min) | Arquitectura básica | Trade-offs justificados | Observability + Security + Failure modes |
| LLD (40 min) | Clases básicas | Patrones GoF aplicados | Clean Arch + SOLID + extensibilidad |

### Señales de que Estás Listo para Entrevistas

**Para roles Senior (L5 equivalente):**
- [ ] Resuelves el 80% de los problemas Medium en menos de 25 minutos
- [ ] Completas un System Design básico (URL Shortener, Pastebin) con fluidez
- [ ] Implementas 5+ patrones GoF de memoria
- [ ] Tienes 3–5 historias STARL pulidas para behavioral

**Para roles Staff (L6 equivalente):**
- [ ] Resuelves el 60% de los problemas Hard en menos de 40 minutos
- [ ] Diseñas sistemas complejos (Twitter, Uber, YouTube) con trade-offs profundos
- [ ] Incluyes consistencia distribuida, observabilidad y seguridad en tus diseños espontáneamente
- [ ] Has leído y puedes discutir al menos 4 papers de sistemas distribuidos
- [ ] Puedes discutir PACELC, Saga Pattern, Event Sourcing y Consensus sin referencias

---

*Roadmap Zero to Hero — Versión 2.0 Expandida*  
*Diseñado para ingenieros que apuntan a FAANG Staff Level*  
*Todas las plataformas de pago mencionadas (AlgoExpert, Log2Base2, Educative) son suscripciones activas del usuario*
