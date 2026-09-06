# ⛑️ Safety (Seguridad Operacional)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 10 — Safety. En español suele traducirse como "seguridad operacional" o "seguridad física/funcional", para no confundirla con Security (Cap. 11, seguridad informática).
> 

# Escenario General de Safety

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | Una fuente de datos (sensor, componente de software que calcula un valor, canal de comunicación), una fuente de tiempo (reloj), o una acción de usuario | Instancias específicas de: sensor, componente de software, canal de comunicación, dispositivo (ej. reloj) |
| Estímulo | Una omisión, comisión, u ocurrencia de datos o timing incorrectos | Omisión: un valor nunca llega / una función nunca se ejecuta · Comisión: una función se ejecuta mal / un dispositivo produce un evento o dato espúreo · Datos incorrectos: un sensor o componente reporta mal · Falla de timing: datos llegan tarde/temprano, un evento ocurre en el orden equivocado |
| Ambiente | El modo de operación del sistema | Operación normal · Operación degradada · Operación manual · Modo de recuperación |
| Artefactos | Alguna parte del sistema | Porciones críticas para la seguridad del sistema |
| Respuesta | El sistema no abandona el espacio de estados seguro, o vuelve a él, o continúa en modo degradado para prevenir o minimizar daño/lesión. Se avisa a los usuarios y se registra el evento | Reconocer el estado inseguro y: evitarlo · recuperarse · continuar en modo degradado/seguro · apagarse · pasar a operación manual · conmutar a un sistema de respaldo · notificar a entidades apropiadas · registrar el estado inseguro (y la respuesta dada) |
| Medida de Respuesta | Tiempo de vuelta al estado seguro; daño o lesión causados | Cantidad/% de entradas a estados inseguros evitadas · Cantidad/% de estados inseguros de los que el sistema se recupera automáticamente · Cambio en la exposición al riesgo: tamaño(pérdida) × prob(pérdida) · % de tiempo en que el sistema puede recuperarse · Tiempo en modo degradado/seguro · Cantidad/% de tiempo apagado · Tiempo transcurrido para entrar y salir de un modo manual/degradado |

**Ejemplo concreto (del libro):** un sensor de un sistema de monitoreo de pacientes falla en reportar un valor crítico después de 100 ms; se registra la falla, se enciende una luz de advertencia en la consola y se activa un sensor de respaldo (menor fidelidad); el sistema monitorea al paciente con el sensor de respaldo en no más de 300 ms.

# Tácticas para Safety

Se agrupan en **evitar estado inseguro**, **detectar estado inseguro**, **contener** y **recuperar**.

## Evitar Estado Inseguro

- **Substitution**: usar mecanismos de protección (típicamente hardware: watchdogs, monitores, interlocks) en lugar de sus versiones en software, que pueden quedarse sin recursos.
- **Predictive model**: predice el estado de salud del sistema para advertir tempranamente de un problema potencial (ej. control de crucero que calcula la tasa de acercamiento a un obstáculo).

## Detectar Estado Inseguro

- **Timeout**: detecta que un componente no cumplió sus restricciones de timing.
- **Timestamp**: detecta secuencias incorrectas de eventos (igual que en Disponibilidad).
- **Condition monitoring**: chequea condiciones/supuestos de diseño; alimenta al predictive model y al sanity checking.
- **Sanity checking**: verifica validez/razonabilidad de resultados, entradas o salidas.
- **Comparison**: compara salidas de componentes replicados/sincronizados para detectar un estado inseguro (con 3+ réplicas, también identifica cuál falló).

## Contener (limitar el daño de un estado inseguro ya ocurrido)

- **Redundancy**: replication, functional redundancy, analytic redundancy (igual lógica que en Disponibilidad, pero acá el objetivo es seguir operando en vez de un total shutdown).
- **Limit consequences**: *abort* (abortar la operación insegura antes de que cause daño), *degradation* (mantener funciones críticas, descartar el resto de forma controlada), *masking* (enmascarar la falla comparando/votando entre componentes redundantes).
- **Barrier**: *firewall* (limita acceso a recursos), *interlock* (protege contra secuenciación incorrecta de eventos controlando el acceso a componentes protegidos).

## Recuperar

- **Rollback**: volver a un estado bueno conocido (rollback line) tras detectar una falla.
- **Repair state**: reparar el estado erróneo y continuar (ej. lane keep assist que corrige la posición del vehículo); no apto para fallas no anticipadas.
- **Reconfiguration**: remapear la arquitectura lógica sobre los recursos que quedan funcionando, manteniendo toda o parte de la funcionalidad.

> Nota: hay fuerte solapamiento con las tácticas de Disponibilidad (Cap. 4), porque los problemas de disponibilidad suelen derivar en problemas de safety y comparten muchas soluciones de diseño.
> 

# Cómo reconocer Safety en un escenario

Aparece cuando lo que está en juego es **evitar daño físico, lesión o entrada a un estado peligroso** para personas o el entorno (no solo la continuidad del servicio, que sería Disponibilidad). Típico en sistemas médicos, automotrices, aeroespaciales e industriales. Palabras clave: *estado seguro/inseguro, hazard, daño, lesión, modo degradado seguro, apagado de emergencia*.