# Skills de Claude para Atributos de Calidad (SEI)

**TP3 — Diseño de Software · Ejercicio 4 · Informe técnico de construcción y validación**

| | |
|---|---|
| **Tipo de skill** | Agent Skills nativas de Claude (formato `.claude/skills/`) — 3 skills independientes, formato "estilo Claude" pedido por el enunciado |
| **Skills implementadas** | `generar-escenarios-calidad` · `chequear-completitud-escenario` · `arbol-utilidad` |
| **Alcance** | Corresponde a los 3 incisos del ejercicio 4: (i) generar escenarios con el template de 6 partes del SEI, (ii) chequear completitud de un escenario dado, (iii) elaborar árbol de utilidad |

---

## 1. Composición y construcción de la skill

El repositorio se organiza en una carpeta `shared/` con la base teórica común a las 3 skills (template de 6 partes del SEI, glosario de atributos de calidad, método de árbol de utilidad), y una carpeta por skill con su `SKILL.md` y una subcarpeta `docs/` dividida en `condiciones/` (anti-patrones a evitar) y `conocimiento/` (procedimientos propios de razonamiento). Un directorio separado `tests-no-leer/` contiene los casos de validación, explícitamente excluido de lo que cada skill puede leer.

La construcción se hizo en capas, en este orden:

- **Fuente de verdad:** migración de los apuntes propios (Notion) sobre atributos de calidad, template SEI y árbol de utilidad — copiados como base literal, no resumidos.
- **Complementos:** revisión del libro *Software Architecture in Practice* (Bass, Clements & Kazman) para cubrir huecos puntuales no explicitados en los apuntes propios.
- **Reglas de proceso:** distribución de "tips prácticos" (errores propios documentados al resolver ejercicios anteriores del TP) como anti-patrones específicos por skill.
- **Procedimientos nuevos:** redacción de heurísticas propias no cubiertas por ninguna fuente externa (inferencia ante datos faltantes, rúbrica de completitud, criterio de priorización H/M/L).
- **Casos de test:** 4 casos de validación — el sistema de monopatines del propio TP, dos ejemplos concretos del libro SAP (Disponibilidad, Performance), y un árbol de utilidad completo ya resuelto por los autores del libro (sistema de salud, Tabla 19.1), usado como *benchmark externo*.

---

## 2. Problemas detectados y correcciones aplicadas

La validación se realizó corriendo cada skill con pedidos naturales (sin invocarla explícitamente, dejando que se activara sola por su descripción) y comparando el resultado contra los casos de `tests-no-leer/`. Se detectaron y corrigieron 2 problemas de fondo.

### Problema 1 — Invención de premisas ante inputs ambiguos (skill `generar-escenarios-calidad`)

Ante un requerimiento que admite más de una lectura de atributo de calidad válida, el paso de clasificación tendía a elegir la interpretación narrativamente más rica, incluso si eso implicaba asumir un evento no mencionado en el texto (por ejemplo, imaginar una falla de un servicio externo que el enunciado no describe).

> **Comportamiento esperado:** ante ambigüedad real, declarar los atributos candidatos con evidencia textual y resolver a favor de la lectura que no requiere inventar ningún hecho adicional.

> ⚠️ **Output antes de la corrección (extracto):** ante un fragmento que solo menciona que varios usuarios comparten el saldo de una cuenta, la skill construyó un escenario de Seguridad describiendo una condición de carrera y un "ataque" tipo double-spend entre dos usuarios actuando en simultáneo — ninguno de esos hechos (simultaneidad, intención de explotar el sistema) estaba en el enunciado original.

> ✅ **Corrección aplicada:** se agregó al paso de clasificación del `SKILL.md` una regla explícita de prohibición ("no elegir una interpretación que dependa de un evento no mencionado si existe una alternativa que no lo requiera") junto con un autochequeo obligatorio previo a redactar el escenario final.

> ✅ **Output después de la corrección (extracto):** ante el mismo input, la skill enumeró explícitamente los atributos con evidencia real (Interoperabilidad y una lectura de Seguridad/Integridad), señaló que ambos se apoyaban en hechos del texto, y redactó el escenario evitando ya el armado de un ataque completo no sugerido por el enunciado. La mejora fue sustancial aunque no perfecta: en casos de ambigüedad genuina persiste cierta variabilidad de criterio entre corridas, documentada como limitación (ver sección 3).

### Problema 2 — Sesgo de sobreestimación en la priorización del árbol de utilidad (skill `arbol-utilidad`)

Comparando contra el árbol de utilidad ya resuelto por los propios autores del libro SAP (Tabla 19.1, sistema de salud), se detectó un patrón sistemático: de 12 comparaciones directas, 5 escenarios quedaron con prioridad más alta que la del libro y ninguno más baja — no era variabilidad aleatoria, sino un sesgo unidireccional.

> **Comportamiento esperado:** etiquetas H/M/L (negocio, técnica) alineadas con el criterio del libro en la mayoría de los casos, con discrepancias ocasionales y bidireccionales (no siempre hacia arriba).

**⚠️ Output antes de la corrección (extracto):**

| Escenario | Libro (original) | Skill (antes) |
|---|---|---|
| Throughput bajo carga pico | (M, M) | (H, H) |
| Acceso web 24/7/365 | (M, M) | (H, H) |
| Upgrade de componente de terceros (COTS) | (H, M) | (M, H) |

Causa raíz: el criterio de dificultad técnica alta ("afecta múltiples componentes") se cumplía casi siempre en sistemas reales, y la regla de desempate por "datos sensibles" se aplicaba a todo el sistema en general y no al escenario puntual.

**✅ Corrección aplicada (3 iteraciones sobre `criterio-priorizacion.md`):**

- Se acotó el criterio de H técnico: tocar varios componentes con tácticas ya conocidas no alcanza para H.
- Se acotó la regla de desempate por datos sensibles al escenario puntual, no al dominio general del sistema.
- Se agregó un principio de comparación de evidencia cuantitativa relativa entre escenarios del mismo conjunto.

**✅ Output después de la corrección:**

| Escenario | Libro (original) | Skill (después) | Resultado |
|---|---|---|---|
| Throughput bajo carga pico | (M, M) | (M, M) | ✅ Corregido |
| Acceso web 24/7/365 | (M, M) | (M, M) | ✅ Corregido |
| Upgrade de componente de terceros (COTS) | (H, M) | (M, H) | ❌ Persiste |

El ajuste corrigió los casos de sobreestimación por generalización de categoría, pero el caso de dependencia de terceros se mantuvo estable en las 3 iteraciones: la skill privilegia consistentemente el esfuerzo cuantitativo (días-persona) por sobre el riesgo de una dependencia externa no controlada por el equipo. Se documenta como diferencia de criterio, no como error de proceso (ver limitación 4).

---

## 3. Limitaciones conocidas

**① Ambigüedad de clasificación en inputs genuinamente ambiguos.**
El autochequeo anti-invención reduce pero no elimina por completo la posibilidad de que la skill construya un supuesto sutil no explicitado en el texto al resolver una ambigüedad real entre dos atributos con evidencia similar.

**② `chequear-completitud-escenario` puede cuestionar la clasificación, no solo la completitud.**
Al compartir las condiciones de clasificación con la skill generadora, en ciertos casos la skill de chequeo objeta el atributo declarado además de evaluar las 6 partes — comportamiento coherente con el diseño, pero más amplio que lo originalmente previsto.

**③ El criterio de "estímulo vago" en `rubric-completitud.md` es menos estricto de lo asumido al diseñar los tests.**
Un estímulo con cierta especificidad numérica ("muchos usuarios simultáneos") fue aceptado como válido cuando el diseño del test esperaba que se rechazara por genérico.

**④ Sesgo persistente en la priorización de dependencias de terceros.**
Confirmado en dos sistemas independientes — el benchmark del libro SAP (upgrade de base de datos comercial) y la prueba grupal MarketHub (integración con pasarelas de pago externas) — la skill subestima consistentemente la dificultad técnica de depender de un proveedor externo cuando el propio escenario ya trae una medida de esfuerzo cuantificada, priorizando esa cifra por sobre el riesgo de no controlar la dependencia. Es una heurística de razonamiento interna, consistente y justificada en cada corrida, pero no coincide con el criterio de referencia en este tipo de escenario puntual.

**⑤ Testing manual, sin automatización de regresión.**
La comparación output-vs-expected se realizó a mano en cada corrida; no existe un script que la automatice ante cambios futuros al contenido de `shared/` o `docs/`.

---

## 4. Prueba grupal — Sistema "MarketHub" (E-commerce)

Como validación adicional e independiente, se corrió el pipeline completo de las 3 skills contra un sistema nuevo (MarketHub, plataforma de e-commerce) acordado en grupo, con una referencia de resultados ideales provista aparte para comparar. A diferencia de los casos de `tests-no-leer/`, este caso no formaba parte del contenido usado para ajustar la skill — funciona como prueba ciega de generalización, corrida sin invocar la skill explícitamente (activación automática por descripción). A continuación se reproduce textualmente el prompt utilizado en cada etapa y el output completo obtenido, sin resumir.

### 4.1 Etapa 1 — Generación de escenarios

**Prompt utilizado:**

```
Contexto del Sistema: "MarketHub Plataforma Global de E-commerce"

Una empresa minorista planea lanzar MarketHub, una plataforma integral de
comercio electrónico multicanal orientada tanto a clientes finales como a
vendedores externos (partners). El sistema se compondrá de una aplicación
Web progresiva (PWA) para compradores, una aplicación móvil nativa y un
panel de administración centralizado en la nube.

El modelo de negocio exige que la plataforma se conecte de manera fluida
con múltiples pasarelas de pago regionales (como Mercado Pago, Stripe y
PayPal) y con sistemas logísticos de terceros (como Andreani y DHL) para
calcular costos de envío en tiempo real y emitir etiquetas de despacho.
Estas integraciones deben adaptarse a los cambiantes protocolos y SDKs que
cada proveedor exija, sin comprometer el funcionamiento general del núcleo
del e-commerce.

Asimismo, la empresa opera en distintos países, por lo que el front-end de
la tienda y la aplicación móvil deben garantizar una experiencia de usuario
idéntica y un correcto renderizado tanto en dispositivos móviles iOS y
Android como en navegadores de escritorio (Chrome, Safari, Firefox y Edge),
adaptándose dinámicamente a las normativas de visualización locales.

Por otro lado, al manejar volúmenes masivos de transacciones financieras y
datos personales sensibles (tarjetas de crédito, domicilios, historiales de
compra), el sistema se enfrenta a constantes intentos de ataques
informáticos. Se requiere proteger estrictamente la base de datos de
usuarios frente a brechas de seguridad, garantizar que las credenciales e
información de pago estén cifradas tanto en tránsito como en reposo, y
asegurar que cualquier intento de acceso no autorizado a los paneles de
gestión sea bloqueado y auditado de inmediato, evitando la manipulación de
precios o la exfiltración de datos de clientes.

A partir de este contexto, tengo 5 requerimientos informales que necesito
convertir en escenarios de atributos de calidad bien definidos:

1. "El sistema tiene que poder conectarse con las APIs de las diferentes
empresas de logística y pasarelas de pago de forma que si una cambia algo,
no se rompa todo el núcleo de la tienda."

2. "La aplicación web de la tienda online tiene que poder correr sin
problemas raros de visualización tanto en computadoras como en los
celulares de los clientes, sin importar el navegador que usen."

3. "Los datos de las tarjetas de crédito de los compradores y las
contraseñas tienen que estar recontra seguros para que ningún hacker pueda
robarlos de la base de datos."

4. "Si un usuario malintencionado intenta vulnerar el panel de
administración o hacer fuerza bruta para entrar a cuentas ajenas, el
sistema tiene que bloquearlo rápido y registrar la auditoría."

5. "Cuando un vendedor externo quiera sincronizar su propio catálogo de
productos con el inventario de MarketHub, la plataforma debe permitir
integrar ese sistema externo mediante adaptadores estándar."

Ayudame a convertir cada uno de estos 5 requerimientos en un escenario de
atributo de calidad bien definido.
```

**Output completo obtenido:**

#### 1. Integración con pasarelas de pago y logística

*Clasificación:* Hay evidencia real para dos atributos: Modificabilidad ("si una cambia algo" = pedido/evento de cambio en una interfaz externa) e Integrabilidad ("conectarse con APIs de terceros" = sumar/combinar componentes externos). Ninguna lectura requiere inventar un hecho no mencionado, así que la ambigüedad es legítima. Se prioriza Modificabilidad porque el anclaje textual más directo es "si una cambia algo, no se rompa todo el núcleo" (un estímulo de cambio).

| Parte | Valor |
|---|---|
| Fuente del estímulo | Un proveedor externo de pagos o logística (Mercado Pago, Stripe, PayPal, Andreani, DHL) |
| Estímulo | El proveedor modifica su protocolo, API o SDK de integración |
| Artefacto | El módulo/adaptador de integración con pasarelas de pago y logística |
| Ambiente | Sistema en operación normal (runtime), con el núcleo del e-commerce activo |
| Respuesta | El cambio se absorbe en la capa de integración (se actualiza el adaptador correspondiente) sin afectar el núcleo del e-commerce ni las demás integraciones |
| Medida de la respuesta | *Asunción:* se estimó que el ajuste se completa en menos de 5 días-persona de esfuerzo, afectando únicamente el módulo adaptador (0 cambios en el núcleo) y sin downtime del checkout, por ser la función más crítica de negocio. |

#### 2. Renderizado consistente en navegadores y dispositivos

*Clasificación:* Portabilidad (sabor de Modificabilidad en el libro SEI): el texto ancla explícitamente en "correr sin problemas... en computadoras y celulares... sin importar el navegador".

| Parte | Valor |
|---|---|
| Fuente del estímulo | Un cliente final que accede a la tienda online |
| Estímulo | El cliente abre la aplicación web desde un navegador (Chrome, Safari, Firefox o Edge) o dispositivo (desktop, iOS, Android) distinto al usado en pruebas previas |
| Artefacto | La interfaz de usuario de la PWA de la tienda |
| Ambiente | Operación normal, en cualquier combinación soportada de navegador/dispositivo |
| Respuesta | La interfaz se renderiza correctamente, sin errores visuales ni de layout, y mantiene las mismas funcionalidades disponibles |
| Medida de la respuesta | *Asunción:* 0 defectos visuales bloqueantes en el 100% de las combinaciones soportadas (4 navegadores × desktop/iOS/Android), verificado mediante pruebas de regresión visual antes de cada release. |

#### 3. Protección de tarjetas de crédito y contraseñas

*Clasificación:* Seguridad — hay un actor adversario explícito ("ningún hacker pueda robarlos"), no es solo una validación funcional.

| Parte | Valor |
|---|---|
| Fuente del estímulo | Un atacante externo desconocido |
| Estímulo | Intento de acceder o exfiltrar datos de tarjetas de crédito y contraseñas almacenados en la base de datos, interceptando el tráfico o accediendo directamente al almacenamiento |
| Artefacto | La base de datos de usuarios y el canal de comunicación entre cliente y servidor |
| Ambiente | Sistema online, en operación normal |
| Respuesta | Los datos permanecen ilegibles para el atacante: viajan cifrados en tránsito (TLS) y se almacenan cifrados en reposo (hashing, tokenización), y el intento queda registrado |
| Medida de la respuesta | *Asunción:* 0% de exposición en texto plano (tránsito o reposo), TLS 1.2+ y AES-256 o equivalente en reposo, detección y registro del intento en menos de 1 minuto (dato regulado, ej. PCI-DSS). |

#### 4. Bloqueo y auditoría de accesos no autorizados / fuerza bruta

*Clasificación:* Seguridad — actor adversario explícito ("usuario malintencionado", "fuerza bruta").

| Parte | Valor |
|---|---|
| Fuente del estímulo | Un usuario malintencionado (interno o externo, no identificado previamente) |
| Estímulo | Intentos repetidos y fallidos de inicio de sesión (fuerza bruta) contra el panel de administración o una cuenta de usuario |
| Artefacto | El módulo de autenticación del panel de administración y de cuentas de usuario |
| Ambiente | Sistema online, en operación normal |
| Respuesta | El sistema bloquea temporalmente la cuenta o la IP de origen, registra el incidente en el log de auditoría y notifica al equipo de seguridad |
| Medida de la respuesta | *Asunción:* bloqueo tras un máximo de 5 intentos fallidos en 1 minuto, detección y bloqueo en menos de 10 segundos, evento auditado con 100% de trazabilidad (usuario/IP, hora, cantidad de intentos). |

#### 5. Sincronización de catálogo de un vendedor externo

*Clasificación:* Integrabilidad — el texto ancla explícitamente en "integrar ese sistema externo mediante adaptadores estándar" (sumar un componente de un tercero, no un cambio sobre algo propio).

| Parte | Valor |
|---|---|
| Fuente del estímulo | Un vendedor externo (partner) con su propio sistema de gestión de catálogo |
| Estímulo | El partner solicita sincronizar su catálogo de productos con el inventario de MarketHub |
| Artefacto | El módulo de integración de catálogos / adaptador de inventario de MarketHub |
| Ambiente | Tiempo de integración (onboarding del partner), sistema en operación |
| Respuesta | Se implementa o configura un adaptador estándar que traduce el formato del catálogo externo al modelo de inventario, sin modificar el núcleo del sistema |
| Medida de la respuesta | *Asunción:* integración completada en menos de 3 días-persona (formato ya soportado, ej. CSV/API REST), sin cambios en el código del núcleo de inventario. |

### 4.2 Etapa 2 — Chequeo de completitud (escenario 5, tomado como muestra)

**Prompt utilizado:**

```
Evaluá si el siguiente escenario está completo:

5. Sincronización de catálogo de un vendedor externo

Clasificación: Integrabilidad — el texto ancla explícitamente en "integrar
ese sistema externo mediante adaptadores estándar" (sumar un
componente/sistema de un tercero, no un cambio sobre algo propio).

Parte | Valor
Fuente del estímulo | Un vendedor externo (partner) con su propio sistema
de gestión de catálogo
Estímulo | El partner solicita sincronizar su catálogo de productos con el
inventario de MarketHub
Artefacto | El módulo de integración de catálogos / adaptador de
inventario de MarketHub
Ambiente | Tiempo de integración (onboarding del partner), sistema en
operación
Respuesta | Se implementa o configura un adaptador estándar que traduce el
formato del catálogo externo al modelo de inventario de MarketHub, sin
modificar el núcleo del sistema de inventario
Medida de la respuesta | Asunción: se estimó que la integración de un
nuevo partner (usando un formato de catálogo ya soportado, ej. CSV o API
REST estándar) se completa en menos de 3 días-persona, sin cambios en el
código del núcleo de inventario, dado que la plataforma apunta a escalar
a múltiples vendedores externos.

Escenario: Cuando un vendedor externo solicita sincronizar su catálogo, la
plataforma lo integra mediante un adaptador estándar en menos de 3
días-persona, sin requerir cambios en el núcleo del sistema de inventario.
```

**Output completo obtenido:**

Aplicando la rúbrica de completitud (`rubric-completitud.md`) parte por parte:

| Parte | Veredicto | Justificación |
|---|---|---|
| Fuente del estímulo | ✅ Completa | Actor concreto y nombrable: "un vendedor externo (partner) con su propio sistema de gestión de catálogo". |
| Estímulo | ✅ Completa | Condición concreta y específica: el partner solicita sincronizar su catálogo — no es un caso de uso genérico. |
| Artefacto | ✅ Completa | Señala la parte del sistema afectada con precisión: "el módulo de integración de catálogos / adaptador de inventario". |
| Ambiente | ✅ Completa | Estado explícito: tiempo de integración (onboarding), sistema en operación. |
| Respuesta | ❌ Incompleta | Describe la táctica técnica interna ("se implementa o configura un adaptador estándar que traduce el formato") en vez del resultado observable que percibiría el partner o el sistema de inventario. Cae directo en el criterio de detección #8 de `condiciones.md`: la respuesta habla de una acción de arquitectura, no del efecto observado desde afuera. |
| Medida de la respuesta | ✅ Completa | Cuantificable: "< 3 días-persona", "0 cambios en el código del núcleo" — números y umbrales verificables. |

**Veredicto: Incompleto.** Falla únicamente la parte Respuesta.

> **Corrección sugerida:** "El catálogo del partner queda sincronizado correctamente con el inventario de MarketHub — altas, bajas y modificaciones de productos se reflejan sin errores de mapeo — sin requerir cambios en el núcleo del sistema." Con ese cambio, las 6 partes pasarían el criterio positivo y el escenario quedaría completo.

### 4.3 Etapa 3 — Árbol de utilidad

**Prompt utilizado:**

```
Armá el árbol de utilidad para este sistema, con los siguientes escenarios
de calidad ya generados:

1. Fuente: Un proveedor externo de pagos o logística (Mercado Pago,
Stripe, PayPal, Andreani, DHL). Estímulo: El proveedor modifica su
protocolo, API o SDK de integración. Artefacto: El módulo/adaptador de
integración con pasarelas de pago y logística. Ambiente: Sistema en
operación normal (runtime), con el núcleo del e-commerce activo.
Respuesta: El cambio se absorbe en la capa de integración sin afectar el
núcleo ni las demás integraciones. Medida: menos de 5 días-persona de
esfuerzo, 0 cambios en el núcleo, sin downtime del checkout.

2. Fuente: Un cliente final que accede a la tienda online. Estímulo: El
cliente abre la aplicación web desde un navegador o dispositivo distinto
al usado en pruebas previas. Artefacto: La interfaz de usuario de la PWA
de la tienda. Ambiente: Operación normal, en cualquier combinación
soportada de navegador/dispositivo. Respuesta: La interfaz se renderiza
correctamente, sin errores visuales ni de layout. Medida: 0 defectos
visuales bloqueantes en el 100% de las combinaciones soportadas,
verificado en cada release.

3. Fuente: Un atacante externo desconocido. Estímulo: Intento de acceder o
exfiltrar datos de tarjetas de crédito y contraseñas. Artefacto: La base
de datos de usuarios y el canal de comunicación cliente-servidor.
Ambiente: Sistema online, en operación normal. Respuesta: Los datos
permanecen ilegibles para el atacante, cifrados en tránsito y en reposo,
intento registrado. Medida: 0% de exposición en texto plano, TLS
1.2+/AES-256, detección en menos de 1 minuto.

4. Fuente: Un usuario malintencionado. Estímulo: Intentos repetidos y
fallidos de inicio de sesión (fuerza bruta) contra el panel de
administración o una cuenta. Artefacto: El módulo de autenticación del
panel y de cuentas de usuario. Ambiente: Sistema online, en operación
normal. Respuesta: El sistema bloquea temporalmente la cuenta o IP y
registra el incidente. Medida: bloqueo tras 5 intentos fallidos en 1
minuto, detección en menos de 10 segundos, 100% de trazabilidad auditada.

5. Fuente: Un vendedor externo (partner) con su propio sistema de
catálogo. Estímulo: El partner solicita sincronizar su catálogo con el
inventario de MarketHub. Artefacto: El módulo de integración de catálogos
/ adaptador de inventario. Ambiente: Tiempo de integración (onboarding),
sistema en operación. Respuesta: Se sincroniza el catálogo mediante un
adaptador estándar sin modificar el núcleo del inventario. Medida: menos
de 3 días-persona, 0 cambios en el código del núcleo.
```

**Output completo obtenido:**

Se validaron los 5 escenarios contra `condiciones.md` (paso 2): cada uno está anclado en una característica concreta y distinta del sistema (SDK de terceros, renderizado cross-browser, cifrado de datos financieros, fuerza bruta sobre auth, integración de catálogos de partners) — ninguno es una regla de negocio disfrazada ni se superpone con otro sin diferenciarse. Los 5 pasan la validación y entran al árbol.

**Árbol de utilidad — MarketHub**

```
Utility (raíz)
├── Modificabilidad
│   ├── Adaptabilidad a cambios de proveedores externos (pagos/logística)
│   │   └── [Escenario 1] (H, M)
│   └── Portabilidad de renderizado (multi-navegador/dispositivo)
│       └── [Escenario 2] (H, M)
├── Seguridad
│   ├── Confidencialidad de datos financieros (cifrado en tránsito/reposo)
│   │   └── [Escenario 3] (H, M)
│   └── Autenticación y auditoría (control de acceso al panel admin)
│       └── [Escenario 4] (H, M)
└── Integrabilidad
    └── Integración de sistemas externos de partners (catálogo)
        └── [Escenario 5] (M, L)
```

**Detalle tabular**

| Atributo | Refinamiento | Escenario | (Negocio, Técnica) |
|---|---|---|---|
| Modificabilidad | Adaptabilidad a cambios de proveedores externos | Un proveedor modifica su SDK/protocolo → el adaptador absorbe el cambio sin tocar el núcleo, <5 días-persona, sin downtime en checkout | (H, M) |
| Modificabilidad | Portabilidad de renderizado | Cliente accede desde cualquier combinación soportada de navegador/dispositivo → 0 defectos visuales bloqueantes en el 100% de las combinaciones | (H, M) |
| Seguridad | Confidencialidad de datos financieros | Atacante intenta interceptar/exfiltrar tarjetas y contraseñas → datos cifrados en tránsito y reposo, sin exposición en texto plano, detección <1 min | (H, M) |
| Seguridad | Autenticación y auditoría | Usuario malintencionado hace fuerza bruta contra admin/cuentas → bloqueo en <10 seg, auditoría 100% trazable | (H, M) |
| Integrabilidad | Integración de sistemas externos de partners | Partner solicita sincronizar catálogo → adaptador estándar sincroniza correctamente, <3 días-persona, sin cambios en el núcleo | (M, L) |

**Justificación de las etiquetas**

- **Escenario 1 — Pasarelas de pago/logística (H, M):** Negocio = H, el checkout es la función más crítica del negocio; una integración de pago rota bloquea directamente la venta. Técnica = M, el patrón adapter ya aísla el cambio a un módulo conocido y el propio escenario ya cuantifica el esfuerzo con confianza (5 días-persona) — evidencia de que se resuelve con una táctica probada, no un enfoque nuevo.
- **Escenario 2 — Renderizado cross-browser/dispositivo (H, M):** Negocio = H, la propia asunción del escenario liga la falla de renderizado a pérdida directa de ventas en checkout. Técnica = M, se resuelve con tácticas estándar y conocidas (diseño responsive, testing de regresión visual); tocar varios "componentes" no sube a H si la técnica ya es conocida.
- **Escenario 3 — Cifrado de tarjetas/contraseñas (H, M):** Negocio = H, dato financiero regulado (PCI-DSS) + pérdida reputacional/legal directa si se filtra. Técnica = M, TLS y hashing/tokenización son tácticas estándar de la industria.
- **Escenario 4 — Bloqueo por fuerza bruta + auditoría (H, M):** Negocio = H, protege el panel que puede manipular precios y exponer datos de clientes. Técnica = M, rate limiting y logging de auditoría son mecanismos estándar.
- **Escenario 5 — Sincronización de catálogo de partner (M, L):** Negocio = M, importante para el modelo multi-vendor, pero su ausencia no bloquea el uso del sistema para el cliente final. Técnica = L, adaptador estándar, formato ya soportado, esfuerzo bajo y localizado.

> *Lectura del árbol: los cuatro escenarios (H, M) —pagos/logística, renderizado, cifrado y autenticación— son los que más atención arquitectural inmediata requieren; el escenario (M, L) de integración de partners puede especificarse con menos profundidad inicial.*

### 4.4 Comparación contra la referencia del grupo

**Clasificación de atributos**

| Escenario vago (input) | Atributo — skill | Atributo — referencia | Resultado |
|---|---|---|---|
| 1. Pago / logística | Modificabilidad | Integrabilidad | ⚠️ Clasificación distinta |
| 2. Renderizado multi-dispositivo | Modificabilidad (var. Portabilidad) | Portabilidad | ⚠️ Clasificación distinta |
| 3. Cifrado de tarjetas/contraseñas | Seguridad | Seguridad | ✅ Coincide |
| 4. Fuerza bruta / auditoría | Seguridad | Seguridad | ✅ Coincide |
| 5. Catálogo de partner | Integrabilidad | — (no incluido en la referencia) | ✅ Consistente |

**Priorización en el árbol de utilidad**

| Escenario | Prioridad — referencia | Prioridad — skill | Observación |
|---|---|---|---|
| 1. Pago / logística | (M, H) | (H, M) | Reaparece el sesgo de limitación 4: dependencia de terceros subestimada técnicamente por privilegiar la medida cuantitativa propia del escenario (5 días-persona) |
| 2. Renderizado cross-browser | (M, M) | (H, M) | Negocio sobreestimado; eje técnico coincide |
| 3. Cifrado de datos | (H, M) | (H, M) | ✅ Coincide |
| 4. Fuerza bruta / auditoría | (H, L) | (H, M) | Técnica sobreestimada: la referencia considera rate-limiting una táctica trivial (L) |

### 4.5 Conclusiones de la prueba grupal

De los 4 escenarios con referencia comparable, 2 coincidieron exactamente en clasificación (los dos de Seguridad); los 2 restantes (integración de pagos y renderizado) difirieron en el atributo asignado, en ambos casos por un criterio de anclaje textual distinto al del grupo, no por una lectura arbitraria ni por invención de hechos. En priorización, ningún escenario coincidió en ambos ejes simultáneamente: el eje de negocio tendió a sobreestimarse en 2 de 4 casos, y el eje técnico se sobreestimó en 3 de 4 casos — siguiendo el mismo patrón unidireccional (nunca hacia abajo) ya documentado en la limitación 4 contra el benchmark del libro SAP.

En particular, el caso de integración con pasarelas de pago reprodujo casi en espejo la discrepancia del caso "Upgrade COTS" del sistema de salud: la skill privilegia la medida de esfuerzo cuantificada por sobre el riesgo de depender de un proveedor externo no controlado por el equipo. Esta coincidencia, en dos sistemas y dos evaluadores externos independientes (libro y grupo), consolida la limitación 4 como un patrón de razonamiento estable de la skill, y no como una falla puntual de un caso aislado.

---

*Informe generado a partir del proceso de construcción y testing documentado en el repositorio `skill-tp-atributos-calidad`. El detalle completo de fuentes, condiciones y casos de test se encuentra en `README.md` y en las carpetas `shared/`, `docs/` y `tests-no-leer/` de cada skill.*
