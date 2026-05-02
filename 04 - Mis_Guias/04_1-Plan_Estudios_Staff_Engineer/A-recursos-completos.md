# Apéndice A — Recursos Completos del Curriculum

> **Documento de referencia permanente.** No es un documento de estudio — es un índice de recursos.
> Consúltalo cuando necesitas saber: ¿qué recurso uso aquí? ¿qué capítulo del libro corresponde a este archivo? ¿en qué orden consumo mis suscripciones en este módulo?
> Se abre, se consulta el dato, se cierra. No se lee linealmente.

---

## Sección 1 — Cómo usar este apéndice

Este apéndice está organizado en dos ejes: **por suscripción activa** (¿en qué módulos uso AlgoMonster?) y **por módulo** (¿qué recursos gratuitos aplican al Módulo 4?). Para preguntas del tipo "¿qué capítulos de DDIA leer para distributed systems?", ve directo a la Sección 3. Para preguntas del tipo "¿cuándo activo AlgoExpert Systems Expert?", ve a la Sección 2. Para libros con capítulos específicos, Sección 4. Para recursos de entrevistas, Sección 5.

La regla general: los recursos de suscripción tienen un **orden de consumo diseñado** — no son intercambiables ni opcionales en su secuencia. Los recursos gratuitos son complementarios, no sustitutos.

---

## Sección 2 — Suscripciones activas: mapa de uso por módulo

### 🎯 AlgoMonster

**Rol en el curriculum:** Andamiaje inicial de patrones DSA. Primera exposición estructurada a cada patrón con explicación del mecanismo y problemas guiados. No es práctica de volumen — es construcción del modelo mental.

| Módulo | Path / Sección | Orden de consumo | Propósito |
|---|---|---|---|
| Módulo 1 | Data Structures path completo | Semanas 1-4 de M1, en paralelo con `01-01` | Fundamentos de estructuras de datos con implementación |
| Módulo 2 | Two Pointers | Primer patrón lineal — con `02-01` | Modelo mental del patrón con problemas guiados |
| Módulo 2 | Sliding Window | Después de Two Pointers, mismo archivo | Extensión natural del patrón anterior |
| Módulo 2 | BFS / DFS | Al entrar a `02-02` patrones no-lineales | Base para árboles y grafos |
| Módulo 2 | Binary Search | Con `02-01`, sección binary search | Patrón transversal |
| Módulo 2 | Dynamic Programming | Con `02-03` completo | Identificación de subproblemas |
| Módulo 2 | Graphs (Topological Sort, Union-Find) | Con `02-04` | Patrones avanzados de grafos |
| Módulo 7 | Repetición espaciada — patrones débiles | Semanas 4-6 previas a entrevista | Consolidación de patrones con baja tasa de éxito |

**Instrucción de uso:** Abre AlgoMonster en el patrón correspondiente ANTES de leer el archivo del curriculum. AlgoMonster da el andamiaje — el archivo del curriculum da la profundidad y el contexto de por qué ese patrón importa.

---

### 🎯 AlgoExpert

**Rol en el curriculum:** Práctica complementaria de volumen en DSA + pilar de system design (Systems Expert). No es el primer contacto con ningún patrón — siempre es segunda pasada después de AlgoMonster o primer contacto en system design.

| Módulo | Sección | Cuándo | Propósito |
|---|---|---|---|
| Módulo 2 | Algorithms section — por patrón | Después de AlgoMonster en cada patrón | Práctica adicional con variantes |
| Módulo 2 | Trees and Graphs | Después de `02-02` patrones no-lineales | Volumen en estructuras no-lineales |
| Módulo 2 | Dynamic Programming | Después de `02-03` + NeetCode 150 DP | Práctica adicional de DP difícil |
| Módulo 4 | Systems Expert — todos los videos en orden | Al iniciar `04-00-overview.md` | Fundamentos visuales de system design antes de los archivos de texto |
| Módulo 4 | Systems Expert — Caching, Databases, Queues | En paralelo con `04-03`, `04-02`, `04-04` | Video complementario al texto |
| Módulo 7 | Systems Expert mock interviews | Semanas 3-4 previas a entrevista | Simulación de entrevistas de system design |
| Módulo 7 | Coding — patrones débiles identificados | Semanas 4-6 previas a entrevista | Práctica focalizada en gaps |

**Instrucción de uso para Systems Expert:** Los videos de Systems Expert funcionan mejor antes de leer los archivos de texto del curriculum — dan un mapa visual que hace que el texto sea mucho más fácil de anclar. Para Módulo 4, abre Systems Expert primero, después lee el archivo correspondiente.

---

### 🎯 Pluralsight

**Rol en el curriculum:** Stack específico .NET/Azure + arquitectura aplicada. Siempre complementario al texto del curriculum — nunca sustituto.

| Módulo | Path / Curso | Archivo del curriculum | Notas |
|---|---|---|---|
| Módulo 1 | .NET Memory Management | `01-03-memoria-y-gestion.md` | Ver después de leer el archivo, no antes |
| Módulo 1 | Concurrency in C# | `01-04-os-y-concurrencia-base.md` | Implementación .NET de conceptos del archivo |
| Módulo 3 | SOLID Principles in C# | `03-02-solid.md` | Ejemplos C# concretos de cada principio |
| Módulo 3 | Design Patterns in C# | `03-03-patrones-gof.md` | Implementación .NET de GoF |
| Módulo 3 | Clean Architecture with ASP.NET Core | `03-04-clean-architecture.md` | Implementación práctica de la arquitectura |
| Módulo 3 | Domain-Driven Design Fundamentals | `03-05-ddd.md` | Complemento visual de DDD |
| Módulo 3 | CQRS and Event Sourcing | `03-06-cqrs-event-sourcing.md` | Implementación con MediatR en .NET |
| Módulo 3 | Testing .NET Applications | `03-08-testing-strategy.md` | Testing en el stack concreto |
| Módulo 5 | ASP.NET Core Internals | `05-01-dotnet-avanzado.md` | Core del módulo 5 |
| Módulo 5 | Azure Architecture | `05-02-azure-primario.md` | Azure desde perspectiva de arquitecto |
| Módulo 5 | Azure DevOps | `04-09-deployment-y-cicd.md` + `05-02` | CI/CD con Azure DevOps |
| Módulo 5 | React Fundamentals | `05-05-react-frontend-arquitectura.md` | Base de React |
| Módulo 5 | TypeScript Fundamentals | `05-04-typescript-suficiente.md` | Type system y generics |
| Módulo 7 | Repaso paths relevantes al rol objetivo | Pre-entrevista | Revisar gaps identificados en diagnóstico |

**Instrucción de uso:** Pluralsight es implementación, no fundamento. Lee el archivo del curriculum primero, entiende el "qué" y el "por qué", luego usa Pluralsight para ver el "cómo" en código .NET real.

---

### 🎯 Codecademy

**Rol en el curriculum:** Python y TypeScript desde cero hasta suficiente. Módulo 5 específicamente.

| Módulo | Path / Curso | Módulos específicos a completar | Archivo del curriculum |
|---|---|---|---|
| Módulo 5 | Learn Python 3 | Functions, Loops, Dictionaries, List Comprehensions, File I/O, Classes | `05-03-python-suficiente.md` |
| Módulo 5 | Intermediate Python | Async, Context Managers | `05-03-python-suficiente.md` |
| Módulo 5 | Learn TypeScript | Type System, Generics, Utility Types | `05-04-typescript-suficiente.md` |

**Módulos de Codecademy que NO necesitas para este curriculum:** Testing en Python (cubierto en Módulo 3), proyectos de machine learning, cursos de SQL (cubiertos en Módulo 1 y 4 con recursos más apropiados).

---

## Sección 3 — Recursos externos gratuitos por módulo

### Módulo 1 — CS Fundamentals

| Recurso | Tipo | Cubre | Cuándo usarlo |
|---|---|---|---|
| 🆓 MIT OCW 6.006 — Lectures 1-6 | Video lecture | Complejidad algorítmica formal, análisis con recurrencias, Master Theorem | Con `01-02-complejidad-algoritmica.md` — da el rigor matemático que el archivo introduce conceptualmente |
| 🆓 Google SRE Book — Cap. 1-4 (online) | Libro gratuito online | Fundamentos de reliability, SLOs, error budgets | Prerequisito para `04-06-observability-reliability.md` — leer al final de M1 |
| 🆓 ByteByteGo YouTube — Networking series | Video | TCP/IP internals, HTTP/1-2-3, DNS resolution, CDN | Con `01-05-redes-y-protocolos.md` — visualización de los protocolos descritos |
| 🆓 Visualgo.net | Herramienta interactiva | Visualización de estructuras de datos y algoritmos en tiempo real | Con `01-01-estructuras-de-datos.md` y durante Módulo 2 para depuración mental |

**Link Google SRE Book:** https://sre.google/sre-book/table-of-contents/

---

### Módulo 2 — Algoritmos y Patrones

| Recurso | Tipo | Cubre | Cuándo usarlo |
|---|---|---|---|
| 🆓 NeetCode.io — NeetCode 150 | Lista de problemas + soluciones video | 150 problemas esenciales organizados por patrón | Después de completar cada patrón en AlgoMonster — consolidación con volumen |
| 🆓 NeetCode YouTube channel | Video | Explicación visual de cada patrón y solución de cada problema NeetCode 150 | Refuerzo visual después de leer el patrón — ver ANTES de intentar el problema |
| 🆓 LeetCode (free tier) | Plataforma de problemas | Simulación de entrevista, timing real, evaluación de solución | **Solo en las 2-3 semanas previas a entrevista específica** — no como recurso de aprendizaje |
| 🆓 CP-Algorithms.com | Referencia técnica | Algoritmos de grafos, strings, matemáticas discretas | Con `02-04-grafos-avanzados.md` y `02-05-temas-complementarios.md` para detalles de implementación |

**Instrucción crítica sobre LeetCode:** LeetCode free es inteligencia de empresa pre-entrevista, no plataforma de aprendizaje. Buscar "Company: [empresa objetivo]" en las últimas 2 semanas antes del proceso. Antes de ese punto, NeetCode 150 tiene mejor ROI.

---

### Módulo 3 — Software Design

| Recurso | Tipo | Cubre | Cuándo usarlo |
|---|---|---|---|
| 🆓 Educative — Grokking Design Patterns | Curso online (free tier limitado) | Referencia visual de patrones GoF con ejemplos interactivos | Complemento de `03-03-patrones-gof.md` para patrones que necesitan visualización |
| 🆓 Repositorios ADR públicos en GitHub | Repositorios | Ejemplos reales de ADRs escritos en producción | Con `03-09-refactoring-y-adr.md` — ver cómo se escriben en la realidad |
| 🆓 Martin Fowler — bliki (martinfowler.com) | Blog técnico | Refactoring, patrones de arquitectura, DDD, CQRS, microservicios | Lectura continua durante M3 — cada artículo va con el archivo correspondiente |
| 🆓 Refactoring.guru | Referencia visual | Catálogo visual de patrones GoF y refactoring | Con `03-03-patrones-gof.md` — referencia rápida durante práctica |

**ADR públicos recomendados:** Buscar en GitHub "awesome-architecture-decision-records" para una lista curada de repos con ADRs reales de empresas tech.

---

### Módulo 4 — System Design

| Recurso | Tipo | Cubre | Cuándo usarlo |
|---|---|---|---|
| 🆓 ByteByteGo newsletter (gratuita) | Newsletter semanal | System design semanal: un componente o caso por semana | Suscribir al inicio de M4 y leer continuamente — https://bytebytego.com |
| 🆓 ByteByteGo YouTube | Video | Visualizaciones de componentes, casos clásicos de system design | Con cada archivo del módulo — ver el video del componente ANTES de leer el archivo |
| 📚 DDIA (Designing Data-Intensive Applications) | Libro | Distributed systems profundo — la referencia canónica | Ver tabla detallada de capítulos en Sección 4 |
| 🆓 Google SRE Book — online | Libro gratuito | SLOs, error budgets, reliability, incident management | Con `04-06-observability-reliability.md` — capítulos 2-4 y 13-14 |
| 🆓 OWASP Top 10 (owasp.org) | Referencia de seguridad | Los 10 riesgos más críticos en aplicaciones web con ejemplos | Con `04-07-security-en-system-design.md` — es la referencia base del archivo |
| 🆓 Raft paper — "In Search of an Understandable Consensus Algorithm" | Paper académico | Consensus algorithm, leader election, log replication | Después de `04-05-distributed-systems.md` — la implementación real de Paxos simplificado |
| 🆓 AWS Architecture Center (docs.aws.amazon.com/wellarchitected) | Documentación técnica | Well-Architected Framework — los 6 pilares de arquitectura cloud | Con `04-09-deployment-y-cicd.md` y `05-02-azure-primario.md` — aplica a Azure también |
| 🆓 High Scalability blog (highscalability.com) | Blog | Arquitecturas reales de sistemas a escala (Twitter, YouTube, Uber, etc.) | Lecturas complementarias durante `04-08-casos-clasicos.md` |

**Raft paper:** https://raft.github.io/raft.pdf — es un paper intencionalmente legible, sin background académico previo requerido.

---

### Módulo 5 — Stack Específico

| Recurso | Tipo | Cubre | Cuándo usarlo |
|---|---|---|---|
| 🆓 Microsoft .NET Blog (devblogs.microsoft.com/dotnet) | Blog oficial | Novedades del runtime, performance, nuevas features | Lectura periódica durante `05-01-dotnet-avanzado.md` |
| 🆓 Microsoft Azure Architecture Center (learn.microsoft.com/azure/architecture) | Documentación | Reference architectures, patterns en Azure, ejemplos reales | Con `05-02-azure-primario.md` y `04-09-deployment-y-cicd.md` |
| 🆓 BenchmarkDotNet — GitHub + docs | Herramienta | Benchmarking de performance en .NET | Con `05-01-dotnet-avanzado.md` — para validar claims de performance |
| 🆓 Stephen Cleary — blog (blog.stephencleary.com) | Blog | async/await en .NET — la referencia más profunda disponible | Con `05-01-dotnet-avanzado.md`, sección async/await |

---

### Módulo 6 — IA Integrada

| Recurso | Tipo | Cubre | Cuándo usarlo |
|---|---|---|---|
| 🆓 DeepLearning.AI — "Building Systems with the ChatGPT API" | Curso corto (gratuito) | RAG fundamentals, pipelines de integración LLM | Con `06-02-llm-system-design.md` — curso de 2-3 horas que complementa el archivo |
| 🆓 DeepLearning.AI — "Multi AI Agent Systems with crewAI" | Curso corto (gratuito) | Agentes, orquestación, patrones multi-agente | Con `06-03-agentes-y-orquestacion.md` |
| 🆓 Anthropic Documentation — Prompt Engineering | Documentación técnica | Context engineering, prompting, structured outputs | Con `06-04-context-engineering.md` — la fuente primaria del archivo |
| 🆓 OpenAI Cookbook (github.com/openai/openai-cookbook) | Repositorio de ejemplos | Patrones de integración con LLMs, function calling, RAG | Con `06-02-llm-system-design.md` y `06-03-agentes-y-orquestacion.md` |
| 🆓 W&B Academy — LLMOps (wandb.ai/courses) | Curso gratuito | Evaluación de outputs LLM, observabilidad, ciclo de vida de modelos | Con `06-05-evaluacion-seguridad-llm.md` |
| 🆓 LangSmith documentation (smith.langchain.com) | Documentación | Tracing y observabilidad de aplicaciones LLM | Con `06-05-evaluacion-seguridad-llm.md` — herramienta de observabilidad LLM |

**DeepLearning.AI:** Todos los cursos cortos son gratuitos en deeplearning.ai — requieren registro pero no suscripción de pago.

---

### Módulo 7 — Entrevistas

| Recurso | Tipo | Cubre | Cuándo usarlo |
|---|---|---|---|
| 🆓 NeetCode 150 (neetcode.io) | Lista de problemas | Práctica estructurada de DSA con soluciones video | Semanas 3-6 previas a entrevista — práctica activa cronometrada |
| 🆓 Levels.fyi | Benchmarking de compensación | Salarios por empresa, nivel y ubicación — datos actuales | Cuando hay oferta o proceso avanzado — referencia para `07-06-negociacion-post-oferta.md` |
| 🆓 Glassdoor — sección interviews | Reportes de candidatos | Proceso específico de entrevistas por empresa, preguntas reales | 1-2 semanas antes del loop de entrevistas con esa empresa |
| 🆓 Blind (teamblind.com) | Foro anónimo de ingenieros | Información granular sobre empresas específicas, comp, cultura | En paralelo con Glassdoor — datos más detallados pero menos estructurados |
| 🆓 Pramp (pramp.com) | Mock interviews gratuitas | Práctica de coding y system design con otra persona real en tiempo real | Semanas 4-6 previas a entrevista — dos sesiones por semana mínimo |
| 🆓 Interviewing.io | Mock interviews | Mock interviews anónimas con ingenieros de empresas reales | Opción premium para simulación más realista — recomendado si el presupuesto lo permite |

---

## Sección 4 — Libros esenciales con capítulos específicos

Solo libros con ROI real para el objetivo de Staff Engineer. Para cada uno: qué leer, qué saltar, y cuándo.

### 📚 Designing Data-Intensive Applications (DDIA) — Kleppmann

**El libro más importante del curriculum.** Base del Módulo 4 y referencia permanente para distributed systems.

| Capítulo(s) | Contenido | Archivo del curriculum | Leer / Saltar |
|---|---|---|---|
| Cap. 1 — Reliable, Scalable, Maintainable | Fundamentos conceptuales | `04-01-componentes-fundamentales.md` | ✅ Leer |
| Cap. 2 — Data Models and Query Languages | Relacional vs NoSQL vs Graph | `01-06-bases-de-datos-fundamentos-cs.md` | ✅ Leer |
| Cap. 3 — Storage and Retrieval | B-Trees, LSM-Trees, SSTables | `01-06` + `04-02-bases-de-datos-system-design.md` | ✅ Leer — es el capítulo más técnico y más importante |
| Cap. 4 — Encoding and Evolution | Protobuf, Avro, JSON | — | ⏭ Saltar — nivel de detalle excesivo para el objetivo actual |
| Cap. 5 — Replication | Leader-follower, multi-leader, leaderless | `04-02-bases-de-datos-system-design.md` | ✅ Leer |
| Cap. 6 — Partitioning | Sharding strategies, consistent hashing | `04-02-bases-de-datos-system-design.md` | ✅ Leer |
| Cap. 7 — Transactions | ACID, isolation levels, distributed txns | `01-06` + `04-02` | ✅ Leer — crítico para entrevistas |
| Cap. 8 — Trouble with Distributed Systems | Faults, clocks, network issues | `04-05-distributed-systems.md` | ✅ Leer |
| Cap. 9 — Consistency and Consensus | Linearizability, Raft, CAP | `04-05-distributed-systems.md` | ✅ Leer — base del razonamiento Staff |
| Cap. 10-12 — Batch / Stream processing | MapReduce, Kafka Streams | — | ⏭ Opcional — solo si el rol objetivo involucra data pipelines |

**Cuándo leer DDIA:** Durante el Módulo 4. Cap. 1-3 pueden leerse antes (al terminar Módulo 1). No intentar leer DDIA antes de tener los fundamentos de CS del Módulo 1.

---

### 📚 Clean Architecture — Robert C. Martin

| Parte | Contenido | Leer / Saltar |
|---|---|---|
| Part I — What is Design and Architecture | Filosofía y definiciones | ⏭ Saltar — demasiado abstracto, poca densidad técnica |
| Part II — Starting with the Bricks: Programming Paradigms | Paradigmas de programación | ⏭ Saltar — base conocida |
| Part III — Design Principles | SOLID aplicado a Clean Architecture | ✅ Leer — conecta SOLID con la arquitectura |
| Part IV — Component Principles | Cohesion, coupling, component design | ✅ Leer — base para decisiones de modularidad |
| Part V — Architecture | Clean Architecture layers, boundaries | ✅ Leer — el núcleo del libro |
| Part VI — Details | Frameworks, databases as details | ✅ Leer — cambia la perspectiva sobre frameworks |

**Cuándo:** Durante `03-04-clean-architecture.md`. Leer después del archivo, no antes.

---

### 📚 Software Architecture: The Hard Parts — Richards & Ford

| Capítulo(s) | Leer / Saltar |
|---|---|
| Cap. 1-6 — Pulling things apart, component-based decomposition | ✅ Leer — trade-offs de descomposición |
| Cap. 9-10 — Reuse patterns, data ownership | ✅ Leer — decisiones frecuentes en microservicios |
| Cap. 11-12 — Distributed workflows, sagas | ✅ Leer — conecta con `03-06-cqrs-event-sourcing.md` |
| Cap. 13-18 — Transactional sagas variants (detalle exhaustivo) | ⏭ Saltar para primer pass — volver si necesitas implementar |

**Cuándo:** Después de completar el Módulo 3. Este libro es el puente entre diseño de software y system design.

---

### 📚 Building Microservices — Sam Newman (2nd ed.)

| Capítulo(s) | Leer / Saltar |
|---|---|
| Cap. 1-3 — What are microservices, migration, splitting | ✅ Leer — el "por qué" y "cuándo" de microservicios |
| Cap. 4-8 — Communication, workflow, build, deployment | ⏭ Saltar si no hay microservicios en el rol objetivo inmediato |
| Cap. 9-11 — Testing, monitoring, security en microservicios | ✅ Leer — conecta con Módulos 3 y 4 |

**Cuándo:** Módulo 4 avanzado, específicamente con `04-04-message-queues.md` y `04-05-distributed-systems.md`.

---

### 📚 LLM Engineering Handbook — Iusztin & Labonne

| Capítulo(s) | Leer / Saltar |
|---|---|
| Cap. 1-2 — LLM fundamentals, prompt engineering | ✅ Leer — fundamentos del Módulo 6 |
| Cap. 3-4 — RAG pipelines, vector databases | ✅ Leer — base para `06-02-llm-system-design.md` |
| Cap. 5 — Fine-tuning fundamentals | ✅ Leer — necesario para decidir RAG vs Fine-tuning |
| Cap. 6-7 — MLOps, training infrastructure | ⏭ Saltar — territorio de ML Engineer, no de Staff Software Engineer |
| Cap. 8-10 — Inference, deployment, evaluation | ✅ Leer — producción de sistemas LLM |

**Cuándo:** Módulo 6, en orden con los archivos del módulo.

---

### 📚 AI Engineering — Chip Huyen

| Capítulo(s) | Leer / Saltar |
|---|---|
| Cap. 1-2 — Introduction, understanding LLMs | ✅ Leer — framing correcto del rol del Software Engineer en IA |
| Cap. 3-4 — Evaluation, RAG | ✅ Leer — `06-05` y `06-02` respectivamente |
| Cap. 5-6 — Fine-tuning, agents | ⏭ Cap. 5 saltar (training — ML engineer territory) / Cap. 6 leer |
| Cap. 7 — Inference and deployment | ✅ Leer — producción real de sistemas IA |

**Cuándo:** Módulo 6, en paralelo con LLM Engineering Handbook.

---

## Sección 5 — Recursos específicos para entrevistas

### Preparación activa (activar en el período pre-entrevista)

| Recurso | Tipo | Para qué | Cuándo activar |
|---|---|---|---|
| 🆓 NeetCode 150 (neetcode.io) | Lista estructurada | Práctica cronometrada de DSA — los 150 problemas esenciales organizados por patrón | Semanas 3-6 previas a la primera entrevista técnica |
| 🆓 Pramp (pramp.com) | Mock interviews con personas reales | Práctica de coding y system design con feedback real en tiempo real | Semanas 4-6 previas — al menos 2 sesiones por semana |
| 🆓 LeetCode free | Plataforma | Problemas filtrados por empresa — inteligencia táctica de entrevista | Semanas 1-2 antes del loop específico con esa empresa |
| 🎯 AlgoExpert Systems Expert — mock interviews | Plataforma | Simulación de entrevista de system design con criterios de evaluación | Semanas 3-5 previas |

### Investigación de empresa y compensación

| Recurso | Para qué | Cuándo |
|---|---|---|
| 🆓 Levels.fyi | Benchmarking de comp total — base, equity, bonus por empresa y nivel | Cuando hay oferta en mano o proceso en ronda final — ver `07-06-negociacion-post-oferta.md` |
| 🆓 Glassdoor — sección Interviews | Reportes de candidatos sobre preguntas reales y proceso | 1-2 semanas antes del loop de entrevistas |
| 🆓 Blind (teamblind.com) | Información más granular que Glassdoor, foro anónimo de ingenieros | En paralelo con Glassdoor para la misma empresa |
| 🆓 LinkedIn — Job posts de la empresa | Entender qué tecnologías y niveles buscan activamente | Al iniciar investigación de empresa — ver `07-05-estrategia-por-empresa.md` |

### Papers que un Staff debe poder discutir

Estos papers no requieren lectura profunda — requieren entender el problema que resuelven, la solución propuesta, y los trade-offs. Nivel de detalle: poder discutirlos en conversación técnica, no implementarlos.

| Paper | Problema que resuelve | Cuándo leer |
|---|---|---|
| Raft — "In Search of an Understandable Consensus Algorithm" | Consensus en sistemas distribuidos sin la complejidad de Paxos | Con `04-05-distributed-systems.md` |
| Google MapReduce (2004) | Procesamiento distribuido a escala — el paradigma que originó el big data moderno | Con `04-05-distributed-systems.md` (contexto histórico) |
| Amazon Dynamo (2007) | Eventual consistency, consistent hashing, quorum reads/writes | Con `04-02-bases-de-datos-system-design.md` |
| Google Spanner (2012) | Globally-distributed SQL con external consistency | Con `04-02` — para entender el estado del arte en BDs distribuidas |
| Attention Is All You Need (2017) | Transformers — la arquitectura base de todos los LLMs modernos | Con `06-02-llm-system-design.md` — solo la intuición, no las matemáticas |

---

> **Nota de mantenimiento:** Este apéndice refleja el estado del curriculum en su versión final (v2). Si algún recurso cambia de disponibilidad o precio, abrir una sesión en el proyecto Claude de zero-to-hero-v2 para actualizar la entrada correspondiente.

> **Siguiente referencia:** [B-tracking-autoevaluacion.md](./B-tracking-autoevaluacion.md) para seguimiento de progreso.
