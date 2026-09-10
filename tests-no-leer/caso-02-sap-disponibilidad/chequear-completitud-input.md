# Caso 02 (SAP — Disponibilidad) — input para skill-chequear-completitud-escenario

> Basado en Software Architecture in Practice (Bass/Clements/Kazman), Figura 4.1, con la Medida de la respuesta deliberadamente vuelta vaga (reemplazada por una frase cualitativa) para que la skill detecte la falta de cuantificación.

## Escenario a chequear

**Atributo de calidad declarado:** Disponibilidad — falla de servidor en operación normal.

| Parte | Valor |
|---|---|
| Fuente del estímulo | Un servidor (dentro de una granja de servidores / server farm) |
| Estímulo | El servidor falla |
| Artefacto | El servidor (Server) |
| Entorno | Operación normal |
| Respuesta | El sistema informa al operador y continúa operando |
| Medida de la respuesta | El sistema responde bien ante la falla |

## Consigna para la skill

Evaluar si las 6 partes están completas según `docs/conocimiento/rubric-completitud.md`. Si alguna no lo está, señalar cuál y sugerir cómo completarla.
