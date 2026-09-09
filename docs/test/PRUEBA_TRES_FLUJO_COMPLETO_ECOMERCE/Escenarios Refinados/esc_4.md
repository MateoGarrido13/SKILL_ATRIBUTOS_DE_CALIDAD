[ESTADO: VALIDADO]
**Atributo de Calidad:** Seguridad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Usuario malintencionado, externo y no confiable |
| **Estímulo** | Intento malintencionado de acceso al panel de administración |
| **Artefacto** | Panel de administración centralizado |
| **Ambiente** | Operación normal |
| **Respuesta** | Exige un control de doble factor (reconocimiento facial y confirmación por email) antes de conceder el acceso |
| **Medida de Respuesta** | El 100% de los intentos de acceso al panel sin ambos factores (facial y email) son rechazados |

> **Nota de Auditoría:** Elegiste la opción 1: la medida pasa a ser el 100% de rechazos cuando faltan el facial o el email. Se mantienen fuente (atacante externo), estímulo (acceso al panel), artefacto (panel centralizado), ambiente (operación normal) y la respuesta de doble factor.
