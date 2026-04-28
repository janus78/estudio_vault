# 02_arquitectura_general_cli_first.md

# Multi-AI Router (CLI-First)

## Arquitectura General

------------------------------------------------------------------------

# Tabla de Contenido

1.  Propósito del documento
2.  Resumen ejecutivo de arquitectura
3.  Vista C4 del sistema
4.  Arquitectura lógica por capas
5.  Arquitectura física inicial
6.  Flujo principal: Route Prompt
7.  Diseño CLI-first
8.  Router Engine
9.  Patrones arquitectónicos
10. Persistencia inicial
11. Observabilidad
12. Testing
13. Seguridad
14. Evolución por fases
15. Qué NO hacer todavía
16. ADRs
17. Explicación para entrevistas
18. Resumen final

------------------------------------------------------------------------

## 1. Propósito del documento

Este documento define la arquitectura general del sistema Multi-AI
Router bajo un enfoque CLI-first.

Se basa en: - 00_indice_y\_vision_general.md -
01_contexto_objetivos_y\_alcance.md

Sirve como: - Base arquitectónica - Guía de implementación - Material de
estudio avanzado - Referencia para entrevistas senior

------------------------------------------------------------------------

## 2. Resumen ejecutivo

Arquitectura elegida: - Monolito modular - Clean Architecture ligera -
Vertical Slice en Application - Integración CLI-first

  ---------------------------------------------------------------------------
  Decisión        Elección        Justificación             Trade-off
  --------------- --------------- ------------------------- -----------------
  Arquitectura    Monolito        Simplicidad inicial       Escala limitada
                  modular                                   

  Integración IA  CLI-first       Uso de suscripciones      Complejidad
                                                            técnica

  Organización    VSA + Clean     Balance                   Curva aprendizaje
                                  claridad/extensibilidad   

  Persistencia    SQLite inicial  Ligero y local            Limitado a escala
  ---------------------------------------------------------------------------

------------------------------------------------------------------------

## 3. Vista C4

### Contexto

``` mermaid
flowchart LR
User --> UI
UI --> Router
Router --> ClaudeCLI
Router --> GPTCLI
Router --> GeminiCLI
```

------------------------------------------------------------------------

## 4. Arquitectura por capas

``` mermaid
flowchart TD
UI --> API
API --> Application
Application --> Domain
Application --> Infrastructure
Infrastructure --> ExternalCLI
```

------------------------------------------------------------------------

## 5. Estructura física

src/ Web/ Application/ Domain/ Infrastructure/

tests/ Unit/ Integration/

------------------------------------------------------------------------

## 6. Flujo Route Prompt

``` mermaid
sequenceDiagram
User->>UI: Prompt
UI->>API: Request
API->>App: Command
App->>Router: Decide
Router->>CLI: Execute
CLI-->>Router: Output
Router-->>App: Result
App-->>API: Response
API-->>UI: Show
```

------------------------------------------------------------------------

## 7. Diseño CLI-first

### Modelo ejecución

-   command
-   args
-   stdout
-   stderr
-   timeout

### Riesgos

  Riesgo      Mitigación
  ----------- -------------
  CLI falla   Retry
  Timeout     Cancelación
  Parsing     Tolerancia

------------------------------------------------------------------------

## 8. Router Engine

Responsable de: - Decidir proveedor - Aplicar reglas - Fallback

------------------------------------------------------------------------

## 9. Patrones

-   Strategy
-   Adapter
-   CQRS
-   Decorator

------------------------------------------------------------------------

## 10. Persistencia

Inicial: - SQLite

Entidades: - PromptRequest - Response - Metrics

------------------------------------------------------------------------

## 11. Observabilidad

-   Logs estructurados
-   Métricas básicas

------------------------------------------------------------------------

## 12. Testing

  Tipo           Uso
  -------------- --------
  Unit           lógica
  Integration    CLI
  Architecture   capas

------------------------------------------------------------------------

## 13. Seguridad

-   Sanitización inputs
-   No ejecutar comandos directos

------------------------------------------------------------------------

## 14. Evolución

Fases: 1. MVP 2. Routing 3. Persistencia 4. Métricas

------------------------------------------------------------------------

## 15. Qué NO hacer

-   Microservicios
-   Kubernetes
-   ML avanzado

------------------------------------------------------------------------

## 16. ADRs

-   CLI-first
-   Monolito
-   SQLite

------------------------------------------------------------------------

## 17. Entrevistas

Pitch: Sistema que enruta prompts a distintas IAs usando CLI,
optimizando costo y calidad.

------------------------------------------------------------------------

## 18. Resumen

Arquitectura: - Modular - Extensible - Evolutiva
