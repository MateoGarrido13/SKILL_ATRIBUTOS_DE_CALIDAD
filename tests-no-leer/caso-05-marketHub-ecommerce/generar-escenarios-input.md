# Caso 05 — MarketHub (E-commerce) — input para `generar-escenarios-calidad`

Caso compartido a nivel de grupo/curso (usado también por las ramas `mateo`, `rossi` y `otaño`). Se incluye acá como benchmark cruzado: sirve para detectar en qué difiere esta skill de las otras implementaciones del mismo enunciado, no solo para confirmar que "funciona".

## Contexto del sistema

Contexto del Sistema: "MarketHub Plataforma Global de E-commerce"

Una empresa minorista planea lanzar MarketHub, una plataforma integral de comercio electrónico multicanal orientada tanto a clientes finales como a vendedores externos (partners). El sistema se compondrá de una aplicación Web progresiva (PWA) para compradores, una aplicación móvil nativa y un panel de administración centralizado en la nube.

El modelo de negocio exige que la plataforma se conecte de manera fluida con múltiples pasarelas de pago regionales (como Mercado Pago, Stripe y PayPal) y con sistemas logísticos de terceros (como Andreani y DHL) para calcular costos de envío en tiempo real y emitir etiquetas de despacho. Estas integraciones deben adaptarse a los cambiantes protocolos y SDKs que cada proveedor exija, sin comprometer el funcionamiento general del núcleo del e-commerce.

Asimismo, la empresa opera en distintos países, por lo que el front-end de la tienda y la aplicación móvil deben garantizar una experiencia de usuario idéntica y un correcto renderizado tanto en dispositivos móviles iOS y Android como en navegadores de escritorio (Chrome, Safari, Firefox y Edge), adaptándose dinámicamente a las normativas de visualización locales.

Por otro lado, al manejar volúmenes masivos de transacciones financieras y datos personales sensibles (tarjetas de crédito, domicilios, historiales de compra), el sistema se enfrenta a constantes intentos de ataques informáticos. Se requiere proteger estrictamente la base de datos de usuarios frente a brechas de seguridad, garantizar que las credenciales e información de pago estén cifradas tanto en tránsito como en reposo, y asegurar que cualquier intento de acceso no autorizado a los paneles de gestión sea bloqueado y auditado de inmediato, evitando la manipulación de precios o la exfiltración de datos de clientes.

## Requerimientos vagos (input real)

1. "El sistema tiene que poder conectarse con las APIs de las diferentes empresas de logística y pasarelas de pago de forma que si una cambia algo, no se rompa todo el núcleo de la tienda."
2. "La aplicación web de la tienda online tiene que poder correr sin problemas raros de visualización tanto en computadoras como en los celulares de los clientes, sin importar el navegador que usen."
3. "Los datos de las tarjetas de crédito de los compradores y las contraseñas tienen que estar recontra seguros para que ningún hacker pueda robarlos de la base de datos."
4. "Si un usuario malintencionado intenta vulnerar el panel de administración o hacer fuerza bruta para entrar a cuentas ajenas, el sistema tiene que bloquearlo rápido y registrar la auditoría."
5. "Cuando un vendedor externo quiera sincronizar su propio catálogo de productos con el inventario de MarketHub, la plataforma debe permitir integrar ese sistema externo mediante adaptadores estándar."
