# Apéndice B — Tracking y Autoevaluación

> **Documento activo.** A diferencia del resto del curriculum, este archivo se modifica continuamente.
> Es el único archivo del sistema que Omar edita — todos los demás son de solo lectura.
> Marca los checkboxes en Obsidian conforme completas cada criterio. No avances sin verificar los gates.

---

## Sección 1 — Cómo usar este sistema de tracking

**Regla de los checkboxes:** Marca un checkbox solo cuando puedes ejecutar el criterio sin ayuda — no cuando crees que podrías hacerlo con algo de ayuda. La diferencia importa cuando estás bajo presión en una entrevista.

**Regla de los gates:** Las señales de "listo para avanzar" al final de cada módulo son gates reales, no sugerencias. Si no alcanzas el 80% de los criterios de un módulo, no pases al siguiente. El curriculum está diseñado para que cada módulo sea requisito real del siguiente.

**Cadencia de revisión:** Revisa este archivo completo cada 2 semanas. Actualiza fechas, agrega notas, recalibra expectativas de timeline. Esta revisión tarda 20 minutos y evita que sigas una trayectoria incorrecta durante semanas.

**Regla del 80/20:** Cuando el 80% de un módulo está marcado como completado, puedes empezar el siguiente — pero debes completar el 20% restante antes de terminar el nuevo módulo. No dejes gaps indefinidamente abiertos.

---

## Sección 2 — Tracking por módulo

---

### 📁 Módulo 0 — Diagnóstico y Orientación

**Estado actual:**
- [ ] No iniciado
- [ ] En progreso
- [x] Completado *(marcar cuando aplique)*

**Criterios de completado:**

- [ ] Completé la autoevaluación en `00-diagnostico.md` con honestidad técnica real
- [ ] Respondí las preguntas de calibración de la Sección 4 antes de asignarme niveles
- [ ] Registré mis niveles en el diagnóstico con fecha
- [ ] Identifiqué mis top-3 gaps críticos (áreas con nivel ≤ 2)
- [ ] Definí mi punto de entrada según la tabla de la Sección 5 del diagnóstico
- [ ] Tengo claridad sobre el timeline estimado para mis gaps específicos

**Gaps críticos identificados:**
```
1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
```

**Fecha de completado:** ___________

---

### 📦 Módulo 1 — CS Fundamentals

**Estado actual:**
- [ ] No iniciado
- [ ] En progreso
- [ ] Completado

**Archivos completados:**

- [ ] `01-00-overview.md`
- [ ] `01-01-estructuras-de-datos.md`
- [ ] `01-02-complejidad-algoritmica.md`
- [ ] `01-03-memoria-y-gestion.md`
- [ ] `01-04-os-y-concurrencia-base.md`
- [ ] `01-05-redes-y-protocolos.md`
- [ ] `01-06-bases-de-datos-fundamentos-cs.md`

**Señales de que el módulo está listo para avanzar:**

*Estructuras de datos:*
- [ ] Puedo implementar una HashTable con separate chaining desde cero en C# sin referencia
- [ ] Puedo explicar por qué un HashMap tiene O(1) amortizado en get/put, y qué lo degrada a O(n)
- [ ] Puedo describir cuándo usar un Heap en lugar de una lista ordenada, con la diferencia de costo de inserción

*Complejidad algorítmica:*
- [ ] Puedo calcular la complejidad de un algoritmo con loops anidados de tamaño variable sin ayuda
- [ ] Puedo explicar qué significa "amortizado O(1)" con el ejemplo de `List<T>.Add()` en C#
- [ ] Puedo diferenciar complejidad de tiempo vs espacio y articular el trade-off entre ambas

*Memoria y GC:*
- [ ] Puedo explicar qué ocurre en el GC de .NET cuando un objeto sobrevive a Gen0 y pasa a Gen1
- [ ] Puedo describir el Large Object Heap y por qué es una fuente de memory pressure en producción
- [ ] Puedo explicar la diferencia entre stack y heap con un ejemplo de código C# concreto

*OS y concurrencia:*
- [ ] Puedo dibujar el TCP three-way handshake de memoria con los flags correctos
- [ ] Puedo explicar qué es un context switch y por qué crear miles de threads es contraproducente
- [ ] Puedo describir la diferencia entre paralelismo e I/O concurrente con ejemplos de cuándo usar cada uno

*Redes:*
- [ ] Puedo explicar la diferencia entre HTTP/1.1, HTTP/2 y HTTP/3 con el problema que resuelve cada versión
- [ ] Puedo describir qué ocurre técnicamente desde que el usuario escribe una URL hasta que recibe la respuesta

*Bases de datos fundamentos:*
- [ ] Puedo explicar por qué un B+Tree es preferible a un B-Tree para índices de BD en disco
- [ ] Puedo definir ACID con la garantía específica de cada letra y el mecanismo interno que la implementa
- [ ] Puedo dar dos razones técnicas concretas para usar Cassandra en lugar de PostgreSQL (no solo "escala mejor")

**Duración real:** _________ semanas  
**Fecha de completado:** ___________  
**Notas:** _________________________________

---

### 📦 Módulo 2 — Algoritmos y Patrones

**Estado actual:**
- [ ] No iniciado
- [ ] En progreso
- [ ] Completado

**Archivos completados:**

- [ ] `02-00-overview.md`
- [ ] `02-01-patrones-lineales.md`
- [ ] `02-02-patrones-no-lineales.md`
- [ ] `02-03-dynamic-programming.md`
- [ ] `02-04-grafos-avanzados.md`
- [ ] `02-05-temas-complementarios.md`
- [ ] `02-06-csharp-especifico-dsa.md`
- [ ] `02-07-simulacion-entrevistas-coding.md`

**Señales de que el módulo está listo para avanzar:**

*Velocidad y proceso:*
- [ ] Resuelvo LC Easy en < 10 minutos con el framework CTICA completo (Clarify, Think, Code, Test, Analyze)
- [ ] Resuelvo LC Medium en < 25 minutos con framework CTICA
- [ ] Reconozco el patrón correcto (Two Pointers, Sliding Window, BFS, etc.) en un enunciado nuevo en < 2 minutos
- [ ] Verbalizo mi proceso de pensamiento de forma coherente mientras codifico — el entrevistador nunca está en silencio más de 30 segundos

*Patrones lineales (`02-01`):*
- [ ] Puedo resolver un problema de Two Pointers que no he visto antes sin consultar referencia
- [ ] Puedo resolver un problema de Sliding Window con constraint dinámico sin referencia
- [ ] Puedo hacer Binary Search en un array rotado sin referencia

*Patrones no-lineales (`02-02`):*
- [ ] Puedo implementar BFS y DFS en un grafo desde cero sin referencia
- [ ] Puedo resolver un problema de árboles binarios con recursión y con BFS iterativo
- [ ] Sé cuándo usar BFS (camino más corto, niveles) vs DFS (backtracking, path existence) y por qué

*Dynamic Programming (`02-03`):*
- [ ] Puedo identificar si un problema tiene optimal substructure y overlapping subproblems
- [ ] Puedo resolver problemas de DP 1D (Fibonacci, coin change, climbing stairs) con memoization y tabulation
- [ ] Puedo resolver al menos un problema de DP 2D (longest common subsequence, edit distance)

*Grafos avanzados (`02-04`):*
- [ ] Puedo implementar Topological Sort (Kahn's algorithm o DFS-based) sin referencia
- [ ] Entiendo cuándo usar Union-Find y puedo describir su implementación con path compression
- [ ] Puedo describir Dijkstra y sus precondiciones (sin pesos negativos) sin ayuda

*Edge cases y calidad de código:*
- [ ] Detecto edge cases proactivamente — el entrevistador no necesita señalármelos
- [ ] Mi código en entrevista es limpio, con nombres de variables significativos, sin necesidad de revisión

**Duración real:** _________ semanas  
**Fecha de completado:** ___________  
**Notas:** _________________________________

---

### 📦 Módulo 3 — Software Design

**Estado actual:**
- [ ] No iniciado
- [ ] En progreso
- [ ] Completado

**Archivos completados:**

- [ ] `03-00-overview.md`
- [ ] `03-01-principios-base.md`
- [ ] `03-02-solid.md`
- [ ] `03-03-patrones-gof.md`
- [ ] `03-04-clean-architecture.md`
- [ ] `03-05-ddd.md`
- [ ] `03-06-cqrs-event-sourcing.md`
- [ ] `03-07-api-design.md`
- [ ] `03-08-testing-strategy.md`
- [ ] `03-09-refactoring-y-adr.md`

**Señales de que el módulo está listo para avanzar:**

*SOLID (`03-02`):*
- [ ] Puedo explicar DIP (Dependency Inversion Principle) con un ejemplo de código C# sin mencionar "interfaz" en la primera oración
- [ ] Puedo identificar qué principio SOLID viola un código concreto que no he visto antes y articular el impacto real (no teórico)
- [ ] Puedo describir cuándo NO usar el patrón Repository con un argumento técnico sólido

*Patrones GoF (`03-03`):*
- [ ] Puedo identificar qué patrón de diseño resuelve un problema nuevo dado — no memorizar los 23, sino reconocer la familia
- [ ] Puedo implementar los patrones más usados en producción (.NET): Factory, Repository, Strategy, Decorator, Observer

*Clean Architecture (`03-04`):*
- [ ] Puedo explicar el problema que resuelve Clean Architecture y el costo real de adoptarla en un proyecto pequeño
- [ ] Puedo describir cuándo Clean Architecture es over-engineering y proponer una alternativa más simple

*DDD (`03-05`):*
- [ ] Puedo diferenciar un Aggregate Root de una entidad normal y explicar qué consecuencia tiene para el diseño de la BD
- [ ] Puedo explicar la diferencia entre Domain Event e Integration Event y por qué la distinción importa
- [ ] Puedo describir un Bounded Context con un ejemplo de un dominio que no sea e-commerce (el ejemplo de libro)

*CQRS + Event Sourcing (`03-06`):*
- [ ] Puedo articular cuándo CQRS es una solución correcta y cuándo es over-engineering — con un proyecto concreto de cada tipo
- [ ] Puedo describir Event Sourcing: qué problema resuelve, qué ganas, qué complejidad introduces

*Testing (`03-08`):*
- [ ] Puedo describir la pirámide de testing y articular por qué la proporción importa en un proyecto real
- [ ] Puedo explicar qué es un test de integración vs un test unitario con la distinción que realmente importa (no solo "usa la BD" vs "no usa la BD")

**Duración real:** _________ semanas  
**Fecha de completado:** ___________  
**Notas:** _________________________________

---

### 📦 Módulo 4 — System Design

**Estado actual:**
- [ ] No iniciado
- [ ] En progreso
- [ ] Completado

**Archivos completados:**

- [ ] `04-00-overview.md`
- [ ] `04-01-componentes-fundamentales.md`
- [ ] `04-02-bases-de-datos-system-design.md`
- [ ] `04-03-caching-en-profundidad.md`
- [ ] `04-04-message-queues.md`
- [ ] `04-05-distributed-systems.md`
- [ ] `04-06-observability-reliability.md`
- [ ] `04-07-security-en-system-design.md`
- [ ] `04-08-casos-clasicos.md`
- [ ] `04-09-deployment-y-cicd.md`

**Señales de que el módulo está listo para avanzar:**

*Componentes y bases de datos:*
- [ ] Puedo explicar el CAP Theorem con un escenario concreto de partición de red, no solo definir los tres términos
- [ ] Puedo describir cuándo usar SQL vs NoSQL con dos razones técnicas concretas (no solo "NoSQL escala mejor")
- [ ] Puedo explicar consistent hashing y por qué minimiza el rehashing al agregar/remover nodos

*Caching y mensajería:*
- [ ] Puedo describir las estrategias de cache (cache-aside, write-through, write-behind) y cuándo usar cada una
- [ ] Puedo articular la diferencia entre Azure Service Bus y Azure Event Hub con razones técnicas específicas
- [ ] Puedo explicar el problema del thundering herd y cómo mitigarlo

*Distributed systems:*
- [ ] Puedo explicar Raft leader election en 2 minutos sin notas
- [ ] Puedo describir la diferencia entre consistencia eventual y linearizability con un ejemplo de impacto real en el usuario
- [ ] Puedo articular las diferencias entre Sagas y 2PC con sus trade-offs de resiliencia

*Observabilidad y reliability:*
- [ ] Puedo definir SLI, SLO y SLA con la distinción correcta entre los tres (no solo definirlos por separado)
- [ ] Puedo describir el patrón Circuit Breaker con sus tres estados y cuándo transicionar entre ellos

*System design en entrevista:*
- [ ] Puedo diseñar un sistema como "Notification System" o "URL Shortener" en 45 minutos con trade-offs articulados
- [ ] Clarifiqué requirements ANTES de diseñar — no empecé a dibujar componentes sin entender las constraints
- [ ] Llegué al Deep Dive en al menos 3 de los 5 casos de práctica del módulo

**Duración real:** _________ semanas  
**Fecha de completado:** ___________  
**Notas:** _________________________________

---

### 📦 Módulo 5 — Stack Específico

**Estado actual:**
- [ ] No iniciado
- [ ] En progreso
- [ ] Completado

**Archivos completados:**

- [ ] `05-00-overview.md`
- [ ] `05-01-dotnet-avanzado.md`
- [ ] `05-02-azure-primario.md`
- [ ] `05-03-python-suficiente.md`
- [ ] `05-04-typescript-suficiente.md`
- [ ] `05-05-react-frontend-arquitectura.md`

**Señales de que el módulo está listo para avanzar:**

*NET avanzado (`05-01`):*
- [ ] Puedo explicar qué genera el compilador cuando escribe `async/await` y cuántos estados tiene la máquina de estados
- [ ] Puedo describir cuándo `IAsyncEnumerable<T>` es mejor que `Task<List<T>>` con impacto real en memoria y latencia
- [ ] Puedo explicar cómo EF Core genera SQL, cuándo es subóptimo, y las técnicas de optimización correctas (no solo "usa Include")

*Azure (`05-02`):*
- [ ] Puedo describir cuándo usar Azure Service Bus vs Event Hub vs Event Grid con criterios técnicos específicos
- [ ] Puedo diseñar la resiliencia de un servicio Azure ante falla de región con componentes específicos

*Python suficiente (`05-03`):*
- [ ] Puedo escribir un script Python funcional que lee archivos, procesa datos con diccionarios y list comprehensions
- [ ] Entiendo async/await en Python y la diferencia con threading (sin GIL awareness)

**Duración real:** _________ semanas  
**Fecha de completado:** ___________  
**Notas:** _________________________________

---

### 📦 Módulo 6 — IA Integrada

**Estado actual:**
- [ ] No iniciado
- [ ] En progreso
- [ ] Completado

**Archivos completados:**

- [ ] `06-00-overview.md`
- [ ] `06-01-ia-herramienta-desarrollo.md`
- [ ] `06-02-llm-system-design.md`
- [ ] `06-03-agentes-y-orquestacion.md`
- [ ] `06-04-context-engineering.md`
- [ ] `06-05-evaluacion-seguridad-llm.md`

**Señales de que el módulo está listo para avanzar:**

- [ ] Puedo articular la diferencia entre RAG, Fine-tuning y Long Context con sus trade-offs de costo, latencia y mantenimiento
- [ ] Puedo describir las tres capas de un pipeline RAG en producción (estática, frecuente, tiempo real)
- [ ] Puedo distinguir Context Engineering de Prompt Engineering con precisión técnica
- [ ] Puedo describir qué es un Context Zombie y cómo prevenirlo en un sistema agéntico de producción
- [ ] Puedo explicar qué evalúa un sistema de Evals para LLMs y por qué "el modelo respondió algo" no es suficiente

**Duración real:** _________ semanas  
**Fecha de completado:** ___________  
**Notas:** _________________________________

---

### 📦 Módulo 7 — Entrevistas

**Estado actual:**
- [ ] No iniciado
- [ ] En progreso
- [ ] Completado

**Archivos completados:**

- [ ] `07-00-overview.md`
- [ ] `07-01-coding-interviews.md`
- [ ] `07-02-system-design-interviews.md`
- [ ] `07-03-ai-system-design-interviews.md`
- [ ] `07-04-behavioral-interviews.md`
- [ ] `07-05-estrategia-por-empresa.md`
- [ ] `07-06-negociacion-post-oferta.md`

**Señales de que el módulo está completo:**

- [ ] Tengo el framework CTICA de coding interiorizado — lo aplico automáticamente, no tengo que pensar en él
- [ ] Tengo el framework RESHADED de system design interiorizado — estructura la entrevista sin esfuerzo consciente
- [ ] Tengo 5 historias STARL escritas y memorizadas con métricas reales — puedo adaptarlas a distintas preguntas
- [ ] Completé al menos 3 mock interviews de coding con Pramp con feedback externo
- [ ] Completé al menos 2 mock interviews de system design (Pramp o AlgoExpert Systems Expert)
- [ ] Investigué el proceso específico de entrevistas de al menos 2 empresas objetivo (Glassdoor + Blind)

**Fecha de completado:** ___________  
**Notas:** _________________________________

---

## Sección 3 — Métricas de velocidad en coding

Registra el tiempo promedio de 5 problemas por semana. Usa LeetCode o NeetCode.io para los timings.
La tendencia descendente a lo largo de las semanas confirma el progreso real más que cualquier percepción subjetiva.

| Semana | Easy (objetivo < 10 min) | Medium (objetivo < 25 min) | Hard (objetivo < 45 min) | Patrón trabajado |
|--------|--------------------------|---------------------------|--------------------------|-----------------|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |
| 6 | | | | |
| 7 | | | | |
| 8 | | | | |
| 9 | | | | |
| 10 | | | | |
| 11 | | | | |
| 12 | | | | |

**Cómo leer esta tabla:** Si la columna Medium no baja de 35 minutos después de 6 semanas de práctica constante, hay un gap de modelo mental — no de velocidad. Identificar qué patrón específico está tomando más tiempo y trabajarlo en AlgoMonster + NeetCode YouTube antes de continuar.

---

## Sección 4 — Señales de "listo para entrevistar"

Estos son los criterios que determinan si el curriculum cumplió su función. Son binarios — se cumplen o no.

### Para empresas tech sólidas (objetivo de corto plazo)

*Coding:*
- [ ] LC Medium: tiempo promedio < 25 minutos en los últimos 10 problemas consecutivos
- [ ] LC Medium: tasa de resolución correcta ≥ 80% en primer intento
- [ ] Detecto edge cases proactivamente en ≥ 90% de los problemas

*System Design:*
- [ ] Puedo diseñar un sistema como "Notification System", "URL Shortener", o "Rate Limiter" en 45 minutos con trade-offs articulados
- [ ] Clarifiqué requirements antes de diseñar en los últimos 5 simulacros — sin excepción
- [ ] El entrevistador no necesita pedirme que hable de trade-offs — los introduzco yo

*Behavioral:*
- [ ] Tengo 5 historias STARL con métricas reales listas para usar
- [ ] Puedo adaptar cada historia a distintas preguntas de behavioral sin que suene forzado

*Conocimiento:*
- [ ] Completo los criterios del Módulo 3 al 90%+
- [ ] Completo los criterios del Módulo 4 al 80%+

---

### Señales adicionales para FAANG

*Coding:*
- [ ] LC Medium: tiempo promedio < 20 minutos en los últimos 10 problemas consecutivos
- [ ] Resuelvo algunos LC Hard (no todos — pero no me bloqueo completamente ante un Hard)
- [ ] Puedo optimizar una solución correcta de O(n²) a O(n log n) o O(n) en la mayoría de los casos

*System Design:*
- [ ] Diseño sistemas a escala de 100M+ usuarios con técnicas avanzadas (Hybrid Fan-Out, multi-tier caching)
- [ ] Articulo los trade-offs de consistency (eventual vs strong) con impacto en el usuario final, no solo en el sistema
- [ ] Completo los criterios del Módulo 4 al 95%+

*Behavioral:*
- [ ] Tengo al menos 2 historias STARL preparadas por cada uno de los 5 Amazon Leadership Principles más comunes en entrevistas
- [ ] Mis historias demuestran impacto en el equipo, no solo en mi trabajo individual

*Conocimiento profundo:*
- [ ] Puedo discutir los papers de Raft, Dynamo y Spanner a nivel conversacional con trade-offs
- [ ] Completo el Módulo 2 al 95%+

---

## Sección 5 — Template de seguimiento semanal

Copiar y llenar este template al final de cada semana de estudio. Guardarlo en el vault de Obsidian en una nota separada por semana.

```
## Semana __ — Fecha: __________

### Coding practice
- Problemas resueltos esta semana: ___
- Tiempo promedio en Easy: ___ min
- Tiempo promedio en Medium: ___ min
- Patrones trabajados: _______________
- Gap identificado esta semana: _______________

### Contenido estudiado
Archivos completados:
- [ ] 
- [ ] 
- [ ] 

### System design practice
- Casos practicados esta semana: _______________
- Tiempo para estructurar requirements al inicio: ___ min
- ¿Llegué al Deep Dive en el caso? [ ] Sí / [ ] No
- Feedback que me dí a mí mismo: _______________

### Estado del tracking B
- Checkboxes nuevos marcados esta semana: ___
- Porcentaje de completado en módulo activo: ___%

### Observaciones
- Lo que funcionó bien esta semana:

- Lo que necesita más práctica:

- Plan concreto para la próxima semana:
```

---

## Sección 6 — Cómo usar Claude como mentor activo

Estos prompts están listos para copiar y usar en una sesión nueva de Claude en el proyecto zero-to-hero-v2. Cada prompt está calibrado para el tipo de práctica específica — no son intercambiables.

---

### Para validar conceptos estudiados

```
Acabo de estudiar [tema específico] del archivo [nombre del archivo] 
del módulo [N] del curriculum zero-to-hero-v2. 

Hazme 3 preguntas de nivel Staff para validar que lo entendí correctamente. 
Si alguna respuesta está incompleta o incorrecta, corrígeme directamente 
sin suavizar el feedback. Dime específicamente qué parte de mi modelo mental 
está mal y por qué.
```

---

### Para mock interview de coding

```
Actúa como entrevistador técnico Staff de una empresa tech sólida (no FAANG).
Dame un problema de [patrón específico: Two Pointers / BFS / DP 1D / etc.] 
de dificultad Medium. 

Después de que lo resuelva, evalúa:
1. ¿Usé el framework CTICA completo? (Clarify, Think, Code, Test, Analyze)
2. ¿Detecté edge cases proactivamente o esperé que me los señalaras?
3. ¿Verbalizé mi pensamiento de forma coherente?
4. ¿Qué señales de nivel Staff demostré y cuáles faltaron?

Sé directo — no suavices el feedback. Si mi solución fue nivel Promedio, 
dime exactamente qué la diferencia de una respuesta nivel Staff.
```

---

### Para mock interview de system design

```
Actúa como entrevistador de system design Staff de una empresa tech sólida.

Dime: "Design a [sistema: Notification System / URL Shortener / Rate Limiter / etc.]"

Durante la entrevista, no me des hints a menos que lleve más de 3 minutos 
atascado en el mismo punto. 

Al final, evalúa con el framework RESHADED:
- ¿Clarifiqué requirements antes de diseñar?
- ¿Estimé capacidad antes de elegir componentes?
- ¿Articulé trade-offs proactivamente?
- ¿Llegué al Deep Dive?
- ¿Qué nivel demostré (Senior / Staff) y por qué?

Sé específico sobre qué faltó y cómo hubiera sido una respuesta nivel Staff.
```

---

### Para mock interview de AI system design

```
Actúa como entrevistador de AI System Design Staff en 2026.

Dime: "Design a [sistema: RAG-based enterprise chatbot / AI content moderation pipeline / 
LLM-powered code review system]"

Al final, evalúa:
- ¿Articulé el trade-off entre RAG vs Fine-tuning vs Long Context?
- ¿Consideré latencia y costo como constraints de diseño desde el inicio?
- ¿Describí cómo evaluaría la calidad del sistema (Evals)?
- ¿Mencioné riesgos de prompt injection y guardrails?
- ¿Qué señales de nivel Staff demostré en el diseño de un sistema con componentes de IA?
```

---

### Para generar preguntas de práctica sobre un tema

```
Genera 5 preguntas de entrevista de nivel Staff sobre [tema específico] 
del archivo [nombre del archivo] del módulo [N] del curriculum zero-to-hero-v2. 

Mix de preguntas:
- 2 conceptuales (explica el mecanismo interno)
- 2 aplicadas (dado este escenario de producción, qué harías)
- 1 de trade-offs (compara X vs Y con criterios técnicos)

Después de que responda cada pregunta, evalúa mi respuesta y dime:
- ¿Es nivel Senior o Staff?
- ¿Qué falta para que sea nivel Staff?
```

---

### Para revisar una decisión de arquitectura

```
Estoy diseñando [descripción del sistema/feature] y tengo que decidir entre:
- Opción A: [descripción]
- Opción B: [descripción]

Contexto del sistema: [constraints relevantes: escala, equipo, latencia, presupuesto]

Como arquitecto Staff, dame tu análisis:
1. Cuál elegirías en este contexto y por qué
2. Qué trade-offs estoy aceptando con cada opción
3. Qué información adicional necesitarías para estar seguro de la decisión
4. Si hay una Opción C que no estoy considerando
```

---

> **Nota:** Los prompts están escritos para funcionar sin modificación. Reemplaza solo los placeholders en corchetes [].
> Siempre usar en una sesión nueva — no en una conversación con contexto acumulado de otro tema.

---

> **Siguiente referencia:** [C-glosario-terminos-clave.md](./C-glosario-terminos-clave.md) para definiciones precisas de términos técnicos.
