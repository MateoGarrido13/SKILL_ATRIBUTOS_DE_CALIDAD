[ESTADO: VALIDADO]
**Atributo de Calidad:** Modificabilidad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Desarrollador |
| **Estímulo** | Directiva de agregar una nueva función de pago, incluida la integración con una pasarela externa (por ejemplo, Mercado Pago) |
| **Artefacto** | Código y componentes del flujo de pago, e interfaces de integración con las pasarelas externas |
| **Ambiente** | En desarrollo, antes de que el sistema esté en producción (tiempo de diseño) |
| **Respuesta** | El desarrollador implementa la nueva función de pago, la prueba e incorpora el cambio al sistema sin alterar el comportamiento de las funciones de pago ya existentes |
| **Medida de Respuesta** | Se modifican a lo sumo 8 módulos de código fuente ya existentes. Los módulos o archivos nuevos creados para la pasarela no entran en el conteo |

> **Nota de Auditoría:** Se unificó la unidad de la medida: solo módulos de código fuente existentes, umbral 8. Elección: Medida opción 1.
