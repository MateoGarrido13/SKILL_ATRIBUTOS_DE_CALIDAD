# Asistente IA (Gemini) para ASRs y Atributos de Calidad

**Trabajo Práctico: Diseño de Software · Memoria Técnica de Desarrollo y Pruebas**

| | |
|---|---|
| **Tipo de herramienta** | Prompt de Sistema (System Prompt) estructurado con etiquetas XML, diseñado para Google Gemini. |
| **Funcionalidades** | `1. Creación de Atributos de Calidad` · `2. Verificación de Completitud` · `3. Generación de Árbol de Utilidad` |
| **Objetivo principal** | Automatizar el análisis y la validación de Requerimientos Arquitectónicamente Significativos (ASRs) usando el estándar de 6 partes del SEI y el enfoque *Straw man*. |

---

## 1. Arquitectura y proceso de desarrollo

El repositorio fue organizado manteniendo una estructura minimalista y enfocada en la portabilidad . Se priorizaron archivos de texto plano (`.md` y `.xml`) para garantizar su fácil lectura e integración en cualquier entorno.

El armado de la herramienta se dividió en las siguientes fases:

- **Base de conocimiento:** Se destiló la teoría fundamental de la materia (los 10 atributos de calidad válidos, la plantilla de 6 partes del SEI y patrones arquitectónicos clave como N-Tier o Pub-Sub) . Se omitió deliberadamente el texto del enunciado del TP para evitar que el modelo sufriera "Confusión de Roles" .
- **Núcleo del asistente (`skill_prompt.xml`):** Se diseñó el prompt maestro utilizando un esquema de etiquetas XML (`<role>`, `<rules>`, `<tasks>`) . Esto permitió encapsular el contexto y evitar que las instrucciones interfirieran entre sí .
- **Enrutamiento Inteligente:** Se incorporó un bloque `<input_classifier>` que dota a la skill de autonomía . El asistente analiza silenciosamente la morfología del texto ingresado (por ejemplo, si es una oración aislada o una lista de requerimientos) y decide por su cuenta qué tarea ejecutar .
- **Batería de pruebas (`testing/`):** Se diseñaron 5 casos de validación exhaustivos basados en la bibliografía de la asignatura (cubriendo Performance, Disponibilidad, Escalabilidad, Modificabilidad y el armado del Utility Tree) .

---

## 2. Desafíos técnicos y ajustes de configuración

Durante las iteraciones de prueba y error en Gemini, nos topamos con algunas alucinaciones del modelo y desviaciones en el formato que nos obligaron a recalibrar el prompt .

### Inconveniente 1 — Pérdida del formato de salida (Falta de bloque Markdown)

Para que el resultado fuera fácil de exportar, la skill tenía la obligación de devolver su análisis exclusivamente dentro de un bloque de código Markdown .

> **Comportamiento esperado:** La IA debía procesar la solicitud y retornar solo el recuadro negro de código, sin saludos iniciales ni comentarios de cierre .
>
> **Output inicial:** Si bien el modelo resolvía bien el ejercicio (identificando partes faltantes y aplicando el *Straw man*), entregaba la respuesta como texto enriquecido normal . Ignoraba la restricción de formato porque esta instrucción había quedado muy arriba en el contexto.
>
> **Solución implementada:** Retiramos la regla de formato de la sección general `<rules>` y creamos un bloque `<output_format>` crítico ubicado al final absoluto del archivo XML . Además, le dimos un ejemplo visual exacto de cómo debía verse la salida.
>
> **Resultado:** A partir de ese ajuste, la skill comenzó a encapsular el 100% de sus respuestas dentro del formato requerido, eliminando el "ruido" extra y permitiendo copiar el análisis con un clic .

### Inconveniente 2 — Mala elección de tarea en inputs breves (Fallo del clasificador)

Al testear un requerimiento simple y crudo (como "procesar transacciones con latencia de 2 segundos"), la herramienta no supo qué camino tomar .

> **Comportamiento esperado:** Frente a un requerimiento único e incompleto, el modelo debía activar automáticamente la Tarea 2 (Verificación de Completitud) .
>
> **Output inicial (extracto):** La IA se confundió e intentó ejecutar la Tarea 3 (Árbol de Utilidad), armando una tabla jerárquica forzada con columnas de Riesgo y Valor para una sola oración descriptiva .
>
> **Solución implementada:** Introdujimos el `<input_classifier>` . Le dimos reglas estrictas para que evalúe el input antes de responder: si detecta una oración breve -> va a la Tarea 2; si detecta una enumeración de casos -> va a la Tarea 3 .
>
> **Resultado:** El asistente logró independencia total . Con el mismo input de latencia, detectó que faltaba el "Artefacto", propuso un Gestor de Transacciones (*Straw man*) y construyó el escenario estructurado en las 6 partes del SEI sin requerir instrucciones extra por parte del usuario .

---

## 3. Restricciones y puntos de mejora

**① Limitación a una sola tarea por interacción.**
Debido a la arquitectura que le dimos al prompt, el asistente solo puede ejecutar *una* de las tres funciones a la vez . Si el usuario envía un texto largo y pide simultáneamente que se completen los escenarios y se genere el árbol de utilidad, la IA priorizará solo una de las acciones . El flujo de trabajo con esta skill debe ser iterativo por naturaleza .

**② Riesgo de saturación de contexto ("Lost in the middle").**
La efectividad y precisión del asistente dependen de mantener un archivo de teoría (`referencia_catedra.md`) muy conciso . Si en un futuro se intentara escalar la herramienta adjuntándole capítulos enteros de libros o manuales extensos en PDF, el exceso de tokens provocaría que el LLM diluya su atención, olvidando las reglas del clasificador interno y rompiendo el formato de salida .

---

## 4. Prueba de Integración — Ejecución del Asistente

Como validación final, se corrió el pipeline de la skill contra los escenarios provistos por la cátedra para comprobar su autonomía. El objetivo era verificar si la herramienta lograba primero diagnosticar y completar escenarios ambiguos empleando el *Straw man* (Tarea 2), y luego consolidar dichos resultados en un Árbol de Utilidad formal (Tarea 3). A continuación se reproduce el output obtenido de las pruebas en Gemini.

### 4.1 Etapa 1 — Chequeo de Completitud y Generación de Escenarios (Tarea 2)

**Prompt utilizado:**
> Una empresa minorista planea lanzar MarketHub, una plataforma integral de

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

Para cada uno de estos 5 requqerimientos Generar Escenario de Atributo de Calidad y Chequear Completitud.

**Output obtenido:**
### Tarea 2: Chequear Completitud y Generar Escenarios 

He analizado el material suministrado y detectado los cinco escenarios planteados como "Ejercicios" (Performance, Seguridad, Disponibilidad, Usabilidad y Escalabilidad) . Todos se encuentran incompletos, ya que omiten varias de las 6 partes del template de atributos de calidad del SEI o poseen medidas ambiguas . A continuación, identifico las partes faltantes, aplico tácticas arquitectónicas y defino valores iniciales (*Straw man response measure*) para reescribirlos íntegramente .

#### Escenario 1: Performance 
**Texto original:** "Bajo condiciones normales de operación, el sistema debe procesar las transacciones de los usuarios con una latencia promedio de 2 segundos." 
**Diagnóstico:** Faltan definir explícitamente la Fuente del estímulo, el Estímulo, el Artefacto y la Respuesta .

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Usuario final / Cliente del sistema.  |
| **Estímulo** | Inicia un flujo de transacciones de negocio.  |
| **Artefacto** | Servidor principal / Módulo de procesamiento.  |
| **Ambiente** | Condiciones normales de operación.  |
| **Respuesta** | El sistema recibe, encola y procesa la solicitud asíncronamente optimizando los recursos (Táctica: Incrementar eficiencia de recursos / Concurrencia).  |
| **Medida de Respuesta** | El 100% de las transacciones se procesan con una latencia promedio de **2 segundos**.  |

#### Escenario 2: Seguridad 
**Texto original:** "El sistema debe mantener información de auditoría... En caso de un ataque, la imagen correcta de los datos... debe restaurarse en menos de 1 día." 
**Diagnóstico:** Faltan identificar la Fuente del estímulo, el Estímulo exacto y el Artefacto .

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Atacante externo o usuario interno malicioso.  |
| **Estímulo** | Modifica, corrompe o elimina información saltándose los controles convencionales.  |
| **Artefacto** | Base de datos principal / Repositorio.  |
| **Ambiente** | Tiempo de ejecución (Sistema bajo ataque).  |
| **Respuesta** | El sistema registra el evento de modificación en un log inmutable y provee un mecanismo seguro para que el administrador aplique un rollback (Táctica: Mantener registro de auditoría / Restaurar estado).  |
| **Medida de Respuesta** | La imagen original de los datos se restaura correctamente en menos de **1 día**, garantizando **0%** de pérdida de registros previos no comprometidos.  |

#### Escenario 3: Disponibilidad 
**Texto original:** "Si el controlador detecta una falla en el procesador principal durante la operación normal, pasará el control al procesador de backup." 
**Diagnóstico:** Falta definir la Fuente del estímulo y la Medida de respuesta es inexistente . Se requiere aplicar *Straw man* .

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Falla interna de hardware o corrupción del sistema operativo.  |
| **Estímulo** | El procesador principal colapsa (crash) y deja de responder peticiones.  |
| **Artefacto** | Controlador / Procesador principal.  |
| **Ambiente** | Operación normal.  |
| **Respuesta** | El controlador detecta la caída mediante la ausencia de señal (*heartbeat*), aísla el componente y transfiere de inmediato el tráfico al procesador de respaldo (Táctica: Ping-Echo / Redundancia Activa).  |
| **Medida de Respuesta (Straw man)** | El failover automático se completa en un tiempo menor a **500 milisegundos** sin interrumpir las sesiones activas de los usuarios.  |

#### Escenario 4: Usabilidad 
**Texto original:** "Los usuarios deben poder minimizar el impacto de los errores cancelando la operación en curso, siendo el tiempo de cancelación menor a 1 segundo." 
**Diagnóstico:** Faltan identificar el Artefacto, el Ambiente y precisar detalladamente el Estímulo .

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Usuario interactuando con el sistema.  |
| **Estímulo** | Ejecuta accidentalmente una operación errónea y presiona la opción de "Cancelar".  |
| **Artefacto** | Interfaz de Usuario (UI) / Hilo de ejecución de la tarea.  |
| **Ambiente** | Tiempo de ejecución (Operación prolongada en curso).  |
| **Respuesta** | El sistema provee un control siempre visible para abortar, detiene el procesamiento en background y revierte el estado a su punto inicial seguro (Táctica: Cancelar comando).  |
| **Medida de Respuesta** | El tiempo transcurrido desde el clic hasta abortar la operación y liberar la interfaz es menor a **1 segundo**.  |

#### Escenario 5: Escalabilidad 
**Texto original:** "El sistema debe ser capaz de gestionar despliegues de hasta 1000 dispositivos... sin degradar sus tiempos de procesamiento" 
**Diagnóstico:** Falta la Respuesta arquitectónica formal y la Medida de respuesta es muy ambigua ("sin degradar") . Se requiere aplicar *Straw man* .

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Crecimiento planificado de la infraestructura operativa.  |
| **Estímulo** | Se integran simultáneamente hasta 1000 nuevos dispositivos que generan volúmenes masivos de eventos.  |
| **Artefacto** | Módulo de ingesta de datos / Balanceador de carga.  |
| **Ambiente** | Tiempo de ejecución (Saturación de carga extendida).  |
| **Respuesta** | El sistema detecta el incremento de conexiones, aprovisiona dinámicamente recursos adicionales y distribuye la carga entre los nodos activos (Táctica: Escalado horizontal).  |
| **Medida de Respuesta (Straw man)** | El sistema procesa ininterrumpidamente **10.000 eventos por segundo**, manteniendo en el **99%** de los casos la latencia base menor a **2 segundos**.  |

### 4.2 Etapa 2 — Elaboración del Árbol de Utilidad (Tarea 3)

**Prompt utilizado:**
> en base a todo esto podrias armar el utility tree en formato markdown 

**Output obtenido:**

| Atributo de Calidad | Refinamiento | ASR (Resumen) | Prioridad (Valor, Riesgo) |
|---|---|---|---|
| Performance | Latencia de transacciones | Procesar transacciones de usuarios con latencia promedio de 2 segundos bajo operación normal. | (H, M) | 
| Seguridad | Auditoría y Recuperación | Mantener log de modificaciones y restaurar datos comprometidos por ataques en menos de 1 día. | (H, H) | 
| Disponibilidad | Tolerancia a fallos | Transferir el control al procesador de backup si el controlador detecta falla en el principal. | (H, H) | 
| Usabilidad | Cancelación de operaciones | Permitir a los usuarios cancelar una operación en curso en menos de 1 segundo para minimizar errores. | (M, L) | 
| Escalabilidad | Soporte de dispositivos | Gestionar despliegues masivos de hasta 1000 dispositivos sin degradar tiempos de procesamiento. | (H, M) | 

### 4.3 Conclusiones de la Integración
La herramienta demostró una excelente capacidad para aplicar la teoría arquitectónica de forma contextual. En la primera fase (Tarea 2), detectó con éxito la falta de métricas (Medidas de Respuesta) y tácticas formales en todos los casos provistos, inyectando justificaciones técnicas válidas (ej. táctica *Ping-Echo* para Disponibilidad o latencias de *10.000 eventos/s* para Escalabilidad) y forzando la toma de decisiones. En la segunda fase (Tarea 3), logró abstraer esos mismos requerimientos extendidos y mapearlos en la matriz de un Árbol de Utilidad, asignando prioridades lógicas de Negocio y Riesgo de forma coherente y lista para la revisión de stakeholders.
