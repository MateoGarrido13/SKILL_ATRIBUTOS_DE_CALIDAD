# Caso 01 — monopatines — output esperado de skill-chequear-completitud-escenario

## Veredicto

**Incompleto.** 5 de las 6 partes cumplen el criterio de completitud de `docs/conocimiento/rubric-completitud.md`. Falla la parte 6.

## Detalle por parte

| Parte | Estado | Motivo |
|---|---|---|
| Fuente del estímulo | ✅ Completa | Identifica actores concretos (usuario / administrador). |
| Estímulo | ✅ Completa | Condición concreta (solicitud de ver monopatines en el mapa de su zona). |
| Artefacto | ✅ Completa | Señala el módulo de localización/mapa. |
| Entorno | ✅ Completa | Indica operación normal. |
| Respuesta | ✅ Completa | Acción observable conectada al atributo (muestra posición actualizada). |
| Medida de la respuesta | ❌ **Incompleta** | "Antigüedad menor a X segundos" no es cuantificable: "X" es un placeholder sin resolver, no un número. Un escenario no puede considerarse testeable mientras la medida de respuesta no tenga un valor concreto. |

## Sugerencia de corrección

Aplicando el criterio de asunción explícita de `heuristica-inferencia.md` (skill-generar-escenarios-calidad):

> Medida de la respuesta (corregida): la posición mostrada tiene una antigüedad menor a **30 segundos** en el 99% de las consultas.
> *Asunción: se estimó un umbral de 30 segundos por tratarse de un sistema de localización de vehículos en vía pública, donde una demora mayor haría que el usuario llegue al punto marcado y no encuentre el monopatín.*

Se recomienda confirmar este valor con el stakeholder (Administrador de Monopatines) antes de darlo por definitivo.
