# 07-04 — Behavioral Interviews: STARL y Señales de Nivel Staff

> **Por qué este archivo importa más de lo que crees:** En entrevistas Senior, behavioral es un 20-30% del peso. En entrevistas Staff, behavioral puede ser 40-50%. La razón es simple: un Staff Engineer tiene influencia que va más allá de su código. Los entrevistadores necesitan evidencia de historial — no teoría sobre "cómo manejarías" una situación hipotética.

---

## Sección 1 — Por Qué Behavioral Importa Más en Staff que en Senior

El razonamiento del entrevistador en roles Senior: "¿Este developer puede ejecutar bien técnicamente?"

El razonamiento en roles Staff: "¿Este developer puede **liderar técnicamente** — influir sin autoridad formal, navegar conflictos, tomar decisiones con información incompleta, y escalar el conocimiento del equipo?"

La respuesta a esa segunda pregunta no sale de preguntas técnicas — sale de evidencia histórica de comportamiento real. Por eso behavioral pesa más.

**La trampa más común:** Preparar respuestas de nivel Senior para entrevistas Staff. Una respuesta donde el impacto es "mi equipo hizo X" cuando el nivel Staff espera "mi decisión afectó cómo trabajan 3 equipos" es una señal de nivel incorrecto.

---

## Sección 2 — El Framework STARL Expandido

**S — Situation (2-3 oraciones):**
El contexto con suficiente detalle para que el entrevistador entienda la complejidad — pero no tanto que pierdas el hilo antes de llegar a tus acciones. Error común: gastar 2 minutos en el contexto y 30 segundos en lo que hiciste.

**T — Task (1-2 oraciones):**
Qué se esperaba de ti específicamente. ¿Eras el único responsable o parte de un equipo? ¿Tenías restricciones de tiempo, presupuesto, o equipo? Esta sección establece la dificultad del contexto.

**A — Action (el núcleo — 60-70% del tiempo total):**
Qué hiciste TÚ específicamente. El error más común en behavioral: describir lo que "el equipo" hizo. El entrevistador evalúa tu contribución individual, no la del equipo. Usa "yo" no "nosotros".

Señales de nivel Staff en el Action:
- Tomaste iniciativa que iba más allá de tu responsabilidad formal
- Influenciaste a personas que no reportaban a ti
- Navegaste un conflicto con criterio explícito, no por intuición
- Usaste datos para respaldar una decisión controversial

**R — Result (métricas, siempre):**
Qué pasó como consecuencia directa de tus acciones. La regla de oro: si no tienes un número, busca uno. Algunos ejemplos de cómo medir si inicialmente no tienes métrica:

```
"El time-to-deploy bajó de 2 horas a 15 minutos"        ← cuantificable
"Reducimos los bugs en producción en un 40%"              ← cuantificable
"El equipo pasó de 3 deploys/semana a 10 deploys/semana"  ← cuantificable
"Resolvimos el conflicto y el proyecto se entregó a tiempo" ← al menos verificable
```

Si de verdad no tienes un número, describe el resultado en términos de comportamiento observable: "El equipo adoptó el nuevo proceso — ya nadie pregunta cómo hacer X, ahora está en la documentación que yo creé."

**L — Learning (diferenciador de Staff):**
Qué aprendiste y cómo cambió tu approach posterior. Esta parte es la que más diferencia a los candidatos Staff. Muestra autoconsciencia, capacidad de mejora, y que conviertes experiencias en conocimiento transferible.

La trampa: un Learning genérico. "Aprendí la importancia de la comunicación" es inútil. El Learning de Staff: "Aprendí que cuando hay desacuerdo técnico entre seniors, lo más productivo no es debatir el approach — es acordar primero los criterios de evaluación. Ahora, antes de proponer una solución, propongo el framework de decisión."

---

## Sección 3 — Las 5 Preguntas que Siempre Aparecen

Para cada una: qué busca el entrevistador, respuesta nivel promedio vs nivel Staff, y template.

---

### Pregunta 1: "Tell me about a time you had a technical disagreement with your team."

**Qué busca el entrevistador:**
- ¿Puedes defender tu posición técnica con argumentos, no con autoridad?
- ¿Eres capaz de cambiar de opinión cuando el otro tiene razón?
- ¿Puedes mantener la relación mientras navegas el conflicto?
- ¿Tienes proceso para resolver desacuerdos, o lo resuelves por quién "grita más fuerte"?

**Respuesta nivel promedio:**
> "Tuve un desacuerdo con un compañero sobre qué framework de testing usar. Él quería seguir con xUnit, yo quería migrar a algo más moderno. Al final usamos lo que yo propuse y funcionó bien."

Por qué es promedio: no hay proceso visible, el "resultado" es vago, no hay learning concreto.

**Respuesta nivel Staff:**
> "Mi equipo quería migrar un módulo a microservicios. Yo creía que el monolito bien estructurado era suficiente para nuestra escala actual. [Situation]
>
> Era el tech lead de ese módulo, así que la decisión me afectaba directamente. [Task]
>
> En lugar de bloquer la propuesta o simplemente aceptarla, hice un análisis de la complejidad operacional adicional: necesitaríamos service discovery, distributed tracing, network boundaries — todo esto para un equipo de 8 personas que deployaba 2 veces por semana. Presenté el análisis con los costos estimados (2 sprints adicionales de setup, +$800/mes en infraestructura).
>
> Pero propuse también un criterio de decisión: si el módulo necesitara deploy independiente más de 2 veces al mes, valdría la pena el overhead. Monitoreamos por 3 meses — nunca superó 1 vez al mes. El equipo acordó mantener el monolito. [Action]
>
> El módulo siguió en producción con 0 incidentes relacionados con la arquitectura por los siguientes 9 meses. [Result]
>
> El aprendizaje más importante: las decisiones de arquitectura necesitan criterios medibles, no solo intuiciones o preferencias. Ahora, cuando propongo o evalúo cambios arquitectónicos, siempre propongo primero el criterio de éxito — no la solución. Eso cambia completamente la calidad del debate." [Learning]

**Template para construir tu historia:**

```
Situación: [proyecto, equipo, contexto — 2-3 oraciones]
La decisión en disputa: [qué se estaba decidiendo y por qué importaba]
Mi posición y el argumento técnico: [no "yo quería X" sino "yo argumenté que X porque Y"]
Cómo propuse resolverlo: [proceso — no solo el resultado]
Resultado verificable: [métricas o comportamiento observable]
Learning: [qué cambió en cómo tomas decisiones técnicas ahora]
```

---

### Pregunta 2: "Tell me about a time you led a technical initiative without formal authority."

**Qué busca:** ¿Puedes influir sin título? ¿Sabes construir coaliciones, comunicar visión técnica, y mover a otros hacia una dirección sin poder formal?

**Respuesta nivel Staff:**
> "Vi que 3 equipos estaban solucionando el problema de autenticación de forma diferente — uno con JWT custom, uno con cookies de sesión, uno con Azure AD directo. Eso era deuda técnica acumulando silenciosamente. [Situation]
>
> No era mi responsabilidad formal — cada equipo tenía su propio tech lead. Pero el costo de tener 3 implementaciones distintas era real: onboarding más lento, bugs difíciles de rastrear, imposibilidad de centralizar auditoría. [Task]
>
> Empecé por hablar 1-on-1 con cada tech lead — no para proponer la solución, sino para entender por qué habían tomado sus decisiones. Encontré que el problema no era preferencia técnica — era que no había un camino claro aprobado por la organización.
>
> Propuse un RFC (Request for Comments): un documento de 4 páginas con el problema, las opciones, los trade-offs, y una recomendación. Lo circulé a los 3 tech leads y al arquitecto principal para feedback antes de enviarlo al director técnico. 6 comentarios después, teníamos consenso.
>
> Luego escribí la librería de integración con Azure AD, la documenté, y la presenté como "esto ya funciona, solo tienen que usar el paquete NuGet". Eso removió la resistencia: no era "vayan a migrar todo", era "aquí está el trabajo ya hecho". [Action]
>
> Los 3 equipos migraron en 6 semanas. El siguiente dev nuevo en la empresa encontró la documentación en < 10 minutos y tuvo autenticación funcionando en 1 hora. [Result]
>
> Aprendí que la resistencia a los estándares comunes casi nunca es sobre la tecnología — es sobre el costo de cambio. Si reduces ese costo (proveyes el trabajo ya hecho), la adopción se acelera radicalmente." [Learning]

---

### Pregunta 3: "Tell me about a time you had to deliver bad news to stakeholders."

**Qué busca:** ¿Eres honesto con información difícil, o la evitas o la suavizas? ¿Sabes enmarcar malas noticias con opciones, no solo con el problema?

**Respuesta nivel Staff:**
> "A 2 semanas del launch, descubrí que la estimación de performance del sistema de pagos estaba completamente equivocada. Habíamos estimado soportar 1,000 transacciones/segundo. Las pruebas de carga reales mostraban que el sistema colapsaba a 200 TPS con el volumen de datos reales. [Situation]
>
> Yo era el responsable técnico del módulo. El VP de Producto y el CEO estaban contando con esa fecha. [Task]
>
> Antes de escalar, pasé 4 horas identificando exactamente el cuello de botella (queries N+1 en EF Core sin índices adecuados) y estimando el tiempo real de fix: 3-4 días de trabajo intensivo con 2 devs.
>
> Luego escribí un email de 5 párrafos (no un Slack): el problema concreto, el impacto si se lanzaba así, el fix técnico, el tiempo estimado, y 3 opciones: (a) retrasar 5 días y lanzar bien, (b) lanzar con traffic throttling al 20% de la capacidad y escalar gradualmente, (c) lanzar y aceptar el riesgo de downtime con plan de rollback listo.
>
> Presenté las opciones en persona al VP de Producto, no solo las malas noticias. [Action]
>
> Eligieron la opción (b). Lanzamos a tiempo pero limitado, escalamos gradualmente mientras aplicábamos los fixes. 0 downtime. El VP de Producto me dijo después que apreciaba que hubiera llegado con opciones, no solo con un problema. [Result]
>
> Aprendí que la gente no recuerda que había un problema — recuerdan cómo lo manejaste. Y que llegar con opciones en lugar de solo el problema es la diferencia entre ser un mensajero y ser un líder técnico." [Learning]

---

### Pregunta 4: "Tell me about a technical failure you were responsible for."

**Qué busca:** Honestidad genuina sobre failures. Autoconsciencia. Ownership sin victimización. Y lo más importante: ¿aprendiste algo real?

⚠️ **Anti-patrón de candidatos promedio:** Elegir un failure tan menor que no muestra nada, o uno donde "el equipo falló" pero no tú específicamente.

**Respuesta nivel Staff:**
> "Diseñé una migración de base de datos en producción que causó 45 minutos de downtime no planificado para 15,000 usuarios. [Situation]
>
> Era el responsable técnico de la migración. Habíamos planificado hacerla en una ventana de mantenimiento de 2 horas. [Task]
>
> La migración requería un índice en una tabla de 50M registros. Probé en staging con 1M registros y tardó 12 minutos — dentro de la ventana. No consideré que en PostgreSQL, CREATE INDEX tarda no-linealmente: para 50M registros tardó 3 horas, no 1. Durante las 3 horas, la tabla estaba bloqueada para writes.
>
> Me equivoqué en dos cosas: no hice la estimación de tiempo con datos de producción reales, y no investigé si existía `CREATE INDEX CONCURRENTLY` que no bloquea writes en PostgreSQL.
>
> Durante el incidente, tomé la decisión de no hacer rollback porque eso hubiera causado más downtime — completar era más rápido. Comuniqué el estado cada 15 minutos al equipo y a los stakeholders. [Action]
>
> La migración se completó. Escribí un postmortem con 5 acciones concretas: checklist de migraciones en producción, test de performance con dump de datos reales, y la regla de que toda migración de tabla > 10M registros requiere CREATE INDEX CONCURRENTLY. [Result]
>
> El aprendizaje real fue más profundo que el checklist: yo sabía que debía probar con datos de producción — no lo hice por presión de tiempo. Aprendí que la presión de tiempo es exactamente cuando más importante es no saltarse los pasos de validación, no cuando menos." [Learning]

---

### Pregunta 5: "Describe a time you had to significantly simplify a technical solution."

**Qué busca:** ¿Puedes reconocer overengineering? ¿Priorizas la solución más simple que funciona sobre la más elegante? Esta pregunta evalúa madurez de juicio técnico.

**Respuesta nivel Staff:**
> "El equipo propuso implementar un sistema de event sourcing + CQRS para un módulo de reportes que generaba PDFs una vez por semana. [Situation]
>
> Era mi responsabilidad revisar la propuesta técnica antes de que el equipo empezara a construirla. [Task]
>
> El diseño propuesto era técnicamente correcto — Event Sourcing es el patrón correcto cuando necesitas auditoría completa y capacidad de reproducir estado en cualquier punto del tiempo. Pero el módulo tenía estos requerimientos reales: generar un PDF semanal con los últimos 30 días de ventas. Sin necesidad de auditoría, sin reproducción de estado, sin queries en tiempo real.
>
> El overhead: 2 semanas de implementación, 3 nuevos conceptos que el equipo no conocía, mayor complejidad operacional para siempre.
>
> Propuse una alternativa: una tabla SQL con los agregados pre-calculados que se actualizan con un cron job diario. 2 días de implementación. El equipo lo entendía completamente. [Action]
>
> Entregamos en 2 días, funciona desde hace 18 meses sin problemas, y el equipo puede maintenerlo sin consultar documentación. [Result]
>
> Aprendí que el overengineering usualmente viene de aplicar el patrón correcto al problema equivocado. Ahora, cuando reviso propuestas técnicas, la primera pregunta que hago es: '¿cuál es el requerimiento más simple que esta solución necesita cumplir?' Si la respuesta es simple, la solución debe serlo también." [Learning]

---

## Sección 4 — Señales de Nivel Staff en Behavioral

| Dimensión | Senior | Staff |
|---|---|---|
| Radio de influencia | "Mi equipo" o "mi manager y yo" | "Otros equipos", "el director técnico", "3 equipos distintos" |
| Métricas | Vagas: "mejoró" o "funcionó bien" | Concretas: "de 2h a 15min", "0 downtime", "40% menos bugs" |
| Conflictos | Los resuelve por consenso o los escala | Los navega con criterio explícito y proceso transparente |
| Failures | Los minimiza o los atribuye al contexto | Los describe con honestidad, ownership total, y aprendizaje demostrable |
| Iniciativas | Asignadas por el manager | Identificadas y lideradas por iniciativa propia |
| Impacto | "El feature se entregó" | "Cambió cómo trabaja el equipo", "adoptado por 3 equipos" |
| Decisiones | Las implementa cuando se las asignan | Las toma con frameworks que diseña para los criterios del problema |

---

## Sección 5 — Amazon Leadership Principles (para Entrevistas Amazon)

Amazon es el caso más estructurado: cada ronda behavioral tiene un Leadership Principle (LP) asignado explícitamente. El entrevistador tiene su LP antes de entrar a la sala.

Los más frecuentes en roles Staff/Principal:

**Dive Deep:** ¿Puedes ir al detalle técnico cuando importa, aunque seas Staff?
→ Historia donde identificaste un problema que otros habían ignorado porque "era un detalle de implementación"

**Think Big:** ¿Piensas en el impacto a largo plazo más allá del sprint actual?
→ Historia donde una decisión tuya consideró impacto a 12-18 meses, no solo el requerimiento inmediato

**Invent and Simplify:** ¿Simplificas sistemas complejos o los complicas más?
→ Historia de simplificación real (como el Pregunta 5 de arriba)

**Disagree and Commit:** ¿Defiendes tu posición técnica Y luego committeas completamente cuando se toma otra decisión?
→ Historia donde perdiste el debate técnico pero ejecutaste la decisión del equipo con 100% de compromiso

**Deliver Results:** ¿Tienes historial de resultados a pesar de obstáculos?
→ Historia donde entregaste a pesar de cambios de requirements, recursos reducidos, o problemas técnicos imprevistos

**Cómo prepararse para Amazon:** Para cada LP de la lista anterior, tener al menos 2 historias STARL distintas. En una ronda de 45 minutos, el entrevistador puede hacer 2-3 preguntas del mismo LP. Tener solo 1 historia por LP es insuficiente.

---

## Sección 6 — Template de Preparación

Usa esta tabla para mapear tus historias reales a las preguntas más comunes. El objetivo es identificar gaps antes de la entrevista, no durante.

| Historia | Técnico disagree | Lead sin autoridad | Bad news a stakeholders | Failure tuyo | Simplificación |
|---|---|---|---|---|---|
| Historia 1 | ✅ | | | | |
| Historia 2 | | ✅ | | | |
| Historia 3 | | | ✅ | | |
| Historia 4 | | | | ✅ | |
| Historia 5 | | | | | ✅ |

Si una celda está vacía, necesitas esa historia antes de entrevistar. No la improvises — escríbela.

**Criterio de calidad para cada historia:**
- [ ] Tiene métricas concretas en el Result
- [ ] El Action usa "yo" no "nosotros"
- [ ] El Learning describe algo concreto que cambió en tu approach
- [ ] El radio de influencia es Staff (afecta más allá de tu equipo inmediato)
- [ ] El delivery toma 2-3 minutos, no 5+

---

## Checklist de Salida

- [ ] Tengo 5 historias STARL escritas (no solo en mi cabeza) con métricas concretas
- [ ] Cada historia toma 2-3 minutos en el delivery
- [ ] Tengo al menos 1 historia de failure con ownership total y learning real
- [ ] Mis historias incluyen impacto en más de un equipo o en la organización
- [ ] Puedo mapear las 5 historias a múltiples preguntas distintas
- [ ] Si la empresa objetivo es Amazon, tengo 2 historias por cada LP de la lista

---

> **Siguiente paso:** [07-05-estrategia-por-empresa.md](./07-05-estrategia-por-empresa.md) — Cómo adaptar todo lo anterior al proceso específico de la empresa que te interesa.
