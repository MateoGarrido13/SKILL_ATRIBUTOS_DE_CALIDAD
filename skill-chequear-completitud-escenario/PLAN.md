# PLAN — skill-chequear-completitud-escenario

## Problema que resuelve
Corresponde al inciso **ii)** del ejercicio 4 de TP3: "chequear si un escenario dado está completo, y en caso de no estarlo cómo podría completarse". Dado un escenario de atributo de calidad ya redactado (por el usuario, por otra skill, o extraído de un TP), la skill evalúa si las 6 partes del template del SEI están presentes y bien definidas, y si falta o está ambigua alguna, propone cómo completarla.

## Inputs que recibe del usuario
- El texto del escenario ya redactado (puede venir en formato libre o ya separado en las 6 partes).
- Opcionalmente, el atributo de calidad al que dice pertenecer el escenario, para poder chequear coherencia entre atributo declarado y contenido del escenario.

## Fuente de conocimiento base

| Fuente | Sección/página específica | Va a shared/ o a docs/conocimiento/ | Para qué sirve |
|---|---|---|---|
| Notion — página "Atributos de Calidad" (raíz + subpáginas) | Migrada casi íntegra | `shared/glosario-atributos-calidad.md` | Terminología y definiciones para verificar coherencia entre atributo declarado y contenido del escenario |
| Notion — página "Escenarios de Calidad: el Template SEI de 6 Partes" + subpáginas de ejercicios | Migrada casi íntegra | `shared/template-6-partes-sei.md` | Criterio formal de qué hace válida a cada una de las 6 partes |
| Notion — página "Atributos de Calidad (Tips Prácticos)" (8 subpáginas) | Migrada y luego distribuida | `docs/condiciones/condiciones.md` (los 8 tips, reescritos como criterios de detección) | Señales de mala clasificación del atributo o confusión entre partes, detectadas al corregir los ejercicios 1 y 2 |
| Redactado a medida (no viene de Notion ni del libro) | — | `docs/conocimiento/rubric-completitud.md` | Criterio POSITIVO de aceptación para cada una de las 6 partes (distinto de `docs/condiciones/`, que es negativo) |

**Nota:** *Software Architecture in Practice* (Bass/Clements/Kazman) es la fuente original del template de 6 partes, ya cubierta por completo en el Notion migrado — no hizo falta agregar complemento propio del libro. `caps4-5-keeling.pdf` (Design It!, Michael Keeling) no es fuente de ninguno de los archivos de `shared/`.

## Condiciones / reglas de la skill
Ver `docs/condiciones/condiciones.md`: los mismos 8 tips prácticos de Notion, reescritos como criterios de **detección** al revisar un escenario ya redactado ("marcá como incompleto/mal clasificado si..."), a diferencia de `skill-generar-escenarios-calidad` donde están redactados como reglas de "evitar al construir".

## Ejemplos conocidos (testeo)
El enunciado de TP3 pide explícitamente que "la skill debe ser testeada con ejemplos conocidos". Los casos de prueba viven en `tests-no-leer/` (la skill nunca debe leerlos) y son:

- **`caso-01-monopatines/`** (6 archivos) — usa `chequear-completitud-input.md` (el escenario 6.1 de Disponibilidad, con la medida de respuesta sin resolver: "menor a X segundos") / `chequear-completitud-expected.md` (veredicto Incompleto + corrección con asunción explícita).
- **`caso-02-sap-disponibilidad/`** (4 archivos) — Figura 4.1 del libro SAP, con la medida de respuesta deliberadamente vuelta vaga ("el sistema responde bien"). El veredicto esperado es Incompleto, con corrección **sin** marcar asunción (el valor correcto es conocido del libro, no inferido).
- **`caso-03-sap-performance/`** (4 archivos) — Figura 9.1 del libro SAP, con el Estímulo deliberadamente vuelto vago (para testear la detección sobre una parte distinta a la Medida). Veredicto esperado Incompleto, corrección sin asunción.
- `caso-04-sap-arbol-healthcare/` no aplica a esta skill (no incluye pares input/expected de chequeo de escenarios individuales).
