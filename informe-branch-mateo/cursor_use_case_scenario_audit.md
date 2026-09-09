# Use case scenario audit
_Exported on 8/9/2026 at 20:00:47 ART from Cursor (3.18.9)_

---

**User**

Bajo el contexto de @docs/arquitectura/1-borradores/escenario_ecomerce.txt  auditar los escenarios de casos de uso presentes en @docs/arquitectura/1-borradores/  con @SKILL_ATRIBUTOS_DE_CALIDAD/.cursor/rules/1-generador-sei.mdc 

---

**Cursor**

Voy a leer el escenario de e-commerce, la regla del generador SEI y los borradores de casos de uso para auditarlos con ese criterio.

El archivo de contexto tiene otro nombre. Voy a leer ese contexto, los cinco escenarios y la regla de validación, que es la que corresponde a una auditoría.

Los cinco archivos son requerimientos en texto libre, no escenarios SEI. Sigo el flujo del generador: primero la teoría y las fichas de atributos, luego estructuro cada uno.

Clasifico cada requerimiento y leo las fichas de Integrabilidad, Modificabilidad, Usabilidad y Seguridad, que son las que cubren estos borradores.

El primer borrador mezcla Integrabilidad y Modificabilidad: van en dos escenarios. Redacto los seis como borradores SEI y dejo marcada cada parte que todavía no podés verificar.

Los cinco textos eran requerimientos en lenguaje libre, no escenarios verificables. Quedaron estructurados sobre MarketHub. El primero mezclaba dos atributos: **poder sumar** Mercado Pago, Stripe, PayPal, Andreani o DHL, y **aislar el núcleo** cuando uno de esos proveedores cambia el SDK. Por eso hay **seis** escenarios, todos en borrador.

Ninguno tiene aún una medida con umbral. Confirmá lo inferido y elegí (o proponé) la métrica de cada uno.

---

## 1. Integrar pasarelas y logística

[ESTADO: BORRADOR]  
**Atributo de Calidad:** Integrabilidad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Stakeholder de MarketHub o un proveedor externo de pasarela de pago o de logística (Mercado Pago, Stripe, PayPal, Andreani, DHL) |
| **Estímulo** | Integrar un componente nuevo, o una nueva versión de uno existente, para cobro o para cálculo de envío y emisión de etiquetas de despacho |
| **Artefacto** | [INFERIDO] Capa de integración de MarketHub hacia pasarelas de pago y operadores logísticos |
| **Ambiente** | [INFERIDO] Etapa de integración |
| **Respuesta** | Los componentes de la nueva configuración intercambian información de forma correcta (sintáctica y semántica) y colaboran con el núcleo de la tienda sin alterar su funcionamiento |
| **Medida de Respuesta** | [INCOMPLETO] |

### ⚠️ Faltan Datos para Estructurar el Escenario

Para poder generar un escenario SEI preciso, necesito que me aclares lo siguiente:

1. **Artefacto**: ¿El estímulo pega en una capa de integración (adaptadores de pago y envío) o en otro componente concreto de MarketHub?
2. **Ambiente**: ¿Esto ocurre en etapa de integración, en desarrollo, en despliegue o ya en runtime?
3. **Medida**: ¿Cómo mediremos que la integración está cumplida? Tres medidas pertinentes; el umbral lo fijás vos:
   - esfuerzo de integración, en persona-días;
   - tiempo calendario hasta que cobro o etiquetas intercambian datos de forma correcta, en días;
   - número de componentes del núcleo de la tienda que hay que modificar, en conteo.

*Por favor, responde a estas preguntas para generar la tabla.*

---

## 2. Cambio de protocolo o SDK de un proveedor ya conectado

[ESTADO: BORRADOR]  
**Atributo de Calidad:** Modificabilidad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Un proveedor externo de pasarela de pago o de logística que publica un cambio de protocolo o de SDK |
| **Estímulo** | Directiva de modificar la integración existente para acomodar ese cambio de protocolo o de SDK |
| **Artefacto** | [INFERIDO] Adaptadores de integración hacia pasarelas y logística; el núcleo de la tienda debe quedar fuera del cambio |
| **Ambiente** | [INFERIDO] Tiempo de diseño |
| **Respuesta** | Se hace, se testea y se incorpora la modificación en los adaptadores, sin comprometer el funcionamiento del núcleo del e-commerce |
| **Medida de Respuesta** | [INCOMPLETO] |

### ⚠️ Faltan Datos para Estructurar el Escenario

Para poder generar un escenario SEI preciso, necesito que me aclares lo siguiente:

1. **Artefacto**: ¿Lo que se modifica son solo los adaptadores, o también interfaces, configuraciones u otro módulo?
2. **Ambiente**: ¿El cambio se acomoda en tiempo de diseño, de build, de inicio o en runtime?
3. **Medida**: ¿Cómo mediremos que el núcleo no se rompió? Tres medidas pertinentes; el umbral lo fijás vos:
   - esfuerzo para hacer y testear el cambio, en persona-horas;
   - número de artefactos del núcleo modificados, en conteo;
   - tiempo calendario para incorporar el cambio sin efectos secundarios sobre cobro o despacho, en días.

*Por favor, responde a estas preguntas para generar la tabla.*

---

## 3. Renderizado de la tienda en distintos dispositivos y navegadores

[ESTADO: BORRADOR]  
**Atributo de Calidad:** Usabilidad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Cliente comprador (usuario final) |
| **Estímulo** | Usar la tienda online desde una computadora o un celular, con cualquiera de los navegadores soportados (Chrome, Safari, Firefox, Edge) |
| **Artefacto** | [INFERIDO] Interfaz gráfica de la aplicación web progresiva (PWA) de compradores |
| **Ambiente** | [INFERIDO] Runtime, operación normal |
| **Respuesta** | Renderiza la interfaz de forma correcta y mantiene una experiencia de uso equivalente entre dispositivos y navegadores, adaptándose a las normativas de visualización locales |
| **Medida de Respuesta** | [INCOMPLETO] |

### ⚠️ Faltan Datos para Estructurar el Escenario

Para poder generar un escenario SEI preciso, necesito que me aclares lo siguiente:

1. **Artefacto**: ¿Esto aplica solo a la PWA, o también a la app nativa iOS/Android?
2. **Ambiente**: ¿Lo evaluamos en operación normal, o también bajo pico de tráfico o en un país con normativa de visualización distinta?
3. **Medida**: “Sin problemas raros de visualización” no se puede testear. Tres medidas pertinentes; el umbral lo fijás vos:
   - número de defectos de renderizado por página o por sesión, en conteo;
   - ratio de páginas usables sobre el total, en %;
   - ratio de operaciones de compra iniciadas que el comprador completa sin abandonar por un error de interfaz, en %.

*Por favor, responde a estas preguntas para generar la tabla.*

---

## 4. Protección de tarjetas y contraseñas en la base de datos

[ESTADO: BORRADOR]  
**Atributo de Calidad:** Seguridad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Atacante humano externo (no confiable) que intenta robar datos |
| **Estímulo** | Intento no autorizado de capturar datos de tarjetas de crédito y contraseñas desde la base de datos |
| **Artefacto** | Base de datos de usuarios e información de pago (credenciales y tarjetas) |
| **Ambiente** | [INFERIDO] Sistema online, operación normal |
| **Respuesta** | Protege esos datos frente al acceso no autorizado; las credenciales y la información de pago permanecen cifradas en reposo |
| **Medida de Respuesta** | [INCOMPLETO] |

### ⚠️ Faltan Datos para Estructurar el Escenario

Para poder generar un escenario SEI preciso, necesito que me aclares lo siguiente:

1. **Ambiente**: ¿El ataque ocurre con el sistema online y operativo, detrás de un perímetro, o también en un modo degradado u offline?
2. **Medida**: “Recontra seguros” no permite un pasa/no-pasa. Tres medidas pertinentes; el umbral lo fijás vos:
   - porcentaje de credenciales e información de pago cifrados en reposo, en %;
   - cantidad de registros de tarjetas o contraseñas comprometidos en un intento, en conteo o %;
   - número (o %) de intentos de exfiltración resistidos.

*Por favor, responde a estas preguntas para generar la tabla.*

---

## 5. Bloqueo y auditoría de accesos indebidos al panel y a cuentas

[ESTADO: BORRADOR]  
**Atributo de Calidad:** Seguridad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Usuario malintencionado, externo y no confiable |
| **Estímulo** | Intento de acceso no autorizado al panel de administración o ataque de fuerza bruta contra cuentas ajenas |
| **Artefacto** | Panel de administración centralizado e inicio de sesión de cuentas de MarketHub |
| **Ambiente** | [INFERIDO] Sistema online, operación normal |
| **Respuesta** | Bloquea el intento, registra la auditoría y evita la manipulación de precios o la exfiltración de datos de clientes |
| **Medida de Respuesta** | [INCOMPLETO] |

### ⚠️ Faltan Datos para Estructurar el Escenario

Para poder generar un escenario SEI preciso, necesito que me aclares lo siguiente:

1. **Ambiente**: ¿El intento llega con el panel online y operativo, o también en un modo parcialmente operativo?
2. **Medida**: “Rápido” y “de inmediato” no son umbrales. Tres medidas pertinentes; el umbral lo fijás vos:
   - tiempo hasta detectar y bloquear el intento, en segundos (máximo o percentil que elijas);
   - precisión de la detección, en % de intentos maliciosos detectados;
   - porcentaje de ataques de fuerza bruta o de acceso al panel que quedan bloqueados y auditados, en %.

*Por favor, responde a estas preguntas para generar la tabla.*

---

## 6. Sincronizar el catálogo de un vendedor externo

[ESTADO: BORRADOR]  
**Atributo de Calidad:** Integrabilidad

| Parte del SEI | Definición Específica |
| :--- | :--- |
| **Fuente del Estímulo** | Vendedor externo (partner) que opera su propio catálogo |
| **Estímulo** | Integrar ese sistema de catálogo e inventario existente con el inventario de MarketHub |
| **Artefacto** | [INFERIDO] Capa de adaptadores de catálogo e inventario de MarketHub |
| **Ambiente** | [INFERIDO] Runtime |
| **Respuesta** | Integra el sistema externo mediante adaptadores estándar; ambos intercambian información de catálogo e inventario de forma correcta |
| **Medida de Respuesta** | [INCOMPLETO] |

### ⚠️ Faltan Datos para Estructurar el Escenario

Para poder generar un escenario SEI preciso, necesito que me aclares lo siguiente:

1. **Artefacto**: ¿La integración entra por una capa de adaptadores de catálogo e inventario, o por otro componente?
2. **Ambiente**: ¿El partner se conecta en runtime, o el trabajo ocurre en una etapa de integración / desarrollo?
3. **Medida**: “Adaptadores estándar” no dice cuándo la integración está bien hecha. Tres medidas pertinentes; el umbral lo fijás vos:
   - esfuerzo para completar, testear y habilitar la sincronización, en persona-días;
   - tiempo calendario hasta que catálogo e inventario intercambian datos de forma correcta, en días;
   - número de componentes propios de MarketHub que hay que cambiar para admitir a ese partner, en conteo.

*Por favor, responde a estas preguntas para generar la tabla.*

---

Respondé por escenario: confirmá o corregí artefacto y ambiente, y elegí una medida con su umbral (o proponé otra). Con eso reescribo las tablas sin marcas.

