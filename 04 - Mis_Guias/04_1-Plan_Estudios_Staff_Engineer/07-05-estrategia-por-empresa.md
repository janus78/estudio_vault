# 07-05 — Estrategia por Empresa

> **Cuándo leer este archivo:** Cuando tengas empresa(s) objetivo concreta(s) — 2-4 semanas antes de entrevistar. Antes de eso, los archivos 07-01 a 07-04 son más prioritarios. La estrategia específica de empresa refina la preparación general — no la reemplaza.

---

## Sección 1 — Google

### Estructura del proceso (Staff/L6+)

4-5 rondas en un loop:
- 2 rondas de Coding (LC Medium/Hard, 45 min cada una)
- 1-2 rondas de System Design (60 min)
- 1 ronda de Behavioral — "Googleyness and Leadership" (45 min)
- Opcional: ronda de dominio específico si el rol lo requiere

### Coding en Google

Google tiene el bar de algoritmos más alto entre FAANG para roles Staff. LC Hard aparece frecuentemente, y los problemas LC Medium suelen tener follow-ups que los convierten en difíciles.

**Características distintivas:**
- El follow-up es casi garantizado: "Ahora hazlo en O(1) espacio", "¿Qué pasa si el array tiene 10B elementos?"
- Esperan que identifiques múltiples approaches y compares explícitamente
- El whiteboard (físico o virtual) importa: la legibilidad del código es parte de la evaluación
- Errores de syntax se perdonan más que en otros FAANG, pero el código debe ser "casi correcto"

**Señales que busca Google en Coding:**
- Clarificación con pensamiento visible: "Pregunto si está ordenado porque eso cambia la complejidad de O(n log n) a O(n)"
- Manejo elegante de edge cases sin que el entrevistador los señale
- Código limpio y nombrado descriptivamente (no variables de 1 letra excepto índices)

### System Design en Google

Google piensa a escala de Google: millones de usuarios, petabytes de datos, latencia global.

**Lo que diferencia a un candidato Staff en Google System Design:**
- Escalar el diseño proactivamente: "Este diseño funciona para 10M usuarios. Para la escala de Google, necesitaría [cambios concretos]."
- Mencionar consistency models con precisión: no solo "usaría eventual consistency" sino "usaría eventual consistency porque el write path puede tolerar lag de 2 segundos en este caso, pero el read path necesita read-your-own-writes consistency para la UX"
- Hablar de observabilidad: qué métricas monitorearías, qué alertas configurarías

**Temas frecuentes en Google System Design 2026:**
- Sistemas distribuidos a escala global (consistent hashing, replication strategies)
- Search systems y indexing
- ML/AI systems (cómo diseñarías un sistema de recomendaciones)
- Video/media streaming

### Behavioral (Googleyness)

Google no tiene Leadership Principles formales como Amazon, pero evalúa:
- **Colaboración:** Trabajo en equipo en situaciones ambiguas
- **Humildad intelectual:** Capacidad de cambiar de opinión con evidencia
- **Impacto:** Contribuciones que van más allá del código
- **Comunicación:** ¿Puedes explicar conceptos complejos a audiencias no técnicas?

**Diferencia clave vs Amazon:** Las preguntas de Googleyness son más abiertas, menos estructuradas. "Tell me about a time you worked on a challenging project" sin el LP explícito. Tus historias STARL de 07-04 aplican directamente.

---

## Sección 2 — Microsoft

### Estructura del proceso (Principal/L65+)

4-5 rondas:
- 2 rondas de Coding (LC Medium principalmente, algunas Hard para niveles altos)
- 1-2 rondas de System Design (60 min)
- 1 ronda de Behavioral enfocada en cultura

### Coding en Microsoft

Menos extremo que Google en algoritmos. El foco está más en claridad del pensamiento que en velocidad pura.

**Características:**
- LC Medium es la norma, Hard aparece para roles Staff/Principal pero no en todos
- Más tolerancia para preguntar hints si estás atascado — lo que cuentan es el proceso
- Si el rol es Azure-adjacent, pueden preguntar sobre arquitecturas de cloud o distributed systems dentro del coding

### System Design en Microsoft

**Diferencia vs Google:** Microsoft a menudo da System Design con más contexto del negocio. "Diseña el sistema de notificaciones para Teams" es más frecuente que "Diseña un sistema de notificaciones genérico".

**Señal Staff en Microsoft:** Si el rol es Azure-adjacent (que es probable dado tu stack), puedes usar Azure-first en el diseño. "Para el message queue usaría Azure Service Bus porque el equipo ya tiene experiencia operacional con Azure y el SLA de 99.9% cumple el requirement." Eso muestra stack awareness que es signal positivo en Microsoft.

**Temas frecuentes:**
- Colaboración tools (Teams-like, SharePoint-like)
- Cloud services design (especialmente si el rol es en Azure)
- Developer tools

### Behavioral en Microsoft

Foco en:
- **Growth mindset:** Historial de aprendizaje y mejora continua
- **Colaboración:** Cómo trabajas con personas de diferentes backgrounds y roles
- **Customer obsession:** (influencia de la cultura satya nadella) ¿Piensas en el impacto al usuario?

---

## Sección 3 — Meta

### Estructura del proceso (E6/Staff)

4-5 rondas:
- 2 rondas de Coding — foco en velocidad de ejecución
- 1-2 rondas de System Design — escala de Meta (3B usuarios)
- 1 ronda de Behavioral — culture fit, "Move Fast"

### Coding en Meta

Meta prioriza velocidad más que Google. El objetivo es demostrar fluidez con LC Medium en < 20 minutos, dejando tiempo para follow-ups.

**Señal Staff específica de Meta:** Meta usa mucho arrays de strings y trees — tener esos patrones especialmente fluidos.

**Diferencia clave:** Meta permite usar la librería de Java/Python/C# más libremente. No te penalicen por usar `List.Sort` en lugar de implementar tu propio sort. El foco es el algoritmo, no la memoria de API.

### System Design en Meta

Meta piensa en social graph, ads, news feed, y escala de 3B usuarios activos.

**Temas más frecuentes en Meta:**
- News Feed / Timeline (Hybrid Fan-Out que ya está en 07-02)
- Social Graph queries a escala
- Ad targeting / auction systems
- Privacy-preserving systems (Meta tiene mucha presión regulatoria)

**Señal Staff en Meta System Design:**
- Hablar de la escala del social graph: "Con 3B usuarios y promedio de 500 amigos, el social graph tiene ~1.5 trillion edges. Eso requiere un grafo distribuido específico, no un SQL genérico."
- Mencionar ads como contexto de negocio: "El news feed prioriza contenido de amigos pero también ads — el sistema de ranking necesita balancear relevancia orgánica con revenue."

### Behavioral en Meta

La cultura de Meta es más informal que Amazon o Google. Las preguntas son más directas y el entrevistador quiere ver:
- Velocidad de decisión: ¿puedes tomar decisiones con información incompleta?
- Impacto a escala: ¿piensas en millones de usuarios, no en tu equipo de 8?
- "Move fast": ¿tienes historial de entregar rápido iterando, no de planear perfectamente?

---

## Sección 4 — Amazon

### Estructura del proceso (L6/L7)

4-6 rondas + Bar Raiser:
- 2 rondas de Coding
- 2 rondas de System Design (una puede ser con enfoque en ML/AI para roles relevantes)
- 2 rondas de Behavioral (cada una evaluando un LP diferente)
- Bar Raiser: un entrevistador neutral que puede vetar la oferta incluso si todos los demás dicen sí

### El Bar Raiser: qué es y cómo funciona

El Bar Raiser es un entrevistador senior de Amazon entrenado específicamente para este rol. No tiene interés en contratar al candidato para su equipo — su trabajo es asegurarse de que la contratación elevaría el bar promedio de Amazon, no solo lo igualara.

**Qué busca el Bar Raiser:**
- Las mismas señales de nivel Staff de 07-00, pero con criterio más estricto
- Evidencia de que el candidato opera consistentemente en el nivel L6, no "a veces"
- Señales de que la persona puede crecer al siguiente nivel

**Cómo prepararse para el Bar Raiser:** No hay preparación específica — la única preparación es tener el nivel real. Si tus historias STARL muestran impacto organizacional con métricas reales, el Bar Raiser lo verá.

### Leadership Principles de Amazon (los 16)

Amazon evalúa behavioral con sus LPs de forma explícita. Para posiciones Staff/L6, los más críticos:

| LP | Qué demostrar para nivel Staff |
|---|---|
| **Dive Deep** | Historia donde tú — no tu equipo — identificaste el root cause de un problema que otros habían ignorado |
| **Think Big** | Historia donde tu decisión consideró impacto a 2+ años, no solo el sprint actual |
| **Invent and Simplify** | Historia donde simplificaste un sistema complejo (no un feature — un sistema) |
| **Disagree and Commit** | Historia donde defendiste tu posición, no ganaste, y luego ejecutaste con 100% de commitment |
| **Deliver Results** | Historia donde entregaste algo con impacto medible a pesar de obstáculos reales |
| **Ownership** | Historia donde tomaste responsabilidad de algo que "no era tu problema" |
| **Hire and Develop the Best** | Para Staff: historia donde tu mentoring cambió el trayectory de otro engineer |

**Preparación para Amazon:** Mínimo 2 historias por LP de la tabla anterior. Amazon es el único FAANG donde esta preparación explícita es obligatoria — no opcional.

---

## Sección 5 — Tech Sólida No-FAANG

### Características del proceso

El proceso típico en empresas tech maduras no-FAANG (Stripe, Shopify, Atlassian, Nubank, Mercado Libre, y similares):

- 3-4 rondas totales vs 5-6 en FAANG
- Menos énfasis en LC Hard — el foco está más en pragmatismo que en algorithmic purity
- System Design tiene mayor peso relativo que Coding
- Mayor énfasis en experiencia práctica relevante al negocio de la empresa
- Behavioral más conversacional, menos structured frameworks como LPs

### Cómo investigar el proceso específico

```
Glassdoor → Reviews → "Interview" filter
→ Ver reportes de entrevistas de los últimos 6 meses
→ Qué preguntas se repiten, qué tipo de problemas

Blind → buscar el nombre de la empresa
→ Más detalle técnico sobre las rondas
→ Devs reales hablando de sus experiencias recientes

LinkedIn → "People" filter en la empresa
→ Ver qué tecnologías aparecen en los perfiles del equipo
→ Si el equipo usa Azure y .NET, tu stack es un asset
→ Si el equipo usa Go y Kubernetes, prep adicional en esas áreas

GitHub → repos públicos de la empresa (si existen)
→ El stack real, el estilo de código, los patrones que usan
→ Mención de esto en la entrevista ("vi que usan EF Core con el patrón X") es señal muy positiva
```

### El diferenciador en empresas no-FAANG

En FAANG, el proceso es estandarizado — el entrevistador individual importa menos que el sistema.

En empresas no-FAANG, el hiring manager y el equipo tienen más poder. Eso significa:

- Mostrar que entiendes el negocio de la empresa específica tiene más peso
- La fit cultural importa más — la persona que te entrevista puede ser tu manager
- Hacer preguntas inteligentes sobre el producto, los desafíos técnicos del equipo, y la cultura es parte de la evaluación

**Preguntas inteligentes que un Staff debe hacer al entrevistador:**
- "¿Cuál es la mayor deuda técnica que el equipo está navegando ahora?"
- "¿Cómo se toman las decisiones técnicas en el equipo — hay proceso de RFC, design docs, o es más informal?"
- "¿Cuál sería el impacto de la persona en este rol en los primeros 90 días?"
- "¿Qué separa a alguien que funciona bien en este rol de alguien que es excepcional?"

---

## Sección 6 — Nubank y Mercado Libre (Contexto LATAM)

Relevante para Omar dada su ubicación en Guadalajara, México.

### Nubank

**Stack principal:** Clojure backend, Kotlin mobile, AWS, data-heavy con Flink y Spark  
**Proceso:** 4-5 rondas estructuradas, similar a tech sólida non-FAANG  
**Foco:** System Design real — Nubank procesa millones de transacciones financieras, y buscan engineers que entiendan consistency, idempotency, y financial systems  
**Diferenciador:** Mención de conocimiento en sistemas financieros (idempotency, double-entry bookkeeping, reconciliation) es muy valorada. El stack C#/.NET no es el de Nubank pero el conocimiento de distributed systems trasciende el stack.

### Mercado Libre

**Stack principal:** Java backend principalmente, Go creciente, AWS y on-premise  
**Proceso:** 4-5 rondas, similar a tech sólida  
**Foco:** Escala latinoamericana — Mercado Libre procesa $30B+ en transacciones al año. El e-commerce y los sistemas de pagos son el core.  
**Diferenciador:** Mercado Libre tiene un mercado agnóstico al stack — buscan el modelo mental correcto, no un stack específico. System Design con foco en e-commerce (inventory, orders, payments, shipping) es muy relevante.

**Consejo práctico para LATAM:** Ambas empresas publican ingeniería en sus blogs técnicos (Nubank Engineering, Mercado Libre Engineering). Leer 5-10 posts recientes antes de entrevistar da contexto real de los problemas que están resolviendo — y eso se puede mencionar en la entrevista.

---

## Sección 7 — Red Flags en Procesos de Entrevista

Señales de que el proceso — o la empresa — tiene problemas:

**Red flags del proceso:**
- No hay ronda de System Design para un rol Staff → subestiman el nivel o el rol no es realmente Staff
- El proceso dura > 10 semanas sin updates claros → problemas organizacionales o el pipeline está estancado
- El reclutador no puede responder preguntas básicas sobre el rol (responsabilidades, equipo, manager) → el rol no está bien definido
- Las preguntas técnicas son muy específicas al stack interno ("¿Cómo configuras el scheduler interno de X herramienta propietaria?") → evalúan conocimiento específico, no habilidades generales. Señal de que el rol es más de mantenimiento que de diseño
- No hay feedback post-rechazo → señal de proceso inmaduro o cultura de bajo accountability

**Red flags de la empresa:**
- El hiring manager no puede describir el impacto esperado del rol en los próximos 12 meses
- Todos los entrevistadores parecen desconectados del trabajo real del equipo
- No hay respuesta a la pregunta "¿cuál es la mayor deuda técnica?" → o no saben o no quieren decir
- Los niveles de compensación son significativamente por debajo del mercado sin una razón clara (misión, equity, etc.)

**Cómo evaluar red flags sin rechazar oportunidades prematuramente:**
Tener 1-2 red flags es normal. Tener 4+ es señal de salir del proceso. El costo de unirse a una empresa con problemas organizacionales reales es mayor que el costo de perder una entrevista.

---

## Checklist de Salida

- [ ] Para cada empresa objetivo, investigué el proceso en Glassdoor/Blind
- [ ] Identifiqué el peso relativo de cada tipo de ronda para esa empresa
- [ ] Preparé preguntas inteligentes para hacer al entrevistador
- [ ] Si es Amazon: tengo 2 historias STARL por cada LP relevante de la lista
- [ ] Identifiqué al menos 1 red flag del proceso y tengo criterio para evaluar si es bloqueante

---

> **Siguiente paso:** [07-06-negociacion-post-oferta.md](./07-06-negociacion-post-oferta.md) — Cómo evaluar y negociar la oferta que recibas después de todo este proceso.
