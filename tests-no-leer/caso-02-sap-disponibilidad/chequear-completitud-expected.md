# Caso 02 (SAP — Disponibilidad) — output esperado de skill-chequear-completitud-escenario

## Veredicto

**Incompleto.** 5 de las 6 partes cumplen el criterio de completitud de `docs/conocimiento/rubric-completitud.md`. Falla la parte 6.

## Detalle por parte

| Parte | Estado | Motivo |
|---|---|---|
| Fuente del estímulo | ✅ Completa | Identifica un artefacto concreto (un servidor de la granja). |
| Estímulo | ✅ Completa | Condición concreta y desafiante (falla del servidor). |
| Artefacto | ✅ Completa | Señala el servidor específico. |
| Entorno | ✅ Completa | Indica operación normal. |
| Respuesta | ✅ Completa | Acción observable conectada al atributo (informa al operador y sigue operando). |
| Medida de la respuesta | ❌ **Incompleta** | "El sistema responde bien ante la falla" es un adjetivo cualitativo ("bien") sin ningún número, porcentaje o unidad asociada. Según la rúbrica, un adjetivo cualitativo nunca es suficiente por sí solo. |

## Sugerencia de corrección

A diferencia de un caso donde falta el dato y hay que asumirlo, acá el valor correcto **es conocido** (viene del ejemplo original del libro, Figura 4.1 de Software Architecture in Practice) — no hace falta inferir ni marcar una asunción:

> Medida de la respuesta (corregida): sin tiempo de inactividad (*no downtime*).
