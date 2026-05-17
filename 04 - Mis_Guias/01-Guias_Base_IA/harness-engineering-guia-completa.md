# Harness Engineering: Guía Completa de Cero a Experto

> **El concepto que está redefiniendo cómo los ingenieros construyen con IA en 2026**

---

## Tabla de Contenidos

1. [¿Qué es Harness Engineering?](#1-qué-es-harness-engineering)
2. [La Evolución: De Prompts a Harness](#2-la-evolución-de-prompts-a-harness)
3. [La Analogía del Automóvil](#3-la-analogía-del-automóvil)
4. [Componentes de un Harness](#4-componentes-de-un-harness)
5. [Por qué los Agentes Fallan Sin un Harness](#5-por-qué-los-agentes-fallan-sin-un-harness)
6. [Casos Reales que Validaron el Concepto](#6-casos-reales-que-validaron-el-concepto)
7. [Cómo Construir tu Primer Harness (Nivel Principiante)](#7-cómo-construir-tu-primer-harness-nivel-principiante)
8. [Harness Intermedio: Patrones y Técnicas](#8-harness-intermedio-patrones-y-técnicas)
9. [Harness Avanzado: Sistemas Multi-Agente](#9-harness-avanzado-sistemas-multi-agente)
10. [El Principio de Hashimoto: Ingeniería Iterativa del Harness](#10-el-principio-de-hashimoto-ingeniería-iterativa-del-harness)
11. [Anti-Patrones: Lo que NO debes hacer](#11-anti-patrones-lo-que-no-debes-hacer)
12. [Checklist del Harness Engineer](#12-checklist-del-harness-engineer)
13. [Recursos y Lecturas Recomendadas](#13-recursos-y-lecturas-recomendadas)

---

## 1. ¿Qué es Harness Engineering?

**Harness Engineering** es la disciplina de diseñar el *entorno completo* en el que opera un agente de IA, de modo que pueda realizar trabajo autónomo de manera confiable, reproducible y escalable.

En términos simples:

> **No es lo que le dices al modelo. Es el mundo que construyes para que el modelo viva en él.**

La palabra *harness* viene del inglés y significa "arnés" o "aparejo": el conjunto de correas y mecanismos que permiten a un animal (o máquina) trabajar de forma controlada y dirigida. Aplicado a IA, el harness es toda la infraestructura que rodea al agente: sus herramientas, sus restricciones, su memoria, sus ciclos de retroalimentación y sus mecanismos de recuperación ante fallos.

### Definición formal

OpenAI definió el harness como:

> *"La infraestructura circundante que cierra la brecha entre la inteligencia central de un agente de IA y su capacidad de realizar trabajo productivo."*

Esta infraestructura incluye:
- Estructura del repositorio y del código
- Gestión de estado y memoria
- Integraciones de herramientas (MCP, APIs)
- Bucles de evaluación y retroalimentación
- Mecanismos de recuperación ante errores
- Restricciones y guardrails deterministas

---

## 2. La Evolución: De Prompts a Harness

El campo ha pasado por tres fases claramente diferenciadas:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    EVOLUCIÓN DEL AI ENGINEERING                      │
├──────────────────┬──────────────────────┬───────────────────────────┤
│  FASE 1          │  FASE 2              │  FASE 3                   │
│  Prompt Eng.     │  Context Eng.        │  Harness Eng.             │
│  2022–2024       │  2025                │  2026+                    │
├──────────────────┼──────────────────────┼───────────────────────────┤
│ "¿Qué pregunto?" │ "¿Qué información    │ "¿Qué sistema construyo?" │
│                  │  le doy?"            │                           │
├──────────────────┼──────────────────────┼───────────────────────────┤
│ Una instrucción, │ Ventana de contexto  │ Entorno completo del      │
│ una respuesta    │ cuidadosamente       │ agente con restricciones, │
│                  │ diseñada             │ feedback y herramientas   │
├──────────────────┼──────────────────────┼───────────────────────────┤
│ Few-shot,        │ RAG, MCP, memoria,   │ Linters, CI/CD, agentes   │
│ chain-of-thought │ resúmenes, retrieval │ evaluadores, AGENTS.md,   │
│ role-playing     │                      │ skill files, hooks        │
├──────────────────┼──────────────────────┼───────────────────────────┤
│ Nivel: Mensaje   │ Nivel: Sesión        │ Nivel: Sistema            │
└──────────────────┴──────────────────────┴───────────────────────────┘
```

### ¿Son competidores?

**No.** Son capas anidadas. El harness contiene al contexto, y el contexto contiene al prompt. Mejorar el prompt sin mejorar el contexto tiene límites. Mejorar el contexto sin un harness también tiene límites. La madurez llega cuando las tres capas están bien diseñadas.

```
╔══════════════════════════════════════╗
║         HARNESS (Sistema)            ║
║  ┌────────────────────────────────┐  ║
║  │     CONTEXTO (Sesión)          │  ║
║  │  ┌──────────────────────────┐  │  ║
║  │  │   PROMPT (Mensaje)       │  │  ║
║  │  └──────────────────────────┘  │  ║
║  └────────────────────────────────┘  ║
╚══════════════════════════════════════╝
```

### Hitos clave de la historia

| Fecha | Evento |
|---|---|
| Jun 2025 | Andrej Karpathy populariza "context engineering" |
| Ago 2025 | OpenAI inicia experimento interno con Codex (0 líneas manuales) |
| Feb 5, 2026 | Mitchell Hashimoto publica su blog acuñando "harness engineering" |
| Feb 11, 2026 | OpenAI publica *"Harness engineering: leveraging Codex in an agent-first world"* |
| Mar 2026 | El término se viraliza; múltiples frameworks y guías emergen |

---

## 3. La Analogía del Automóvil

Esta es la analogía más clara para entender las tres capas:

```
┌────────────────────────────────────────────────────────┐
│                   EL AUTOMÓVIL                         │
├──────────────────────────────────────────────────────  │
│  🔧 MOTOR          →   El MODELO (LLM)                 │
│     El corazón. No lo construyes tú.                   │
│     Viene dado por Anthropic / OpenAI.                 │
│                                                        │
│  ⛽ COMBUSTIBLE     →   El CONTEXTO                    │
│     RAG, historial, documentos, herramientas.          │
│     Puedes optimizarlo. Es tu "gasolina premium".      │
│                                                        │
│  🚗 EL AUTO COMPLETO →  El HARNESS                    │
│     Volante, frenos, semáforos, GPS, airbags,          │
│     carril definido, mantenimiento preventivo.         │
│     Sin esto, el motor y el combustible no bastan      │
│     para llegar a destino sin accidentarse.            │
└────────────────────────────────────────────────────────┘
```

> *"Si solo te enfocas en el motor y el combustible, igual puedes construir un auto terrible."*
> — Louis Bouchard

---

## 4. Componentes de un Harness

Un harness maduro se compone de los siguientes elementos:

### 4.1 Archivos de Contexto del Proyecto

Son documentos que el agente lee al iniciar. Los más comunes:

| Archivo | Propósito |
|---|---|
| `AGENTS.md` | Mapa del repositorio, reglas arquitectónicas, convenciones |
| `CLAUDE.md` | Instrucciones específicas para Claude Code |
| `.cursorrules` | Reglas para Cursor AI |
| `SKILL.md` | Procedimientos para tareas recurrentes |

**Principio crítico:** Estos archivos deben ser un **mapa**, no una enciclopedia. Si pones todo en un solo `AGENTS.md` gigante, el agente ignora las partes importantes. El contexto es escaso.

**Estructura recomendada (OpenAI):**
```
docs/
├── AGENTS.md              ← índice + reglas globales
├── design-docs/
│   ├── index.md
│   └── arquitectura-core.md
├── exec-plans/
│   └── tech-debt-tracker.md
├── product-specs/
└── references/
    └── design-system-reference.txt
```

### 4.2 Servidores MCP (Model Context Protocol)

MCP es el "USB" de la IA: un protocolo estándar para que los agentes se conecten a herramientas externas (bases de datos, APIs, sistemas de tickets, navegadores, etc.).

```bash
# Ejemplo: añadir MCP servers en Claude Code
claude mcp add --transport http jira https://mcp.jira.example.com/mcp
claude mcp add --transport stdio github -- npx -y @modelcontextprotocol/server-github
```

**Regla de oro:** Más MCP servers ≠ mejor. Cada definición de herramienta consume tokens. Conecta solo lo que la tarea actual requiere.

### 4.3 Skill Files

Documentan procedimientos recurrentes para que el agente no tenga que "reinventar la rueda" cada vez.

Ejemplo de `SKILL.md` para code review:
```markdown
# Skill: Code Review

## Cuándo usar este skill
Cuando se te pida revisar un Pull Request.

## Pasos
1. Lee el diff completo
2. Verifica que los tests cubran los nuevos cambios
3. Comprueba que no se violan las reglas de arquitectura en /docs/design-docs/
4. Genera comentarios en formato GitHub markdown
5. Al finalizar, actualiza este skill si encontraste un patrón nuevo
```

### 4.4 Enforcement Mecánico (Linters y Tests)

Esta es la parte que separa más claramente harness engineering de context engineering:

> **No le pidas al agente que siga una regla. Construye un sistema que haga imposible romperla.**

```
Prompt Engineering:  "No uses variables globales."
Context Engineering: [Incluye ejemplo de código sin variables globales]
Harness Engineering: CI falla si detecta variables globales. El agente no puede mergear sin pasar el lint.
```

Herramientas comunes:
- ESLint / Pylint / Rubocop para reglas de código
- Tests de arquitectura (e.g., ArchUnit, dependency-cruiser)
- Custom linters para convenciones del equipo
- Pre-commit hooks

### 4.5 Bucles de Retroalimentación (Feedback Loops)

Los agentes aprenden de su entorno a través del feedback. Un harness bien diseñado hace que ese feedback sea inmediato, específico y accionable.

```
┌──────────────┐    output     ┌──────────────┐
│   AGENTE     │──────────────►│  EVALUADOR   │
│ (Generator)  │               │  (Checker)   │
└──────────────┘◄──────────────└──────────────┘
                   feedback
                (pasa / falla + razón)
```

**Insight de Anthropic:** Los modelos **no pueden evaluar su propio trabajo** de forma confiable. Siempre expresarán confianza incluso cuando el output está roto. Por eso, el evaluador debe ser un agente separado o un sistema determinista (tests, linters).

### 4.6 Gestión de Estado y Memoria

Como los agentes no tienen memoria entre sesiones, el harness externaliza el estado:

| Mecanismo | Qué almacena |
|---|---|
| Git commits | Progreso del código |
| Progress log (JSON) | Estado de tareas en formato estructurado |
| Feature tracking file | Qué features están completas/pendientes |
| Init script | Permite a un agente nuevo orientarse instantáneamente |

**Por qué JSON y no Markdown para logs estructurados:** Los agentes son menos propensos a sobreescribir datos estructurados. Un JSON de estado es más robusto que un Markdown narrativo.

### 4.7 Permisos y Guardrails

Define qué puede y qué no puede hacer el agente:

```
┌─────────────────────────────────────────────┐
│              MODELO DE PERMISOS             │
├────────────────────┬────────────────────────┤
│   PERMITIDO        │   BLOQUEADO             │
├────────────────────┼────────────────────────┤
│ Leer archivos      │ Borrar archivos         │
│ Ejecutar tests     │ Hacer push a main       │
│ Crear branches     │ Acceder a credenciales  │
│ Hacer PRs          │ Ejecutar SQL en prod    │
└────────────────────┴────────────────────────┘
```

### Resumen visual de los componentes

```
╔═══════════════════════════════════════════════════════════╗
║                    EL HARNESS                             ║
║                                                           ║
║  📄 Archivos de contexto   🔧 MCP Servers                 ║
║     (AGENTS.md, skills)       (herramientas externas)     ║
║                                                           ║
║  ✅ Enforcement mecánico   🔄 Feedback loops              ║
║     (linters, tests, CI)      (evaluador separado)        ║
║                                                           ║
║  💾 Estado externo         🔒 Permisos y guardrails       ║
║     (JSON, git, logs)         (qué puede/no puede hacer)  ║
║                                                           ║
║           ┌─────────────────────────┐                    ║
║           │   CONTEXTO (RAG, etc.)  │                    ║
║           │   ┌─────────────────┐   │                    ║
║           │   │    PROMPT       │   │                    ║
║           │   └─────────────────┘   │                    ║
║           └─────────────────────────┘                    ║
╚═══════════════════════════════════════════════════════════╝
```

---

## 5. Por qué los Agentes Fallan Sin un Harness

Cuando un agente potente opera sin harness, aparecen patrones de fallo predecibles:

| Fallo | Descripción | Causa raíz |
|---|---|---|
| **Feature hallucination** | El agente declara una tarea completa sin haberla implementado realmente | Sin evaluador externo |
| **Plot drift** | En tareas largas, el agente "pierde el hilo" y empieza a hacer algo diferente | Sin estado externo |
| **Local fixes that break architecture** | Arregla un bug rompiendo límites arquitectónicos | Sin enforcement de arquitectura |
| **Infinite loops** | Se queda atascado en micro-edits sin salir | Sin criterio de parada |
| **Convention violations** | Ignora las convenciones del equipo | Sin reglas en archivos de contexto |
| **Parallel collisions** | Dos agentes modifican los mismos archivos | Sin orquestación |

---

## 6. Casos Reales que Validaron el Concepto

### Caso 1: OpenAI Codex — 1 Millón de Líneas, 0 Manuales

**Período:** Agosto 2025 – Enero 2026
**Equipo:** 3 → 7 ingenieros
**Resultado:**

```
📊 MÉTRICAS DEL EXPERIMENTO OPENAI
─────────────────────────────────────
Líneas de código generadas:  ~1,000,000
Líneas escritas manualmente:  0
Pull Requests mergeados:      ~1,500
PRs promedio por día:         3.5 por ingeniero
Velocidad estimada:           ~10x vs desarrollo manual
```

**El secreto no fue el modelo. Fue el harness:**
- Repositorio = única fuente de verdad
- Código legible por agentes (comentarios verbose, estructura consistente)
- Restricciones arquitectónicas aplicadas por linters, no prompts
- Autonomía otorgada incrementalmente
- Si un PR requería intervención humana significativa, el problema era el harness, no el modelo

**Lección clave (Ryan Lopopolo, lead engineer):**
> *"Los agentes no son difíciles. El Harness es difícil."*

### Caso 2: Stripe — 1,300 PRs por semana sin supervisión humana

El sistema interno "Minions" de Stripe usa una orquestación "Blueprint" que separa:
- **Nodos deterministas:** ejecutar linter, hacer commit, correr CI
- **Nodos agénticos:** implementar feature, corregir error de CI

**Regla de dos strikes:** Si el primer fix del agente falla en CI, la tarea escala inmediatamente a un humano. No se permiten ciclos infinitos de retry. Esta es una restricción clásica de harness.

### Caso 3: Anthropic — Agentes de Larga Duración

El equipo de Anthropic documentó el problema de agentes trabajando en shifts (sin memoria entre sesiones) y su solución:

- Logs de progreso estructurados en JSON (no Markdown)
- Feature tracking files
- Init script para orientar a un agente nuevo al instante
- Separación de roles: agente inicializador vs. agente codificador

---

## 7. Cómo Construir tu Primer Harness (Nivel Principiante)

### Paso 1: Define el contexto del proyecto

Crea un `AGENTS.md` en la raíz de tu proyecto:

```markdown
# AGENTS.md

## Descripción del Proyecto
[Nombre del proyecto]: API REST para gestión de inventario
Stack: Node.js + TypeScript + PostgreSQL

## Estructura del Repositorio
src/
├── controllers/    ← Lógica de rutas HTTP
├── services/       ← Lógica de negocio
├── repositories/   ← Acceso a datos
└── models/         ← Tipos e interfaces

## Reglas de Arquitectura
- Los controllers NO acceden a la BD directamente (siempre via services)
- Cada función debe tener un test unitario en /tests/unit/
- Los nombres de variables siguen camelCase
- Las interfaces se definen en /models/

## Cómo correr los tests
npm test

## Cómo hacer lint
npm run lint
```

### Paso 2: Establece enforcement básico

```bash
# Instala ESLint con reglas estrictas
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin

# Crea .eslintrc.json
{
  "rules": {
    "no-var": "error",
    "@typescript-eslint/no-explicit-any": "error"
  }
}

# Añade al package.json
"scripts": {
  "lint": "eslint src/**/*.ts --max-warnings 0",
  "test": "jest"
}
```

### Paso 3: Dale al agente un ciclo de verificación

En lugar de pedirle al agente que "implemente X", pídele:

```
❌ Malo (sin harness):
"Implementa la función de login"

✅ Bueno (con harness):
"Implementa la función de login. 
Después de implementar:
1. Corre npm run lint y corrige cualquier error
2. Corre npm test y verifica que todos los tests pasan
3. Si algún test falla, corrígelo antes de reportar como completo"
```

### Paso 4: Externaliza el estado

Si el trabajo toma múltiples sesiones, crea un archivo de estado:

```json
// progress.json
{
  "sprint": "2026-W19",
  "features": {
    "user-auth": "complete",
    "product-catalog": "in-progress",
    "checkout-flow": "pending"
  },
  "current_task": "Implementar endpoint GET /products",
  "blockers": [],
  "last_updated": "2026-05-10T14:30:00Z"
}
```

---

## 8. Harness Intermedio: Patrones y Técnicas

### Patrón 1: El Mapa de Contexto Jerárquico

No pongas todo en un solo `AGENTS.md`. Usa una estructura jerárquica donde el agente carga solo las instrucciones relevantes a su directorio actual:

```
proyecto/
├── AGENTS.md                    ← reglas globales (corto, denso)
├── src/
│   ├── AGENTS.md                ← reglas del módulo src
│   ├── auth/
│   │   └── AGENTS.md            ← reglas específicas de auth
│   └── payments/
│       └── AGENTS.md            ← reglas específicas de payments
└── docs/
    ├── architecture.md
    └── decisions/
        └── 001-use-postgres.md
```

### Patrón 2: Generator + Evaluator

Inspirado en GANs (Generative Adversarial Networks):

```
┌─────────────────────────────────────────────────────────┐
│                PATRÓN GENERATOR/EVALUATOR               │
│                                                         │
│  GENERATOR AGENT              EVALUATOR AGENT           │
│  ─────────────                ───────────────           │
│  - Escribe código             - NO escribe código       │
│  - Implementa features        - Ejecuta tests           │
│  - Crea interfaces            - Interactúa con la UI    │
│  - Genera PRs                 - Verifica API responses  │
│                               - Revisa BD               │
│                               - Reporta fallos          │
│                                                         │
│  Insight de Anthropic: Es más fácil hacer un            │
│  evaluador estricto que enseñarle autocrítica           │
│  a un generador.                                        │
└─────────────────────────────────────────────────────────┘
```

Ejemplo de prompt para el evaluador:
```
Eres un QA engineer implacable. Tu trabajo es encontrar fallos,
no confirmar que todo está bien. Para evaluar el trabajo del
generator:

1. Lee el código generado
2. Ejecuta los tests: npm test
3. Si hay UI, usa Playwright para interactuar con ella
4. Verifica que los endpoints retornen los status codes correctos
5. Comprueba que la BD tenga los registros esperados

Reporta ÚNICAMENTE en este formato:
STATUS: PASS | FAIL
ISSUES: [lista de problemas encontrados, vacía si PASS]
NEXT_ACTION: [qué debe hacer el generator para corregir]
```

### Patrón 3: Autonomía Incremental

No le des acceso total al agente desde el inicio. Otorga permisos progresivamente:

```
NIVEL 0: Solo puede leer archivos
    ↓
NIVEL 1: Puede crear archivos en /tmp
    ↓
NIVEL 2: Puede crear/editar en branches de feature
    ↓
NIVEL 3: Puede crear PRs (pero no mergear)
    ↓
NIVEL 4: Puede mergear PRs que pasan CI + revisión
    ↓
NIVEL 5: Puede mergear PRs que pasan CI (autonomía total en ciertos módulos)
```

### Patrón 4: Skill Files para Procedimientos Recurrentes

```markdown
# SKILL.md — Deploy a Staging

## Cuándo usar
Cuando se pida desplegar cambios a staging.

## Pre-requisitos
- Todos los tests pasan: `npm test`
- Lint sin errores: `npm run lint`
- PR aprobado o en rama feature

## Pasos
1. `git pull origin main`
2. `npm run build`
3. `npm run test:integration`
4. `docker build -t app:staging .`
5. `kubectl apply -f k8s/staging/`
6. `kubectl rollout status deployment/app -n staging`
7. Verifica health check: `curl https://staging.app.com/health`

## Si algo falla
- Paso 6 falla: revisa logs con `kubectl logs -n staging`
- Paso 7 falla: rollback con `kubectl rollout undo deployment/app -n staging`

## Al finalizar
Actualiza este skill si encontraste un paso que faltaba.
```

---

## 9. Harness Avanzado: Sistemas Multi-Agente

### Arquitectura de Orquestación

```
┌─────────────────────────────────────────────────────────────────┐
│                    ORQUESTADOR (Mission Control)                │
│                                                                 │
│  Decide qué agentes corren, en qué orden, y cuándo intervenir  │
└──────┬──────────────┬──────────────┬──────────────┬────────────┘
       │              │              │              │
       ▼              ▼              ▼              ▼
  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
  │ AGENTE  │   │ AGENTE  │   │ AGENTE  │   │ AGENTE  │
  │ Feature │   │ Tests   │   │ Docs    │   │ Cleanup │
  │ (Gen.)  │   │ (Eval.) │   │         │   │ (BG)    │
  └─────────┘   └─────────┘   └─────────┘   └─────────┘
```

**Tipos de agentes en un sistema maduro:**

| Tipo | Rol | Cuándo corre |
|---|---|---|
| **Generator** | Implementa features | Cuando hay tarea asignada |
| **Evaluator** | Verifica el output | Después de cada PR |
| **Orchestrator** | Coordina el flujo | Siempre (meta-agente) |
| **Cleanup** | Elimina deuda técnica | Background, periodic |
| **Init** | Onboarding de nueva sesión | Al inicio de cada sesión |

### Ejecución Paralela vs. Secuencial

```
PARALELO (cuando las tareas son independientes):
──────────────────────────────────────────────
  Agente A: feature-auth ──────────────────►
  Agente B: feature-catalog ───────────────►
  Agente C: bugfix-payments ───────────────►
                                    ↓
                          Merge / conflict resolution

SECUENCIAL (cuando las tareas tienen dependencias):
────────────────────────────────────────────────────
  Agente A: feature-auth ──► Agente B: feature-profile ──► Agente C: tests
  (completa primero)         (depende de auth)              (verifica todo)
```

### Regla de los dos strikes (Stripe)

```
Intento 1 del agente ──► CI pasa? ──► SÍ ──► Merge ✅
                              │
                              ▼ NO
                         Agente intenta fix
                              │
                              ▼
Intento 2 del agente ──► CI pasa? ──► SÍ ──► Merge ✅
                              │
                              ▼ NO
                    🚨 ESCALACIÓN A HUMANO
```

### Init Script: Memoria Inter-Sesión

Este es uno de los componentes más críticos para agentes de larga duración:

```bash
#!/bin/bash
# init_agent.sh — ejecutar al inicio de cada sesión de agente

echo "=== ORIENTACIÓN DEL AGENTE ==="
echo ""
echo "1. ESTADO ACTUAL DEL PROYECTO:"
cat progress.json | jq '.features'

echo ""
echo "2. TAREA ACTUAL:"
cat progress.json | jq '.current_task'

echo ""
echo "3. ÚLTIMOS COMMITS:"
git log --oneline -10

echo ""
echo "4. TESTS ACTUALES:"
npm test --silent 2>&1 | tail -5

echo ""
echo "5. REGLAS DEL PROYECTO:"
head -50 AGENTS.md

echo ""
echo "=== FIN DE ORIENTACIÓN. LISTO PARA TRABAJAR. ==="
```

---

## 10. El Principio de Hashimoto: Ingeniería Iterativa del Harness

Mitchell Hashimoto, creador de Terraform, formuló el principio central del harness engineering:

> **"Cada vez que descubres que un agente cometió un error, tómate el tiempo para ingeniar una solución de modo que nunca pueda cometer ese mismo error otra vez."**

Este principio cambia el mindset radicalmente:

```
ANTES (mentalidad de prompts):
"Este modelo es tonto" → esperar mejor modelo → seguir igual

DESPUÉS (mentalidad de harness):
"Mi sistema permitió este modo de fallo" 
    → identificar la causa raíz
    → agregar linter / test / restricción / regla
    → el agente no puede volver a cometer ese error
    → el sistema mejora permanentemente
```

### Ciclo de mejora continua del harness

```
    ┌─────────────────────────────────────────────┐
    │                                             │
    ▼                                             │
Agente comete error                               │
    │                                             │
    ▼                                             │
Identificar causa raíz                            │
    │                                             │
    ▼                                             │
¿Se puede prevenir mecánicamente?                 │
    │                                             │
    ├──► SÍ ──► Añadir linter/test/hook ──────────┤
    │                                             │
    └──► NO ──► Mejorar AGENTS.md / skill ────────┤
                                                  │
              Harness es ahora más fuerte ────────┘
```

### Ejemplo práctico del principio

**Error del agente:** Creó una función que accede directamente a la BD desde un controller (violando la arquitectura).

**Respuesta según el principio de Hashimoto:**

```javascript
// Añadir al .eslintrc.json una regla custom:
{
  "rules": {
    "no-restricted-imports": ["error", {
      "paths": [{
        "name": "pg",
        "message": "Los controllers no pueden importar 'pg' directamente. Usa el service layer."
      }]
    }]
  }
}
```

Ahora el CI falla si un agente (o humano) comete el mismo error. El harness aprendió.

---

## 11. Anti-Patrones: Lo que NO debes hacer

### Anti-patrón 1: El AGENTS.md Enciclopédico

```
❌ MALO:
AGENTS.md con 500 líneas cubriendo cada detalle imaginable.

Por qué falla:
- El agente no puede priorizar qué reglas son críticas
- El contexto window se llena de instrucciones irrelevantes
- Las reglas importantes quedan enterradas

✅ BUENO:
AGENTS.md corto (50-100 líneas) con reglas críticas.
Reglas específicas de módulo en AGENTS.md locales.
Referencias a docs/ para detalles.
```

### Anti-patrón 2: Depender Solo de Prompts para Enforcement

```
❌ MALO:
"Por favor, siempre escribe tests antes de implementar."

Por qué falla:
- El agente puede "olvidar" bajo presión de tokens
- No hay verificación objetiva
- No se puede auditar

✅ BUENO:
CI que falla si el coverage baja del 80%.
Pre-commit hook que corre los tests.
PR template que requiere campo "Tests añadidos: [sí/no]".
```

### Anti-patrón 3: Dar Demasiados Herramientas MCP

```
❌ MALO:
Conectar 15 MCP servers "por si acaso".

Por qué falla:
- Cada definición de tool consume tokens
- El agente puede usar herramientas equivocadas
- Contexto contaminado con capacidades irrelevantes

✅ BUENO:
Conectar solo los MCP servers necesarios para la tarea actual.
Usar perfiles de herramientas según el tipo de trabajo.
```

### Anti-patrón 4: Sin Evaluador Externo

```
❌ MALO:
"¿Terminaste la feature? ¿Está todo bien?"
Agente: "Sí, todo está correcto y funcionando."

Por qué falla:
- Los agentes no pueden evaluar su propio trabajo confiablemente
- Siempre expresan confianza, incluso si el código está roto

✅ BUENO:
Tests automáticos que verifican el comportamiento
Agente evaluador separado que interactúa con el output
CI/CD que da feedback objetivo (pasa/falla)
```

### Anti-patrón 5: Harness de Una Sola Vez

```
❌ MALO:
Crear AGENTS.md al inicio del proyecto y nunca actualizarlo.

Por qué falla:
- El código evoluciona, las reglas se vuelven obsoletas
- Los errores nuevos no se capturan en el harness
- Los skill files se vuelven incorrectos

✅ BUENO:
Actualizar AGENTS.md cuando cambia la arquitectura.
Añadir reglas al harness cada vez que un agente comete un error.
Revisar y limpiar skill files periódicamente.
```

---

## 12. Checklist del Harness Engineer

Usa esta lista para evaluar la madurez de tu harness:

### Nivel Básico (0-3 meses)
- [ ] `AGENTS.md` existe y cubre la estructura del proyecto
- [ ] El agente tiene un ciclo de verificación (lint + test después de cada cambio)
- [ ] El estado de las tareas está externalizado (progress.json o similar)
- [ ] El agente sabe qué herramientas tiene disponibles

### Nivel Intermedio (3-6 meses)
- [ ] Estructura jerárquica de archivos de contexto (global + módulo-específico)
- [ ] Enforcement mecánico de al menos 3 reglas arquitectónicas clave
- [ ] Evaluador separado del generator
- [ ] Autonomía incremental definida por niveles
- [ ] Skill files para los 5 procedimientos más frecuentes
- [ ] Init script para orientación al inicio de sesión

### Nivel Avanzado (6+ meses)
- [ ] Orquestador multi-agente
- [ ] Estrategia de ejecución paralela vs. secuencial definida
- [ ] Regla de strikes para escalación humana
- [ ] Agente de cleanup en background
- [ ] Retrospectiva del harness (el harness mejora después de cada error del agente)
- [ ] Métricas del harness: tasa de éxito de PRs, tiempo promedio de tarea, errores por tipo

### Preguntas de Madurez

| Pregunta | Si respuesta es NO → Acción |
|---|---|
| ¿Puede el agente orientarse sin tu ayuda en una sesión nueva? | Crear init script |
| ¿Hay errores que el agente repite? | Añadir enforcement mecánico |
| ¿El agente puede verificar su propio output? | Añadir evaluador externo |
| ¿Las reglas de arquitectura se aplican automáticamente? | Añadir linters de arquitectura |
| ¿Sabes qué tipos de errores comete el agente más? | Instrumentar y medir |

---

## 13. Recursos y Lecturas Recomendadas

### Artículos Fundacionales

| Recurso | Autor | Fecha | Por qué leerlo |
|---|---|---|---|
| *"My AI Adoption Journey"* | Mitchell Hashimoto | Feb 2026 | Acuñó el término y el principio central |
| *"Harness Engineering: Leveraging Codex in an Agent-First World"* | OpenAI | Feb 2026 | El caso práctico con 1M líneas de código |
| *"Building Effective Agents"* | Anthropic | Dic 2025 | Framework de Anthropic para agentes confiables |
| *"Context Engineering"* | Andrej Karpathy | Jun 2025 | La base sobre la que se construye el harness |

### Repositorios y Herramientas

| Recurso | Descripción |
|---|---|
| `awesome-harness-engineering` (GitHub) | Lista curada de herramientas, patrones y casos de uso |
| Claude Code | CLI de Anthropic con soporte nativo para CLAUDE.md y skills |
| Cursor | Editor con `.cursorrules` para enforcement en el editor |
| LangChain / LangGraph | Frameworks para orquestación multi-agente |
| MCP SDK (Anthropic) | Para construir servidores MCP personalizados |

### Conceptos Relacionados para Profundizar

- **Context Engineering** — La capa que vive dentro del harness
- **Agentic RAG** — Retrieval adaptativo para el contexto del agente
- **RLHF y evaluación de agentes** — Cómo entrenar evaluadores más estrictos
- **Agent Observability** — Logging y trazabilidad de comportamiento agéntico
- **Multi-agent coordination** — Estrategias de sincronización y conflicto

---

## Conclusión

Harness engineering no es el reemplazo de prompt engineering ni de context engineering. Es la capa superior que los contiene a ambos y que cierra la brecha entre "el agente puede hacer esto" y "el agente hace esto de forma confiable y repetible en producción".

El mensaje central es claro:

> **Los agentes no son difíciles. El Harness es difícil.**

Y esa dificultad es donde ahora vive el valor real del ingeniero de software: no en escribir código línea por línea, sino en diseñar los entornos que permiten a los agentes escribirlo bien.

---

*Guía compilada el 10 de mayo de 2026. Basada en investigación de OpenAI, Anthropic, Mitchell Hashimoto, Louis Bouchard, Epsilla, Data Science Dojo y la comunidad de awesome-harness-engineering.*
