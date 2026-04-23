- Cambiar el [[modelo mental]] para usar la IA

### Cosas  cuidar al usar la IA
- Evitar muchos intercambios en el mismo contexto o chat ya que en cada llamada se reprocesa todo el historial, archivos, enfoque lo que ademas de salir mas caro puede ir degradando o dañando la calidad del chat.
- No es recomendable llegar con prompts e improvisar sobre todo ya para trabajos serios en un uso casual se puede llevar cuidando el tamaño del contexto pero para le trabajo ya mas serio evitar refinar sobre la marcha o tratar de reducir esto lo mas que se pueda.
- Tratar y practiicar para siempre decidir el mejor modelo para cada tarea por ejemplo para consultas simples, revisiones cortas de texto no es necesario usar los modelos mas potentes como Opus o Pro ya que gastaran mucho en cosas triviales.
- Este es el cambio mental inical aproximarse al trabajo con IA de esta forma, la IA aun no tiene la capacidar de razonar como nosotros asi que tenemos que realizar este trabajo previamente.
### Ingenieria de contexto ([[Context Engineering]])

- Es la evolución del prompt engineering.
- Se trata de informar lo mas posible la ventana del contexto a la IA para obtener mejores resultados
- Tratar de integrar la mayor cantidad de informacion al modelo desde el historial, archivos el mismo [[prompt]] y herramientas extras.
- Acumula el estado en la sesión.
- Y ahora ya lleva el tema de arquitectura de información para alimentar al modelo.

#### Ventana de contexto
 Es el total de lo que un modelo puede ver de una vez.
 Lo que queda fuera de esa ventana no existe para le modelo.
Esta ventana se consume con:
- El prompt actual
- Todo el historial del chat - <font color="#2DC26B">Este es alerta porque crece de forma exponencial</font>
- Adjuntos - <font color="#2DC26B">Estos archivos se reprocesan una y otra vez en cada mensaje.</font>
- Si el proyecto o gema tiene instrucciones predefinidas
- Las respuestas anteriores

### Estrategias de ingeniería de contexto

Se divide principalmente en 4 acciones:
- Write - lo que se escribe
- Select - lo que se usa y es util
- Compress - migrar datos importantes en resumenes de contexto a nuevos chats
- Isolate - Mantener contextos aislados

#### Write

- Escribir correctamente el contexto
	- Tratar de proporcionar al modelo toda la informacion que pueda necesitar en el contexto del prompt para que su respuesta y busquedas sean mas efectivas
	- Por ejemplo el rol requerido o especificaciones tecnicas o enfoque del tema.
	- El problema presentado detras de esas especificaciones.
	- Las restricciones importantes a tomar en cuenta.
	- SI ya se ha intentado algo y no ha funcionado mencionarlo tambien con detalle.
- Que NO escribir en el contexto
	- Usar palabras de cortesia o agradecimiento, esto gasta tokens.
	- Contexto que no sea de utilidad para el enfoque del chat.
	- SI hay historial anterior en el chat que ya no es relevante o el historial ya no sirve del chat actual iniciar uno nuevo.
#### Select - Como seleccionar que se puede incluir

- Si el historial ya no aporta valor iniciar nuevo chat
- Si los archivos adjuntos ya no son relevantes iniciar chat nuevo sin archivos.
- SI solo se continua en el mismo chat por comodidad generar historial de contexto e iniciar nuevo chat.

#### Compress

Se debe tratar siempre de no mexclar temas o conceptos en chats sino que sean atomicos de un tema en particular
se pueden usar projects en claude y codex y gems en gemini

### Reglas y recomendaciones de higiene de contexto

Se debe siempre de hacer el esfuerzo por usar estas reglas hasta que sea natural implementarlas

1. Siempre iniciar con chat en blanco a no ser que sea muy necesario arrastrar contexto
2. Una vez se superen 12 intercambios en un chat evaluar si se lleva. aun nuevo chat con resumen de contexto.
3. Revisar si los archivos adjuntos a un chat ya no son relevantes para eliminarlos y evitar que se reprocesen en cada petición
4. Si se requiere mucha persistencia en un intervalo largo de tiempo usar projests o gems, no usarlos para conversaciones casuales o irrelevantes.
5. Tratar de evitar el contexto inutil, si se detecta que ya hay contexto que no sirve en una conversacion pedir un resumen de contexto solo de lo que es relevante y cambiar a un chat nuevo llevandose ese contexto.

### Prompting y su importancia


- Es determinante para la calidad de la salida.
- Es el factor que como usuarios mas podemos controlar
- Los prompt de alto nivel usualmente tienen estos 5 componentes aunque depende de la necesidad pueden o no usarse todos:
	- Rol - Perspectiva
	- Contexto
	- Tarea a realizar
	- Formato - Salida
	- Restricciones

Los modelos procesan el contexto completo pero genera los tokens secuenciales.

- Lo inicial o primero tiene mas relevancia.
- Entre mas especifico se obtienen respuestas mas precisas.
- Si se le pueden dar ejemplos mejor tratar de evitar o reducir las instrucciones abstractas.
- tratar de evitar las negaciones y usar mejor afirmaciones positivas es mas probable que el modelo entienda esas evitar "no uses", "que no tenga" y mejor usar "explica para alguien que tiene cero conocimiento" o sentencias similares.
- Tratar de encontrar o definir correctamente el rol ya que incrementa mucho la calidad de las respuestas.

