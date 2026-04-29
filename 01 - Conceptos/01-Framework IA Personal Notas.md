# 🧠 Framework IA Personal: Notas de Estudio

Notas personales sobre el cambio de mentalidad y estrategias técnicas para maximizar el uso de modelos de lenguaje.

---

## ⚠️ Higiene y Cuidados al usar IA

> [!CAUTION] **Cuidado con el historial largo**
> Evitar muchos intercambios en el mismo contexto o chat. En cada llamada se reprocesa todo el historial, archivos y enfoque. Esto no solo es más caro, sino que **degrada la calidad de la respuesta**.

*   **Evita la improvisación:** No es recomendable llegar con prompts e improvisar para trabajos serios. Evita refinar sobre la marcha; planifica el contexto previamente.
*   **Selección de Modelo:** Decidir el mejor modelo para cada tarea. No gastar modelos potentes (como Opus o Pro) en tareas triviales o revisiones cortas.
*   **Mentalidad Base:** La IA aún no razona como nosotros. El trabajo de estructuración y razonamiento previo debe ser realizado por el usuario.

---

## 🏗️ Ingeniería de Contexto (Context Engineering)

Es la evolución del *prompt engineering*. Se trata de informar lo más posible la ventana del contexto para obtener mejores resultados superiores.

### La Ventana de Contexto
Es el total de lo que un modelo puede "ver" de una vez. Lo que queda fuera, no existe para el modelo. Se consume por:
*   El prompt actual.
*   **Todo el historial del chat** (Crecimiento exponencial ⚠️).
*   **Adjuntos** (Se reprocesan en cada mensaje ⚠️).
*   Instrucciones predefinidas (Project Instructions / Gems).
*   Respuestas anteriores.

### 🛡️ Estrategias Principales (W.S.C.I.)

| Acción | Descripción |
| :--- | :--- |
| **Write** | Escribir correctamente el contexto y las instrucciones. |
| **Select** | Elegir solo lo que es útil y necesario. |
| **Compress** | Migrar datos importantes mediante resúmenes a nuevos chats. |
| **Isolate** | Mantener los contextos aislados y atómicos por tema. |

---

## ✍️ Profundización de Estrategias

### 1. Write (Lo que se escribe)
*   **Información necesaria:** Rol requerido, especificaciones técnicas, enfoque del tema y el problema de fondo.
*   **Restricciones:** Mencionar qué se ha intentado y qué no ha funcionado con detalle.
*   **Qué NO escribir:** Evitar palabras de cortesía o agradecimiento (gastan tokens) y contexto irrelevante.

### 2. Select (Lo que se incluye)
*   Si el historial o los archivos adjuntos ya no aportan valor, **iniciar un chat nuevo**.
*   Si continúas por "comodidad", es mejor generar un resumen de contexto y saltar a una sesión limpia.

---

## 📏 Reglas de Higiene de Contexto

1.  **Chat en blanco:** Iniciar siempre con chat limpio a menos que sea estrictamente necesario arrastrar contexto.
2.  **Regla de los 12:** Al superar 12 intercambios, evaluar migrar a un nuevo chat con un resumen de contexto.
3.  **Gestión de adjuntos:** Eliminar archivos que ya no sean relevantes para evitar su reprocesamiento constante.
4.  **Persistencia:** Usar *Projects* (Claude) o *Gems* (Gemini) para persistencia a largo plazo, no para charlas casuales.
5.  **Contexto útil:** Si detectas ruido, pide un resumen de lo relevante y cámbiate de chat.

---

## 🚀 Prompting de Alto Nivel

El factor que más controlamos como usuarios y que determina la calidad de la salida.

### Los 5 Componentes Esenciales
| Componente | Función |
| :--- | :--- |
| **Rol** | Define la perspectiva y el tono. |
| **Contexto** | Antecedentes y situación actual. |
| **Tarea** | Qué acción específica debe realizar. |
| **Formato** | Cómo debe entregarse la salida (JSON, Markdown, tabla). |
| **Restricciones** | Límites y lo que se debe evitar. |

### Mejores Prácticas
*   **Prioridad:** Lo que aparece al inicio del prompt tiene más relevancia para el modelo.
*   **Especificidad:** A mayor detalle, mayor precisión. Evitar instrucciones abstractas.
*   **Ejemplos (Few-shot):** Dar ejemplos es mejor que dar instrucciones.
*   **Afirmaciones Positivas:** Evitar negaciones ("no hagas x"). Es mejor decir "haz y" o "explica para alguien con cero conocimiento".
*   **Definición de Rol:** Incrementar la calidad de las respuestas encontrando el rol experto adecuado.
