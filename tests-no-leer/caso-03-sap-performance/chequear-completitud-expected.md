# Caso 03 (SAP — Performance) — output esperado de skill-chequear-completitud-escenario

## Veredicto

**Incompleto.** 5 de las 6 partes cumplen el criterio de completitud de `docs/conocimiento/rubric-completitud.md`. Falla la parte 2.

## Detalle por parte

| Parte | Estado | Motivo |
|---|---|---|
| Fuente del estímulo | ✅ Completa | Identifica un actor concreto (los usuarios del sistema). |
| Estímulo | ❌ **Incompleta** | "Muchos usuarios usan el sistema al mismo tiempo" es una descripción vaga de interacción, sin una condición desafiante concreta y cuantificada (no dice cuántos usuarios, cuántas solicitudes, ni en qué intervalo de tiempo). |
| Artefacto | ✅ Completa | Señala el sistema como artefacto afectado. |
| Entorno | ✅ Completa | Indica operación normal. |
| Respuesta | ✅ Completa | Acción observable conectada al atributo (procesa todas las solicitudes). |
| Medida de la respuesta | ✅ Completa | Cuantificable ("latencia promedio de 2 segundos"). |

## Sugerencia de corrección

A diferencia de un caso donde el dato no existe y hay que asumirlo, acá el valor correcto **es conocido** (viene del ejemplo original del libro, Figura 9.1 de Software Architecture in Practice) — no hace falta inferir ni marcar una asunción:

> Estímulo (corregido): 500 usuarios inician 2000 solicitudes en un intervalo de 30 segundos.
