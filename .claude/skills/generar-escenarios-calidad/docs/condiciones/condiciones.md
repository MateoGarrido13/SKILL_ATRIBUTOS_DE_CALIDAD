# Condiciones de proceso — skill-generar-escenarios-calidad

> Reglas destiladas de errores concretos cometidos al resolver los ejercicios 1 y 2 del TP (fuente: Notion, página "Atributos de Calidad (Tips Prácticos)"). Redactadas como checklist de cosas a **evitar al construir** un escenario desde cero o a partir de un enunciado.

## Al clasificar / elegir el atributo de calidad

1. **No marques como atributo de calidad una condición que en realidad es una regla de negocio.** Antes de anclar un candidato, preguntate qué se rompe si sacás esa condición: si se rompe una regla de negocio puntual (facturación, alta de usuario, integridad de un dato de formulario) es funcional, no QA. Solo es atributo de calidad si se rompe una propiedad general del sistema (tiempo, escala, tolerancia a fallos, facilidad de uso) o si un actor podría aprovecharse de que la condición no exista.

2. **No clasifiques algo como Seguridad solo porque "causa un problema" si falla.** Antes de etiquetar Seguridad, nombrá explícitamente el actor adversario que intenta engañar, falsificar o suplantar algo. Si el sistema solo está validando un dato que asume real (sin que nadie intente falsificarlo), es una validación funcional, no Seguridad.

3. **No fuerces un atributo de calidad sobre un caso de uso base sin ninguna tensión.** Antes de cerrar un candidato, intentá completar mentalmente las 6 partes del template. Si no aparece un estímulo específico ni una medida de respuesta natural, agregá la variable que estresa la funcionalidad (carga, pérdida de conexión, dificultad del usuario) en vez de forzar el QA sobre el caso normal.

4. **No te quedes buscando el atributo de calidad como una oración textual explícita en el enunciado.** Si después de revisar varios párrafos no aparece ningún QA "textual", cambiá de método: repasá el sistema completo contra preguntas generales (¿qué pasa con muchos usuarios/dispositivos a la vez? ¿y si se pierde la conexión? ¿hay datos sensibles o dinero de por medio? ¿qué podría necesitar cambiar a futuro? ¿hay usuarios con necesidades particulares?) y anclá cada candidato en una característica concreta del sistema, no en una frase suelta.

5. **No dupliques un mismo hecho del enunciado en dos atributos de calidad distintos sin diferenciarlos.** Si dos atributos podrían justificarse con la misma frase, buscá una evidencia distinta en el enunciado para cada uno (por ejemplo: estado actual → Disponibilidad; historial/auditoría → Observabilidad). Si no hay evidencia distinta y el atributo nuevo se superpone con uno clásico, fusionalos o dejá explícito por escrito por qué conviene tratarlos aparte (tácticas o preocupación distintas).

## Al armar el escenario de 6 partes

6. **No redactes un estímulo genérico o amplio.** El estímulo tiene que ser una condición concreta y específica, porque la respuesta se deriva de él. Si la respuesta que se te ocurre serviría para casi cualquier estímulo, el estímulo necesita ser más concreto (ej.: no "interactuar con el sistema", sí "intenta realizar una operación sin asistencia").

7. **No escribas una respuesta puramente funcional que no conecte con el atributo.** La respuesta tiene que ser la reacción específica al problema que plantea el estímulo (la dificultad de uso, el fallo, la carga), no la operación normal que el sistema hace siempre. Si la respuesta describe lo que el sistema hace en cualquier caso, falta conectarla con la preocupación de calidad en juego.

8. **No confundas la respuesta observable con la táctica técnica de solución.** La respuesta debe describir lo que percibe quien generó el estímulo, no el mecanismo interno de arquitectura con el que el sistema lo logra. Si la respuesta describe una acción sobre la infraestructura o el sistema mismo (se expande, se reconfigura, se redimensiona), reformulala en términos del resultado observable (tiempo de respuesta, disponibilidad, etc.) y dejá la táctica para la etapa de diseño de la solución.
