[ESTADO: VALIDADO]
**Atributo de Calidad:** Disponibilidad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | El proceso del asistente virtual (falla interna de software) |
| **Estímulo** | Caída (crash) del proceso |
| **Artefacto** | El asistente virtual (chatbot) |
| **Ambiente** | Mantenimiento (modo reparación) |
| **Respuesta** | El sistema detecta la caída, habilita un conjunto acotado de preguntas por defecto e informa que hay inconvenientes |
| **Medida de Respuesta** | El modo acotado (preguntas por defecto + aviso) queda accesible en menos de 5 minutos en cada incidente (máximo). El asistente completo puede tardar más |

> **Nota de Auditoría:** Se corrigió la fuente (proceso interno de software, no “una falla” genérica), el ambiente (un solo modo: mantenimiento) y la medida (tiempo de accesibilidad del modo acotado, estadístico máximo, umbral 5 minutos). Elecciones: Fuente opción 1, Ambiente opción 3, Medida opción 3.
