# Heurística de inferencia — partes faltantes del template

Qué hacer cuando el input del usuario (enunciado, descripción de sistema, pedido suelto) no trae explícitas las 6 partes del template (ver `../../../shared/template-6-partes-sei.md`).

## Regla general

Primero distinguir **qué tipo de parte** falta, porque no todas se tratan igual:

- **Ambiente, Artefacto y Medida de la respuesta** se pueden inferir e inventar un valor razonable marcado como asunción (pasos 1-5 de abajo).
- **Fuente del estímulo, Estímulo y Respuesta nunca se inventan.** Si alguna de estas tres no está explícita ni se puede derivar con confianza del contexto ya dado, **no propongas un valor** — preguntá directamente, ofreciendo 2 o 3 lecturas concretas plausibles para que el usuario elija o corrija, en vez de una pregunta abierta. Esto es así porque estas tres partes definen *qué pasó realmente*; inventarlas no es "completar un dato que falta", es inventar el evento que dispara todo el escenario. Ver el ejemplo 3 más abajo.
  - Cuando el usuario responda a esa pregunta, el escenario final debe **citar textualmente su respuesta** al redactar la parte correspondiente — no reformularla de forma libre ni resumirla como "según lo indicado". Esto deja trazable que el dato salió de una decisión real del usuario y no de una inferencia del modelo.

Para las partes que sí se pueden inferir (Ambiente, Artefacto, Medida de la respuesta):

1. **Proponé un valor razonable y cuantificable** en su lugar (no dejes la parte vacía ni la omitas del escenario final).
2. **Marcalo siempre como asunción propia**, nunca como si fuera un dato dado por el usuario. Formato fijo:
   > *Asunción: se estimó [valor] por [justificación breve basada en el contexto del sistema].*
3. **Priorizá inferir con cuidado la Medida de Respuesta** por sobre las demás partes — es la que más rompe la testeabilidad del escenario si queda vaga o ausente (ver `docs/condiciones/condiciones.md`, tips 6-8, y `rubric-completitud.md` de `skill-chequear-completitud-escenario` para el criterio de "cuantificable"). Si el enunciado no da ninguna pista de negocio para anclar el número, en vez de inventar un valor único ofrecé 2 o 3 medidas alternativas pertinentes (con su unidad), y dejá el umbral exacto a cargo del usuario — no completes vos el número solo porque "hay que poner algo".
4. Si el input da pistas parciales (ej. menciona "sistema crítico" o "muchos usuarios"), usalas para justificar la asunción — no inventes un número desconectado del contexto.
5. Nunca marques como asunción algo que el input sí especificaba — eso sería subestimar información real dada por el usuario.

## Ejemplos del mecanismo (no son casos de test completos)

**Ejemplo 1 — falta la Medida de Respuesta:**
Input: "El sistema de alarmas debe notificar al operador ante una falla del sensor principal."
Fragmento del escenario generado:
> Respuesta: el sistema notifica al operador y activa el sensor de respaldo.
> Medida de respuesta: *Asunción: se estimó un tiempo de notificación menor a 2 segundos, por tratarse de un sistema de alarmas críticas donde la demora en notificar un fallo tiene consecuencias directas sobre la seguridad.*

**Ejemplo 2 — falta el Ambiente:**
Input: "Un usuario pide cambiar su método de pago."
Fragmento del escenario generado:
> Ambiente: *Asunción: se asumió que el cambio ocurre en tiempo de ejecución (runtime), ya que no se menciona ningún proceso de mantenimiento o downtime programado para esta operación.*

**Ejemplo 3 — falta el Estímulo (parte que NO se infiere, se pregunta):**
Input: "El sistema tiene que poder conectarse con las APIs de las diferentes empresas de logística y pasarelas de pago de forma que si una cambia algo, no se rompa todo el núcleo de la tienda."
En vez de inventar qué tipo de cambio hace el proveedor (¿una nueva versión de API? ¿un cambio de formato de datos? ¿una baja del servicio?), la skill pregunta:
> ⚠️ Antes de fijar el Estímulo, necesito que elijas o corrijas: ¿el cambio del proveedor es (a) una nueva versión de su API/SDK, (b) un cambio de formato o contrato de datos sin nueva versión, o (c) otra cosa (especificá)? Y para la Medida de la respuesta: ¿preferís medirlo en (1) días-persona de esfuerzo para adaptar el adaptador, (2) cantidad de componentes del núcleo que hay que tocar (objetivo: 0), o (3) tiempo hasta que la integración vuelve a intercambiar datos correctamente? Elegí el umbral en cada caso.
Si el usuario responde, por ejemplo, "es una nueva versión de API, y quiero medirlo en días-persona, máximo 5 días", el escenario final debe citar eso tal cual, no reformularlo como "se estimó un esfuerzo razonable".

## Qué NO hacer

- No dejar una parte del template vacía "porque no está en el input" — siempre proponer algo, marcado como asunción.
- No mezclar la asunción dentro de la oración del escenario sin marcarla — debe quedar identificable por separado.
- No inventar una asunción sin ligarla a algo del contexto dado (dominio del sistema, criticidad, tipo de usuario, etc.).
