# Plantilla de 6 partes de un escenario de calidad (SEI)

Fuente: Keeling, *Design It!*, Cap. 5 "Dig for Architecturally Significant
Requirements" (pág. 52-54); Díaz Pace, "Atributos de Calidad" (diapositivas
11-21).

Un atributo de calidad es solo una palabra (Performance, Seguridad,
Usabilidad...). Para darle sentido y hacerlo diseñable, se usa un
**escenario de atributo de calidad**, compuesto por 6 partes:

1. **Fuente del estímulo (Source of stimulus)** — persona o sistema que
   origina el estímulo (usuario, componente interno, sistema externo).
2. **Estímulo (Stimulus)** — condición que llega al sistema y requiere una
   respuesta. Varía según el atributo (para disponibilidad puede ser un
   nodo que se vuelve inalcanzable; para modificabilidad, un pedido de
   cambio).
3. **Artefacto (Artifact)** — la parte del sistema (o el sistema entero)
   cuyo comportamiento se caracteriza en el escenario.
4. **Entorno (Environment)** — condición operacional en la que ocurre el
   estímulo (operación normal, carga pico, falla parcial, etc.). Siempre
   debe definirse, incluso si es "normal".
5. **Respuesta (Response)** — acción observable que realiza el artefacto
   como resultado del estímulo.
6. **Medida de la respuesta (Response measure)** — criterio específico y
   medible/testeable que define si la respuesta fue satisfactoria (ej.
   latencia, % de éxito, tiempo de recuperación).

## Ejemplos de la bibliografía

**Disponibilidad** (diapositiva 13):
> Fuente: externa al sistema. Estímulo: mensaje inesperado. Artefacto:
> proceso. Entorno: operación normal. Respuesta: informa al operador y
> continúa operando. Medida: sin downtime.

**Performance** (ejercicio, diapositiva 14):
> "Bajo condiciones normales de operación, el sistema debe procesar las
> transacciones de los usuarios con una latencia promedio de 2 segundos."

**Seguridad** (diapositiva 15):
> "El sistema debe mantener información de auditoría sobre los datos que
> modifique cualquier individuo correctamente identificado. En caso de un
> ataque, la imagen correcta de los datos modificados debe restaurarse en
> menos de 1 día."

**Usabilidad** (diapositiva 17):
> "Los usuarios deben poder minimizar el impacto de los errores cancelando
> la operación en curso, siendo el tiempo de cancelación menor a 1
> segundo."

**Interoperabilidad** (diapositiva 21): la medida de respuesta puede ser el
porcentaje de información intercambiada correctamente (ej. 99.9% del
tiempo).

## Straw man para la medida de respuesta

Cuando el usuario no tiene un número concreto, se puede usar la técnica de
**Response Measure Straw Man** (Keeling, Activity 9, pág. 219): proponer un
valor razonable (honesto o deliberadamente exagerado) para arrancar la
conversación con los stakeholders, en vez de partir de una página en
blanco. Aclarar siempre que es un valor a validar, no un dato confirmado.
