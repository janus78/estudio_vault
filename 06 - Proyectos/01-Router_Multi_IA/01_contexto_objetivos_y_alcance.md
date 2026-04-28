# 01_contexto_objetivos_y_alcance.md

# Contexto, Objetivos y Alcance  
## Multi-AI Router (CLI-First)

---

## 1. Contexto del problema (análisis profundo)

El ecosistema actual de modelos de inteligencia artificial ha evolucionado hacia un entorno multi-proveedor, donde diferentes modelos presentan capacidades especializadas.

Ya no existe “la mejor IA”, existe la mejor IA para cada contexto.

### Problema real

- Selección manual del modelo
- Falta de abstracción unificada
- Ineficiencia en costo y tiempo
- Fragmentación de contexto

### Problema técnico

- No hay capa estándar de abstracción
- Diferencias entre CLIs
- Acoplamiento a proveedores

### Problema económico

- Uso ineficiente de tokens
- No optimización de suscripciones

### Problema de contexto

- Pérdida de continuidad
- Fragmentación del conocimiento

### Impacto si no se resuelve

- Baja productividad
- Costos innecesarios
- Arquitecturas frágiles

---

## 2. Motivación del proyecto

### Técnica

- Crear capa de abstracción
- Routing inteligente
- Desacoplamiento

### Aprendizaje

- Arquitectura real
- System design
- Integración con herramientas externas

### Profesional

- Preparación entrevistas senior
- Demostrar decisiones reales

---

## 3. Objetivos

### Funcionales

- Recibir prompts
- Analizar
- Seleccionar IA
- Ejecutar CLI
- Retornar respuesta
- Guardar historial

### No funcionales

- Performance aceptable
- Extensibilidad
- Resiliencia
- Mantenibilidad

### Aprendizaje

- Strategy
- CQRS
- Clean Architecture
- Integración CLI

---

## 4. Alcance

### Incluye

- Router básico
- 2+ CLIs
- Clasificación simple
- Reglas de routing
- Ejecución CLI
- Parsing básico
- Persistencia mínima

### No incluye

- ML avanzado
- Microservicios
- Observabilidad avanzada
- Cloud
- UI compleja

---

## 5. Supuestos

- Uso local
- Bajo volumen
- Usuario único
- CLIs configurados

---

## 6. Restricciones

- CLI-only
- Dependencias externas
- Parsing no estructurado

---

## 7. Riesgos

- Fallos CLI → mitigación: timeouts
- Parsing → mitigación: tolerancia
- Latencia → mitigación: selección
- Dependencia externa → fallback

---

## 8. Criterios de éxito

- Router funcional
- Código extensible
- Integración estable

---

## 9. Evolución

- Distribuido
- ML routing
- Workflows complejos
- Cloud

---

## 10. Trade-offs

### CLI vs API

- CLI: simple pero complejo internamente

### Simplicidad vs Escalabilidad

- MVP primero

### Velocidad vs Diseño

- Balanceado
