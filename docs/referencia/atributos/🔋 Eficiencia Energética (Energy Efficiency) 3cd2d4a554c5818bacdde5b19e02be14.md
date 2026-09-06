# 🔋 Eficiencia Energética (Energy Efficiency)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 6 — Energy Efficiency.
> 

# Escenario General de Eficiencia Energética

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | Quién/qué dispara la gestión de energía | Un individuo (usuario, administrador) o el propio sistema (ej. un proceso que decide autogestionar su consumo) |
| Estímulo | La necesidad de ahorrar o gestionar energía | Se necesita gestionar el consumo de un recurso computacional |
| Artefacto | Qué recurso se gestiona | Dispositivos, servidores, VMs, clusters, etc. específicos |
| Ambiente | Se gestiona típicamente en runtime, pero hay casos especiales según características del sistema | Runtime, conectado, alimentado por batería, modo de batería baja, modo de conservación de energía |
| Respuesta | Qué acciones toma el sistema para conservar/gestionar el uso de energía | Deshabilitar servicios · Liberar servicios en runtime · Cambiar la asignación de servicios a servidores · Correr servicios en un modo de menor consumo · Asignar/liberar servidores · Cambiar niveles de servicio · Cambiar la planificación (scheduling) |
| Medida de Respuesta | Girar en torno a la energía ahorrada/consumida y su efecto sobre otras funciones o atributos de calidad | Carga máxima/promedio en kilowatts · Cantidad promedio/total de energía ahorrada · Total de kilowatts-hora usados · Período durante el cual el sistema debe permanecer encendido — manteniendo el nivel de funcionalidad requerido y niveles aceptables de otros atributos de calidad |

**Ejemplo concreto (del libro):** un gerente quiere ahorrar energía en runtime liberando recursos no utilizados en períodos de baja demanda; el sistema libera recursos manteniendo una latencia máxima de 2 segundos en consultas a la base de datos, ahorrando en promedio el 50% de la energía total requerida.

# Tácticas para Eficiencia Energética

Se agrupan en tres categorías: **monitorear recursos**, **asignar recursos** y **reducir la demanda de recursos**.

## Monitorear Recursos

- **Metering**: medir el consumo real de energía vía sensores en (casi) tiempo real (medidores de potencia, PDUs medidas, sistemas de gestión de baterías).
- **Static classification**: estimar el consumo catálogando recursos y sus características conocidas (benchmarks, specs del fabricante) cuando no hay datos en tiempo real.
- **Dynamic classification**: estimar el consumo con modelos que consideran condiciones transitorias (carga de trabajo), vía tabla de búsqueda, regresión o simulación.

## Asignar Recursos

- **Reduce usage**: reducir el uso a nivel de dispositivo (bajar el refresh rate, apagar CPUs/servidores no usados, correr a menor clock, consolidar VMs en menos servidores físicos, delegar cómputo a la nube).
- **Discovery**: anotar los pedidos de servicio con información energética para elegir el proveedor más eficiente (ej. un "green service directory").
- **Schedule resources**: planificar tareas considerando el consumo energético además del rendimiento, migrando dinámicamente hacia el proveedor más eficiente.

## Reducir la Demanda de Recursos

Comparte tácticas con Performance (Cap. 9): manage event arrival, limit event response, prioritize events, reduce computational overhead, bound execution times, increase resource usage efficiency. La diferencia con "reduce usage" es que acá se reduce la demanda misma, no solo el consumo dado un nivel de demanda constante.

# Patrones para Eficiencia Energética

- **Sensor fusion**: usar datos de sensores de bajo consumo para inferir si vale la pena consultar un sensor de mayor consumo (ej. acelerómetro antes de actualizar el GPS). Riesgo: si la inferencia consulta seguido el sensor caro, puede terminar consumiendo más.
- **Kill abnormal tasks**: monitorear y matar tareas que consumen energía de forma anómala (útil en apps móviles de origen desconocido); cuidado con el impacto en usabilidad.
- **Power monitor**: apagar automáticamente dispositivos/interfaces no usados activamente; el costo es la latencia extra al reactivarlos.

# Cómo reconocer Eficiencia Energética en un escenario

Aparece cuando el foco del escenario es el **consumo de energía/batería** en sí (no la velocidad de respuesta ni la disponibilidad), y la medida de respuesta se expresa en kW, kWh, autonomía de batería o "% de energía ahorrada". Muy relevante en sistemas móviles, IoT y data centers.