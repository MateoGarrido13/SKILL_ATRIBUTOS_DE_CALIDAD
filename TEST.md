# INPUT PROMPT

```
Contexto del Sistema: "MarketHub Plataforma Global de E-commerce"

Una empresa minorista planea lanzar MarketHub, una plataforma integral de comercio electrónico multicanal orientada tanto a clientes finales como a vendedores externos (partners). El sistema se compondrá de una aplicación Web progresiva (PWA) para compradores, una aplicación móvil nativa y un panel de administración centralizado en la nube.
El modelo de negocio exige que la plataforma se conecte de manera fluida con múltiples pasarelas de pago regionales (como Mercado Pago, Stripe y PayPal) y con sistemas logísticos de terceros (como Andreani y DHL) para calcular costos de envío en tiempo real y emitir etiquetas de despacho. Estas integraciones deben adaptarse a los cambiantes protocolos y SDKs que cada proveedor exija, sin comprometer el funcionamiento general del núcleo del e-commerce.
Asimismo, la empresa opera en distintos países, por lo que el front-end de la tienda y la aplicación móvil deben garantizar una experiencia de usuario idéntica y un correcto renderizado tanto en dispositivos móviles iOS y Android como en navegadores de escritorio (Chrome, Safari, Firefox y Edge), adaptándose dinámicamente a las normativas de visualización locales.
Por otro lado, al manejar volúmenes masivos de transacciones financieras y datos personales sensibles (tarjetas de crédito, domicilios, historiales de compra), el sistema se enfrenta a constantes intentos de ataques informáticos. Se requiere proteger estrictamente la base de datos de usuarios frente a brechas de seguridad, garantizar que las credenciales e información de pago estén cifradas tanto en tránsito como en reposo, y asegurar que cualquier intento de acceso no autorizado a los paneles de gestión sea bloqueado y auditado de inmediato, evitando la manipulación de precios o la exfiltración de datos de clientes.
Escenarios Vagos (Inputs para la Skill 1 / borradores)
A partir del relato anterior, se desprenden 5 requerimientos informales o "vagos" que mezclan atributos de calidad y sobre los cuales tu pipeline de skills (Generación $\rightarrow$ Validación $\rightarrow$ Árbol de Utilidad) deberá trabajar:
Escenario Vago 1 (Integrabilidad):
"El sistema tiene que poder conectarse con las APIs de las diferentes empresas de logística y pasarelas de pago de forma que si una cambia algo, no se rompa todo el núcleo de la tienda."
Escenario Vago 2 (Portabilidad):
"La aplicación web de la tienda online tiene que poder correr sin problemas raros de visualización tanto en computadoras como en los celulares de los clientes, sin importar el navegador que usen."
Escenario Vago 3 (Seguridad):
"Los datos de las tarjetas de crédito de los compradores y las contraseñas tienen que estar recontra seguros para que ningún hacker pueda robarlos de la base de datos."
Escenario Vago 4 (Seguridad):
"Si un usuario malintencionado intenta vulnerar el panel de administración o hacer fuerza bruta para entrar a cuentas ajenas, el sistema tiene que bloquearlo rápido y registrar la auditoría."
Escenario Vago 5 (Integrabilidad):
"Cuando un vendedor externo quiera sincronizar su propio catálogo de productos con el inventario de MarketHub, la plataforma debe permitir integrar ese sistema externo mediante adaptadores estándar."

```




# OUTPUT: MarketHub — Escenarios de Atributos de Calidad

**Plataforma Global de E-commerce — Generación, Validación y Árbol de Utilidad (Método SEI / ATAM)**

Este documento aplica el proceso de tres etapas del método SEI/ATAM sobre los cinco requerimientos informales relevados para MarketHub: (1) mapeo del enunciado original a la plantilla de 6 partes, (2) auditoría de completitud y propuesta de valores para completar cada parte faltante o vaga, y (3) construcción del árbol de utilidad para priorizar los escenarios frente al análisis de arquitectura.

---

## Escenario 1 — Integrabilidad: APIs de pasarelas de pago y logística

**Enunciado original:**
> "El sistema tiene que poder conectarse con las APIs de las diferentes empresas de logística y pasarelas de pago de forma que si una cambia algo, no se rompa todo el núcleo de la tienda."

### Auditoría de completitud

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ⚠️ Vaga | "Las empresas de logística y pasarelas de pago" — hay que nombrar que es un proveedor externo puntual (ej. Stripe, Andreani) el que actúa, no todas a la vez. |
| Estímulo | ⚠️ Vaga | "Cambia algo" no dice qué: ¿versión de API, formato de respuesta, endpoint, SDK? |
| Artefacto | ❌ Falta | Se dice qué NO debe romperse ("el núcleo de la tienda") pero no qué componente recibe el cambio. |
| Entorno | ❌ Falta | No se especifica si ocurre en producción, en despliegue, etc. |
| Respuesta | ⚠️ Vaga | "No se rompa todo" es cualitativo; falta describir cómo se aísla el impacto. |
| Medida de la respuesta | ❌ Falta | No hay ningún número o criterio verificable. |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un proveedor externo de pagos o logística (ej. Stripe, Mercado Pago, Andreani, DHL) |
| Estímulo | Modifica su contrato de integración (nueva versión de API, cambio de formato de respuesta o de SDK) |
| Artefacto | La capa de adaptadores de integración (adapter de pagos / adapter logístico), aislada del núcleo de checkout e inventario |
| Entorno | Operación normal en producción |
| Respuesta | El sistema absorbe el cambio dentro del adaptador correspondiente; el núcleo de checkout e inventario no requiere modificaciones y sigue operando sin interrupciones |
| Medida de la respuesta | El cambio se implementa en ≤ 3 días-persona (supuesto: ajustar según SLA real), tocando únicamente el adaptador afectado, con 0% de downtime del checkout durante la actualización |

---

## Escenario 2 — Portabilidad: renderizado multi-dispositivo y multi-navegador

*Nota: la portabilidad no está en el catálogo estándar de atributos de la skill (que cubre interoperabilidad, no portabilidad); se construyó con la misma plantilla de 6 partes.*

**Enunciado original:**
> "La aplicación web de la tienda online tiene que poder correr sin problemas raros de visualización tanto en computadoras como en los celulares de los clientes, sin importar el navegador que usen."

### Auditoría de completitud

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ⚠️ Vaga | "Los clientes" — falta especificar que el disparador es el tipo de dispositivo/navegador que usan. |
| Estímulo | ⚠️ Vaga | "Sin importar el navegador" no dice cuáles navegadores/SO en concreto. |
| Artefacto | ⚠️ Vaga | El sistema tiene PWA + app nativa + panel admin; "la aplicación web" hay que acotarlo al front-end de la PWA. |
| Entorno | ⚠️ Vaga | Se menciona "normativas de visualización locales" en el contexto general pero no en el escenario mismo. |
| Respuesta | ⚠️ Vaga | "Sin problemas raros de visualización" no es verificable. |
| Medida de la respuesta | ❌ Falta | No hay número ni criterio de aceptación. |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un comprador final que accede desde un dispositivo/navegador específico |
| Estímulo | Abre la PWA de la tienda desde Chrome, Safari, Firefox o Edge (desktop) o desde la app nativa en iOS/Android, en un país con normativas de visualización propias (moneda, idioma, formato de fecha) |
| Artefacto | El front-end de la PWA y la app móvil nativa (capa de presentación) |
| Entorno | Uso normal, en cualquiera de los mercados donde opera MarketHub |
| Respuesta | La interfaz se renderiza con el mismo layout, funcionalidad y datos localizados correctamente, sin errores visuales ni de funcionalidad |
| Medida de la respuesta | 0 defectos visuales críticos en la matriz de prueba (4 navegadores × iOS × Android) (supuesto: definir la matriz exacta con QA), tiempo de carga inicial < 3s en 4G (supuesto: ajustar según benchmark de negocio) |

---

## Escenario 3 — Seguridad: datos de tarjetas y contraseñas

**Enunciado original:**
> "Los datos de las tarjetas de crédito de los compradores y las contraseñas tienen que estar recontra seguros para que ningún hacker pueda robárselos de la base de datos."

### Auditoría de completitud

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ⚠️ Vaga | "Ningún hacker" está presente pero sin especificar tipo de amenaza (externo, interno, automatizado). |
| Estímulo | ⚠️ Vaga | "Robárselos" no dice el vector: acceso directo a BD, inyección SQL, dump de backup, etc. |
| Artefacto | ⚠️ Vaga | "La base de datos" es correcto pero se puede acotar a qué datos (tarjetas, credenciales). |
| Entorno | ❌ Falta | No se dice si es en operación normal o durante un ataque activo. |
| Respuesta | ⚠️ Vaga | "Recontra seguros" no es una respuesta accionable del sistema. |
| Medida de la respuesta | ❌ Falta | Sin ningún estándar o número verificable. |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un atacante externo (o interno malicioso) que obtiene acceso no autorizado a la infraestructura de datos |
| Estímulo | Intenta leer o exfiltrar datos de tarjetas de crédito o credenciales almacenadas (ej. mediante acceso directo a la BD, inyección, o robo de backup) |
| Artefacto | El almacenamiento de datos de pago y credenciales de usuarios |
| Entorno | Operación normal o durante un intento de intrusión activo |
| Respuesta | Los datos permanecen ilegibles para el atacante: cifrado en reposo, TLS en tránsito, contraseñas hasheadas (no reversibles) |
| Medida de la respuesta | Cifrado AES-256 en reposo, TLS 1.2+ en tránsito, hashing con Argon2/bcrypt (supuesto: alinear con requisito PCI-DSS real de la empresa), 0 incidentes de datos expuestos en texto plano |

---

## Escenario 4 — Seguridad: panel de administración y fuerza bruta

**Enunciado original:**
> "Si un usuario malintencionado intenta vulnerar el panel de administración o hacer fuerza bruta para entrar a cuentas ajenas, el sistema tiene que bloquearlo rápido y registrar la auditoría."

### Auditoría de completitud

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ✅ | "Usuario malintencionado" está presente; se puede precisar como bot/script de fuerza bruta. |
| Estímulo | ⚠️ Vaga | "Vulnerar" o "hacer fuerza bruta" no especifica el umbral (¿cuántos intentos?). |
| Artefacto | ✅ | "Panel de administración" / "cuentas ajenas" — razonablemente acotado al módulo de autenticación. |
| Entorno | ❌ Falta | No se indica el entorno (operación normal, fuera de horario, etc.). |
| Respuesta | ⚠️ Vaga | "Bloquearlo rápido" y "registrar la auditoría" están bien orientados pero sin mecanismo concreto. |
| Medida de la respuesta | ❌ Falta | "Rápido" no es medible. |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un actor malicioso (usuario o bot automatizado) |
| Estímulo | Realiza múltiples intentos fallidos de inicio de sesión contra el panel de administración o cuentas de otros usuarios (fuerza bruta) |
| Artefacto | El módulo de autenticación del panel de administración |
| Entorno | Operación normal, en cualquier horario |
| Respuesta | El sistema bloquea la cuenta/IP de origen, registra el intento en el log de auditoría y notifica al equipo de seguridad |
| Medida de la respuesta | Bloqueo automático tras 5 intentos fallidos en 60 segundos (supuesto: ajustar según política de seguridad real), 100% de los intentos quedan auditados, notificación al equipo de seguridad en ≤ 5 minutos |

---

## Escenario 5 — Integrabilidad: sincronización de catálogo de vendedores

**Enunciado original:**
> "Cuando un vendedor externo quiera sincronizar su propio catálogo de productos con el inventario de MarketHub, la plataforma debe permitir integrar ese sistema externo mediante adaptadores estándar."

### Auditoría de completitud

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ✅ | "Vendedor externo" está claramente identificado. |
| Estímulo | ⚠️ Vaga | "Quiera sincronizar su catálogo" no dice si es carga inicial, actualización periódica o evento puntual. |
| Artefacto | ⚠️ Vaga | "Adaptadores estándar" se menciona como mecanismo pero no como artefacto acotado (¿API REST? ¿motor de mapeo?). |
| Entorno | ❌ Falta | No se especifica el entorno. |
| Respuesta | ⚠️ Vaga | "Permitir integrar" es cualitativo; falta el resultado esperado del sistema. |
| Medida de la respuesta | ❌ Falta | Sin criterio verificable. |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un vendedor externo (partner) que gestiona su propio sistema de inventario |
| Estímulo | Solicita sincronizar (alta, baja o actualización de) su catálogo de productos con el inventario de MarketHub |
| Artefacto | El módulo de integración de catálogo (adaptador estándar, ej. API REST/JSON) |
| Entorno | Operación normal |
| Respuesta | El sistema integra los datos del catálogo externo sin requerir cambios en el núcleo de inventario, y reporta errores de formato si el adaptador detecta incompatibilidad |
| Medida de la respuesta | Onboarding de un nuevo vendedor en ≤ 2 días-persona (supuesto: ajustar según capacidad del equipo), ≥ 99% de sincronizaciones exitosas, latencia de sincronización < 10 min |

---

## Árbol de utilidad

*Importancia y Dificultad son propuestas a validar con los stakeholders de MarketHub.*

- **Utilidad (raíz)**
  - **Interoperabilidad / Integrabilidad**
    - Integración con pasarelas de pago y logística
      - Escenario 1: adaptador absorbe cambio de proveedor externo sin tocar el núcleo — (Alta, Alta)
    - Sincronización de catálogos de vendedores
      - Escenario 5: onboarding de vendedor vía adaptador estándar — (Media, Media)
  - **Portabilidad**
    - Renderizado consistente multi-dispositivo/navegador
      - Escenario 2: 0 defectos visuales críticos en matriz navegador/SO — (Media, Media)
  - **Seguridad**
    - Protección de datos sensibles en reposo/tránsito
      - Escenario 3: cifrado y hashing de tarjetas/credenciales — (Alta, Alta)
    - Defensa del panel de administración
      - Escenario 4: bloqueo y auditoría ante fuerza bruta — (Alta, Media)

### Tabla de priorización

| Escenario | Atributo | Importancia | Dificultad | Prioridad |
|---|---|---|---|---|
| 3 | Seguridad (datos de pago) | Alta | Alta | **1** |
| 1 | Integrabilidad (pagos/logística) | Alta | Alta | **2** |
| 4 | Seguridad (panel admin) | Alta | Media | **3** |
| 5 | Integrabilidad (catálogo vendedores) | Media | Media | **4** |
| 2 | Portabilidad | Media | Media | **5** |

Los escenarios (Alta, Alta) — protección de datos de pago (Escenario 3) y la integración desacoplada con proveedores externos (Escenario 1) — son los candidatos prioritarios para el análisis de arquitectura, por ser los de mayor impacto de negocio y mayor esfuerzo de diseño.
