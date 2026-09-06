# 👥 Usabilidad (Usability)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 13 — Usability.
> 

# Escenario General de Usabilidad

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | De dónde viene el estímulo | El usuario final (que puede tener un rol especializado, como administrador de sistema/red) es la fuente principal · También puede ser un evento externo que llega al sistema y al que el usuario reacciona |
| Estímulo | Qué quiere el usuario final | Usar el sistema eficientemente · Aprender a usar el sistema · Minimizar el impacto de errores · Adaptar el sistema · Configurar el sistema |
| Ambiente | Cuándo llega el estímulo al sistema | Siempre en runtime o en tiempo de configuración del sistema |
| Artefactos | Qué parte del sistema se estimula | Una GUI · Una interfaz de línea de comandos · Una interfaz de voz · Una pantalla táctil |
| Respuesta | Cómo debería responder el sistema | Proveer al usuario las funciones que necesita · Anticipar las necesidades del usuario · Dar feedback apropiado al usuario |
| Medida de Respuesta | Cómo se mide la respuesta | Tiempo de tarea · Número de errores · Tiempo de aprendizaje · Ratio tiempo de aprendizaje/tiempo de tarea · Número de tareas completadas · Satisfacción del usuario · Ganancia de conocimiento del usuario · Ratio de operaciones exitosas/totales · Cantidad de tiempo o datos perdidos cuando ocurre un error |

**Ejemplo concreto (del libro):** un usuario descarga una nueva aplicación y la usa productivamente después de solo 2 minutos de experimentación.

# La distinción clave: iniciativa del usuario vs. del sistema

Las tácticas de usabilidad se organizan según quién toma la iniciativa en la interacción. Hay además una conexión fuerte con **Modificabilidad**: el diseño de UI se itera constantemente (diseñar → testear → corregir), así que una arquitectura fácil de modificar hace ese ciclo menos doloroso.

# Tácticas para Usabilidad

## Soportar la Iniciativa del Usuario

- **Cancel**: el sistema debe estar "escuchando" el comando de cancelar, terminar la actividad, liberar recursos usados y avisar a los componentes colaboradores.
- **Undo**: mantener suficiente información de estado (snapshots/checkpoints u operaciones reversibles) para restaurar un estado anterior a pedido del usuario. No todas las operaciones son reversibles (ej. no se puede "des-enviar" un paquete).
- **Pause/resume**: pausar y reanudar una operación larga (ej. una descarga), liberando recursos temporalmente.
- **Aggregate**: agrupar objetos de bajo nivel para aplicarles una operación en conjunto, evitando repetición manual y errores (ej. cambiar la fuente de todos los objetos de una diapositiva a la vez).

## Soportar la Iniciativa del Sistema

Requieren que el sistema mantenga un modelo:

- **Maintain task model**: modelo de qué está intentando hacer el usuario, para dar asistencia contextual (ej. autocompletado predictivo, corrector ortográfico).
- **Maintain user model**: modelo explícito del conocimiento/comportamiento del usuario o clase de usuarios (ej. apps de idiomas que detectan errores recurrentes y refuerzan esos temas); incluye la personalización explícita de UI.
- **Maintain system model**: modelo explícito del propio sistema, usado para dar feedback apropiado (ej. una barra de progreso que predice el tiempo restante).

# Cómo reconocer Usabilidad en un escenario

Aparece cuando el estímulo viene de un **usuario final interactuando con el sistema** (o de un evento al que el usuario debe reaccionar), y lo que importa es la eficiencia, facilidad de aprendizaje, manejo de errores o satisfacción de esa interacción — no el rendimiento técnico interno (eso sería Performance) ni la seguridad del acceso (eso sería Security). Palabras clave: *usuario, feedback, cancelar/deshacer, curva de aprendizaje, satisfacción*.