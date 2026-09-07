# Reglas y heurística — skill generar-escenarios-calidad

---

## Parte 1 — Al construir: heurística de inferencia

Qué hacer cuando el input del usuario (enunciado, descripción de sistema,
pedido suelto) no trae explícitas las 6 partes del template
(ver `docs/template-6-partes-sei.md`).

### Regla general

Si una parte no está explícita en el input:

1. **Proponé un valor razonable y cuantificable** en su lugar (no dejes la
   parte vacía ni la omitas del escenario final).
2. **Marcalo siempre como asunción propia**, nunca como si fuera un dato dado
   por el usuario. Formato fijo:
   > *Asunción: se estimó [valor] por [justificación breve basada en el
   > contexto del sistema].*
3. **Priorizá inferir con cuidado la Medida de Respuesta** por sobre las
   demás partes — es la que más rompe la testeabilidad del escenario si
   queda vaga o ausente. Ver **Parte 2, regla 9** de este mismo documento
   para el criterio de "cuantificable".
4. Si el input da pistas parciales (ej. menciona "sistema crítico" o "muchos
   usuarios"), usalas para justificar la asunción — no inventes un número
   desconectado del contexto.
5. Nunca marques como asunción algo que el input sí especificaba — eso sería
   subestimar información real dada por el usuario.

### Ejemplos del mecanismo (no son casos de test completos)

**Ejemplo 1 — falta la Medida de Respuesta:**

Input: "El sistema de alarmas debe notificar al operador ante una falla del
sensor principal."

Fragmento del escenario generado:

> Respuesta: el operador recibe la notificación de la falla y mantiene
> visibilidad continua del estado del sensor.
> Medida de respuesta: *Asunción: se estimó un tiempo de notificación menor
> a 2 segundos, por tratarse de un sistema de alarmas críticas donde la
> demora en notificar un fallo tiene consecuencias directas sobre la
> seguridad.*

> **Nota de corrección:** la versión anterior de este ejemplo describía la
> respuesta como "el sistema notifica al operador y activa el sensor de
> respaldo" — eso es una táctica de arquitectura (conmutación a un sensor
> redundante), no el resultado observable por quien generó el estímulo.
> Ver Parte 2, regla 8.

**Ejemplo 2 — falta el Ambiente:**

Input: "Un usuario pide cambiar su método de pago."

Fragmento del escenario generado:

> Ambiente: *Asunción: se asumió que el cambio ocurre en tiempo de ejecución
> (runtime), ya que no se menciona ningún proceso de mantenimiento o
> downtime programado para esta operación.*

### Qué NO hacer

- No dejar una parte del template vacía "porque no está en el input" —
  siempre proponer algo, marcado como asunción.
- No mezclar la asunción dentro de la oración del escenario sin marcarla —
  debe quedar identificable por separado.
- No inventar una asunción sin ligarla a algo del contexto dado (dominio del
  sistema, criticidad, tipo de usuario, etc.).

---

## Parte 2 — Al validar: condiciones de proceso

> Reglas destiladas de errores concretos cometidos al resolver los
> ejercicios 1 y 2 del TP (fuente: Notion, página "Atributos de Calidad
> (Tips Prácticos)"). Redactadas como checklist de cosas a **evitar al
> construir** un escenario desde cero o a partir de un enunciado.

### Al clasificar / elegir el atributo de calidad

1. **No marques como atributo de calidad una condición que en realidad es
   una regla de negocio.** Antes de anclar un candidato, preguntate qué se
   rompe si sacás esa condición: si se rompe una regla de negocio puntual
   (facturación, alta de usuario, integridad de un dato de formulario) es
   funcional, no QA. Solo es atributo de calidad si se rompe una propiedad
   general del sistema (tiempo, escala, tolerancia a fallos, facilidad de
   uso) o si un actor podría aprovecharse de que la condición no exista.
2. **No clasifiques algo como Seguridad solo porque "causa un problema" si
   falla.** Antes de etiquetar Seguridad, nombrá explícitamente el actor
   adversario que intenta engañar, falsificar o suplantar algo. Si el
   sistema solo está validando un dato que asume real (sin que nadie
   intente falsificarlo), es una validación funcional, no Seguridad.
3. **No fuerces un atributo de calidad sobre un caso de uso base sin
   ninguna tensión.** Antes de cerrar un candidato, intentá completar
   mentalmente las 6 partes del template. Si no aparece un estímulo
   específico ni una medida de respuesta natural, agregá la variable que
   estresa la funcionalidad (carga, pérdida de conexión, dificultad del
   usuario) en vez de forzar el QA sobre el caso normal.
4. **No te quedes buscando el atributo de calidad como una oración textual
   explícita en el enunciado.** Si después de revisar varios párrafos no
   aparece ningún QA "textual", cambiá de método: repasá el sistema
   completo contra preguntas generales (¿qué pasa con muchos
   usuarios/dispositivos a la vez? ¿y si se pierde la conexión? ¿hay datos
   sensibles o dinero de por medio? ¿qué podría necesitar cambiar a futuro?
   ¿hay usuarios con necesidades particulares?) y anclá cada candidato en
   una característica concreta del sistema, no en una frase suelta.
5. **No dupliques un mismo hecho del enunciado en dos atributos de calidad
   distintos sin diferenciarlos.** Si dos atributos podrían justificarse
   con la misma frase, buscá una evidencia distinta en el enunciado para
   cada uno (por ejemplo: estado actual → Disponibilidad; historial/
   auditoría → Observabilidad). Si no hay evidencia distinta y el atributo
   nuevo se superpone con uno clásico, fusionalos o dejá explícito por
   escrito por qué conviene tratarlos aparte (tácticas o preocupación
   distintas).

### Al armar el escenario de 6 partes

6. **No redactes un estímulo genérico o amplio.** El estímulo tiene que ser
   una condición concreta y específica, porque la respuesta se deriva de
   él. Si la respuesta que se te ocurre serviría para casi cualquier
   estímulo, el estímulo necesita ser más concreto (ej.: no "interactuar
   con el sistema", sí "intenta realizar una operación sin asistencia").
7. **No escribas una respuesta puramente funcional que no conecte con el
   atributo.** La respuesta tiene que ser la reacción específica al
   problema que plantea el estímulo (la dificultad de uso, el fallo, la
   carga), no la operación normal que el sistema hace siempre. Si la
   respuesta describe lo que el sistema hace en cualquier caso, falta
   conectarla con la preocupación de calidad en juego.
8. **No confundas la respuesta observable con la táctica técnica de
   solución.** La respuesta debe describir lo que percibe quien generó el
   estímulo, no el mecanismo interno de arquitectura con el que el sistema
   lo logra. Si la respuesta describe una acción sobre la infraestructura o
   el sistema mismo (se expande, se reconfigura, se redimensiona,
   conmuta a un componente de respaldo), reformulala en términos del
   resultado observable (tiempo de respuesta, disponibilidad, etc.) y dejá
   la táctica para la etapa de diseño de la solución.
9. **No dejes una Medida de Respuesta cualitativa o vaga.** "Cuantificable"
   significa: un número, rango, porcentaje o umbral de tiempo verificable
   — nunca una palabra suelta como "rápido", "aceptable" o "sin
   problemas". Si el input no da un número, la Medida de Respuesta debe
   completarse igual con un valor concreto marcado como asunción (ver
   Parte 1, regla 2), nunca dejarse en forma cualitativa aunque esté
   marcada como asunción. Este es el criterio único de "cuantificable"
   para todo el set — no depende de ningún archivo externo a esta skill.
   Para atributos donde el fenómeno relevante es "qué tanto se degrada
   algo a medida que crece la carga" (típicamente Escalabilidad), una
   medida puramente absoluta puede no capturar la preocupación real —
   preferí combinar un valor **relativo** (ej. "no más de 10% peor que
   el baseline sin carga aumentada") con un piso **absoluto** (ej. "y en
   ningún caso por encima de 500 ms"), para que el escenario no apruebe
   un sistema que ya partía de un desempeño malo.
   
