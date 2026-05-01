# 05-00 — Módulo 5: Stack Específico — Por Qué Universal Primero

> **Posición en el curriculum:** Este módulo viene exactamente después de los 4 módulos universales por una razón que no es arbitraria. Si llegaste aquí sin haber trabajado los módulos 1-4, este módulo no tendrá el peso que debe tener — serás un Senior que sabe usar herramientas, no un Staff que sabe por qué funcionan.
>
> **Lo que este módulo NO es:** Un tutorial de cómo usar .NET o Azure. La documentación oficial existe para eso. Este módulo es la capa de comprensión que la documentación no te da: por qué el compilador genera lo que genera, cuándo un servicio de Azure es la elección correcta y cuándo es overengineering, y cómo articular esas decisiones bajo presión en una entrevista.

---

## Sección 1 — Por qué los módulos universales van primero

### El argumento real

Llevas 10 años escribiendo C#. Puedes crear una API en ASP.NET Core dormido. Puedes configurar EF Core, manejar migrations, crear middlewares personalizados. En eso eres Senior sólido.

**Entonces, ¿qué te falta para ser Staff?**

No es conocer más APIs de .NET. Es tener el framework mental para hacer preguntas que los Seniors no se hacen:

- *¿Por qué el middleware pipeline funciona como una cadena de delegados y no como un bus de eventos?*
- *¿Qué problema de diseño resuelve la Dependency Rule en Clean Architecture, y por qué ese problema no existe cuando el equipo tiene 2 personas?*
- *¿Cuándo Azure Functions introduce más riesgo que Container Apps, aunque Functions sea "más simple"?*
- *¿Por qué ConfigureAwait(false) importa en una librería pero es irrelevante en ASP.NET Core moderno?*

Esas preguntas requieren el framework de los módulos 1-4. Sin ellos, puedes implementar el patrón correctamente pero no puedes defenderlo — y en una entrevista Staff, la defensa es todo.

### La diferencia que marca el nivel

Un Senior especializado en .NET puede responder: *"¿Cómo implemento CQRS en ASP.NET Core?"* — instala MediatR, crea Commands y Queries, registra los handlers.

Un Staff puede responder: *"¿Cuándo CQRS con MediatR agrega valor real vs cuándo es overhead que ralentiza el equipo?"* — porque en el módulo 3 entendió el problema que CQRS resuelve, y en este módulo entiende cómo se manifiesta específicamente en .NET.

La diferencia no es conocer más. Es pensar en consecuencias de arquitectura, no en implementaciones.

### Por qué esto importa en entrevistas Staff

Las entrevistas Staff en empresas tech serias no preguntan cómo se usa una API. Preguntan:

- *"Diseña un sistema de procesamiento de órdenes para 1M de usuarios. Qué servicios de Azure usarías y por qué no los otros."*
- *"Tu equipo está usando MediatR con CQRS. El sistema tiene problemas de performance. ¿Dónde miras primero y cómo lo diagnosticas?"*
- *"Explica cómo funciona el Change Tracker de EF Core y en qué escenarios es un problema."*

Esas respuestas requieren los módulos 1-4 como base. Este módulo construye la capa de implementación específica sobre ese fundamento.

---

## Sección 2 — Mapa de profundidad por tecnología

No todas las tecnologías en este módulo merecen el mismo nivel de inversión. La profundidad objetivo varía según el rol que cada tecnología juega en tu carrera y en las entrevistas.

| Tecnología | Profundidad objetivo | Razón |
|---|---|---|
| **.NET / C# / ASP.NET Core / EF Core** | **Staff-level internals** | Stack primario. Aquí está tu ventaja competitiva. Un entrevistador que ve "Senior .NET" en tu CV espera respuestas de internals. Si das respuestas de documentación, señal negativa. |
| **Azure** | **Decisiones arquitectónicas con criterio** | Cloud primario. No necesitas saber configurar cada servicio de memoria — necesitas saber cuándo Azure Functions vs Container Apps vs AKS, y poder argumentarlo con trade-offs reales. |
| **Python** | **Suficiente para entrevistas y AI tooling** | Demanda de mercado alta en 2026, especialmente para integración con sistemas de IA. No necesitas nivel Senior — necesitas poder leer y escribir código Python con fluidez, y entender FastAPI y async patterns. |
| **TypeScript** | **Suficiente para no ser ciego en fullstack** | Colaboración con equipos de frontend. En entrevistas Staff de plataformas fullstack, preguntan arquitectura de frontend. Necesitas entender los patterns, no implementarlos a nivel Senior. |
| **React** | **Arquitectura y patterns, no CSS** | Blazor es tu stack de UI. Pero el mercado tiene más React que Blazor. Necesitas entender el modelo de componentes, state management, y cuándo SSR vs CSR desde una perspectiva de arquitecto, no de frontend developer. |

**La regla de calibración:** En .NET/Azure irás a la profundidad que un entrevistador de arquitectura espera. En Python, TypeScript y React irás a la profundidad que un Staff necesita para hacer decisiones técnicas correctas sin ser el experto del equipo.

---

## Sección 3 — Cómo este módulo conecta con los anteriores

Cada archivo del módulo 5 es la implementación específica de conceptos universales. No son contenidos independientes — son la concreción de lo que ya estudiaste.

```mermaid
graph TD
    subgraph Módulo 3 — Software Design
        M3_CA[03-04 Clean Architecture\nDependency Rule, capas]
        M3_CQRS[03-06 CQRS + Event Sourcing\nComandos, Queries, Proyecciones]
        M3_DDD[03-05 DDD\nAggregates, Value Objects]
        M3_API[03-07 API Design\nREST, versioning, contratos]
    end

    subgraph Módulo 4 — System Design
        M4_BD[04-02 Bases de Datos\nSQL vs NoSQL, índices, sharding]
        M4_MQ[04-04 Message Queues\nPatrones de mensajería]
        M4_SEC[04-07 Security\nAuth, secrets management]
        M4_DEP[04-09 Deployment\nBlue-green, CI/CD, containers]
    end

    subgraph Módulo 5 — Stack Específico
        M5_DN[05-01 .NET Avanzado\nMiddleware pipeline, DI, EF Core,\nasync/await internals, Minimal APIs]
        M5_AZ[05-02 Azure Primario\nCompute decision, Service Bus,\nCosmosDB vs SQL, Managed Identity]
        M5_PY[05-03 Python Suficiente\nFastAPI, async, pydantic]
        M5_TS[05-04 TypeScript Suficiente\nTypes, patterns, Node.js]
        M5_RE[05-05 React Arquitectura\nState, SSR/CSR, componentes]
    end

    M3_CA --> M5_DN
    M3_CQRS --> M5_DN
    M3_DDD --> M5_DN
    M3_API --> M5_DN
    M4_BD --> M5_AZ
    M4_MQ --> M5_AZ
    M4_SEC --> M5_AZ
    M4_DEP --> M5_AZ

    style M5_DN fill:#1e40af,color:#fff
    style M5_AZ fill:#059669,color:#fff
    style M5_PY fill:#7c3aed,color:#fff
    style M5_TS fill:#b45309,color:#fff
    style M5_RE fill:#dc2626,color:#fff
```

### Las conexiones más importantes

**05-01 es la implementación de Módulo 3 en .NET:**
- Clean Architecture → cómo se estructura una solución ASP.NET Core con capas correctas
- CQRS con MediatR → el pattern que ya conoces del módulo 3, aplicado con la librería que usas en producción
- DDD → cómo los Aggregates mapean a EF Core Entities con el Change Tracker
- API Design → Minimal APIs vs Controllers como decisión de diseño, no de preferencia

**05-02 es la implementación de Módulo 4 en Azure:**
- Message Queues → Azure Service Bus vs Event Hubs vs Event Grid con criterio real
- Databases → Azure SQL vs Cosmos DB con los trade-offs de 04-02 aplicados a la plataforma
- Security → Managed Identity como la implementación del "no secrets en código" de 04-07
- Deployment → CI/CD con Azure DevOps/GitHub Actions y las estrategias de 04-09 en la nube

---

## Sección 4 — Duración estimada y checklist de salida

### Duración estimada

| Archivo | Tiempo de estudio |
|---|---|
| 05-01 — .NET Avanzado | 4-6 horas (el más denso) |
| 05-02 — Azure Primario | 3-4 horas |
| 05-03 — Python Suficiente | 2-3 horas |
| 05-04 — TypeScript Suficiente | 2-3 horas |
| 05-05 — React Arquitectura | 2-3 horas |
| **Total módulo 5** | **13-19 horas** |

El módulo 5 puede solaparse con práctica activa de los módulos anteriores. Mientras estudias los internals de .NET/Azure aquí, puedes estar practicando problemas de algoritmos del módulo 2 o simulando entrevistas de system design del módulo 4.

### Checklist de salida del módulo

Antes de considerar el módulo 5 completo, debes poder:

**Sobre .NET internals:**
- [ ] Explicar cómo funciona el middleware pipeline como cadena de delegados, con orden correcto de registro
- [ ] Identificar un Captive Dependency en código existente y explicar por qué es un bug silencioso
- [ ] Explicar qué genera el compilador cuando escribes `async/await` y qué implica eso para `ConfigureAwait(false)`
- [ ] Diagnosticar un problema de N+1 en EF Core usando las herramientas correctas
- [ ] Explicar cuándo Minimal APIs es la elección correcta sobre Controllers con criterios técnicos
- [ ] Decidir el lifetime correcto (Singleton/Scoped/Transient) para un servicio dado y argumentarlo

**Sobre Azure:**
- [ ] Dado un nuevo workload, trazar el árbol de decisión de compute (Functions vs Container Apps vs AKS) con razonamiento
- [ ] Explicar cuándo Azure Service Bus vs Event Hubs es la elección correcta, con casos concretos
- [ ] Argumentar cuándo Cosmos DB es overengineering y cuándo es la única opción viable
- [ ] Implementar el patrón Managed Identity correctamente sin connection strings con credenciales

**Sobre los demás stacks:**
- [ ] Leer y explicar código Python de nivel intermedio sin asistencia
- [ ] Entender TypeScript generics y decorators suficiente para discutir arquitectura con equipos frontend
- [ ] Explicar la diferencia entre SSR y CSR en React desde una perspectiva de arquitecto

---

## Siguiente paso

Empieza por → [05-01-dotnet-avanzado.md](./05-01-dotnet-avanzado.md)

Es el archivo más denso del módulo y el que más impacto tiene en tu diferenciación como candidato — porque es tu stack primario y los entrevistadores van a profundizar aquí.
