# Informe Skill: Escenarios de Calidad y Árbol de Utilidad

Este es un resumen sobre cómo le fue a la Skill generando los escenarios de calidad y el árbol de utilidad.

## 1. Cuando falta información

Una de las cosas que hace la Skill cuando el usuario le pasa un escenario por la mitad es que, en vez de trabarse o dar error, **asume datos usando la teoría del SEI**.

### Lo bueno de que adivine/sugiera:
* **No te frena:** El proceso fluye. Si recién estás arrancando el diseño y no tenés métricas exactas, la Skill te arma un borrador coherente igual.
* **Da buenos ejemplos:** Si no sabés qué poner en "medida de respuesta", la herramienta asume algo razonable (ej: "tiempo de recuperación < 5 seg"). Eso sirve como punto de partida.
* **Borradores rápidos:** Ideal para tener una idea funcional rápido.

### Lo malo (El riesgo de asumir sin preguntar):
* **Se puede desviar de la realidad:** Si inventa métricas de rendimiento por su cuenta, puede llevar la arquitectura para un lado que nada que ver con lo que necesita el cliente.
* **Faltó repreguntar:** Se notó que en casos muy ambiguos, las respuestas hubieran sido mucho mejores si la Skill, en vez de sugerir, frenaba un segundo y le preguntaba al usuario sobre los datos faltantes.

## 2. Sinergia de las 3 funciones

Tener la validación, el armado de las 6 partes y el árbol de utilidad en la misma Skill ayuda a su funcionamiento gracias a la coherencia del contexto. 

Si la Skill asume en el primer paso que un escenario necesita "tolerancia a fallos", esa misma idea viaja automáticamente al template y se usa después para definir qué tan difícil es implementarlo en el Árbol de Utilidad. Al estar todo conectado en una sola herramienta, no hay contradicciones lógicas cuando sugiere cosas.

## 3. Resultados de los test

* **Chequeo de completitud:** En algunos casos sentí que se podría mejorar por el tema de las sugerencias cuando el contexto estaba incompleto.
* **Escenarios SEI (6 partes):** Dio resultados precisos. Agarró descripciones que estaban flojas y las separó perfecto en *Fuente, Estímulo, Entorno, Artefacto, Respuesta y Medida*.
* **Árbol de Utilidad:** En su mayoría, las respuestas fueron las esperadas. Clasificó bien el atributo principal, los sub-atributos y las prioridades (Impacto/Dificultad).

## Conclusión y mejora sugerida

La Skill funciona bien y genera outputs con muy buen nivel técnico. Pero habría que evitar que asuma automáticamente cuando le falta data crítica; hay que meterle un freno de confirmación. 

> **Sugerencia:** Estaría bueno que dé la opción para que, si falta algo muy importante (como el estímulo exacto o la métrica), pause la generación, te dé un par de opciones y espere tu confirmación antes de dar el reporte final.

## TEST

Contexto del Sistema: "MarketHub Plataforma Global de E-commerce"

Una empresa minorista planea lanzar MarketHub, una plataforma integral de comercio electrónico multicanal orientada tanto a clientes finales como a vendedores externos (partners). El sistema se compondrá de una aplicación Web progresiva (PWA) para compradores, una aplicación móvil nativa y un panel de administración centralizado en la nube.
El modelo de negocio exige que la plataforma se conecte de manera fluida con múltiples pasarelas de pago regionales (como Mercado Pago, Stripe y PayPal) y con sistemas logísticos de terceros (como Andreani y DHL) para calcular costos de envío en tiempo real y emitir etiquetas de despacho. Estas integraciones deben adaptarse a los cambiantes protocolos y SDKs que cada proveedor exija, sin comprometer el funcionamiento general del núcleo del e-commerce.
Asimismo, la empresa opera en distintos países, por lo que el front-end de la tienda y la aplicación móvil deben garantizar una experiencia de usuario idéntica y un correcto renderizado tanto en dispositivos móviles iOS y Android como en navegadores de escritorio (Chrome, Safari, Firefox y Edge), adaptándose dinámicamente a las normativas de visualización locales.
Por otro lado, al manejar volúmenes masivos de transacciones financieras y datos personales sensibles (tarjetas de crédito, domicilios, historiales de compra), el sistema se enfrenta a constantes intentos de ataques informáticos. Se requiere proteger estrictamente la base de datos de usuarios frente a brechas de seguridad, garantizar que las credenciales e información de pago estén cifradas tanto en tránsito como en reposo, y asegurar que cualquier intento de acceso no autorizado a los paneles de gestión sea bloqueado y auditado de inmediato, evitando la manipulación de precios o la exfiltración de datos de clientes.

1. "El sistema tiene que poder conectarse con las APIs de las diferentes empresas de logística y pasarelas de pago de forma que si una cambia algo, no se rompa todo el núcleo de la tienda."

## Paso 1 — Evaluación de completitud

Tu escenario da pistas claras de la **Fuente** y el **Estímulo**, y una versión negativa de la **Respuesta**, pero faltan o son ambiguas 3 partes:

- **Artefacto**: decís "no se rompa todo el núcleo de la tienda", pero el núcleo es lo que *no* debe verse afectado, no necesariamente la parte cuyo comportamiento se caracteriza. Falta identificar el componente que absorbe el cambio (típicamente una capa de adaptadores/integración).
- **Entorno**: no se especifica la condición operacional (¿producción normal? ¿durante un despliegue?).
- **Estímulo**: "si una cambia algo" es genérico — conviene precisar qué tipo de cambio (versión de API, formato de datos, endpoint, credenciales de autenticación, etc.).
- **Medida de la respuesta**: no hay ningún criterio medible (tiempo de adaptación, % de disponibilidad del núcleo, etc.).

Avanzo con los mejores supuestos posibles, marcados explícitamente como tales.

## Paso 2 — Escenario estructurado (6 partes)

| Parte | Contenido | Origen |
|---|---|---|
| Fuente del estímulo | Proveedor externo de logística o pasarela de pago | Dado |
| Estímulo | Cambio en su API (versión, formato de datos o contrato del servicio) | Dado (generalizado) / **[Supuesto]** el tipo específico de cambio |
| Artefacto | Capa de adaptadores/integración que conecta el núcleo con las APIs externas | **[Supuesto]** — inferido, no nombrado explícitamente en el texto |
| Entorno | Operación normal de la tienda (no durante un pico de tráfico) | **[Supuesto]** |
| Respuesta | El cambio se absorbe en el adaptador correspondiente; el núcleo de e-commerce sigue funcionando sin modificaciones ni interrupciones | Dado |
| Medida de la respuesta | El núcleo permanece con 0% de downtime; el adaptador se actualiza en un plazo acotado (ej. <5 días hábiles) | **[Supuesto - Straw Man]**, a validar con stakeholders |

**Atributo de calidad principal: Modificabilidad** (no Interoperabilidad — la interoperabilidad mide el *intercambio correcto de datos en tiempo real*; esto describe la *capacidad del sistema de absorber cambios externos sin propagar el impacto*, que es justamente la definición de Modificabilidad).

## Paso 3 — Integración en el Árbol de Utilidad

Este escenario refina y reemplaza al **QA-2** que había estimado en el turno anterior — ahora con la fuente y el mecanismo de aislamiento (capa de adaptadores) más claros.

```
Utility
│
└── Modificabilidad
      └── Aislamiento de cambios en integraciones externas
            └── QA-2 (cambio de API de proveedor no rompe el núcleo) → (Alta, Media)
```

| ID | Atributo | Sub-característica | Escenario | Importancia | Dificultad |
|----|---|---|---|---|---|
| QA-2 | Modificabilidad | Aislamiento de cambios externos | Un cambio en la API de un proveedor de logística/pago se absorbe en la capa de integración sin afectar el núcleo | Alta | Media |

*Importancia Alta porque el modelo de negocio depende de múltiples proveedores cambiantes (Mercado Pago, Stripe, PayPal, Andreani, DHL); Dificultad Media porque es resoluble con un patrón arquitectónico conocido (adapter/gateway pattern), aunque mantenerlo actualizado para 5+ proveedores exige disciplina. Son valores sugeridos, a validar con dot voting.*

**Nota de tradeoff:** para que este aislamiento funcione bien, cada adaptador suele necesitar sus propias pruebas de contrato (contract testing), lo que compite un poco con **Testability/Time-to-Market** si el equipo es chico.

2. /atributos-calidad-sei "La aplicación web de la tienda online tiene que poder correr sin problemas raros de visualización tanto en computadoras como en los celulares de los clientes, sin importar el navegador que usen."

## Paso 1 — Evaluación de completitud

Este escenario da con claridad el **Artefacto** (la aplicación web de la tienda) y el **Entorno** (distintos dispositivos y navegadores), pero deja varias partes incompletas o ambiguas:

- **Fuente del estímulo**: no se dice quién origina el estímulo — se infiere que es el cliente al acceder, pero no está explícito.
- **Estímulo**: "correr sin problemas raros de visualización" describe el problema a evitar, no el evento disparador concreto (¿carga inicial de la página? ¿un cambio de tamaño de pantalla u orientación? ¿una actualización de versión del navegador?).
- **Respuesta**: está formulada en negativo ("sin problemas raros"); falta describir la acción positiva y observable del sistema.
- **Medida de la respuesta**: no hay ningún criterio medible/testeable.

Avanzo con los mejores supuestos posibles, marcados como tales.

## Paso 2 — Escenario estructurado (6 partes)

| Parte | Contenido | Origen |
|---|---|---|
| Fuente del estímulo | Cliente que accede a la tienda online | **[Supuesto]** |
| Estímulo | Carga o interacción con la aplicación web desde un dispositivo (desktop o móvil) y un navegador determinado | Dado (generalizado) |
| Artefacto | Aplicación web de la tienda (PWA) | Dado |
| Entorno | Distintas combinaciones de dispositivo (computadora, celular) y navegador (Chrome, Safari, Firefox, Edge) | Dado |
| Respuesta | La página se renderiza correctamente, sin errores visuales ni de layout, manteniendo la misma funcionalidad | **[Supuesto]** — versión positiva de lo que el texto pide evitar |
| Medida de la respuesta | 0 defectos visuales críticos reportados por combinación dispositivo/navegador probada; 100% de paridad funcional en las combinaciones soportadas | **[Supuesto - Straw Man]**, a validar con stakeholders |

**Atributo de calidad principal: Usabilidad**, con un fuerte componente de **Portabilidad** (la capacidad del artefacto de comportarse igual en distintos entornos de ejecución sin modificarse). Ambos coexisten aquí: el usuario percibe "usabilidad" (algo se ve mal, es difícil de usar), pero la causa raíz arquitectónica es de portabilidad (el mismo código corriendo en motores de renderizado distintos).

## Paso 3 — Integración en el Árbol de Utilidad

Este escenario refina al **QA-3** estimado anteriormente.

```
Utility
│
└── Usabilidad / Portabilidad
      └── Consistencia de renderizado cross-browser / cross-device
            └── QA-3 (visualización correcta en distintos navegadores y dispositivos) → (Media, Media)
```

| ID | Atributo | Sub-característica | Escenario | Importancia | Dificultad |
|----|---|---|---|---|---|
| QA-3 | Usabilidad / Portabilidad | Consistencia de renderizado cross-browser/device | La app web se renderiza sin errores visuales en desktop y móvil, en Chrome, Safari, Firefox y Edge | Media | Media |

*Importancia Media-Alta en un negocio de e-commerce (un defecto visual en el checkout puede traducirse directamente en pérdida de ventas, así que si el negocio depende fuertemente de conversión podría subirse a Alta). Dificultad Media porque hay herramientas maduras (testing cross-browser, frameworks responsive), pero motores de renderizado distintos (WebKit en Safari vs Blink en Chrome/Edge vs Gecko en Firefox) siempre generan casos borde. Valores sugeridos, a validar con dot voting.*

**Nota de tradeoff:** una estrategia agresiva de "pixel-perfect" en todos los navegadores puede competir con **Time-to-Market/Modificabilidad**, ya que cada ajuste específico de navegador agrega código condicional que hay que mantener.

3. /atributos-calidad-sei "Si un usuario malintencionado intenta vulnerar el panel de administración o hacer fuerza bruta para entrar a cuentas ajenas, el sistema tiene que bloquearlo rápido y registrar la auditoría."

## Paso 1 — Evaluación de completitud

Este escenario da con bastante claridad la **Fuente**, el **Estímulo** y la **Respuesta**, pero deja partes ambiguas o incompletas:

- **Artefacto**: mencionás dos superficies distintas — el panel de administración y las cuentas de usuarios ajenas. Estrictamente son dos escenarios de seguridad diferentes (uno protege el panel de gestión, otro protege cuentas de clientes), aunque comparten el mismo patrón de respuesta. Los trato por separado para no perder precisión.
- **Entorno**: no se especifica la condición operacional (¿en producción, en horario pico, durante un ataque distribuido?).
- **Medida de la respuesta**: "bloquearlo rápido" no es medible/testeable tal cual está formulado — falta un número concreto (tiempo de bloqueo, cantidad de intentos permitidos, tiempo de generación del registro de auditoría).

Avanzo con los mejores supuestos posibles, marcados como tales.

## Paso 2 — Escenarios estructurados (6 partes)

**QA-5a — Seguridad (fuerza bruta / intrusión en el panel de administración)**

| Parte | Contenido | Origen |
|---|---|---|
| Fuente del estímulo | Usuario malintencionado (externo o interno) | Dado |
| Estímulo | Intento de vulnerar o forzar el acceso al panel de administración | Dado |
| Artefacto | Panel de administración centralizado | Dado |
| Entorno | Operación normal | **[Supuesto]** |
| Respuesta | El sistema bloquea el acceso y genera un registro de auditoría | Dado |
| Medida de la respuesta | Bloqueo tras N intentos fallidos (ej. 5) en <1 s; 100% de los intentos quedan registrados en el log de auditoría | **[Supuesto - Straw Man]**, a validar |

**QA-5b — Seguridad (fuerza bruta sobre cuentas de clientes)**

| Parte | Contenido | Origen |
|---|---|---|
| Fuente del estímulo | Usuario malintencionado (externo) | Dado |
| Estímulo | Intentos repetidos de autenticación (fuerza bruta) contra una cuenta de cliente ajena | Dado |
| Artefacto | Módulo de autenticación de cuentas de clientes | **[Supuesto]** — inferido, no nombrado explícitamente |
| Entorno | Operación normal | **[Supuesto]** |
| Respuesta | El sistema bloquea temporalmente la cuenta/IP y registra el intento en la auditoría | Dado |
| Medida de la respuesta | Bloqueo tras N intentos fallidos (ej. 5) en una ventana de tiempo (ej. 10 min); 100% de intentos auditados; alerta al equipo de seguridad si se detecta un patrón distribuido | **[Supuesto - Straw Man]**, a validar |

**Atributo de calidad principal: Seguridad**, sub-característica de **Autenticación / Control de acceso** con **Auditoría** como respuesta asociada — ambos aparecen juntos en la bibliografía (Díaz Pace, diapositiva 15) como parte del mismo patrón de detección-respuesta.

## Paso 3 — Integración en el Árbol de Utilidad

Estos escenarios refinan y reemplazan al **QA-5** estimado en el primer turno, separándolo en dos ramas concretas.

```
Utility
│
└── Seguridad
      └── Autenticación / Control de acceso
            ├── QA-5a (fuerza bruta contra el panel de administración) → (Alta, Media)
            └── QA-5b (fuerza bruta contra cuentas de clientes) → (Alta, Media)
```

| ID | Atributo | Sub-característica | Escenario | Importancia | Dificultad |
|----|---|---|---|---|---|
| QA-5a | Seguridad | Autenticación / Auditoría (panel admin) | Bloqueo y registro ante intento de vulnerar el panel de administración | Alta | Media |
| QA-5b | Seguridad | Autenticación / Auditoría (cuentas de clientes) | Bloqueo y registro ante fuerza bruta sobre una cuenta ajena | Alta | Media |

*Importancia Alta en ambos: comprometer el panel de admin habilita manipulación de precios o exfiltración masiva (mencionado en tu contexto original), y comprometer cuentas de clientes expone datos de pago y domicilios. Dificultad Media porque son patrones bien conocidos (rate limiting, lockout progresivo, MFA, logging centralizado), aunque distinguir un ataque distribuido (credential stuffing desde múltiples IPs) de tráfico legítimo agrega complejidad. Valores sugeridos, a validar con los stakeholders reales.*

**Nota de tradeoff:** un bloqueo muy agresivo (pocos intentos permitidos) mejora Seguridad pero perjudica **Usabilidad** (clientes legítimos que se equivocan de contraseña quedan bloqueados) — vale la pena definir el umbral junto con el equipo de producto, no solo con el de seguridad.
