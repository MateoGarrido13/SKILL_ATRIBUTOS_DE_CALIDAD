[ESTADO: VALIDADO]
**Atributo de Calidad:** Seguridad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Atacante humano externo (no confiable) |
| **Estímulo** | Intento no autorizado de acceder al servicio de venta (fuerza bruta de sesión) |
| **Artefacto** | Autenticación del comprador en el checkout |
| **Ambiente** | Operación normal, online |
| **Respuesta** | Activa un corte de tipo circuit breaker y deja de aceptar interacción de ese usuario |
| **Medida de Respuesta** | A partir del quinto intento malintencionado, ese usuario no puede interactuar con la aplicación durante 30 minutos |

> **Nota de Auditoría:** Elegiste la opción 2: estímulo = fuerza bruta de sesión contra el servicio de venta; artefacto = autenticación del comprador en el checkout; ambiente = operación normal, online. Se mantiene el corte al quinto intento por 30 minutos.
