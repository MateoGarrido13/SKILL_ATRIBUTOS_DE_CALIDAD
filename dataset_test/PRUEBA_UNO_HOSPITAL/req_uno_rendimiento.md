[ESTADO: VALIDADO]
**Atributo de Calidad:** Rendimiento

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Usuarios externos (pacientes / público) que solicitan turnos |
| **Estímulo** | Llegada concurrente de pedidos de reserva de turnos |
| **Artefacto** | Módulo de reserva de turnos del sistema de gestión de turnos del hospital |
| **Ambiente** | Pico de carga: concurrencia alta de solicitudes de turnos |
| **Respuesta** | El sistema procesa las solicitudes de reserva y devuelve una respuesta; no deja de atender |
| **Medida de Respuesta** | Latencia promedio de la reserva de turno ≤ 20 s en pico de carga |

> **Nota de Auditoría:** Las 6 partes pasan: fuente externa identificable, estímulo de llegada de pedidos, artefacto a granularidad de módulo, ambiente de pico de carga, respuesta sin métrica embebida, y medida cuantificable (latencia, unidad segundos, estadístico promedio, umbral 20 s). Coherente con Rendimiento. Un solo atributo. Sin marcas heredadas. No hubo fallas ni correcciones en esta auditoría.
