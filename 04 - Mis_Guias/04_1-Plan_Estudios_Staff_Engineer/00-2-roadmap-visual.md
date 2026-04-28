# 00-roadmap-visual — Mapa Panorámico del Sistema

> Consulta este archivo cuando necesites ver el mapa grande.
> No es para estudiar — es para orientarte dentro del sistema.
> ¿Dónde estás? ¿Qué sigue? ¿Qué recurso aplica ahora?
> Esas tres preguntas tienen respuesta aquí.

---

## Sección 1 — Diagrama de dependencias entre módulos

Este diagrama muestra el sistema completo: archivos de navegación, módulos,
dependencias, y los apéndices como infraestructura transversal.

Las dependencias son técnicas, no arbitrarias — están justificadas en cada overview de módulo.

```mermaid
flowchart LR
    subgraph NAV["📁 Navegación"]
        README["00-README"]
        DIAG["00-diagnóstico"]
        ROAD["00-roadmap-visual"]
    end

    subgraph M1["📦 Módulo 1 — CS Fundamentals"]
        M1_0["01-00-overview"]
        M1_1["01-01-estructuras-de-datos"]
        M1_2["01-02-complejidad-algoritmica"]
        M1_3["01-03-memoria-y-gestion"]
        M1_4["01-04-os-y-concurrencia-base"]
        M1_5["01-05-redes-y-protocolos"]
        M1_6["01-06-bases-de-datos-fundamentos-cs"]
        M1_0 --> M1_1 --> M1_2 --> M1_3 --> M1_4 --> M1_5 --> M1_6
    end

    subgraph M2["📦 Módulo 2 — Algoritmos y Patrones"]
        M2_0["02-00-overview"]
        M2_1["02-01-patrones-lineales"]
        M2_2["02-02-patrones-no-lineales"]
        M2_3["02-03-dynamic-programming"]
        M2_4["02-04-grafos-avanzados"]
        M2_5["02-05-temas-complementarios"]
        M2_6["02-06-csharp-especifico-dsa"]
        M2_7["02-07-simulacion-entrevistas-coding"]
        M2_0 --> M2_1 --> M2_2 --> M2_3 --> M2_4 --> M2_5 --> M2_6 --> M2_7
    end

    subgraph M3["📦 Módulo 3 — Software Design"]
        M3_0["03-00-overview"]
        M3_1["03-01-principios-base"]
        M3_2["03-02-solid"]
        M3_3["03-03-patrones-gof"]
        M3_4["03-04-clean-architecture"]
        M3_5["03-05-ddd"]
        M3_6["03-06-cqrs-event-sourcing"]
        M3_7["03-07-api-design"]
        M3_8["03-08-testing-strategy"]
        M3_9["03-09-refactoring-y-adr"]
        M3_0 --> M3_1 --> M3_2 --> M3_3 --> M3_4 --> M3_5 --> M3_6 --> M3_7 --> M3_8 --> M3_9
    end

    subgraph M4["📦 Módulo 4 — System Design"]
        M4_0["04-00-overview"]
        M4_1["04-01-componentes-fundamentales"]
        M4_2["04-02-bases-de-datos-system-design"]
        M4_3["04-03-caching-en-profundidad"]
        M4_4["04-04-message-queues"]
        M4_5["04-05-distributed-systems"]
        M4_6["04-06-observability-reliability"]
        M4_7["04-07-security-en-system-design"]
        M4_8["04-08-casos-clasicos"]
        M4_9["04-09-deployment-y-cicd"]
        M4_0 --> M4_1 --> M4_2 --> M4_3 --> M4_4 --> M4_5 --> M4_6 --> M4_7 --> M4_8 --> M4_9
    end

    subgraph M5["📦 Módulo 5 — Stack Específico"]
        M5_0["05-00-overview"]
        M5_1["05-01-dotnet-avanzado"]
        M5_2["05-02-azure-primario"]
        M5_3["05-03-python-suficiente"]
        M5_4["05-04-typescript-suficiente"]
        M5_5["05-05-react-frontend-arquitectura"]
        M5_0 --> M5_1 --> M5_2 --> M5_3 --> M5_4 --> M5_5
    end

    subgraph M6["📦 Módulo 6 — IA Integrada"]
        M6_0["06-00-overview"]
        M6_1["06-01-ia-herramienta-desarrollo"]
        M6_2["06-02-llm-system-design"]
        M6_3["06-03-agentes-y-orquestacion"]
        M6_4["06-04-context-engineering"]
        M6_5["06-05-evaluacion-seguridad-llm"]
        M6_0 --> M6_1 --> M6_2 --> M6_3 --> M6_4 --> M6_5
    end

    subgraph M7["📦 Módulo 7 — Entrevistas"]
        M7_0["07-00-overview"]
        M7_1["07-01-coding-interviews"]
        M7_2["07-02-system-design-interviews"]
        M7_3["07-03-ai-system-design-interviews"]
        M7_4["07-04-behavioral-interviews"]
        M7_5["07-05-estrategia-por-empresa"]
        M7_6["07-06-negociacion-post-oferta"]
        M7_0 --> M7_1 & M7_2 & M7_3 & M7_4
        M7_4 --> M7_5 --> M7_6
    end

    subgraph AP["📎 Apéndices"]
        AP_A["A-recursos-completos"]
        AP_B["B-tracking-autoevaluacion"]
        AP_C["C-glosario-terminos-clave"]
    end

    NAV --> M1
    M1 --> M2
    M1 --> M3
    M2 --> M4
    M3 --> M4
    M4 --> M5
    M4 --> M6
    M5 --> M7
    M6 --> M7
    M7 -.-> AP
    AP -.-> M1 & M2 & M3 & M4

    style NAV fill:#1a202c,color:#fff
    style M1 fill:#2a4365,color:#fff
    style M2 fill:#2a4365,color:#fff
    style M3 fill:#276749,color:#fff
    style M4 fill:#276749,color:#fff
    style M5 fill:#744210,color:#fff
    style M6 fill:#702459,color:#fff
    style M7 fill:#553c9a,color:#fff
    style AP fill:#1a202c,color:#fff
```

**⚠️ Dependencias no negociables:**
- M2 y M3 requieren M1 completo (no parcialmente)
- M4 requiere M2 mínimo hasta `02-03` y M3 hasta `03-04`
- M6 requiere M4 completo — no hay atajo aquí
- Los Apéndices son referencia permanente, disponibles desde el inicio

---

## Sección 2 — Timeline de 8-12 meses

Este diagrama muestra la distribución temporal de los módulos.
Los solapamientos mostrados son intencionales y posibles cuando se alcanzan los checkpoints.

```mermaid
gantt
    title Timeline zero-to-hero-v2 — 42 semanas (8-12 meses)
    dateFormat  YYYY-MM-DD
    axisFormat  S%W

    section Navegación
    Diagnóstico y setup           :done, nav, 2024-01-01, 3d

    section Módulo 1 — CS Fundamentals
    01-01 Estructuras de datos    :m1a, 2024-01-04, 2w
    01-02 Complejidad algorítmica :m1b, after m1a, 1w
    01-03 Memoria y gestión       :m1c, after m1b, 1w
    01-04 OS y concurrencia base  :m1d, after m1c, 1w
    01-05 Redes y protocolos      :m1e, after m1d, 1w
    01-06 BD fundamentos CS       :m1f, after m1e, 2w

    section Módulo 2 — Algoritmos
    02-01 Patrones lineales       :m2a, 2024-02-05, 2w
    02-02 Patrones no-lineales    :m2b, after m2a, 2w
    02-03 Dynamic Programming     :m2c, after m2b, 3w
    02-04 Grafos avanzados        :m2d, after m2c, 2w
    02-05 Complementarios         :m2e, after m2d, 1w
    02-06 C# específico DSA       :m2f, after m2e, 1w
    02-07 Simulación entrevistas  :m2g, after m2f, 1w

    section Módulo 3 — Software Design
    03-01 a 03-03 Fundamentos     :m3a, 2024-03-04, 3w
    03-04 a 03-06 Arquitectura    :m3b, after m3a, 4w
    03-07 a 03-09 Avanzado        :m3c, after m3b, 3w

    section Módulo 4 — System Design
    04-01 a 04-04 Fundamentos SD  :m4a, 2024-05-06, 4w
    04-05 a 04-07 Distributed     :m4b, after m4a, 4w
    04-08 a 04-09 Casos y CI/CD   :m4c, after m4b, 2w

    section Módulo 5 — Stack Específico
    05-01 a 05-02 .NET y Azure    :m5a, 2024-07-01, 4w
    05-03 a 05-05 Stacks apoyo    :m5b, after m5a, 3w

    section Módulo 6 — IA Integrada
    06-01 a 06-03 IA herramienta  :m6a, 2024-07-15, 3w
    06-04 a 06-05 LLM avanzado    :m6b, after m6a, 3w

    section Módulo 7 — Entrevistas
    07-01 a 07-03 Coding y SD     :m7a, 2024-08-26, 3w
    07-04 a 07-06 Behavioral      :m7b, after m7a, 2w
    Preparación activa mock       :crit, m7c, after m7b, 3w
```

**Nota sobre solapamientos:**
- M2 y M3 se solapan a partir de la semana 5 del curriculum (cuando M1 está completo)
- M5 y M6 tienen solapamiento parcial en las semanas 29-32
- El Módulo 7 incluye práctica activa de mock interviews que debe continuar mientras
  se avanza en los archivos teóricos

---

## Sección 3 — Mapa de recursos por módulo

Esta tabla muestra qué recurso de suscripción o referencia aplica en cada módulo.
El nivel de uso está indicado para ayudarte a planificar tu tiempo con cada plataforma.

| Módulo | AlgoMonster 🎯 | AlgoExpert 🎯 | Pluralsight 🎯 | Codecademy 🎯 | ByteByteGo 🆓 | DDIA 📚 | NeetCode 🆓 | MIT OCW 🆓 |
|--------|--------------|--------------|----------------|--------------|--------------|---------|-------------|-----------|
| M1 — CS Fundamentals | ✅ Intensivo | 〇 Complem. | 〇 Complem. | — | — | 〇 Cap. 1-3 | — | ✅ Lect. 1-6 |
| M2 — Algoritmos | ✅ Intensivo | 〇 Complem. | — | — | — | — | ✅ Intensivo | 〇 Complem. |
| M3 — Software Design | — | — | ✅ Intensivo | — | — | — | — | — |
| M4 — System Design | — | 〇 Systems | ✅ Intensivo | — | ✅ Intensivo | ✅ Completo | — | — |
| M5 — Stack Específico | — | — | ✅ Intensivo | ✅ Python/TS | — | 〇 Cap. espec. | — | — |
| M6 — IA Integrada | — | — | 〇 Complem. | — | 〇 AI posts | — | — | — |
| M7 — Entrevistas | 〇 Repaso | ✅ Intensivo | — | — | ✅ Intensivo | — | ✅ Intensivo | — |

**Leyenda:**
- ✅ Uso intensivo — recurso principal para este módulo, abre la plataforma junto con el archivo
- 〇 Complementario — recurso de apoyo, consume cuando el archivo lo indica inline
- — No aplica en este módulo

**Recursos adicionales por módulo (sin suscripción):**

| Módulo | Recurso adicional clave |
|--------|------------------------|
| M4 | Google SRE Book 🆓 — capítulos de Observability y Reliability |
| M6 | LLM Engineering Handbook 📚, AI Engineering (Chip Huyen) 📚, DeepLearning.AI 🆓 |
| M7 | Hello Interview 🆓 (YouTube), LeetCode free 🆓 (2 semanas pre-entrevista únicamente) |

---

## Sección 4 — Progresión de habilidades a lo largo del curriculum

Esta tabla muestra cómo evolucionan las habilidades clave en cada módulo.
La progresión es acumulativa — cada módulo construye sobre el anterior.

```
Leyenda de progresión:
○ = No cubierto aún
◔ = Fundamentos (puedo definir el concepto)
◑ = Aplicación (puedo usarlo en un contexto específico)
◕ = Dominio (puedo tomar decisiones y explicar trade-offs)
● = Visión Staff (puedo diseñar sistemas, ver el impacto sistémico)
```

| Habilidad | M1 | M2 | M3 | M4 | M5 | M6 | M7 |
|-----------|----|----|----|----|----|----|-----|
| Algoritmos y DSA | ◔ | ◑ | ◑ | ◑ | ◕ | ◕ | ● |
| Software Design | ◔ | ◔ | ◕ | ◕ | ● | ● | ● |
| System Design | ◔ | ◔ | ◑ | ◕ | ◕ | ◕ | ● |
| Distributed Systems | ○ | ◔ | ◔ | ◕ | ◕ | ◕ | ● |
| .NET Avanzado | ◔ | ◔ | ◑ | ◑ | ● | ● | ● |
| Azure | ◔ | ○ | ◔ | ◑ | ● | ◕ | ● |
| IA Integrada | ○ | ○ | ○ | ◔ | ◔ | ◕ | ◕ |
| Mentalidad Staff | ◔ | ◑ | ◑ | ◕ | ◕ | ◕ | ● |

**Lectura de la tabla:**
Los saltos más grandes ocurren en M3→M4 (donde el model mental de system design se construye)
y en M4→M5 (donde todo lo universal se ancla en el stack específico).
La mentalidad Staff no es un módulo — se desarrolla progresivamente a través de todo el sistema.

---

## Sección 5 — Señales de progreso por fase

Estas son las señales concretas y medibles que indican que estás progresando correctamente.
No son métricas genéricas — son señales de que el modelo mental correcto se está formando.

### Fase M1 — CS Fundamentals completado correctamente cuando:

- 🏁 Puedes explicar por qué acceder a un elemento en un array es O(1) y en una linked list es O(n),
  con la referencia al modelo de memoria de la CPU, no solo "porque es así"
- 🏁 Puedes dibujar en papel cómo funciona una tabla hash con colisiones y resolución por encadenamiento
- 🏁 Puedes explicar qué ocurre en el hardware cuando haces `await` en C# y por qué no bloquea el thread
- 🏁 Puedes describir qué es un TCP handshake y por qué HTTP/2 lo reduce a un solo round-trip

### Fase M2 — Algoritmos consolidado correctamente cuando:

- 🏁 Ves un problema de "ventana variable con constraint" y reconoces el patrón Sliding Window
  antes de leer los constraints del enunciado
- 🏁 Puedes implementar BFS y DFS sobre un grafo con lista de adyacencia, desde memoria, en C#,
  en menos de 10 minutos
- 🏁 Puedes analizar la complejidad de espacio de tu propia solución recursiva y saber cuándo
  la stack de llamadas es un problema real en producción
- 🏁 Puedes explicar por qué Dynamic Programming no es "memorizar subproblemas" sino
  "identificar subestructura óptima solapada" — y la diferencia importa

### Fase M3 — Software Design consolidado correctamente cuando:

- 🏁 Lees código que viola SRP y puedes articular el problema concreto que causará en 6 meses,
  no solo "viola el principio"
- 🏁 Puedes describir cuándo CQRS es la solución correcta y cuándo es over-engineering,
  con criterios de decisión específicos (tamaño del equipo, divergencia de modelos, etc.)
- 🏁 Puedes diseñar el modelo de dominio de un sistema de e-commerce en DDD en 30 minutos,
  identificando Aggregates, Entities, Value Objects y Bounded Contexts
- 🏁 Puedes explicar la diferencia entre una API REST bien diseñada y una que solo usa HTTP

### Fase M4 — System Design consolidado correctamente cuando:

- 🏁 Puedes diseñar un Rate Limiter distribuido en 45 minutos con al menos 3 aproximaciones
  técnicas distintas y los trade-offs de cada una
- 🏁 Puedes explicar el CAP Theorem con un escenario de partición de red real, no con definiciones
- 🏁 Dado un sistema que tiene problemas de latencia, puedes diagnosticar si el problema es
  el cacheo, la base de datos, la red, o el procesamiento — y proponer soluciones específicas
- 🏁 Puedes diseñar la arquitectura de mensajería de un sistema de pagos con requisitos de
  exactamente-una-vez (exactly-once delivery) y justificar las decisiones

### Fase M5-M6 — Stack Específico + IA consolidado cuando:

- 🏁 Puedes implementar un pipeline RAG funcional en .NET con Azure OpenAI,
  con chunking estratégico, embeddings, y recuperación semántica
- 🏁 Puedes explicar cuándo fine-tuning supera a RAG con un argumento técnico, no comercial
- 🏁 En una revisión de código, puedes identificar si EF Core generará N+1 queries,
  un cartesian explosion, o si necesita un query compilado

### Fase M7 — Listo para entrevistar cuando:

- 🏁 Completas un problema de LeetCode medium en 25 minutos explicando en voz alta
  tu proceso de pensamiento, incluyendo complejidad de tiempo y espacio al final
- 🏁 Diseñas un sistema estilo "Diseña YouTube" en 45 minutos con estimaciones de escala,
  decisiones de base de datos justificadas, y discusión de trade-offs — sin que te pregunten
- 🏁 En una pregunta behavioral, puedes narrar una historia de impacto técnico que demuestre
  razonamiento de Staff: impacto transversal, decisiones con incertidumbre, y lecciones aprendidas
- 🏁 No te bloqueas en una entrevista. Tienes un proceso para manejar el bloqueo: verbalizar
  lo que sí sabes, pedir un hint estratégicamente, proponer una solución subóptima y mejorarla

---

## Guía de uso de este archivo

**Cuándo abrir este archivo:**
- Al inicio del curriculum (para entender el sistema completo)
- Al terminar un módulo (para ver qué sigue y qué recurso aplica)
- Cuando te sientes perdido o sin momentum (para reorientar)
- Para mostrar a alguien más el alcance del sistema

**Cuándo NO abrir este archivo:**
- Como sustituto de abrir el archivo del módulo actual
- Para "planificar" sin ejecutar
- Cuando deberías estar resolviendo un ejercicio práctico

---

> **¿Ya completaste el diagnóstico?** → [00-diagnostico.md](./00-diagnostico.md)
>
> **¿Listo para empezar?** → [Módulo 1 — Overview](./modulo-01-cs-fundamentals/01-00-overview.md)
>
> **¿Buscando un recurso específico?** → [A-recursos-completos](./apendices/A-recursos-completos.md)
