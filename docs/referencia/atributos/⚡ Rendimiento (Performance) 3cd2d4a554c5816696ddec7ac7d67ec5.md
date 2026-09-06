# ⚡ Rendimiento (Performance)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 9 — Performance.
> 

# Escenario General de Rendimiento

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | De un usuario (o varios), de un sistema externo, o de alguna parte del propio sistema | Externa: pedido de usuario, pedido de sistema externo, datos de un sensor · Interna: un componente pide algo a otro, un timer genera una notificación |
| Estímulo | La llegada de un evento: pedido de servicio o notificación de estado | Llegada de un evento periódico (intervalo predecible), estocástico (según una distribución de probabilidad) o esporádico (ni periódico ni estocástico) |
| Artefacto | Puede ser todo el sistema o solo una parte | Sistema completo · Componente dentro del sistema |
| Ambiente | El estado del sistema/componente cuando llega el estímulo | Runtime, en modo: normal, emergencia, corrección de errores, pico de carga, sobrecarga, operación degradada, u otro modo definido |
| Respuesta | El sistema procesa el estímulo; puede tomar tiempo por cómputo o por bloqueo por contención de recursos | El sistema devuelve una respuesta · Devuelve un error · No genera respuesta · Ignora el pedido si está sobrecargado · Cambia el modo/nivel de servicio · Atiende un evento de mayor prioridad · Consume recursos |
| Medida de Respuesta | Medidas de tiempo o de recursos | Tiempo (máx/mín/promedio/mediana) de respuesta (latencia) · Número/porcentaje de pedidos satisfechos en un intervalo (throughput) · Número/porcentaje de pedidos no satisfechos · Variación del tiempo de respuesta (jitter) · Nivel de uso de un recurso computacional |

**Ejemplo concreto (del libro):** 500 usuarios inician 2000 pedidos en un intervalo de 30 segundos, en operación normal; el sistema procesa todos los pedidos con una latencia promedio de 2 segundos.

# Los dos contribuyentes a la latencia

- **Tiempo de procesamiento**: el sistema está activamente trabajando y consumiendo recursos (CPU, almacenamiento, red, memoria, hilos, buffers).
- **Tiempo bloqueado**: por contención de recursos (varios clientes compiten por el mismo recurso), por no disponibilidad de un recurso, o por dependencia de otro cómputo (sincronización, espera de resultado).

# Tácticas para Rendimiento

Se agrupan en **controlar la demanda de recursos** y **gestionar recursos**.

## Controlar la Demanda de Recursos

- **Manage work requests**: reducir la cantidad de pedidos que entran al sistema.
    - *Manage event arrival*: acordar un SLA que limite la tasa máxima de eventos entrantes.
    - *Manage sampling rate*: reducir la frecuencia de muestreo (ej. fps de un video) a costa de fidelidad.
- **Limit event response**: procesar eventos hasta una tasa máxima, encolando o descartando el resto (con política de qué hacer con los descartados).
- **Prioritize events**: atender primero los eventos más importantes; ignorar los de baja prioridad si faltan recursos.
- **Reduce computational overhead**: *reduce indirection* (menos intermediarios, a costa de modificabilidad), *co-locate communicating resources* (juntar componentes que se comunican mucho para evitar latencia de red), *periodic cleaning* (recalcular/reinicializar estructuras que se vuelven ineficientes).
- **Bound execution times**: limitar cuánto tiempo/iteraciones se usa para responder (a costa de precisión).
- **Increase efficiency of resource usage**: optimizar los algoritmos/lógica crítica (la táctica más "clásica", pero solo una de muchas).

## Gestionar Recursos

- **Increase resources**: más/mejores procesadores, memoria o red (a veces la forma más barata de mejorar rápido).
- **Introduce concurrency**: procesar en paralelo distintos streams/hilos para reducir el tiempo bloqueado.
- **Maintain multiple copies of computations**: réplicas de un servicio + load balancer para repartir la carga.
- **Maintain multiple copies of data**: replicación de datos o cache (distintas velocidades de acceso); hay que decidir qué cachear.
- **Bound queue sizes**: limitar el tamaño máximo de colas de espera (y definir qué pasa si se desbordan).
- **Schedule resources**: elegir la política de planificación adecuada para cada recurso en contención (procesadores, buffers, red).

# Patrones para Rendimiento

- **Service mesh**: infraestructura (sidecars) que maneja concerns transversales de comunicación entre servicios; permite ubicar utilidades en el mismo procesador para reducir tráfico de red, pero agrega procesos y overhead.
- **Load balancer**: reparte pedidos entre múltiples réplicas de un servicio (round-robin, menor carga, etc.); riesgo de convertirse él mismo en cuello de botella o punto único de falla.
- **Throttling**: limita la tasa de pedidos entrantes para manejar picos de demanda con elegancia; la lógica de throttling debe ser muy rápida.
- **Map-reduce**: procesamiento paralelo de grandes volúmenes de datos no ordenados vía funciones map (distribuye/hashea) y reduce (agrega). No conviene con datasets chicos ni si no se pueden particionar en subconjuntos similares.

# Cómo reconocer Rendimiento en un escenario

Aparece cuando el estímulo es la **llegada de un evento/pedido** (único o en ráfaga) y lo que importa es **cuánto tarda** el sistema en responder o **cuántos pedidos por unidad de tiempo** puede procesar. Palabras clave: *latencia, throughput, tiempo de respuesta, carga, concurrencia, jitter*. Suele confundirse con Escalabilidad (que en el libro es un "sabor" de Modificabilidad: qué tan fácil es *agregar* recursos, no la performance en sí) y con Eficiencia Energética (que comparte varias tácticas de "reducir demanda" pero con el foco puesto en la energía, no en el tiempo).