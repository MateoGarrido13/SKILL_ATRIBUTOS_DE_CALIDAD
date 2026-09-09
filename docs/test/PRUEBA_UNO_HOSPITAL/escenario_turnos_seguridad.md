[ESTADO: VALIDADO]
**Atributo de Calidad:** Seguridad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Intruso humano, externo a la organización y no confiable |
| **Estímulo** | Intento de acceso no autorizado a los servicios del sistema de gestión de turnos |
| **Artefacto** | Módulo de autenticación y control de acceso del sistema de gestión de turnos del hospital |
| **Ambiente** | Sistema abierto a una red |
| **Respuesta** | El sistema rechaza el acceso no autorizado; tras intentos fallidos reiterados bloquea nuevos intentos con un temporizador creciente y envía un email al administrador para que habilite el servicio o cambie la contraseña |
| **Medida de Respuesta** | El bloqueo se activa en el 3.er intento fallido; la espera hasta el próximo intento es de 20 min y se incrementa 20 min en cada ciclo sucesivo |

> **Nota de Auditoría:** Las 6 partes pasan: fuente externa no confiable, estímulo de acceso no autorizado, artefacto a granularidad de módulo, ambiente concreto (abierto a una red), respuesta de rechazo/bloqueo/notificación sin umbrales embebidos, y medida cuantificable (conteo de 3 intentos y tiempo de 20 min, con veredicto pasa/no-pasa). Coherente con Seguridad. Un solo atributo. Sin marcas heredadas. No hubo fallas ni correcciones en esta auditoría.
