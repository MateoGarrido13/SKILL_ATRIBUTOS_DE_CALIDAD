# 🟢 Disponibilidad (Availability)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 4 — Availability.
> 

# Escenario General de Disponibilidad

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | De dónde proviene la falla | Interna/externa: personas, hardware, software, infraestructura física, entorno físico |
| Estímulo | El estímulo de un escenario de disponibilidad es una falla (*fault*) | Falla: omisión, caída (crash), timing incorrecto, respuesta incorrecta |
| Artefacto | Qué partes del sistema son responsables de, o se ven afectadas por, la falla | Procesadores, canales de comunicación, almacenamiento, procesos, artefactos afectados en el entorno del sistema |
| Ambiente | El estado del sistema cuando ocurre el estímulo (no solo operación "normal") | Operación normal, arranque, apagado, modo reparación, operación degradada, operación sobrecargada |
| Respuesta | La respuesta más común es evitar que la falla se convierta en fallo (*failure*), pero también puede implicar notificar o registrar | Prevenir que la falla se vuelva fallo · Detectar la falla: registrarla, notificar a entidades apropiadas, recuperarse de ella, deshabilitar la fuente del evento, quedar temporalmente no disponible durante la reparación, enmascarar/reparar el fallo o contener el daño, operar en modo degradado |
| Medida de Respuesta | Distintas métricas según la criticidad del servicio | Tiempo/intervalo en que el sistema debe estar disponible · Porcentaje de disponibilidad (ej. 99.999%) · Tiempo de detección de la falla · Tiempo de reparación · Tiempo/intervalo en modo degradado · Proporción o tasa de fallas que el sistema previene o maneja sin fallar |

**Ejemplo concreto (del libro):** un servidor de una granja de servidores falla durante la operación normal; el sistema informa al operador y continúa operando sin tiempo de inactividad.

# Tácticas para Disponibilidad

Las tácticas se agrupan en tres categorías según su propósito: **detectar fallas**, **recuperarse de fallas** y **prevenir fallas**.

## Detectar Fallas

- **Monitor**: componente que vigila el estado de salud de otras partes del sistema (procesadores, procesos, I/O, memoria); puede orquestar otras tácticas de detección.
- **Ping/echo**: par de mensajes petición/respuesta asíncronos entre nodos para verificar alcanzabilidad y latencia; requiere un umbral de tiempo (timeout).
- **Heartbeat**: intercambio periódico de mensajes entre un monitor y el proceso monitoreado; a diferencia de ping/echo, la iniciativa la tiene el proceso monitoreado (o el monitor, según la variante).
- **Timestamp**: detecta secuencias incorrectas de eventos en sistemas distribuidos de paso de mensajes.
- **Condition monitoring**: chequea condiciones o valida supuestos de diseño (ej. checksums); el monitor debe ser simple para no introducir nuevos errores.
- **Sanity checking**: verifica la validez/razonabilidad de una operación o salida, típicamente en interfaces.
- **Voting**: compara resultados de múltiples fuentes redundantes y decide cuál usar. Variantes: **replicación** (clones idénticos, protege solo contra fallas aleatorias de hardware), **redundancia funcional** (implementaciones diversas, protege contra fallas de modo común) y **redundancia analítica** (diversidad también en especificación de entradas/salidas, tolera errores de especificación).
- **Exception detection**: detecta condiciones que alteran el flujo normal (excepciones de sistema, parameter fence, parameter typing, timeout).
- **Self-test**: un componente o subsistema se testea a sí mismo.

## Recuperarse de Fallas — Preparación y Reparación

- **Redundant spare**: uno o más componentes duplicados toman el control si falla el primario (hot/warm/cold spare).
- **Rollback**: revertir a un estado bueno conocido anterior (checkpoint) tras detectar una falla.
- **Exception handling**: manejar la excepción detectada (desde códigos de error simples hasta clases de excepción con info de correlación).
- **Software upgrade**: actualizar código en servicio sin afectarlo (function patch, class patch, hitless ISSU).
- **Retry**: reintentar una operación asumiendo que la falla es transitoria (con límite de reintentos).
- **Ignore faulty behavior**: ignorar mensajes de una fuente que se determina espuria.
- **Graceful degradation**: mantener las funciones más críticas y descartar las menos críticas ante fallas de componentes.
- **Reconfiguration**: reasignar responsabilidades a los recursos que siguen funcionando.

## Recuperarse de Fallas — Reintroducción

- **Shadow**: operar un componente recién reparado/actualizado en modo "sombra" antes de devolverlo a rol activo.
- **State resynchronization**: sincronizar el estado entre componente activo y de respaldo (vía checksum o hash).
- **Escalating restart**: variar la granularidad del reinicio (de hilos afectados hasta reinicio completo) minimizando el impacto en el servicio.
- **Nonstop forwarding**: separar plano de control y plano de datos (típico en routers) para seguir operando mientras se recupera el control.

## Prevenir Fallas

- **Removal from service**: sacar temporalmente un componente de servicio para "limpiarlo" antes de que acumule fallas (software rejuvenation).
- **Transactions**: semántica ACID para mensajes asíncronos entre componentes distribuidos (ej. two-phase commit); evita condiciones de carrera.
- **Predictive model**: monitorea el estado de salud para predecir fallas y actuar preventivamente.
- **Exception prevention**: técnicas como wrappers, smart pointers o código de corrección de errores para evitar excepciones.
- **Increase competence set**: diseñar un componente para manejar más casos/fallas como parte de su operación normal (en vez de lanzar excepción y "tirar la toalla").

# Patrones para Disponibilidad

- **Active redundancy (hot spare) / Passive redundancy (warm spare) / Spare (cold spare)**: variantes de redundant spare según qué tan sincronizado está el backup con el activo (más sincronizado = recuperación más rápida pero más costoso).
- **Triple modular redundancy (TMR)**: 3 componentes redundantes + lógica de votación; simple y eficaz, punto óptimo costo/disponibilidad.
- **Circuit breaker**: evita reintentos infinitos ante una falla persistente, cortando el ciclo hasta que se "resetea"; previene fallas en cascada en sistemas distribuidos.
- **Process pairs**: checkpointing + rollback, el backup toma el control al fallar el primario.
- **Forward error recovery**: avanzar hacia un estado seguro (posiblemente degradado) en vez de retroceder, usando redundancia de datos.

# Cómo reconocer Disponibilidad en un escenario

Aparece cuando el estímulo es una **falla** (de hardware, software, comunicación o entorno) y lo que importa es si el sistema sigue prestando servicio, cuánto tiempo está caído, y qué tan rápido detecta/repara. Palabras clave: *falla, caída, uptime/downtime, tiempo de reparación, redundancia, failover, recuperación*.