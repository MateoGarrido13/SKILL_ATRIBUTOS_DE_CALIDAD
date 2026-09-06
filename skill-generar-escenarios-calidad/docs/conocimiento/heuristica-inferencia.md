# Heurística de inferencia — partes faltantes del template

Qué hacer cuando el input del usuario (enunciado, descripción de sistema, pedido suelto) no trae explícitas las 6 partes del template (ver `../../../shared/template-6-partes-sei.md`).

## Regla general

Si una parte no está explícita en el input:

1. **Proponé un valor razonable y cuantificable** en su lugar (no dejes la parte vacía ni la omitas del escenario final).
2. **Marcalo siempre como asunción propia**, nunca como si fuera un dato dado por el usuario. Formato fijo:
   > *Asunción: se estimó [valor] por [justificación breve basada en el contexto del sistema].*
3. **Priorizá inferir con cuidado la Medida de Respuesta** por sobre las demás partes — es la que más rompe la testeabilidad del escenario si queda vaga o ausente (ver `docs/condiciones/condiciones.md`, tips 6-8, y `rubric-completitud.md` de `skill-chequear-completitud-escenario` para el criterio de "cuantificable").
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

## Qué NO hacer

- No dejar una parte del template vacía "porque no está en el input" — siempre proponer algo, marcado como asunción.
- No mezclar la asunción dentro de la oración del escenario sin marcarla — debe quedar identificable por separado.
- No inventar una asunción sin ligarla a algo del contexto dado (dominio del sistema, criticidad, tipo de usuario, etc.).
