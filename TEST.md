# INPUT PROMPT

```
Contexto del Sistema: "MarketHub Plataforma Global de E-commerce"

Una empresa minorista planea lanzar MarketHub, una plataforma integral de comercio electrónico multicanal orientada tanto a clientes finales como a vendedores externos (partners). El sistema se compondrá de una aplicación Web progresiva (PWA) para compradores, una aplicación móvil nativa y un panel de administración centralizado en la nube.
El modelo de negocio exige que la plataforma se conecte de manera fluida con múltiples pasarelas de pago regionales (como Mercado Pago, Stripe y PayPal) y con sistemas logísticos de terceros (como Andreani y DHL) para calcular costos de envío en tiempo real y emitir etiquetas de despacho. Estas integraciones deben adaptarse a los cambiantes protocolos y SDKs que cada proveedor exija, sin comprometer el funcionamiento general del núcleo del e-commerce.
Asimismo, la empresa opera en distintos países, por lo que el front-end de la tienda y la aplicación móvil deben garantizar una experiencia de usuario idéntica y un correcto renderizado tanto en dispositivos móviles iOS y Android como en navegadores de escritorio (Chrome, Safari, Firefox y Edge), adaptándose dinámicamente a las normativas de visualización locales.
Por otro lado, al manejar volúmenes masivos de transacciones financieras y datos personales sensibles (tarjetas de crédito, domicilios, historiales de compra), el sistema se enfrenta a constantes intentos de ataques informáticos. Se requiere proteger estrictamente la base de datos de usuarios frente a brechas de seguridad, garantizar que las credenciales e información de pago estén cifradas tanto en tránsito como en reposo, y asegurar que cualquier intento de acceso no autorizado a los paneles de gestión sea bloqueado y auditado de inmediato, evitando la manipulación de precios o la exfiltración de datos de clientes.
Escenarios Vagos (Inputs para la Skill 1 / borradores)
A partir del relato anterior, se desprenden 5 requerimientos que mezclan atributos de calidad sobre los cuales se deberá trabajar:
Escenario 1:
"El sistema tiene que poder conectarse con las APIs de las diferentes empresas de logística y pasarelas de pago de forma que si una cambia algo, no se rompa todo el núcleo de la tienda."
Escenario 2:
"La aplicación web de la tienda online tiene que poder correr sin problemas raros de visualización tanto en computadoras como en los celulares de los clientes, sin importar el navegador que usen."
Escenario 3:
"Los datos de las tarjetas de crédito de los compradores y las contraseñas tienen que estar recontra seguros para que ningún hacker pueda robarlos de la base de datos."
Escenario 4:
"Si un usuario malintencionado intenta vulnerar el panel de administración o hacer fuerza bruta para entrar a cuentas ajenas, el sistema tiene que bloquearlo rápido y registrar la auditoría."
Escenario 5:
"Cuando un vendedor externo quiera sincronizar su propio catálogo de productos con el inventario de MarketHub, la plataforma debe permitir integrar ese sistema externo mediante adaptadores estándar."

```




# OUTPUT: MarketHub — Escenarios de Atributos de Calidad

## Contexto del sistema

MarketHub es una plataforma global de e-commerce multicanal (PWA, app móvil nativa y panel de administración) que se integra con pasarelas de pago regionales (Mercado Pago, Stripe, PayPal) y sistemas logísticos de terceros (Andreani, DHL), opera en múltiples países y maneja datos financieros y personales sensibles.

A continuación se presentan los 5 escenarios originales, su auditoría de completitud según la plantilla de 6 partes del SEI, y la versión completada de cada uno. Al final se incluye el árbol de utilidad para priorizarlos.

---

## Escenario 1 — Modificabilidad / Interoperabilidad
**Original:** *"El sistema tiene que poder conectarse con las APIs de las diferentes empresas de logística y pasarelas de pago de forma que si una cambia algo, no se rompa todo el núcleo de la tienda."*

### Auditoría

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ⚠️ Vaga | No especifica quién genera el cambio (¿el proveedor externo?) |
| Estímulo | ⚠️ Vaga | "si una cambia algo" no define el tipo de cambio |
| Artefacto | ⚠️ Vaga | "el núcleo de la tienda" es demasiado amplio |
| Entorno | ❌ Falta | No se menciona si es en producción, desarrollo o despliegue |
| Respuesta | ⚠️ Vaga | "no se rompa todo" no especifica el mecanismo de aislamiento |
| Medida de la respuesta | ❌ Falta | No hay número ni criterio verificable |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un proveedor externo de pasarela de pago o logística (ej. Stripe, Andreani) |
| Estímulo | Publica una nueva versión de su API/SDK o modifica el contrato de su servicio |
| Artefacto | La capa de adaptadores de integración (payment gateway adapter / shipping provider adapter), separada del núcleo de checkout y catálogo |
| Entorno | Operación normal en producción |
| Respuesta | El sistema aísla el cambio dentro del adaptador correspondiente, sin afectar el núcleo del e-commerce ni las demás integraciones |
| Medida de la respuesta | El cambio se implementa modificando ≤ 1 adaptador, en ≤ 3 días-persona, sin regresiones ni tiempo de inactividad del checkout *(supuesto: ajustar según SLA real del equipo)* |

---

## Escenario 2 — Usabilidad / Portabilidad
**Original:** *"La aplicación web de la tienda online tiene que poder correr sin problemas raros de visualización tanto en computadoras como en los celulares de los clientes, sin importar el navegador que usen."*

### Auditoría

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ⚠️ Vaga | "los clientes" sin distinguir dispositivo/navegador |
| Estímulo | ⚠️ Vaga | "problemas raros de visualización" no es un estímulo concreto |
| Artefacto | ✅ | La aplicación web (PWA) de la tienda |
| Entorno | ⚠️ Vaga | Menciona navegadores pero no dispositivos/resoluciones |
| Respuesta | ⚠️ Vaga | "correr sin problemas" no define qué es correcto |
| Medida de la respuesta | ❌ Falta | No hay número ni criterio verificable |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un cliente final que accede desde un dispositivo móvil o de escritorio |
| Estímulo | Carga la PWA de la tienda en Chrome, Safari, Firefox o Edge, en distintas resoluciones de pantalla |
| Artefacto | La interfaz de la PWA (componentes de catálogo, carrito y checkout) |
| Entorno | Operación normal, en cualquiera de los navegadores y dispositivos soportados (iOS, Android, escritorio) |
| Respuesta | La interfaz se renderiza de forma consistente y funcional, sin errores visuales ni pérdida de funcionalidad |
| Medida de la respuesta | 0 defectos visuales críticos en el 100% de la matriz de navegadores/dispositivos soportados; tiempo de renderizado inicial < 2s en el percentil 95 *(supuesto: ajustar según matriz de soporte definida por negocio)* |

---

## Escenario 3 — Seguridad (datos de pago y credenciales)
**Original:** *"Los datos de las tarjetas de crédito de los compradores y las contraseñas tienen que estar recontra seguros para que ningún hacker pueda robarlos de la base de datos."*

### Auditoría

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ⚠️ Vaga | "ningún hacker" no especifica el vector de ataque |
| Estímulo | ⚠️ Vaga | "robarlos" no distingue acceso a BD vs. intercepción en tránsito |
| Artefacto | ⚠️ Vaga | "la base de datos" sin diferenciar tipos de datos |
| Entorno | ❌ Falta | No dice si es en tránsito, en reposo, o ambos |
| Respuesta | ⚠️ Vaga | "recontra seguros" no es accionable |
| Medida de la respuesta | ❌ Falta | No hay estándar ni número verificable |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un atacante externo que obtiene acceso no autorizado a la infraestructura o intercepta tráfico de red |
| Estímulo | Intenta leer o exfiltrar datos de tarjetas de crédito, contraseñas o domicilios almacenados |
| Artefacto | La base de datos de usuarios/transacciones y el canal de comunicación cliente-backend |
| Entorno | Tanto en tránsito (comunicación cliente-servidor) como en reposo (almacenamiento persistente) |
| Respuesta | Los datos sensibles permanecen ilegibles para el atacante; el sistema cumple con estándares de cifrado y tokenización de datos de pago |
| Medida de la respuesta | Cifrado TLS 1.2+ en tránsito, AES-256 en reposo, cumplimiento PCI-DSS, contraseñas con hashing (bcrypt/argon2); 0 incidentes de exposición en texto plano *(supuesto: nivel de cumplimiento a validar con seguridad/legal)* |

---

## Escenario 4 — Seguridad (intrusión y fuerza bruta)
**Original:** *"Si un usuario malintencionado intenta vulnerar el panel de administración o hacer fuerza bruta para entrar a cuentas ajenas, el sistema tiene que bloquearlo rápido y registrar la auditoría."*

### Auditoría

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ✅ | "un usuario malintencionado" |
| Estímulo | ⚠️ Vaga | No especifica el umbral/frecuencia que dispara la detección |
| Artefacto | ✅ | El panel de administración / cuentas de usuario |
| Entorno | ❌ Falta | No especifica si es en operación normal o ataque sostenido |
| Respuesta | ⚠️ Vaga | "bloquearlo rápido" no define el mecanismo |
| Medida de la respuesta | ❌ Falta | No hay número (tiempo de bloqueo, intentos permitidos) |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un usuario malintencionado (externo o cuenta comprometida) |
| Estímulo | Realiza múltiples intentos fallidos de login (fuerza bruta) o intenta explotar una vulnerabilidad contra el panel de administración |
| Artefacto | El módulo de autenticación del panel de administración |
| Entorno | Durante un ataque activo, en cualquier momento de operación |
| Respuesta | El sistema bloquea la cuenta/IP de origen, exige verificación adicional (MFA) y registra el evento en el log de auditoría, notificando al equipo de seguridad |
| Medida de la respuesta | Bloqueo automático tras 5 intentos fallidos en ≤ 60s; evento auditado y alerta enviada en ≤ 30s; 0 accesos no autorizados exitosos *(supuesto: umbrales a validar con seguridad)* |

---

## Escenario 5 — Interoperabilidad (sincronización de catálogo)
**Original:** *"Cuando un vendedor externo quiera sincronizar su propio catálogo de productos con el inventario de MarketHub, la plataforma debe permitir integrar ese sistema externo mediante adaptadores estándar."*

### Auditoría

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ✅ | "un vendedor externo" |
| Estímulo | ⚠️ Vaga | No especifica formato/protocolo ni frecuencia (tiempo real vs. batch) |
| Artefacto | ⚠️ Vaga | Menciona "adaptadores estándar" pero no el componente específico |
| Entorno | ❌ Falta | No indica operación normal vs. incompatibilidad |
| Respuesta | ⚠️ Vaga | No dice qué pasa si el formato es incompatible |
| Medida de la respuesta | ❌ Falta | No hay criterio verificable |

### Escenario completado

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un vendedor externo (partner) con su propio sistema de gestión de inventario |
| Estímulo | Envía una actualización de catálogo/stock mediante un formato estándar acordado (API REST, feed CSV/XML) |
| Artefacto | El adaptador de integración de catálogo/inventario de MarketHub |
| Entorno | Operación normal, incluyendo casos de formato/campo incompatible |
| Respuesta | El sistema procesa la actualización si el formato es válido, o la rechaza reportando el error al vendedor sin afectar el inventario de otros partners |
| Medida de la respuesta | ≥ 99% de sincronizaciones válidas procesadas en ≤ 5 min; incompatibilidades reportadas en ≤ 1 min; 0 afectación cruzada entre catálogos *(supuesto: SLA a validar con negocio)* |

---

## Árbol de Utilidad

*(Importancia y Dificultad propuestas como punto de partida — a validar con los stakeholders de negocio y seguridad)*

```
Utilidad (raíz)
├── Seguridad
│   ├── Protección de datos de pago y credenciales
│   │   └── Escenario 3: cifrado en tránsito/reposo, PCI-DSS, hashing de contraseñas (Alta, Alta)
│   └── Defensa contra intrusión y fuerza bruta en panel admin
│       └── Escenario 4: bloqueo automático + auditoría ante intentos maliciosos (Alta, Media)
├── Modificabilidad
│   └── Aislamiento de cambios en integraciones externas
│       └── Escenario 1: cambios en APIs de pago/logística no afectan el núcleo (Alta, Media)
├── Usabilidad / Portabilidad
│   └── Consistencia visual multi-dispositivo y multi-navegador
│       └── Escenario 2: renderizado correcto en iOS, Android y navegadores de escritorio (Alta, Media)
└── Interoperabilidad
    └── Sincronización de catálogo con sistemas de vendedores externos
        └── Escenario 5: adaptadores estándar para actualización de inventario (Media, Media)
```

### Tabla resumen de priorización

| Escenario | Atributo | Importancia | Dificultad | Prioridad |
|---|---|---|---|---|
| 3 — Protección de datos de pago y credenciales | Seguridad | Alta | Alta | 1 |
| 4 — Defensa contra intrusión / fuerza bruta | Seguridad | Alta | Media | 2 |
| 1 — Aislamiento de cambios en integraciones | Modificabilidad | Alta | Media | 2 |
| 2 — Consistencia multi-dispositivo/navegador | Usabilidad | Alta | Media | 2 |
| 5 — Sincronización de catálogo de vendedores | Interoperabilidad | Media | Media | 3 |

**Nota:** El Escenario 3 se marca como (Alta, Alta) porque combina alto impacto de negocio/legal (exposición de datos de tarjetas) con alta dificultad técnica (cumplimiento PCI-DSS, gestión de cifrado en múltiples capas), por lo que es el candidato prioritario para el análisis de arquitectura. Los valores de Importancia/Dificultad son una propuesta inicial razonada y deben validarse con los stakeholders del negocio y del equipo de seguridad.


# Informe breve: Diferencias entre resultados obtenidos y esperados

## 1. Nomenclatura del atributo de calidad (Escenario 1/A)

**Diferencia:** El esperado usa "Integrabilidad" como atributo único; se usó "Modificabilidad / Interoperabilidad" combinados.

**Por qué ocurre:** El catálogo de referencia utilizado (`quality-attributes-catalog.md`) no contempla "Integrabilidad" como categoría propia — es un término más usado en ingeniería de software en español para describir la facilidad de integrar sistemas externos, mientras que el estándar SEI/ATAM en inglés lo separa en Modificabilidad (costo de cambio) e Interoperabilidad (intercambio de datos entre sistemas).

**Cómo solventarlo:** Agregar "Integrabilidad" como atributo explícito en el catálogo de referencia, con su propia plantilla de 6 partes, en vez de forzarlo como híbrido de otros dos.

---

## 2. Ubicación de la cuantificación en el estímulo (Escenario 4/B)

**Diferencia:** El esperado pone el umbral numérico (">100 intentos/minuto") directamente en el **Estímulo**; en la versión obtenida se dejó solo en la **Medida de respuesta** (5 intentos/60s).

**Por qué ocurre:** Son dos formas válidas de modelar el mismo problema: cuantificar el disparador (cuándo se activa la detección) vs. cuantificar el resultado esperado (qué tan rápido responde el sistema). Sin un ejemplo de referencia previo, se tomó la convención de dejar los números solo en la parte 6, que es donde el SEI exige obligatoriamente un criterio verificable.

**Cómo solventarlo:** Aclarar en la skill que cuando el estímulo es *cuantificable por naturaleza* (volumen de tráfico, frecuencia de intentos), conviene poner el número también en el Estímulo, reservando la Medida de respuesta para el tiempo/calidad de la reacción del sistema — no solo para el volumen del disparador.

---

## 3. Nivel de agresividad de las métricas (200ms vs 60s, 8h vs 3 días)

**Diferencia:** El esperado propone métricas mucho más estrictas (bloqueo en 200ms, adaptador reimplementado en 8h) que las obtenidas (60s, 3 días-persona).

**Por qué ocurre:** Ninguno de los dos números viene de un SLA real del negocio — ambos son estimaciones. La diferencia refleja distintos supuestos implícitos sobre la madurez técnica del equipo y la infraestructura (ej. 200ms solo es alcanzable con un WAF o rate-limiter en el borde de red, no con lógica de aplicación). La versión obtenida fue conservadora y lo marcó explícitamente como supuesto; el esperado no marca la incertidumbre.

**Cómo solventarlo:** Esto solo se resuelve con el dato real: preguntar al equipo de seguridad/infraestructura qué mecanismo de bloqueo van a usar (WAF, rate-limiter, lógica de app) antes de fijar el número, en vez de que se estime a ciegas.

---

## 4. Discrepancia en la valoración de Dificultad técnica (árbol de utilidad)

**Diferencia:** Cifrado de datos: esperado = **M**, obtenido = **H**. Integración de pagos/logística: esperado = **H**, obtenido = **M**.

**Por qué ocurre:** Son juicios subjetivos sin una rúbrica compartida de qué hace "difícil" un escenario. En la versión obtenida se pesó el cumplimiento normativo (PCI-DSS) como factor que eleva la dificultad del cifrado, y se asumió que el patrón adaptador ya resuelve gran parte del riesgo de integración. El esperado parece pesar más el control sobre el proveedor externo (fuera del alcance del equipo) como el verdadero factor de riesgo técnico, y trata el cifrado como una implementación más estandarizada/conocida.

**Cómo solventarlo:** Definir una rúbrica explícita de Dificultad antes de puntuar (ej.: "H = requiere tecnología/proceso no dominado por el equipo o fuera de su control directo; M = solución conocida pero con esfuerzo de integración; L = patrón estándar ya implementado"). Sin esa rúbrica compartida, dos personas razonables llegan a valores distintos con el mismo escenario.

---

## 5. Formato de presentación del árbol de utilidad

**Diferencia:** El esperado usa una tabla plana de 5 columnas (Atributo | Sub-atributo | Escenario consolidado | Prioridad-Negocio | Dificultad-Técnica); la versión obtenida usó un árbol anidado en Markdown más una tabla resumen aparte.

**Por qué ocurre:** La skill deja el formato del árbol como "lista anidada por defecto" salvo que el usuario pida tabla, y en la versión obtenida se agregó la tabla resumen como complemento en vez de fusionar todo en un solo formato tabular como en el esperado.

**Cómo solventarlo:** Ninguna corrección real de contenido — es preferencia de formato. Si este es el formato esperado por la organización/cátedra, conviene fijarlo como default en la skill (tabla de 5 columnas) en vez de árbol + tabla separada, para no generar diferencias de forma en cada entrega.
