Quedaron dos borradores. El de nuevas funciones de pago es **Modificabilidad**. El del chatbot 24 hs no es Escalabilidad: ese atributo se dispara cuando crece la carga. Lo que pedís es que el asistente esté en pie a cualquier hora, y eso es **Disponibilidad**. Si además te preocupa cuántas conversaciones simultáneas aguanta, eso es otro escenario y hace falta de cuánta carga a cuánta carga.

---

## 1. Nuevas funciones de pago

[ESTADO: BORRADOR]
**Atributo de Calidad:** Modificabilidad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Desarrollador |
| **Estímulo** | Directiva de agregar una nueva función de pago |
| **Artefacto** | [INFERIDO] Código y componentes del flujo de pago |
| **Ambiente** | [INFERIDO] Tiempo de diseño (el cambio se hace sobre el código) |
| **Respuesta** | El desarrollador implementa la nueva función de pago, la prueba e incorpora el cambio al sistema sin alterar el comportamiento de las funciones de pago ya existentes |
| **Medida de Respuesta** | [INCOMPLETO] El requerimiento pide agregar el código “fácil” y “sin romper todo”; esos adjetivos no admiten un veredicto pasa/no-pasa |

> **Nota de Auditoría:** Lo que importa es el costo y el riesgo de acomodar el cambio, no el comportamiento en ejecución. “Fácil” no es una medida. “Sin romper todo” apunta a no romper el pago que ya existe, pero todavía falta una métrica con unidad y umbral.

### ⚠️ Faltan Datos para Estructurar el Escenario

Para poder generar un escenario SEI preciso, necesito que me aclares lo siguiente:

1. **Artefacto**: Tomé como afectado el código del flujo de pago. ¿Es ese el alcance, o también entran pasarelas externas, configuración o datos?
2. **Ambiente**: Asumí que el medio de pago se agrega en tiempo de diseño, tocando código. ¿Debe poder incorporarse en runtime (por configuración) sin recompilar?
3. **Medida de Respuesta**: “Fácil” no se puede testear. Elegí cómo medir el éxito; el umbral lo definís vos:
   - Esfuerzo para implementar, probar e incorporar la nueva función de pago, en **persona-horas**.
   - Cantidad de **módulos o artefactos ya existentes** que hay que modificar (conteo), además de los archivos nuevos.
   - Cantidad de **defectos de regresión** en funciones de pago preexistentes tras el cambio (conteo), medidos en las pruebas posteriores.

*Por favor, responde a estas preguntas para generar la tabla.*

---

## 2. Asistente virtual las 24 hs

[ESTADO: BORRADOR]
**Atributo de Calidad:** Disponibilidad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | No afirmada |
| **Estímulo** | No se describió un evento concreto: el requerimiento fija que el asistente virtual esté disponible las 24 horas |
| **Artefacto** | El asistente virtual (chatbot) |
| **Ambiente** | [INFERIDO] Operación continua, en cualquier momento del día |
| **Respuesta** | El asistente virtual permanece accesible y puede atender consultas |
| **Medida de Respuesta** | [INCOMPLETO] “Las 24 hs” indica la ventana de servicio, no un umbral cuantificable (porcentaje de uptime, tiempo de reparación o duración máxima de una caída) |

> **Nota de Auditoría:** “Las 24 hs” dice cuándo debe prestar servicio, no cuánto downtime se tolera. Sin el evento que pone a prueba el asistente (una falla, una consulta de madrugada) no se puede verificar el compromiso.

### ⚠️ Faltan Datos para Estructurar el Escenario

Para poder generar un escenario SEI preciso, necesito que me aclares lo siguiente:

1. **Fuente y estímulo**: ¿Qué evento pone a prueba el asistente? ¿Una falla (caída del chatbot, de la red o del servidor; interna o externa) o una consulta de un usuario a cualquier hora?
2. **Ambiente**: Asumí operación continua las 24 horas. ¿El mismo compromiso vale en mantenimiento programado o en modo degradado?
3. **Medida de Respuesta**: La ventana de 24 hs no alcanza para un pasa/no-pasa. Elegí cómo medir el éxito; el umbral lo definís vos:
   - **Porcentaje de disponibilidad** del asistente en un período (por ejemplo, mensual), en %.
   - **Tiempo de reparación** hasta restaurar el servicio tras una caída, en minutos (promedio o percentil).
   - **Duración máxima de una indisponibilidad continua** permitida, en minutos.

*Por favor, responde a estas preguntas para generar la tabla.*