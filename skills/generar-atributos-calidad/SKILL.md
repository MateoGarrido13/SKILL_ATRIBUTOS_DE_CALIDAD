---
name: atributos-calidad-sei
description: 'Genera escenarios de atributos de calidad (quality attribute scenarios) siguiendo el template de 6 partes del SEI (Software Engineering Institute) — fuente del estímulo, estímulo, artefacto, entorno, respuesta y medida de respuesta. Usar esta skill cada vez que el usuario pida definir, documentar o revisar atributos de calidad, requisitos no funcionales (RNF/NFR), escenarios de calidad, "quality attribute scenarios" o "QAS", o mencione SEI, ATAM, CBAM, o arquitectura de software y necesite formalizar cómo se comporta un sistema ante un estímulo (carga, fallo, ataque, cambio, usuario). También aplica si el usuario pide "definir criterios de aceptación para performance/seguridad/disponibilidad" u otro atributo, o si da un requisito redactado en lenguaje natural y vago ("debe ser rápido", "tiene que ser seguro") que conviene refinar y hacer verificable, aunque no use el término "escenario de calidad" explícitamente.'
---

# Escenarios de atributos de calidad (template SEI de 6 partes)

Esta skill ayuda a redactar escenarios de atributos de calidad bien formados, siguiendo el formato estándar del SEI (usado en ATAM y en general en diseño de arquitectura de software). Un escenario mal escrito ("el sistema debe ser rápido") no es verificable; el objetivo de esta skill es siempre producir escenarios concretos y medibles.

## Las 6 partes del template

Cada escenario completo tiene estas seis partes. Nunca omitas ninguna — un escenario sin medida de respuesta, por ejemplo, no es verificable y deja de ser útil como requisito.

1. **Fuente del estímulo** — quién o qué genera el estímulo (un usuario, un sistema externo, un componente interno, un atacante, el propio equipo de desarrollo, un pico de carga, un fallo de hardware, etc.)
2. **Estímulo** — la condición que llega al sistema y dispara la respuesta (una petición, una caída de un servicio, un intento de acceso no autorizado, un cambio de requisito, etc.)
3. **Artefacto** — la parte del sistema afectada (todo el sistema, un servicio, la base de datos, la interfaz, un módulo específico, etc.)
4. **Entorno** — el estado del sistema cuando ocurre el estímulo (operación normal, sobrecarga, arranque, modo degradado, horario pico, etc.). Muchas veces el comportamiento esperado cambia según el entorno, así que no lo des por sentado.
5. **Respuesta** — qué debe hacer el sistema (procesar la petición, alertar a un operador, aislar el componente fallido, registrar el evento, rechazar el acceso, etc.)
6. **Medida de respuesta** — el criterio numérico o verificable que determina si la respuesta fue aceptable (tiempo, porcentaje, throughput, etc.). Esta es la parte que más se olvida y la más importante: sin ella el escenario no se puede probar ni usar como criterio de aceptación.

## Cómo generar los escenarios

0. **Si el usuario da un requisito ya redactado en lenguaje natural** (un párrafo de RNF, una frase tipo "debe ser rápido", "tiene que ser seguro", "debe soportar muchos usuarios"), no lo repitas ni lo completes a ciegas: mapealo explícitamente a las 6 partes antes de escribir la tabla final.
   - Identificá qué partes ya están en el texto (aunque sea de forma implícita) y cuáles faltan.
   - Prestá atención a entornos escondidos: frases como "especialmente en períodos de X" o "salvo cuando Y" suelen indicar que el comportamiento esperado cambia según el entorno, y eso normalmente amerita separar el requisito en dos o más escenarios (uno por entorno) en vez de forzarlo en uno solo.
   - La medida de respuesta casi nunca está en el texto original ("rápido", "seguro", "soportar carga" no son verificables) — es la parte que más frecuentemente hay que completar vos. Proponé un valor razonable pero marcalo como supuesto, no lo presentes como si viniera del usuario.
   - Mostrale al usuario, brevemente, cómo mapeaste el texto original a las 6 partes antes o junto con la tabla — eso es lo que hace que el refinamiento sea auditable y no una caja negra.

1. **Determiná el alcance.** Si el usuario ya describió un sistema concreto (dominio, arquitectura, restricciones), generá los escenarios anclados a ese contexto — usá nombres reales de componentes, cargas esperadas, SLAs mencionados, etc. Si el pedido es genérico ("dame ejemplos de escenarios de disponibilidad"), generá escenarios de referencia reutilizables, y aclaralo brevemente para que el usuario sepa que son punto de partida y no requisitos ya validados.

2. **Identificá los atributos relevantes.** Si el usuario no especificó cuáles atributos de calidad le interesan, preguntale (o, si el contexto ya lo deja claro — por ejemplo, "es un sistema de pagos" sugiere seguridad y disponibilidad — proponé los más relevantes y decilo). Ver `references/ejemplos-por-atributo.md` para ejemplos ya trabajados de los atributos clásicos (rendimiento, disponibilidad, seguridad, modificabilidad, usabilidad, testabilidad, interoperabilidad, entre otros) — usalos como inspiración de estructura y de qué tipo de estímulos/medidas son típicos de cada atributo, no los copies literalmente si el usuario tiene un sistema específico en mente.

3. **Generá entre 2 y 4 escenarios por atributo**, salvo que el usuario pida una cantidad distinta. Cubrí variedad de fuentes de estímulo y de entornos (no repitas siempre "usuario final en operación normal"): pensá también en fallos, picos de carga, cambios de requisitos, ataques, condiciones de arranque/apagado, según corresponda al atributo.

4. **Verificá cada escenario antes de entregarlo:** ¿la medida de respuesta es un número o condición verificable (no "rápido", "seguro", "aceptable")? ¿el estímulo es específico (no "el sistema recibe carga", sino "1000 usuarios concurrentes hacen checkout")? Si algo quedó vago, ajustalo antes de mostrarlo.

## Formato de salida

Por defecto, entregá los escenarios como una tabla en Markdown, agrupada por atributo de calidad, con estas columnas exactas en este orden:

```markdown
### [Nombre del atributo de calidad]

| # | Fuente del estímulo | Estímulo | Artefacto | Entorno | Respuesta | Medida de respuesta |
|---|---|---|---|---|---|---|
| 1 | ... | ... | ... | ... | ... | ... |
| 2 | ... | ... | ... | ... | ... | ... |
```

Si el usuario pide explícitamente un documento Word (.docx) o un archivo descargable, generá esa tabla dentro de un .docx usando la skill docx (revisá su SKILL.md antes de crear el archivo) en lugar de mostrarla en el chat. La tabla de 6-7 columnas casi siempre necesita orientación horizontal para no cortarse — usá `orientation: PageOrientation.LANDSCAPE` y recordá el gotcha de la skill docx: hay que pasar las dimensiones de página en *portrait* (`width` < `height`), no en landscape, porque docx-js las invierte internamente; si pasás dimensiones ya invertidas la página termina en portrait. Verificá siempre el resultado renderizando a imagen antes de entregarlo, como indica la skill docx.

Después de la tabla, agregá como máximo 2-3 líneas de comentario si hay algo que el usuario debería revisar o decidir (por ejemplo, un valor de SLA que asumiste porque no se especificó) — no repitas el contenido de la tabla en prosa.
