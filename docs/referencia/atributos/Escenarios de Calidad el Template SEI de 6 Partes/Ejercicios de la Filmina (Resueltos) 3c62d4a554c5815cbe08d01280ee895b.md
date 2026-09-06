# Ejercicios de la Filmina (Resueltos)

# Ejercicio 1: Performance

> "Bajo condiciones normales de operación, el sistema debe procesar las transacciones de los usuarios con una latencia promedio de 2 segundos."
> 
> 
> La filmina propone 4 ejercicios: dada una oración "cruda" con un requerimiento de calidad, hay que identificar dentro de ella las 6 partes del template (Fuente del estímulo, Estímulo, Artefacto, Ambiente, Respuesta, Medida de la respuesta). A continuación, cada uno resuelto en detalle, con el razonamiento completo de por qué cada fragmento de la oración corresponde a cada casillero.
> 

| **Fuente del estímulo** | Los usuarios del sistema (la oración habla de "transacciones de los usuarios") |
| --- | --- |
| **Estímulo** | Llegada/generación de una transacción |
| **Artefacto** | El sistema/módulo de procesamiento de transacciones (**implícito** — no está explícito, hay que inferirlo o preguntar al stakeholder) |
| **Ambiente** | Operación normal (explícito: "bajo condiciones normales de operación") |
| **Respuesta** | Procesar la transacción del usuario |
| **Medida de respuesta** | Latencia promedio ≤ 2 segundos |

**Por qué es un buen escenario:** tiene una medida numérica clara, lo cual permite testearlo directamente. Lo único débil es el Artefacto, que quedó implícito — en un proyecto real, ahí es donde se hace una pregunta de seguimiento al stakeholder.

# Ejercicio 2: Seguridad

> "El sistema debe mantener información de auditoría sobre los datos que modifique cualquier individuo correctamente identificado. En caso de un ataque, la imagen correcta de los datos modificados por el usuario debe restaurarse en menos de 1 día."
> 

Este ejercicio es más rico porque en realidad **mezcla dos momentos distintos**, con dos estímulos diferentes.

## Momento 1 — El mecanismo permanente (precondición)

> "El sistema debe mantener información de auditoría sobre los datos que modifique cualquier individuo correctamente identificado."
> 
- **Estímulo**: cada vez que un usuario identificado modifica datos (una escritura normal, legítima)
- **Respuesta**: el sistema registra/loguea esa modificación como información de auditoría

Esto es un **mecanismo continuo de protección** — no es una reacción a un problema, es una práctica constante que el sistema hace siempre, casi como un "seguro" preventivo. De hecho es casi un requerimiento **funcional** (casi programable directamente como `registrarAuditoria(usuario, cambio)`), más que un escenario de calidad propiamente dicho.

## Momento 2 — El escenario de calidad real (respuesta ante un ataque)

> "En caso de un ataque, la imagen correcta de los datos modificados por el usuario debe restaurarse en menos de 1 día."
> 

| **Fuente del estímulo** | Un atacante (interno o externo — no se aclara en la oración, otro punto para preguntar) |
| --- | --- |
| **Estímulo** | Un ataque que corrompe/modifica datos |
| **Artefacto** | Los datos del sistema (información de auditoría / registros modificados) |
| **Ambiente** | Sistema en operación (implícito) |
| **Respuesta** | Restaurar la imagen correcta de los datos modificados, usando la información de auditoría |
| **Medida de respuesta** | Tiempo de restauración < 1 día |

Este **sí** es el verdadero escenario de atributo de calidad de Seguridad: tiene estimulo disruptivo externo, respuesta de recuperación y medida de respuesta cuantificable.

<aside>
🔗

**La relación entre los dos momentos**: el Momento 1 (auditoría) no desaparece — se convierte en una **precondición o mecanismo de soporte** que hace posible que la respuesta del Momento 2 sea viable.

```
Momento 1 (mecanismo permanente: auditoría)
        ↓ hace posible que exista
Momento 2 (el escenario de calidad real: recuperación ante ataque)
```

Esta misma estructura se repite en el ejemplo oficial de Disponibilidad de la filmina: hay un mecanismo de fondo (el sistema "sabe" detectar mensajes inesperados) y un escenario disparado por un evento puntual (llega el mensaje → el sistema responde). La estructura lógica es la misma: **hay una capacidad de base que el sistema debe tener siempre, y un escenario concreto que dispara y pone a prueba esa capacidad ante un evento específico.**

</aside>

# Ejercicio 3: Disponibilidad

> "Si el controlador detecta una falla en el procesador principal durante la operación normal, pasará el control al procesador de backup."
> 

| **Fuente del estímulo** | El procesador principal (fuente **interna** al sistema) |
| --- | --- |
| **Estímulo** | Falla del procesador principal |
| **Artefacto** | El controlador |
| **Ambiente** | Operación normal (explícito) |
| **Respuesta** | Transferir el control al procesador de backup (*failover*) |
| **Medida de respuesta** | **No especificada — falta en el enunciado** |

<aside>
⚠️

**El punto clave de este ejercicio**: la oración **no da ninguna medida cuantitativa**. No dice en cuánto tiempo debe hacerse el cambio, ni si hay pérdida de datos, ni si el usuario nota la interrupción.

Un escenario sin medida de respuesta cuantificable **no es un buen escenario todavía**, porque no se puede testear ni verificar objetivamente. Comparándolo con el ejemplo "oficial" de Disponibilidad de la filmina (el del template gráfico), que sí tenía una medida clara ("No Downtime"), acá falta ese último dato. Habría que volver a preguntarle al stakeholder: ¿en cuánto tiempo debe completarse el failover? ¿0 segundos? ¿1 segundo? ¿se pierden transacciones en curso?

</aside>

# Ejercicio 4: Usabilidad

> "Los usuarios deben poder minimizar el impacto de los errores cancelando la operación en curso, siendo el tiempo de cancelación menor a 1 segundo."
> 

| **Fuente del estímulo** | El usuario |
| --- | --- |
| **Estímulo** | Solicitud de cancelación de una operación en curso |
| **Artefacto** | La operación/proceso en curso (genérico — no está explícito cuál operación puntual) |
| **Ambiente** | Operación normal (implícito, no está explícito en la oración) |
| **Respuesta** | Cancelar la operación en curso |
| **Medida de respuesta** | Tiempo de cancelación < 1 segundo |

**Por qué es un buen ejemplo de Usabilidad:** conecta con la idea de usabilidad como "minimizar el impacto de los errores del usuario" — no es solo "que la interfaz sea linda", sino que el sistema le dé al usuario mecanismos de control y recuperación rápida cuando comete un error. La medida (1 segundo) hace que sea testeable con precisión.

# Tabla resumen comparativa de los 4 ejercicios

| **Parte** | **Performance** | **Seguridad** | **Disponibilidad** | **Usabilidad** |
| --- | --- | --- | --- | --- |
| Fuente | Usuarios | Atacante | Procesador principal (interno) | Usuario |
| Estímulo | Llegada de transacción | Ataque que modifica datos | Falla del procesador | Solicitud de cancelación |
| Artefacto | Módulo de transacciones (implícito) | Los datos | El controlador | La operación en curso (genérico) |
| Ambiente | Operación normal (explícito) | Operación (implícito) | Operación normal (explícito) | Operación normal (implícito) |
| Respuesta | Procesar la transacción | Restaurar datos correctos | Failover a backup | Cancelar operación |
| Medida | Latencia ≤ 2 seg | Restauración < 1 día | **Falta — no especificada** | Cancelación < 1 seg |

# La gran lección de los 4 ejercicios juntos

Tres de los cuatro escenarios están casi completos, pero el de **Disponibilidad** tiene una parte (la medida de respuesta) que falta por completo, y en varios casos el **Artefacto** queda implícito o ambiguo. Esto es intencional en el ejercicio, para mostrar en la práctica que:

1. Los requerimientos "crudos" en lenguaje natural casi nunca traen las 6 partes completas y explícitas.
2. El trabajo del analista/arquitecto es **detectar qué falta** y volver a preguntar al stakeholder hasta completar el template.
3. Un escenario sin medida de respuesta cuantificable **no sirve para testear ni verificar** — es la parte más crítica de no dejar vacía.