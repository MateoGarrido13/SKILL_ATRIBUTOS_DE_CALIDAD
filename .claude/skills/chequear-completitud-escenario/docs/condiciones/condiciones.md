# Condiciones de proceso — skill-chequear-completitud-escenario

> Reglas destiladas de errores concretos cometidos al resolver los ejercicios 1 y 2 del TP (fuente: Notion, página "Atributos de Calidad (Tips Prácticos)"). Redactadas como criterios de **detección** al revisar un escenario ya redactado.

## Al chequear la clasificación / el atributo elegido

1. **Detectá si lo que se etiquetó como atributo de calidad es en realidad una regla de negocio.** Aplicá el test: ¿qué se rompe si se saca la condición del escenario? Si lo que se rompe es una regla de negocio puntual (facturación, alta de usuario, integridad de un dato de formulario), marcá el escenario como mal clasificado y sugerí tratarlo como requerimiento funcional, no como QA.

2. **Detectá si un escenario de Seguridad no tiene un adversario identificable.** Si el escenario no nombra (ni permite inferir) un actor que intente engañar, falsificar o suplantar algo, marcalo como mal clasificado — probablemente sea una validación de proceso disfrazada de escenario de Seguridad.

3. **Detectá si el escenario fuerza un atributo de calidad sobre un caso de uso sin tensión real.** Si al intentar separar las 6 partes no aparece un estímulo específico ni una medida de respuesta natural (todo se siente genérico o forzado), marcá el escenario como sospechoso de ser un funcional disfrazado de QA, y sugerí agregar la variable que estresa la funcionalidad (carga, fallo, dificultad del usuario).

4. **Detectá si el atributo elegido no está anclado en una característica concreta del sistema.** Si el escenario cita el nombre de un atributo de calidad pero no señala qué parte específica del enunciado o del sistema lo justifica, marcalo como incompleto y pedí que se identifique la característica concreta (integración externa, hardware distribuido, datos sensibles, necesidad de cambio futuro, etc.) que activa ese atributo.

5. **Detectá si dos escenarios distintos están anclados en el mismo hecho sin diferenciarse.** Si dos escenarios (del mismo sistema) usan la misma evidencia del enunciado para justificar atributos distintos, marcalos como potencialmente duplicados y pedí que cada uno señale una evidencia distinta, o que se fusionen, o que se explicite por qué conviene mantenerlos separados (tácticas o preocupación distintas).

## Al chequear las 6 partes del escenario

6. **Detectá si el Estímulo es demasiado genérico o vago.** Si el estímulo no permite anticipar una respuesta específica (cualquier respuesta "serviría"), marcá esa parte como incompleta y sugerí una condición concreta y acotada en su lugar.

7. **Detectá si la Respuesta es puramente funcional y no conecta con el atributo.** Si la respuesta describe algo que el sistema haría siempre, sin relación con el problema planteado por el estímulo (la dificultad de uso, el fallo, la carga), marcá esa parte como incompleta y pedí que se reformule como la reacción específica a esa preocupación de calidad.

8. **Detectá si la Respuesta describe una táctica técnica en vez de un resultado observable.** Si la respuesta habla de acciones sobre la infraestructura o el sistema mismo (se expande, se reconfigura, se redimensiona) en vez de lo que percibe quien generó el estímulo, marcá esa parte como mal formulada y sugerí reescribirla en términos del resultado observable (tiempo de respuesta, disponibilidad, etc.), dejando la táctica fuera del escenario.
