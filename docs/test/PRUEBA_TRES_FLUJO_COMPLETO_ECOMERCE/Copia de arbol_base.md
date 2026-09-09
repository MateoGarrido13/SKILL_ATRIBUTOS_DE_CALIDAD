# Árbol de Utilidad de Arquitectura

**Taxonomía del nivel 2:** ISO 9126. Cubre los atributos presentes en MarketHub: Integrabilidad entra en Functionality (interoperabilidad con terceros), Seguridad en Functionality (security), Modificabilidad en Maintainability y Portabilidad en Portability.

| Atributo de Calidad | Refinamiento | Escenario (ASR) | Prioridad (Negocio) | Dificultad (Técnica) |
| :--- | :--- | :--- | :--- | :--- |
| Integrabilidad | Integración de nueva versión de API logística | [Nueva versión de API de envío/etiquetas (Andreani o DHL) en desarrollo; ≤ 8 componentes](../2-escenarios/esc_1.md) | M | H |
| Integrabilidad | Incorporación de partner de catálogo/inventario | [Cuarto partner en despliegue, además de los tres del prelanzamiento; ≤ 5 módulos](../2-escenarios/esc_5.md) | H | M |
| Modificabilidad | Cambio rutinario de pasarela comercial | [Cambio de protocolo de Mercado Pago en el adaptador, en diseño; ≤ 2 semanas. La API de Mercado Pago es sencilla de integrar y publica documentación oficial (contratos, ejemplos y versiones): eso achica la distancia con el proveedor y permite acotar el cambio al adaptador, sin reabrir el núcleo de la tienda.](../2-escenarios/esc_6.md) | M | L |
| Portabilidad | Adaptabilidad a nueva plataforma de navegador | [Incorporar Brave en la interfaz web, en tiempo de diseño; ≤ 2 semanas](../2-escenarios/esc_2.md) | M | M |
| Seguridad | Resistencia a autenticación fallida en venta | [Fuerza bruta de sesión en el checkout, operación normal online; corte al 5.º intento por 30 minutos](../2-escenarios/esc_3.md) | L | M |
| Seguridad | Autenticación de acceso al panel | [Intento de acceso al panel de administración, operación normal; 100% de rechazos sin facial y email](../2-escenarios/esc_4.md) | M | M |

## Cobertura y alertas

- Seguridad / confidencialidad de credenciales y tarjetas en reposo: *revisar cobertura: posible escenario no capturado*.
- Usabilidad (experiencia de compra equivalente entre dispositivos): *revisar cobertura: posible escenario no capturado*.
- Disponibilidad y Rendimiento del checkout bajo carga: *revisar cobertura: posible escenario no capturado*.
- No hay hojas `(H, H)`: ninguna queda como máxima prioridad de análisis por ese par.
- No aplica alerta de viabilidad/alcance por volumen de `(H, H)` (hace falta 3 o más, o al menos 2 y que sean más de la mitad de las hojas).

## Evidencia de ASR

El arquitecto dictamina si el impacto sobre la arquitectura es profundo. Acá solo la evidencia de las hojas con Prioridad H o M.

**Integrabilidad — nueva versión de API logística (M, H)**
- Artefactos afectados: capa de integración de MarketHub hacia operadores logísticos; el núcleo de la tienda debe seguir colaborando sin alterarse.
- Tácticas: Encapsulate; Use an intermediary; Abstract common services; Adhere to standards; Tailor interface (wrappers / bridges).
- Tradeoffs: aislar la distancia con Andreani o DHL (encapsular e intermediar) suele agregar saltos y afecta Rendimiento; más capas de adaptación también tensionan Modificabilidad si el contrato común se vuelve rígido. La dificultad H la fijaste por las discrepancias tecnológicas entre proveedores.

**Integrabilidad — cuarto partner de catálogo/inventario (H, M)**
- Artefactos afectados: capa de adaptadores de catálogo e inventario; intercambio con el inventario de MarketHub en despliegue.
- Tácticas: Encapsulate; Abstract common services; Adhere to standards; Use an intermediary; Tailor interface.
- Tradeoffs: una abstracción común para partners reduce el costo de sumar el cuarto sistema y puede agregar latencia (Rendimiento versus Modificabilidad / Integrabilidad). Abrir la tienda a un sistema externo también tensiona Seguridad (exposición del inventario).

**Modificabilidad — cambio de protocolo de Mercado Pago (M, L)**
- Artefactos afectados: adaptador de la pasarela Mercado Pago; el núcleo del e-commerce queda fuera del cambio.
- Tácticas: Encapsulate; Use an intermediary; Abstract common services; Restrict dependencies.
- Tradeoffs: encapsular el cobro en un adaptador facilita el cambio de protocolo y puede agregar un salto de Rendimiento (Performance versus Modificabilidad).
- Comentario del escenario: Mercado Pago ofrece una API sencilla de integrar y documentación oficial (contratos, ejemplos y versionado). Eso reduce la distancia sintáctica y semántica con el proveedor: el equipo puede acomodar el cambio en el adaptador, en la cota de 2 semanas, sin redistribuir responsabilidades del núcleo. Por eso la complejidad quedó en L.

**Portabilidad — incorporar Brave (M, M)**
- Artefactos afectados: interfaz gráfica de la aplicación web, en tiempo de diseño.
- Tácticas: Encapsulate; Abstract common services; diferir el binding de dependencias de plataforma (la Portabilidad es un sabor de Modificabilidad: aislar dependencias de plataforma).
- Tradeoffs: aislar la UI de cada navegador facilita sumar Brave y puede degradar Rendimiento (más indirección o tests de plataforma). Performance versus Modificabilidad.

**Seguridad — acceso al panel con doble factor (M, M)**
- Artefactos afectados: panel de administración centralizado.
- Tácticas: Identify actors; Authenticate actors (2FA: biometría facial y confirmación por email); Authorize actors; Limit access.
- Tradeoffs: exigir facial y email endurece el acceso y agrega pasos y tiempo (Performance versus Seguridad). Un bloqueo estricto también puede dejar fuera a un administrador legítimo (Seguridad versus Disponibilidad).
