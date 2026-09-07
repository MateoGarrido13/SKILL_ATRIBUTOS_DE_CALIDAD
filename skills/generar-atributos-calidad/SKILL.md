---
name: generar-escenarios-calidad
description: Genera escenarios de atributos de calidad usando el template de 6 partes del SEI (fuente del estímulo, estímulo, artefacto, ambiente, respuesta, medida de la respuesta) a partir de una descripción de sistema o requerimiento en lenguaje natural. Usar esta skill cuando el usuario pida redactar, generar o completar un escenario de calidad, cuando mencione atributos de calidad (performance, disponibilidad, seguridad, modificabilidad, usabilidad, escalabilidad, etc.) sobre un sistema concreto, o cuando pida aplicar el "template de 6 partes" o "quality attribute scenario" del SEI.
---

# Generar escenarios de atributos de calidad

## Qué hace

Dado un input del usuario (una descripción de sistema, un requerimiento
suelto, o un enunciado de TP), la skill sigue esta secuencia:

### 1. Determinar el/los atributo(s) de calidad a cubrir

- **Si el usuario ya indicó el atributo explícitamente** (ej. "necesito un
  escenario de disponibilidad"), usarlo directamente, sin reclasificar.
- **Si no lo indicó y aparece de forma textual en el input** (el
  enunciado nombra "performance", "seguridad", "disponibilidad", etc.,
  o describe un caso claramente asociado a uno), identificarlo
  directamente.
- **Si no aparece nada textual**, aplicar el método de la **regla 4**
  de `docs/reglas.md` (Parte 2): recorrer el
  sistema contra preguntas generales — ¿qué pasa con muchos
  usuarios/dispositivos a la vez? ¿y si se pierde la conexión? ¿hay
  datos sensibles o dinero de por medio? ¿qué podría necesitar cambiar
  a futuro? ¿hay usuarios con necesidades particulares? — y anclar el
  candidato en una característica concreta del sistema, no en una frase
  suelta. Nunca asumir el atributo en silencio: proponer el más
  probable y decirlo explícitamente antes de continuar.
- **Antes de cerrar cualquier candidato**, aplicar las reglas 1–3 de
  `docs/reglas.md` (Parte 2): descartarlo si en realidad es una
  regla de negocio disfrazada de QA (regla 1), si se etiquetó Seguridad
  sin un actor adversario nombrado (regla 2), o si se está forzando un
  atributo sobre un caso de uso base sin ninguna tensión real (regla 3).
- **Si el input sugiere más de un atributo relevante**, tratarlos como
  escenarios separados (ver paso 5), aplicando la **regla 5** para no
  justificar dos atributos distintos con el mismo hecho del enunciado
  sin evidencia diferenciada.

### 2. Extraer o inferir cada una de las 6 partes por separado

No redactar el escenario de corrido: ir parte por parte — **Fuente del
estímulo, Estímulo, Artefacto, Ambiente, Respuesta, Medida de la
respuesta** — siguiendo `docs/template-6-partes-sei.md`.

Ese documento describe dos caminos según el tipo de input:

- **Camino 1 (desarmar):** el input ya es una oración con el
  requerimiento escrito — identificar qué fragmento responde cada una
  de las 6 preguntas.
- **Camino 2 (armar desde cero):** el input es una preocupación de
  negocio suelta, sin oración lista — construir cada parte desde el
  contexto del sistema.

Tener presente que el estímulo no siempre es un fallo: solo típicamente
en Disponibilidad. En Performance/Usabilidad puede ser una carga de uso
exigente, en Modificabilidad un pedido de cambio, en Seguridad una
amenaza — no forzar la búsqueda de "algo roto" cuando el atributo no es
Disponibilidad.

### 3. Completar las partes que el input no menciona explícitamente

Aplicar `docs/reglas.md` (Parte 1 — Al
construir). Cada parte faltante se completa con un valor concreto y
cuantificable, marcado siempre como:

> *Asunción: se estimó [valor] por [justificación breve basada en el
> contexto del sistema].*

Nunca marcar como asunción algo que el input ya especificaba. Si el
input da pistas parciales (ej. "sistema crítico", "muchos usuarios"),
usarlas para justificar la asunción en vez de inventar un número
desconectado del contexto.

Especial cuidado con la **Medida de la respuesta** — es la parte que
más rompe la testeabilidad si queda vaga (regla 9, Parte 2). Para
atributos donde lo relevante es cuánto se degrada algo a medida que
crece la carga (típicamente Escalabilidad), preferir una medida
combinada: un valor **relativo** (ej. "no más de 10% peor que el
baseline") junto con un piso **absoluto** (ej. "y en ningún caso por
encima de 500 ms"), tal como lo ilustra el ejemplo de escalabilidad de
`docs/template-6-partes-sei.md`.

**Umbral para preguntar en vez de asumir:** si al llegar a este paso
**3 o más de las 6 partes** quedarían marcadas como asunción, no
completar todo en silencio — señalar al usuario qué información falta y
pedirle que la complete, en vez de entregar un escenario armado casi
enteramente sobre supuestos. Con 1 o 2 partes faltantes, seguir el
mecanismo normal de asunción explícita.

### 4. Validar el escenario contra las reglas de proceso

Validar las partes ya extraídas en el paso 2 (no hace falta esperar a
la tabla final del paso 5) contra
`docs/reglas.md` (Parte 2 — Al validar):

- El estímulo no es genérico ni serviría para casi cualquier respuesta
  (regla 6).
- La respuesta es la reacción específica a la preocupación de calidad,
  no la operación normal del sistema (regla 7).
- La respuesta describe un resultado observable por quien generó el
  estímulo, no una táctica de arquitectura (expandirse, reconfigurarse,
  conmutar a un componente de respaldo) — reformular en términos de
  resultado si hace falta (regla 8).
- La Medida de la respuesta es cuantificable: número, rango, porcentaje
  o umbral verificable — nunca una palabra suelta como "rápido" o
  "aceptable" (regla 9).

Si el escenario cae en alguno de estos errores, corregirlo antes de
pasar al paso 5.

### 5. Redactar el escenario en el formato de salida fijo

Una tabla markdown de dos columnas `Parte | Valor`, con exactamente
estas 6 filas, en este orden y con estos nombres exactos (tal como
aparecen en `docs/template-6-partes-sei.md`):

| Parte | Valor |
|---|---|
| Fuente del estímulo | ... |
| Estímulo | ... |
| Artefacto | ... |
| Ambiente | ... |
| Respuesta | ... |
| Medida de la respuesta | ... |

Después de la tabla, agregar opcionalmente la oración final que une las
6 partes en un escenario legible, siguiendo el estilo de los ejemplos
de `docs/template-6-partes-sei.md` (ej. el de Cyber Monday o el de
Escalabilidad IoT).

**Si el paso 1 determinó más de un atributo relevante**, repetir esta
tabla una vez por atributo — una tabla completa por escenario, cada una
con su propio encabezado indicando a qué atributo corresponde — nunca
mezclar dos atributos en una sola tabla.

## Fuente de conocimiento base

- `docs/template-6-partes-sei.md` — estructura y
  terminología canónica de las 6 partes, los dos caminos (desarmar /
  armar desde cero), y ejemplos de referencia por atributo, incluida
  Escalabilidad (pasos 2 y 5).
- `docs/reglas.md` — mecanismo de inferencia
  con asunción explícita (Parte 1, paso 3) y las 9 reglas de validación
  (Parte 2, pasos 1 y 4).

## Reglas de proceso

- `docs/reglas.md`, Parte 2 — 9 reglas de
  "evitar al construir", aplicadas en los pasos 1 y 4.
