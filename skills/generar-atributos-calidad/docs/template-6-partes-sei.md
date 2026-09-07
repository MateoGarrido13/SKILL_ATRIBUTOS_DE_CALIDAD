# Escenarios de Calidad: el Template SEI de 6 Partes

> Fuente: Notion "Escenarios de Calidad: el Template SEI de 6 Partes" y
> subpáginas ("Ejercicios de la Filmina", "Escenario de Escalabilidad").
> Basado en Clase 5 — Diseño de Sistemas de Software (J. Andrés Díaz Pace).

El SEI propone capturar cada atributo de calidad como un **escenario**:
una oración concreta, medible y testeable. Existe una variante del
template por atributo, pero todas comparten el mismo esqueleto de 6
partes.

## Tres niveles de abstracción

**Atributo de Calidad → Escenario(s) → Template específico**

| Nivel | Qué es |
|---|---|
| Atributo de Calidad | Concepto general, no operacional ("Performance", "Seguridad"). No es medible por sí mismo. |
| Template | Estructura genérica de 6 casilleros vacíos, específica por atributo. |
| Escenario | El template ya completado con información real: concreto, medible, testeable. |

## Las 6 partes

1. **Fuente del estímulo** — quién o qué genera el estímulo (interno o externo).
2. **Estímulo** — la condición que llega al sistema y exige una respuesta.
3. **Artefacto** — la parte del sistema afectada.
4. **Ambiente** — el estado del sistema al momento del estímulo (ej. operación normal, sobrecarga).
5. **Respuesta** — la actividad que ocurre luego del estímulo.
6. **Medida de la respuesta** — cómo se evalúa la respuesta, de forma cuantificable.

## El estímulo no siempre es un fallo

Solo en algunos atributos (típicamente Disponibilidad) el estímulo es
algo roto. Lo constante en todos los atributos es más general: **una
condición que le exige al sistema una respuesta observable y medible**
relacionada con esa preocupación de calidad. Puede ser un fallo
(Disponibilidad), una amenaza (Seguridad), una carga de uso exigente
(Performance, Usabilidad) o un pedido de cambio (Modificabilidad).

Ejemplos de estímulos que no son fallos: una carga legítima de usuarios
(Performance), un pedido de un desarrollador de agregar una política
(Modificabilidad), conectar un dispositivo externo nuevo
(Interoperabilidad), un intento de acceso no autorizado (Seguridad — hay
amenaza, pero no es un "fallo" propio del sistema).

Al armar un escenario desde cero, si el atributo no es Disponibilidad,
forzar el estímulo a que sea "algo que falla" lleva a escenarios
artificiales.

## Ejemplo oficial — Disponibilidad

| Parte | Valor |
|---|---|
| Fuente | Externa al sistema |
| Estímulo | Mensaje inesperado (*unanticipated message*) |
| Artefacto | Un proceso |
| Ambiente | Operación normal |
| Respuesta | Informar al operador y continuar operando |
| Medida de respuesta | Sin tiempo de inactividad (*no downtime*) |

## De Template a Escenario: dos caminos

- **Camino 1 — Desarmar:** ya hay una oración con el requerimiento
  escrito; "cazar" qué fragmento responde cada una de las 6 preguntas.
- **Camino 2 — Armar desde cero:** solo hay una preocupación de negocio
  suelta; construir cada parte desde el contexto del sistema.

> **Ejemplo (Camino 2) — e-commerce en Cyber Monday**
> Stakeholder: *"Me preocupa que en el Cyber Monday el sitio se caiga o se
> ponga lentísimo."*
>
> | Parte | Valor |
> |---|---|
> | Fuente del estímulo | Usuarios finales navegando el sitio |
> | Estímulo | Pico de solicitudes concurrentes (10.000 usuarios simultáneos) |
> | Artefacto | Servidor de checkout / carrito de compras |
> | Ambiente | Evento de alta demanda (Cyber Monday) |
> | Respuesta | El sistema sigue procesando pedidos sin caerse |
> | Medida de respuesta | Tiempo de respuesta ≤ 3 seg, 0% de caídas del servicio |

## Método paso a paso

1. Identificar la preocupación real (stakeholder, entrevista, riesgo
   conocido, o una hoja del árbol de utilidad).
2. Recorrer las 6 preguntas del template una por una. Si algo no está
   claro, es señal de volver a preguntar al stakeholder.
3. Ser lo más concreto y numérico posible, especialmente en Estímulo,
   Ambiente y Medida de Respuesta — son las que más fácil quedan vagas.
4. Armar la oración final uniendo las 6 respuestas.
5. Repetir el proceso por cada atributo relevante ya priorizado.

```
Preocupación (vaga) → Árbol de utilidad (prioriza atributos)
   → Template de 6 partes (vacío, específico por atributo)
   → Se completa cada parte con info concreta
   → ESCENARIO final (medible, testeable)
```

---

## Ejercicios resueltos (los 4 de la filmina)

| Parte | Performance | Seguridad (escenario real) | Disponibilidad | Usabilidad |
|---|---|---|---|---|
| Fuente | Usuarios | Atacante | Procesador principal (interno) | Usuario |
| Estímulo | Llegada de transacción | Ataque que modifica datos | Falla del procesador | Solicitud de cancelación |
| Artefacto | Módulo de transacciones (implícito) | Los datos | El controlador | La operación en curso (genérico) |
| Ambiente | Operación normal (explícito) | Operación (implícito) | Operación normal (explícito) | Operación normal (implícito) |
| Respuesta | Procesar la transacción | Restaurar datos correctos | Failover a backup | Cancelar operación |
| Medida | Latencia ≤ 2 seg | Restauración < 1 día | **Falta — no especificada** | Cancelación < 1 seg |

**Seguridad tiene dos momentos** (enunciado original: *"El sistema debe
mantener información de auditoría [...]. En caso de un ataque, la imagen
correcta de los datos [...] debe restaurarse en menos de 1 día"*):
- *Momento 1* (mecanismo permanente: loguear cada modificación) es casi
  funcional, no un escenario de calidad en sí — es la **precondición**
  que hace posible el Momento 2.
- *Momento 2* (restaurar ante un ataque) es el escenario real de
  Seguridad, y es el que está en la tabla de arriba.
- Este patrón (mecanismo de base + escenario disparado por un evento
  puntual) se repite en el ejemplo oficial de Disponibilidad.

**Lecciones clave:**
1. Un requerimiento crudo casi nunca trae las 6 partes completas.
2. El trabajo del analista es detectar qué falta y volver a preguntar.
3. Un escenario sin medida de respuesta cuantificable **no sirve para
   testear** — es la parte más crítica de no dejar vacía (caso
   Disponibilidad arriba).

---

## Escenario de Escalabilidad

Escalabilidad mide el impacto de agregar/quitar recursos de TI, en 3
aspectos: **Capacidad** (volumen de datos), **Tiempo de respuesta**,
**Throughput** (unidades de trabajo por tiempo).

**Diferencia con Performance:** Performance mide qué tan rápido responde
el sistema *hoy*, con la carga actual. Escalabilidad mide si se mantiene
igual de bueno a medida que la carga crece.

> **Ejemplo armado desde cero** — plataforma IoT de logística, de 200 a
> 5000 camiones con sensores en 2 años.
>
> | Parte | Valor |
> |---|---|
> | Fuente del estímulo | Sensores IoT de camiones |
> | Estímulo | El número de dispositivos activos crece de 200 a 5000 (≈500 eventos/seg en pico) |
> | Artefacto | Servicio de ingesta de eventos + BD de series temporales |
> | Ambiente | Operación normal, escalado gradual en 2 años |
> | Respuesta | Se sigue ingiriendo y procesando todo sin pérdida de datos |
> | Medida de respuesta | Ver abajo — combinación relativa + absoluta |

**Medida relativa vs. absoluta:** una medida puramente absoluta (ej.
"< 500 ms") no distingue el efecto de escalar de otras variables. Una
medida puramente relativa (ej. "no empeora más de 10% vs. baseline") no
protege contra un baseline que ya era malo. La medida más robusta
combina ambas:

> *"El tiempo de procesamiento por evento con 5000 dispositivos no debe
> superar en más de un 10% el tiempo observado con 200 dispositivos, y en
> ningún caso debe exceder los 500 ms en el percentil 95."*

**Lección clave:** definir con números concretos los 3 aspectos
(Capacidad, Tiempo de respuesta, Throughput) y, cuando aplique,
complementar con una medida relativa — es la forma más fiel de capturar
qué significa "escalar bien".
