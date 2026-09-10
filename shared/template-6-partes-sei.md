# Escenarios de Calidad: el Template SEI de 6 Partes

> Fuente: página de Notion "Escenarios de Calidad: el Template SEI de 6 Partes" (subpágina de "Atributos de Calidad") y sus subpáginas "Ejercicios de la Filmina (Resueltos)" y "Escenario de Escalabilidad", migradas casi íntegras. Basado en Clase 5 — Diseño de Sistemas de Software (J. Andrés Díaz Pace).

Así como hay templates para capturar requerimientos funcionales, el **SEI** (Software Engineering Institute) propone un template para capturar cada atributo de calidad como un **escenario**. Existe una variante específica de este template para cada atributo, pero todas comparten el mismo esqueleto de **6 partes**.

## Tres niveles de abstracción

El proceso completo tiene tres niveles, de lo más abstracto a lo más concreto:

**Atributo de Calidad → (da soporte a) → Escenario(s) → (se materializa en) → Template específico**

| Nivel | Qué es |
|---|---|
| Atributo de Calidad | El concepto general, no operacional: "Performance", "Seguridad", "Disponibilidad". Solo, no dice nada concreto — no es medible ni testeable por sí mismo. |
| Template | La estructura genérica de 6 casilleros **vacíos**, específica por atributo (el template de Performance sugiere estímulos distintos al de Seguridad, pero el esqueleto de 6 partes es el mismo). Es un formulario en blanco diseñado a medida para cada atributo. |
| Escenario | El template **ya completado** con información real de un caso particular. Es una oración concreta, medible y testeable, que describe una situación específica del sistema. |

## Las 6 partes del template

1. **Fuente del estímulo**: quién o qué genera el estímulo (interno o externo al sistema)
2. **Estímulo**: la condición que llega al sistema y requiere una respuesta
3. **Artefacto**: la parte del sistema afectada (todo el sistema, un componente, un proceso, etc.)
4. **Ambiente**: el estado del sistema en el momento del estímulo (ej. operación normal, sobrecarga)
5. **Respuesta**: la actividad que ocurre luego de la llegada del estímulo
6. **Medida de la respuesta**: cómo se mide/evalúa la respuesta, de forma cuantificable

## ¿El estímulo siempre es un fallo o problema?

No. Es un error común pensar que el Estímulo del template siempre representa algo roto o anómalo — eso solo aplica a algunos atributos (típicamente Disponibilidad). Lo que es constante en **todos** los atributos es algo más general:

> El estímulo es una **condición que le exige al sistema una respuesta observable y medible relacionada con esa preocupación de calidad particular**. A veces esa condición es un fallo (Disponibilidad), a veces es una amenaza (Seguridad), a veces es simplemente una situación de uso normal pero exigente (Performance, Usabilidad), o un pedido de cambio (Modificabilidad).

Ejemplos de estímulos que **no** son fallos:

- **Performance**: el estímulo es una carga de trabajo legítima (ej. "muchos usuarios buscan simultáneamente durante una promoción"). No hay nada roto — el sistema tiene que responder bien a una demanda normal o pico.
- **Modificabilidad**: el estímulo es un pedido de cambio de un desarrollador o stakeholder (ej. "agregar una nueva política de cálculo"). Es una necesidad de evolución, no un problema.
- **Interoperabilidad**: el estímulo es conectar/integrar un sistema o dispositivo externo nuevo. Tampoco es un fallo, es una integración planeada.
- **Seguridad**: el estímulo suele ser un intento de ataque o acceso no autorizado — hay una amenaza, pero no es un "fallo" propio del sistema, sino una acción externa maliciosa.

Esto importa a la hora de armar un escenario desde cero: si el atributo no es Disponibilidad, forzar el estímulo a que sea "algo que falla" lleva a escenarios artificiales o mal encuadrados. Conviene preguntarse primero qué tipo de condición dispara la preocupación de ese atributo puntual, en vez de asumir que siempre hay que buscar un fallo.

## Ejemplo oficial: Disponibilidad

| Parte | Valor |
|---|---|
| Fuente | Externa al sistema |
| Estímulo | Mensaje inesperado (*unanticipated message*) |
| Artefacto | Un proceso |
| Ambiente | Operación normal |
| Respuesta | Informar al operador y continuar operando |
| Medida de respuesta | Sin tiempo de inactividad (*no downtime*) |

## Cómo se pasa de Template a Escenario concreto (la transición en la práctica)

Hay dos caminos posibles, según si partís de un requerimiento ya escrito o de cero.

### Camino 1 — Desarmando una oración ya escrita

Cuando ya tenés un requerimiento "crudo" en lenguaje natural, el truco es leerlo y "cazar" qué fragmento responde cada una de las 6 preguntas. Un requerimiento crudo casi nunca trae las 6 partes explícitas y separadas — hay que inferir lo que falta (ver sección "Ejercicios de la Filmina" más abajo, donde se practica esto con los 4 ejemplos oficiales).

### Camino 2 — Armando el escenario desde cero

Esta es la situación más realista en un proyecto real: no hay una oración lista, hay que **construir** el escenario a partir de una preocupación de negocio.

> **Ejemplo completo — e-commerce en Cyber Monday**
> Un stakeholder dice: *"Me preocupa que en el Cyber Monday el sitio se caiga o se ponga lentísimo."*
>
> | Parte | Valor |
> |---|---|
> | Fuente del estímulo | Usuarios finales navegando el sitio |
> | Estímulo | Pico de solicitudes concurrentes (ej. 10.000 usuarios simultáneos) |
> | Artefacto | El servidor de checkout / carrito de compras |
> | Ambiente | Durante un evento de alta demanda (Cyber Monday) |
> | Respuesta | El sistema sigue procesando pedidos sin caerse |
> | Medida de respuesta | Tiempo de respuesta ≤ 3 segundos, 0% de caídas del servicio |
>
> **Escenario final (la oración completa):** *"Cuando 10.000 usuarios finales generan un pico de solicitudes concurrentes sobre el carrito de compras durante un evento de alta demanda como Cyber Monday, el sistema debe seguir procesando pedidos sin caerse, respondiendo en menos de 3 segundos y sin caídas del servicio."*

## El método paso a paso, para aplicar siempre

1. **Identificar la preocupación real** (de un stakeholder, de una entrevista, de un riesgo conocido, o de una hoja del árbol de utilidad — ver `metodo-arbol-utilidad.md`).
2. **Recorrer las 6 preguntas del template una por una**, en orden, y responderlas con la info disponible. Si algo no está claro, es señal de que hay que **preguntarle al stakeholder** — el template funciona como checklist de qué preguntar.
3. **Ser lo más concreto y numérico posible**, especialmente en Estímulo, Ambiente y Medida de Respuesta — esas tres suelen quedar vagas si no se fuerzan a ser específicas.
4. **Armar la oración final** uniendo las 6 respuestas en una frase natural — esto es el "escenario" que queda documentado.
5. **Repetir el proceso** para cada atributo relevante ya identificado con el árbol de utilidad o las entrevistas a stakeholders.

```
Preocupación/necesidad del stakeholder (vaga, en lenguaje natural)
        ↓
Árbol de utilidad → decide QUÉ atributos priorizar
        ↓
Template de 6 partes (específico por atributo) → estructura vacía a completar
        ↓
Se completa cada una de las 6 preguntas con info concreta del proyecto
        ↓
ESCENARIO final → oración concreta, medible, testeable, lista para diseño/QA
```

---

# Ejercicios de la Filmina (Resueltos)

La filmina propone 4 ejercicios: dada una oración "cruda" con un requerimiento de calidad, hay que identificar dentro de ella las 6 partes del template (Fuente del estímulo, Estímulo, Artefacto, Ambiente, Respuesta, Medida de la respuesta). A continuación, cada uno resuelto en detalle, con el razonamiento completo de por qué cada fragmento de la oración corresponde a cada casillero.

## Ejercicio 1: Performance

> "Bajo condiciones normales de operación, el sistema debe procesar las transacciones de los usuarios con una latencia promedio de 2 segundos."

| Parte | Valor |
|---|---|
| Fuente del estímulo | Los usuarios del sistema (la oración habla de "transacciones de los usuarios") |
| Estímulo | Llegada/generación de una transacción |
| Artefacto | El sistema/módulo de procesamiento de transacciones (**implícito** — no está explícito, hay que inferirlo o preguntar al stakeholder) |
| Ambiente | Operación normal (explícito: "bajo condiciones normales de operación") |
| Respuesta | Procesar la transacción del usuario |
| Medida de respuesta | Latencia promedio ≤ 2 segundos |

**Por qué es un buen escenario:** tiene una medida numérica clara, lo cual permite testearlo directamente. Lo único débil es el Artefacto, que quedó implícito — en un proyecto real, ahí es donde se hace una pregunta de seguimiento al stakeholder.

## Ejercicio 2: Seguridad

> "El sistema debe mantener información de auditoría sobre los datos que modifique cualquier individuo correctamente identificado. En caso de un ataque, la imagen correcta de los datos modificados por el usuario debe restaurarse en menos de 1 día."

Este ejercicio es más rico porque en realidad **mezcla dos momentos distintos**, con dos estímulos diferentes.

### Momento 1 — El mecanismo permanente (precondición)

> "El sistema debe mantener información de auditoría sobre los datos que modifique cualquier individuo correctamente identificado."

- **Estímulo**: cada vez que un usuario identificado modifica datos (una escritura normal, legítima)
- **Respuesta**: el sistema registra/loguea esa modificación como información de auditoría

Esto es un **mecanismo continuo de protección** — no es una reacción a un problema, es una práctica constante que el sistema hace siempre, casi como un "seguro" preventivo. De hecho es casi un requerimiento **funcional** (casi programable directamente como `registrarAuditoria(usuario, cambio)`), más que un escenario de calidad propiamente dicho.

### Momento 2 — El escenario de calidad real (respuesta ante un ataque)

> "En caso de un ataque, la imagen correcta de los datos modificados por el usuario debe restaurarse en menos de 1 día."

| Parte | Valor |
|---|---|
| Fuente del estímulo | Un atacante (interno o externo — no se aclara en la oración, otro punto para preguntar) |
| Estímulo | Un ataque que corrompe/modifica datos |
| Artefacto | Los datos del sistema (información de auditoría / registros modificados) |
| Ambiente | Sistema en operación (implícito) |
| Respuesta | Restaurar la imagen correcta de los datos modificados, usando la información de auditoría |
| Medida de respuesta | Tiempo de restauración < 1 día |

Este **sí** es el verdadero escenario de atributo de calidad de Seguridad: tiene estímulo disruptivo externo, respuesta de recuperación y medida de respuesta cuantificable.

> **La relación entre los dos momentos**: el Momento 1 (auditoría) no desaparece — se convierte en una **precondición o mecanismo de soporte** que hace posible que la respuesta del Momento 2 sea viable.
>
> ```
> Momento 1 (mecanismo permanente: auditoría)
>         ↓ hace posible que exista
> Momento 2 (el escenario de calidad real: recuperación ante ataque)
> ```
>
> Esta misma estructura se repite en el ejemplo oficial de Disponibilidad de la filmina: hay un mecanismo de fondo (el sistema "sabe" detectar mensajes inesperados) y un escenario disparado por un evento puntual (llega el mensaje → el sistema responde). La estructura lógica es la misma: **hay una capacidad de base que el sistema debe tener siempre, y un escenario concreto que dispara y pone a prueba esa capacidad ante un evento específico.**

## Ejercicio 3: Disponibilidad

> "Si el controlador detecta una falla en el procesador principal durante la operación normal, pasará el control al procesador de backup."

| Parte | Valor |
|---|---|
| Fuente del estímulo | El procesador principal (fuente **interna** al sistema) |
| Estímulo | Falla del procesador principal |
| Artefacto | El controlador |
| Ambiente | Operación normal (explícito) |
| Respuesta | Transferir el control al procesador de backup (*failover*) |
| Medida de respuesta | **No especificada — falta en el enunciado** |

> **El punto clave de este ejercicio**: la oración **no da ninguna medida cuantitativa**. No dice en cuánto tiempo debe hacerse el cambio, ni si hay pérdida de datos, ni si el usuario nota la interrupción.
>
> Un escenario sin medida de respuesta cuantificable **no es un buen escenario todavía**, porque no se puede testear ni verificar objetivamente. Comparándolo con el ejemplo "oficial" de Disponibilidad de la filmina (el del template gráfico), que sí tenía una medida clara ("No Downtime"), acá falta ese último dato. Habría que volver a preguntarle al stakeholder: ¿en cuánto tiempo debe completarse el failover? ¿0 segundos? ¿1 segundo? ¿se pierden transacciones en curso?

## Ejercicio 4: Usabilidad

> "Los usuarios deben poder minimizar el impacto de los errores cancelando la operación en curso, siendo el tiempo de cancelación menor a 1 segundo."

| Parte | Valor |
|---|---|
| Fuente del estímulo | El usuario |
| Estímulo | Solicitud de cancelación de una operación en curso |
| Artefacto | La operación/proceso en curso (genérico — no está explícito cuál operación puntual) |
| Ambiente | Operación normal (implícito, no está explícito en la oración) |
| Respuesta | Cancelar la operación en curso |
| Medida de respuesta | Tiempo de cancelación < 1 segundo |

**Por qué es un buen ejemplo de Usabilidad:** conecta con la idea de usabilidad como "minimizar el impacto de los errores del usuario" — no es solo "que la interfaz sea linda", sino que el sistema le dé al usuario mecanismos de control y recuperación rápida cuando comete un error. La medida (1 segundo) hace que sea testeable con precisión.

## Tabla resumen comparativa de los 4 ejercicios

| Parte | Performance | Seguridad | Disponibilidad | Usabilidad |
|---|---|---|---|---|
| Fuente | Usuarios | Atacante | Procesador principal (interno) | Usuario |
| Estímulo | Llegada de transacción | Ataque que modifica datos | Falla del procesador | Solicitud de cancelación |
| Artefacto | Módulo de transacciones (implícito) | Los datos | El controlador | La operación en curso (genérico) |
| Ambiente | Operación normal (explícito) | Operación (implícito) | Operación normal (explícito) | Operación normal (implícito) |
| Respuesta | Procesar la transacción | Restaurar datos correctos | Failover a backup | Cancelar operación |
| Medida | Latencia ≤ 2 seg | Restauración < 1 día | **Falta — no especificada** | Cancelación < 1 seg |

## La gran lección de los 4 ejercicios juntos

Tres de los cuatro escenarios están casi completos, pero el de **Disponibilidad** tiene una parte (la medida de respuesta) que falta por completo, y en varios casos el **Artefacto** queda implícito o ambiguo. Esto es intencional en el ejercicio, para mostrar en la práctica que:

1. Los requerimientos "crudos" en lenguaje natural casi nunca traen las 6 partes completas y explícitas.
2. El trabajo del analista/arquitecto es **detectar qué falta** y volver a preguntar al stakeholder hasta completar el template.
3. Un escenario sin medida de respuesta cuantificable **no sirve para testear ni verificar** — es la parte más crítica de no dejar vacía.

---

# Escenario de Escalabilidad

Escalabilidad se trata en la filmina con más detalle conceptual que los demás atributos, así que acá se desarrolla como un caso aparte, con un ejercicio de la filmina y un escenario armado desde cero.

## Qué es la Escalabilidad

- Se refiere al **impacto de agregar o quitar recursos de TI**.
- Impacta principalmente en 3 aspectos:
  - **Capacidad**: cantidad de datos que el sistema puede manejar o usar
  - **Tiempo de respuesta**: lo que tarda el sistema en responder a un evento o realizar cierto procesamiento
  - **Throughput**: cantidad de unidades de trabajo que el sistema puede realizar en un período de tiempo dado

> "Scalability is the ability of a system to expand to meet your business needs. You scale a system by adding extra hardware or by upgrading the existing hardware without changing much of the application." — Microsoft MSDN

**Diferencia con Performance:** Performance mide qué tan rápido responde el sistema *hoy*, con la carga actual. Escalabilidad mide **qué pasa cuando el sistema crece** (más usuarios, más datos, más demanda) — es decir, si el sistema se mantiene igual de bueno a medida que aumenta la carga, no si es bueno en un punto fijo.

Los escenarios de escalabilidad reutilizan el mismo template de 6 partes, pero definiendo valores propios como número de usuarios, volumen de datos, número de dispositivos externos, etc.

## El ejemplo de la filmina (incompleto a propósito)

> "El sistema debe ser capaz de gestionar despliegues de hasta 1000 dispositivos, que pueden generar un gran volumen de eventos, sin degradar sus tiempos de procesamiento."

| Parte | Valor |
|---|---|
| Fuente del estímulo | Los dispositivos externos conectados al sistema |
| Estímulo | Incremento en el número de dispositivos desplegados (hasta 1000), generando un gran volumen de eventos |
| Artefacto | El módulo de procesamiento de eventos (implícito) |
| Ambiente | Sistema en operación, con despliegue creciente de dispositivos (implícito) |
| Respuesta | El sistema continúa procesando los eventos |
| Medida de respuesta | **No especificada con precisión** — dice "sin degradar sus tiempos de procesamiento" pero no da un número concreto |

Este ejemplo tiene el mismo problema que el ejercicio de Disponibilidad: falta un valor numérico en la medida de respuesta.

## Armando un escenario completo desde cero

**Contexto ficticio:** plataforma de monitoreo de sensores IoT para una empresa de logística. El stakeholder dice: *"Hoy tenemos 200 camiones con sensores. El plan de negocio es escalar a 5000 camiones en 2 años. Me preocupa que el sistema no aguante ese crecimiento."*

| Parte | Valor |
|---|---|
| Fuente del estímulo | Sensores IoT de los camiones |
| Estímulo | El número de dispositivos activos crece de 200 a 5000, generando un aumento proporcional en el volumen de eventos (mensajes de posición/estado) enviados al sistema |
| Artefacto | El servicio de ingesta de eventos (*event ingestion service*) y la base de datos de series temporales |
| Ambiente | Sistema en operación normal, durante el proceso gradual de escalado de la flota a lo largo de 2 años |
| Respuesta | El sistema sigue ingiriendo y procesando todos los eventos de posición sin pérdida de datos ni degradación perceptible |
| Medida de respuesta | Ver sección siguiente — medida absoluta + relativa combinadas |

### Definiendo la medida de respuesta: absoluta vs. relativa

Una medida **absoluta** simple sería: *"el tiempo de procesamiento por evento se mantiene por debajo de 500 ms en el percentil 95, con 0% de eventos perdidos, soportando hasta 5000 dispositivos enviando datos cada 10 segundos (≈500 eventos/segundo en pico)".*

Pero en Escalabilidad tiene mucho sentido usar también (o en combinación) una medida **relativa**, que compara el comportamiento con carga alta contra el comportamiento con carga baja (el *baseline*):

> *"El tiempo de procesamiento por evento con 5000 dispositivos no debe superar en más de un 10% el tiempo de procesamiento observado con 200 dispositivos."*

> **Por qué la medida relativa tiene sentido en Escalabilidad**
> Recordando la definición: la cuestión en Escalabilidad no es "¿es rápido?" (eso es Performance), sino "¿se mantiene igual de rápido a medida que crece la carga?". Una medida relativa captura mejor esa esencia:
> 1. **Aisla el efecto de escalar** de otras variables (hardware, red, etc.) que una medida absoluta aislada no distingue.
> 2. **Es más fácil de defender** frente a un stakeholder que no sabe si "500ms" es mucho o poco, pero sí entiende "que no empeore más de un 10%".
> 3. **Sirve como criterio de aceptación en testing de carga**: se corre el sistema con 200 dispositivos, se mide, después se simulan 5000, se mide de nuevo, y se compara la diferencia porcentual.
>
> **El matiz importante — combinar ambas:** una medida puramente relativa tiene un punto débil: si el baseline ya es malo, "mantenerse igual" no salva nada. Por ejemplo, si con 200 dispositivos el sistema ya tarda 3 segundos, "no degradarse más de un 10%" deja un sistema que sigue siendo lento (3.3 seg) — cumple la medida relativa, pero el negocio sigue insatisfecho. Por eso, la medida más robusta combina las dos:
>
> *"El tiempo de procesamiento por evento con 5000 dispositivos activos no debe superar en más de un 10% el tiempo observado con 200 dispositivos, y en ningún caso debe exceder los 500 ms en el percentil 95."*
>
> Esto da lo mejor de ambos mundos: la parte relativa mide específicamente el impacto de escalar, y la parte absoluta garantiza que el punto de partida también sea aceptable.

### Escenario final completo

> *"Cuando el número de sensores IoT activos en los camiones crece de 200 a 5000 (generando hasta ~500 eventos por segundo en los picos), el servicio de ingesta de eventos y la base de datos de series temporales deben seguir procesando todos los eventos sin pérdida de datos, manteniendo un tiempo de procesamiento por evento que no se degrade más de un 10% respecto al observado con 200 dispositivos, y que en ningún caso supere los 500 ms en el percentil 95."*

### Tabla comparativa: ejemplo de la filmina vs. escenario armado desde cero

| Parte | Escenario de la filmina (incompleto) | Escenario armado desde cero (completo) |
|---|---|---|
| Fuente | Dispositivos | Sensores IoT de camiones |
| Estímulo | Crece a 1000 dispositivos | Crece de 200 a 5000 dispositivos |
| Artefacto | Implícito | Servicio de ingesta + BD de series temporales |
| Ambiente | Implícito | Operación normal, escalado gradual en 2 años |
| Respuesta | "Sin degradar" (vago) | Procesar sin pérdida ni degradación perceptible |
| Medida | ❌ Falta valor numérico | ✅ <500ms percentil 95, 0% pérdida, +relativo 10% |

## La lección clave de Escalabilidad

Hay que prestar especial atención a definir con números concretos los tres aspectos que menciona la filmina — **Capacidad** (200 → 5000 dispositivos), **Tiempo de respuesta** (< 500 ms) y **Throughput** (500 eventos/seg) — y, cuando sea posible, complementarlos con una medida **relativa** (baseline vs. carga aumentada), que es la forma más fiel de capturar qué significa realmente "escalar bien".
