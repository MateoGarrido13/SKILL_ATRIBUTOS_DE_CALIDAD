# Descripción de la prueba — Hospital (turnos)

**Carpeta:** `docs/test/PRUEBA_UNO_HOSPITAL/`  
**Skills bajo prueba:** i (Generador SEI) y ii (Validador SEI)

## Utilidad

Es la prueba **inicial** del recorte de atributos. Un solo enunciado informal (`req_uno.txt`) mezcla dos cualidades del sistema de gestión de turnos: que no entren intrusos y que no se cuelgue cuando mucha gente pide turno a la vez. Sirve para ver si el Generador **parte** el texto en dos escenarios (un archivo por atributo), marca lo que infiere o no puede cuantificar, y si el Validador sella solo cuando las seis partes son coherentes y testeables. No cubre el árbol de utilidad.

## Resultados

El flujo produjo y selló dos escenarios independientes:

| Archivo | Atributo | Medida sellada |
|---|---|---|
| `req_uno_seguridad.md` / `escenario_turnos_seguridad.md` | Seguridad | Bloqueo al 3.er intento; espera de 20 min creciente |
| `req_uno_rendimiento.md` / `escenario_turnos_rendimiento.md` | Rendimiento | Latencia promedio de reserva ≤ 20 s en pico de carga |

Ambos quedaron `[ESTADO: VALIDADO]`, con fuente clasificada, artefacto a granularidad de módulo y ambiente concreto (red abierta / pico de carga). Las notas de auditoría indican que en el sello no hizo falta corrección extra.

## Comentarios

**Positivo**
- Respeta “un escenario, un atributo”: no compactó seguridad y performance en una sola tabla.
- Las medidas tienen unidad, estadístico (cuando aplica) y umbral; el template de 6 partes se mantiene.
- El Validador pudo sellar en limpio una vez cerradas las partes.

**A mejorar**
- El enunciado original no daba los números (3 intentos, 20 min, 20 s): hay que dejar explícito en la corrida qué aportó el usuario y qué no, para no confundir el sello con una inferencia numérica.
- No ejercita la Skill iii ni un rechazo con tres opciones: el camino feliz de i → ii queda cubierto; el veto y el árbol no.*(Se delega al usuario completar con la solicitud a la skill iii, para verificar el flujo)*
