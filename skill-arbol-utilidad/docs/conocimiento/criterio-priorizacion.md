# Criterio de priorización — etiquetas H/M/L

`../../../shared/metodo-arbol-utilidad.md` define la estructura del árbol y las etiquetas (importancia de negocio, dificultad técnica), pero no cómo decidir el valor de cada una. Este archivo da el criterio operacional para asignarlas.

## Eje 1: Importancia para el negocio

| Etiqueta | Criterio |
|---|---|
| **H (High)** | La fuente del estímulo es un stakeholder crítico (usuario final, cliente pagador, regulación legal/normativa) **Y** la ausencia de esa capacidad bloquea el uso del sistema o genera pérdida directa de dinero/reputación/cumplimiento legal. |
| **M (Medium)** | Afecta la experiencia o eficiencia del sistema de forma perceptible, pero no bloquea su uso ni genera pérdida directa (el sistema sigue siendo utilizable, aunque peor). |
| **L (Low)** | Es deseable o mejora la calidad general, pero no es urgente ni tiene impacto directo si se pospone (ej. mejoras marginales de comodidad, casos de uso poco frecuentes). |

## Eje 2: Dificultad / riesgo técnico

| Etiqueta | Criterio |
|---|---|
| **H (High)** | Requiere cambios arquitectónicos transversales (afecta múltiples componentes o capas), o depende de tecnología/infraestructura no probada todavía en el proyecto, o su medida de respuesta es difícil de estimar sin un spike/prototipo (ver straw man en `../../../shared/glosario-atributos-calidad.md`). |
| **M (Medium)** | Se resuelve con tácticas conocidas y acotadas a uno o pocos componentes, con esfuerzo estimable con confianza razonable a partir de experiencia previa o benchmarks. |
| **L (Low)** | Se resuelve con una táctica simple y localizada (un solo componente, patrón ya usado en el sistema), de esfuerzo bajo y predecible. |

## Regla de combinación

1. Evaluar los dos ejes **de forma independiente** — no dejar que la dificultad técnica influya en la importancia de negocio ni viceversa.
2. Si hay ambigüedad entre dos niveles adyacentes (ej. ¿H o M?), preferir el nivel **más alto** cuando el escenario involucra dinero, datos sensibles o cumplimiento legal; preferir el nivel **más bajo** cuando la incertidumbre viene de falta de información (y dejarlo anotado para reconfirmar con el stakeholder, en línea con el straw man de `../../../shared/glosario-atributos-calidad.md`).
3. Un escenario **(H, H)** es candidato prioritario de atención arquitectural inmediata; un **(L, L)** puede posponerse sin riesgo en el diseño arquitectural inicial.

## Antes de asignar la etiqueta

Aplicar primero las validaciones de `../condiciones/condiciones.md` (el escenario debe ser un atributo de calidad real, bien anclado y bien diferenciado de otros solapados) — no tiene sentido priorizar un escenario mal clasificado.
