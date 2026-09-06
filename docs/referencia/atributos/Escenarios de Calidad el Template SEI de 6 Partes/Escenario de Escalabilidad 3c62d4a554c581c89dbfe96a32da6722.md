# Escenario de Escalabilidad

Escalabilidad se trata en la filmina con más detalle conceptual que los demás atributos, así que acá se desarrolla como un caso aparte, con un ejercicio de la filmina y un escenario armado desde cero.

# Qué es la Escalabilidad

- Se refiere al **impacto de agregar o quitar recursos de TI**.
- Impacta principalmente en 3 aspectos:
    - **Capacidad**: cantidad de datos que el sistema puede manejar o usar
    - **Tiempo de respuesta**: lo que tarda el sistema en responder a un evento o realizar cierto procesamiento
    - **Throughput**: cantidad de unidades de trabajo que el sistema puede realizar en un período de tiempo dado

> "Scalability is the ability of a system to expand to meet your business needs. You scale a system by adding extra hardware or by upgrading the existing hardware without changing much of the application." — Microsoft MSDN
> 

**Diferencia con Performance:** Performance mide qué tan rápido responde el sistema *hoy*, con la carga actual. Escalabilidad mide **qué pasa cuando el sistema crece** (más usuarios, más datos, más demanda) — es decir, si el sistema se mantiene igual de bueno a medida que aumenta la carga, no si es bueno en un punto fijo.

Los escenarios de escalabilidad reutilizan el mismo template de 6 partes, pero definiendo valores propios como número de usuarios, volumen de datos, número de dispositivos externos, etc.

# El ejemplo de la filmina (incompleto a propósito)

> "El sistema debe ser capaz de gestionar despliegues de hasta 1000 dispositivos, que pueden generar un gran volumen de eventos, sin degradar sus tiempos de procesamiento."
> 

| **Fuente del estímulo** | Los dispositivos externos conectados al sistema |
| --- | --- |
| **Estímulo** | Incremento en el número de dispositivos desplegados (hasta 1000), generando un gran volumen de eventos |
| **Artefacto** | El módulo de procesamiento de eventos (implícito) |
| **Ambiente** | Sistema en operación, con despliegue creciente de dispositivos (implícito) |
| **Respuesta** | El sistema continúa procesando los eventos |
| **Medida de respuesta** | **No especificada con precisión** — dice "sin degradar sus tiempos de procesamiento" pero no da un número concreto |

Este ejemplo tiene el mismo problema que el ejercicio de Disponibilidad: falta un valor numérico en la medida de respuesta.

# Armando un escenario completo desde cero

**Contexto ficticio:** plataforma de monitoreo de sensores IoT para una empresa de logística. El stakeholder dice: *"Hoy tenemos 200 camiones con sensores. El plan de negocio es escalar a 5000 camiones en 2 años. Me preocupa que el sistema no aguante ese crecimiento."*

| **Fuente del estímulo** | Sensores IoT de los camiones |
| --- | --- |
| **Estímulo** | El número de dispositivos activos crece de 200 a 5000, generando un aumento proporcional en el volumen de eventos (mensajes de posición/estado) enviados al sistema |
| **Artefacto** | El servicio de ingesta de eventos (*event ingestion service*) y la base de datos de series temporales |
| **Ambiente** | Sistema en operación normal, durante el proceso gradual de escalado de la flota a lo largo de 2 años |
| **Respuesta** | El sistema sigue ingiriendo y procesando todos los eventos de posición sin pérdida de datos ni degradación perceptible |
| **Medida de respuesta** | Ver sección siguiente — medida absoluta + relativa combinadas |

## Definiendo la medida de respuesta: absoluta vs. relativa

Una medida **absoluta** simple sería: *"el tiempo de procesamiento por evento se mantiene por debajo de 500 ms en el percentil 95, con 0% de eventos perdidos, soportando hasta 5000 dispositivos enviando datos cada 10 segundos (≈500 eventos/segundo en pico)".*

Pero en Escalabilidad tiene mucho sentido usar también (o en combinación) una medida **relativa**, que compara el comportamiento con carga alta contra el comportamiento con carga baja (el *baseline*):

> *"El tiempo de procesamiento por evento con 5000 dispositivos no debe superar en más de un 10% el tiempo de procesamiento observado con 200 dispositivos."*
> 

<aside>
⚖️

**Por qué la medida relativa tiene sentido en Escalabilidad**

Recordando la definición: la cuestión en Escalabilidad no es "¿es rápido?" (eso es Performance), sino "¿se mantiene igual de rápido a medida que crece la carga?". Una medida relativa captura mejor esa esencia:

1. **Aisla el efecto de escalar** de otras variables (hardware, red, etc.) que una medida absoluta aislada no distingue.
2. **Es más fácil de defender** frente a un stakeholder que no sabe si "500ms" es mucho o poco, pero sí entiende "que no empeore más de un 10%".
3. **Sirve como criterio de aceptación en testing de carga**: se corre el sistema con 200 dispositivos, se mide, después se simulan 5000, se mide de nuevo, y se compara la diferencia porcentual.

**El matiz importante — combinar ambas:** una medida puramente relativa tiene un punto débil: si el baseline ya es malo, "mantenerse igual" no salva nada. Por ejemplo, si con 200 dispositivos el sistema ya tarda 3 segundos, "no degradarse más de un 10%" deja un sistema que sigue siendo lento (3.3 seg) — cumple la medida relativa, pero el negocio sigue insatisfecho. Por eso, la medida más robusta combina las dos:

> *"El tiempo de procesamiento por evento con 5000 dispositivos activos no debe superar en más de un 10% el tiempo observado con 200 dispositivos, y en ningún caso debe exceder los 500 ms en el percentil 95."*
> 

Esto da lo mejor de ambos mundos: la parte relativa mide específicamente el impacto de escalar, y la parte absoluta garantiza que el punto de partida también sea aceptable.

</aside>

## Escenario final completo

> *"Cuando el número de sensores IoT activos en los camiones crece de 200 a 5000 (generando hasta ~500 eventos por segundo en los picos), el servicio de ingesta de eventos y la base de datos de series temporales deben seguir procesando todos los eventos sin pérdida de datos, manteniendo un tiempo de procesamiento por evento que no se degrade más de un 10% respecto al observado con 200 dispositivos, y que en ningún caso supere los 500 ms en el percentil 95."*
> 

## Tabla comparativa: ejemplo de la filmina vs. escenario armado desde cero

| **Parte** | **Escenario de la filmina (incompleto)** | **Escenario armado desde cero (completo)** |
| --- | --- | --- |
| Fuente | Dispositivos | Sensores IoT de camiones |
| Estímulo | Crece a 1000 dispositivos | Crece de 200 a 5000 dispositivos |
| Artefacto | Implícito | Servicio de ingesta + BD de series temporales |
| Ambiente | Implícito | Operación normal, escalado gradual en 2 años |
| Respuesta | "Sin degradar" (vago) | Procesar sin pérdida ni degradación perceptible |
| Medida | ❌ Falta valor numérico | ✅ <500ms percentil 95, 0% pérdida, +relativo 10% |

# La lección clave de Escalabilidad

Hay que prestar especial atención a definir con números concretos los tres aspectos que menciona la filmina — **Capacidad** (200 → 5000 dispositivos), **Tiempo de respuesta** (< 500 ms) y **Throughput** (500 eventos/seg) — y, cuando sea posible, complementarlos con una medida **relativa** (baseline vs. carga aumentada), que es la forma más fiel de capturar qué significa realmente "escalar bien".