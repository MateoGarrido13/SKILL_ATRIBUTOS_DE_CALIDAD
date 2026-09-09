Ninguno de los dos se sella: en ambos hay partes que impiden un pasa/no-pasa. No adelanto la tabla final. Elegí una opción por cada punto (o proponé otra) y reaudito.

---

## Asistente virtual 24 hs — Disponibilidad

### 🔍 Resultado de la Auditoría: CON AMBIGÜEDADES

Se detectaron los siguientes puntos que impiden verificar el requerimiento:
- **Fuente del Estímulo**: “Una falla que provoca la caída” nombra el evento, no la entidad que lo genera (software, hardware, infraestructura o personas). Tampoco dice si es interna o externa. Sin eso no se puede decidir cómo tratar la caída del asistente (reintento del propio proceso, failover de infraestructura, etc.) ni reproducir la prueba.
- **Ambiente**: Quedan mezclados tres modos a la vez (consultas concurrentes a las 23:00, degradado y mantenimiento). La respuesta esperada cambia según el modo: restaurar el asistente no es lo mismo que atender un conjunto acotado de preguntas. “Múltiples consultas” tampoco fija un volumen, así que no se sabe bajo qué carga hay que cronometrar los 5 minutos.
- **Medida de Respuesta**: “Menos de 5 minutos” tiene umbral, pero es una medida de tiempo sin estadístico (promedio, mediana, percentil o máximo por incidente). Además no queda claro si “disponible” es el asistente completo o alcanza el modo de preguntas por defecto. Sin eso, dos pruebas del mismo incidente pueden dar veredictos opuestos.

**Opciones de Resolución — Fuente (Elige una o propone otra):**
- [ ] **Opción 1:** Falla interna de software: el proceso del asistente virtual se cae (crash).
- [ ] **Opción 2:** Falla externa de infraestructura: red, hosting o canal de mensajería deja de responder.
- [ ] **Opción 3:** Falla interna de hardware: el procesador o el nodo que ejecuta el asistente deja de operar.

**Opciones de Resolución — Ambiente (Elige una o propone otra):**
- [ ] **Opción 1:** Un solo modo: operación sobrecargada a las 23:00 (consultas concurrentes). El recuento de consultas concurrentes lo fijás vos con el volumen real de esa hora.
- [ ] **Opción 2:** Un solo modo: operación degradada, con el conjunto acotado de preguntas por defecto y el aviso de inconvenientes.
- [ ] **Opción 3:** Un solo modo: mantenimiento (modo reparación), con el mismo servicio acotado y el aviso de inconvenientes.

**Opciones de Resolución — Medida (Elige una o propone otra):**
- [ ] **Opción 1:** Tiempo de reparación del asistente completo &lt; 5 minutos en cada incidente (máximo / peor caso).
- [ ] **Opción 2:** Tiempo de reparación del asistente completo &lt; 5 minutos en el percentil 95.
- [ ] **Opción 3:** El modo acotado (preguntas por defecto + aviso) queda accesible en &lt; 5 minutos en cada incidente (máximo); el asistente completo puede tardar más.

*Indícame qué opción aplicamos para generar el escenario validado.*

---

## Nuevas funciones de pago — Modificabilidad

### 🔍 Resultado de la Auditoría: CON AMBIGÜEDADES

Se detectaron los siguientes puntos que impiden verificar el requerimiento:
- **Medida de Respuesta**: El umbral 8 está, pero la unidad es “módulos y/o artefactos”. Módulo de código, interfaz, configuración o caso de prueba no son la misma cosa: si tocás 6 módulos y 4 archivos de configuración, no hay veredicto pasa/no-pasa. Hasta que el conteo tenga una sola unidad, no se puede testear la incorporación de Mercado Pago.

**Opciones de Resolución (Elige una o propone otra):**
- [ ] **Opción 1:** Contar solo módulos de código fuente ya existentes que se modifican: umbral ≤ 8 módulos. Los archivos nuevos de la pasarela no suman.
- [ ] **Opción 2:** Contar todos los artefactos tocados (código, interfaces, configuración y pruebas): umbral ≤ 8 artefactos, incluidos los nuevos.
- [ ] **Opción 3:** Contar módulos existentes más los módulos nuevos creados para la pasarela: umbral ≤ 8 módulos en total.

*Indícame qué opción aplicamos para generar el escenario validado.*