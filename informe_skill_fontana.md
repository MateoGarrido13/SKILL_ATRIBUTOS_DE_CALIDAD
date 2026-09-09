# Asistente IA (Gemini) para ASRs y Atributos de Calidad

**Trabajo Práctico: Diseño de Software · Memoria Técnica de Desarrollo y Pruebas**

| | |
|---|---|
| **Tipo de herramienta** | Prompt de Sistema (System Prompt) estructurado con etiquetas XML, diseñado para Google Gemini[cite: 4]. |
| **Funcionalidades** | `1. Creación de Atributos de Calidad` · `2. Verificación de Completitud` · `3. Generación de Árbol de Utilidad`[cite: 4] |
| **Objetivo principal** | Automatizar el análisis y la validación de Requerimientos Arquitectónicamente Significativos (ASRs) usando el estándar de 6 partes del SEI y el enfoque *Straw man*[cite: 4]. |

---

## 1. Arquitectura y proceso de desarrollo

El repositorio fue organizado manteniendo una estructura minimalista y enfocada en la portabilidad[cite: 4]. Se priorizaron archivos de texto plano (`.md` y `.xml`) para garantizar su fácil lectura e integración en cualquier entorno[cite: 4].

El armado de la herramienta se dividió en las siguientes fases:

- **Base de conocimiento (`referencia_catedra.md`):** Se destiló la teoría fundamental de la materia (los 10 atributos de calidad válidos, la plantilla de 6 partes del SEI y patrones arquitectónicos clave como N-Tier o Pub-Sub)[cite: 4]. Se omitió deliberadamente el texto del enunciado del TP para evitar que el modelo sufriera "Confusión de Roles"[cite: 4].
- **Núcleo del asistente (`skill_prompt.xml`):** Se diseñó el prompt maestro utilizando un esquema de etiquetas XML (`<role>`, `<rules>`, `<tasks>`)[cite: 4]. Esto permitió encapsular el contexto y evitar que las instrucciones interfirieran entre sí[cite: 4].
- **Enrutamiento Inteligente:** Se incorporó un bloque `<input_classifier>` que dota a la skill de autonomía[cite: 4]. El asistente analiza silenciosamente la morfología del texto ingresado (por ejemplo, si es una oración aislada o una lista de requerimientos) y decide por su cuenta qué tarea ejecutar[cite: 4].
- **Batería de pruebas (`testing/`):** Se diseñaron 5 casos de validación exhaustivos basados en la bibliografía de la asignatura (cubriendo Performance, Disponibilidad, Escalabilidad, Modificabilidad y el armado del Utility Tree)[cite: 4].

---

## 2. Desafíos técnicos y ajustes de configuración

Durante las iteraciones de prueba y error en Gemini, nos topamos con algunas alucinaciones del modelo y desviaciones en el formato que nos obligaron a recalibrar el prompt[cite: 4].

### Inconveniente 1 — Pérdida del formato de salida (Falta de bloque Markdown)

Para que el resultado fuera fácil de exportar, la skill tenía la obligación de devolver su análisis exclusivamente dentro de un bloque de código Markdown[cite: 4].

> **Comportamiento esperado:** La IA debía procesar la solicitud y retornar solo el recuadro negro de código, sin saludos iniciales ni comentarios de cierre[cite: 4].
>
> ⚠️ **Output inicial:** Si bien el modelo resolvía bien el ejercicio (identificando partes faltantes y aplicando el *Straw man*), entregaba la respuesta como texto enriquecido normal[cite: 4]. Ignoraba la restricción de formato porque esta instrucción había quedado muy arriba en el contexto[cite: 4].
>
> ✅ **Solución implementada:** Retiramos la regla de formato de la sección general `<rules>` y creamos un bloque `<output_format>` crítico ubicado al final absoluto del archivo XML[cite: 4]. Además, le dimos un ejemplo visual exacto de cómo debía verse la salida[cite: 4].
>
> ✅ **Resultado:** A partir de ese ajuste, la skill comenzó a encapsular el 100% de sus respuestas dentro del formato requerido, eliminando el "ruido" extra y permitiendo copiar el análisis con un clic[cite: 4].

### Inconveniente 2 — Mala elección de tarea en inputs breves (Fallo del clasificador)

Al testear un requerimiento simple y crudo (como "procesar transacciones con latencia de 2 segundos"), la herramienta no supo qué camino tomar[cite: 4].

> **Comportamiento esperado:** Frente a un requerimiento único e incompleto, el modelo debía activar automáticamente la Tarea 2 (Verificación de Completitud)[cite: 4].
>
> ⚠️ **Output inicial (extracto):** La IA se confundió e intentó ejecutar la Tarea 3 (Árbol de Utilidad), armando una tabla jerárquica forzada con columnas de Riesgo y Valor para una sola oración descriptiva[cite: 4].
>
> ✅ **Solución implementada:** Introdujimos el `<input_classifier>`[cite: 4]. Le dimos reglas estrictas para que evalúe el input antes de responder: si detecta una oración breve -> va a la Tarea 2; si detecta una enumeración de casos -> va a la Tarea 3[cite: 4].
>
> ✅ **Resultado:** El asistente logró independencia total[cite: 4]. Con el mismo input de latencia, detectó que faltaba el "Artefacto", propuso un Gestor de Transacciones (*Straw man*) y construyó el escenario estructurado en las 6 partes del SEI sin requerir instrucciones extra por parte del usuario[cite: 4].

---

## 3. Restricciones y puntos de mejora

**① Limitación a una sola tarea por interacción.**
Debido a la arquitectura que le dimos al prompt, el asistente solo puede ejecutar *una* de las tres funciones a la vez[cite: 4]. Si el usuario envía un texto largo y pide simultáneamente que se completen los escenarios y se genere el árbol de utilidad, la IA priorizará solo una de las acciones[cite: 4]. El flujo de trabajo con esta skill debe ser iterativo por naturaleza[cite: 4].

**② Riesgo de saturación de contexto ("Lost in the middle").**
La efectividad y precisión del asistente dependen de mantener un archivo de teoría (`referencia_catedra.md`) muy conciso[cite: 4]. Si en un futuro se intentara escalar la herramienta adjuntándole capítulos enteros de libros o manuales extensos en PDF, el exceso de tokens provocaría que el LLM diluya su atención, olvidando las reglas del clasificador interno y rompiendo el formato de salida[cite: 4].

---

## 4. Prueba de Integración — Ejecución del Asistente

Como validación final, se corrió el pipeline de la skill contra los escenarios provistos por la cátedra para comprobar su autonomía. El objetivo era verificar si la herramienta lograba primero diagnosticar y completar escenarios ambiguos empleando el *Straw man* (Tarea 2), y luego consolidar dichos resultados en un Árbol de Utilidad formal (Tarea 3). A continuación se reproduce el output obtenido de las pruebas en Gemini.

### 4.1 Etapa 1 — Chequeo de Completitud y Generación de Escenarios (Tarea 2)

**Prompt utilizado:**
> podrias chequear la completitud de los escenarios?[cite: 2]

**Output obtenido:**
### Tarea 2: Chequear Completitud y Generar Escenarios[cite: 2]

He analizado el material suministrado y detectado los cinco escenarios planteados como "Ejercicios" (Performance, Seguridad, Disponibilidad, Usabilidad y Escalabilidad)[cite: 2]. Todos se encuentran incompletos, ya que omiten varias de las 6 partes del template de atributos de calidad del SEI o poseen medidas ambiguas[cite: 2]. A continuación, identifico las partes faltantes, aplico tácticas arquitectónicas y defino valores iniciales (*Straw man response measure*) para reescribirlos íntegramente[cite: 2].

#### Escenario 1: Performance[cite: 2]
**Texto original:** "Bajo condiciones normales de operación, el sistema debe procesar las transacciones de los usuarios con una latencia promedio de 2 segundos."[cite: 2]
**Diagnóstico:** Faltan definir explícitamente la Fuente del estímulo, el Estímulo, el Artefacto y la Respuesta[cite: 2].

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Usuario final / Cliente del sistema.[cite: 2] |
| **Estímulo** | Inicia un flujo de transacciones de negocio.[cite: 2] |
| **Artefacto** | Servidor principal / Módulo de procesamiento.[cite: 2] |
| **Ambiente** | Condiciones normales de operación.[cite: 2] |
| **Respuesta** | El sistema recibe, encola y procesa la solicitud asíncronamente optimizando los recursos (Táctica: Incrementar eficiencia de recursos / Concurrencia).[cite: 2] |
| **Medida de Respuesta** | El 100% de las transacciones se procesan con una latencia promedio de **2 segundos**.[cite: 2] |

#### Escenario 2: Seguridad[cite: 2]
**Texto original:** "El sistema debe mantener información de auditoría... En caso de un ataque, la imagen correcta de los datos... debe restaurarse en menos de 1 día."[cite: 2]
**Diagnóstico:** Faltan identificar la Fuente del estímulo, el Estímulo exacto y el Artefacto[cite: 2].

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Atacante externo o usuario interno malicioso.[cite: 2] |
| **Estímulo** | Modifica, corrompe o elimina información saltándose los controles convencionales.[cite: 2] |
| **Artefacto** | Base de datos principal / Repositorio.[cite: 2] |
| **Ambiente** | Tiempo de ejecución (Sistema bajo ataque).[cite: 2] |
| **Respuesta** | El sistema registra el evento de modificación en un log inmutable y provee un mecanismo seguro para que el administrador aplique un rollback (Táctica: Mantener registro de auditoría / Restaurar estado).[cite: 2] |
| **Medida de Respuesta** | La imagen original de los datos se restaura correctamente en menos de **1 día**, garantizando **0%** de pérdida de registros previos no comprometidos.[cite: 2] |

#### Escenario 3: Disponibilidad[cite: 2]
**Texto original:** "Si el controlador detecta una falla en el procesador principal durante la operación normal, pasará el control al procesador de backup."[cite: 2]
**Diagnóstico:** Falta definir la Fuente del estímulo y la Medida de respuesta es inexistente[cite: 2]. Se requiere aplicar *Straw man*[cite: 2].

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Falla interna de hardware o corrupción del sistema operativo.[cite: 2] |
| **Estímulo** | El procesador principal colapsa (crash) y deja de responder peticiones.[cite: 2] |
| **Artefacto** | Controlador / Procesador principal.[cite: 2] |
| **Ambiente** | Operación normal.[cite: 2] |
| **Respuesta** | El controlador detecta la caída mediante la ausencia de señal (*heartbeat*), aísla el componente y transfiere de inmediato el tráfico al procesador de respaldo (Táctica: Ping-Echo / Redundancia Activa).[cite: 2] |
| **Medida de Respuesta (Straw man)** | El failover automático se completa en un tiempo menor a **500 milisegundos** sin interrumpir las sesiones activas de los usuarios.[cite: 2] |

#### Escenario 4: Usabilidad[cite: 2]
**Texto original:** "Los usuarios deben poder minimizar el impacto de los errores cancelando la operación en curso, siendo el tiempo de cancelación menor a 1 segundo."[cite: 2]
**Diagnóstico:** Faltan identificar el Artefacto, el Ambiente y precisar detalladamente el Estímulo[cite: 2].

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Usuario interactuando con el sistema.[cite: 2] |
| **Estímulo** | Ejecuta accidentalmente una operación errónea y presiona la opción de "Cancelar".[cite: 2] |
| **Artefacto** | Interfaz de Usuario (UI) / Hilo de ejecución de la tarea.[cite: 2] |
| **Ambiente** | Tiempo de ejecución (Operación prolongada en curso).[cite: 2] |
| **Respuesta** | El sistema provee un control siempre visible para abortar, detiene el procesamiento en background y revierte el estado a su punto inicial seguro (Táctica: Cancelar comando).[cite: 2] |
| **Medida de Respuesta** | El tiempo transcurrido desde el clic hasta abortar la operación y liberar la interfaz es menor a **1 segundo**.[cite: 2] |

#### Escenario 5: Escalabilidad[cite: 2]
**Texto original:** "El sistema debe ser capaz de gestionar despliegues de hasta 1000 dispositivos... sin degradar sus tiempos de procesamiento"[cite: 2]
**Diagnóstico:** Falta la Respuesta arquitectónica formal y la Medida de respuesta es muy ambigua ("sin degradar")[cite: 2]. Se requiere aplicar *Straw man*[cite: 2].

| Parte del SEI | Descripción Refinada |
| :--- | :--- |
| **Fuente del Estímulo** | Crecimiento planificado de la infraestructura operativa.[cite: 2] |
| **Estímulo** | Se integran simultáneamente hasta 1000 nuevos dispositivos que generan volúmenes masivos de eventos.[cite: 2] |
| **Artefacto** | Módulo de ingesta de datos / Balanceador de carga.[cite: 2] |
| **Ambiente** | Tiempo de ejecución (Saturación de carga extendida).[cite: 2] |
| **Respuesta** | El sistema detecta el incremento de conexiones, aprovisiona dinámicamente recursos adicionales y distribuye la carga entre los nodos activos (Táctica: Escalado horizontal).[cite: 2] |
| **Medida de Respuesta (Straw man)** | El sistema procesa ininterrumpidamente **10.000 eventos por segundo**, manteniendo en el **99%** de los casos la latencia base menor a **2 segundos**.[cite: 2] |

### 4.2 Etapa 2 — Elaboración del Árbol de Utilidad (Tarea 3)

**Prompt utilizado:**
> en base a todo esto podrias armar el utility tree en formato markdown[cite: 3]

**Output obtenido:**

| Atributo de Calidad | Refinamiento | ASR (Resumen) | Prioridad (Valor, Riesgo) |
|---|---|---|---|
| Performance | Latencia de transacciones | Procesar transacciones de usuarios con latencia promedio de 2 segundos bajo operación normal. | (H, M) |[cite: 3]
| Seguridad | Auditoría y Recuperación | Mantener log de modificaciones y restaurar datos comprometidos por ataques en menos de 1 día. | (H, H) |[cite: 3]
| Disponibilidad | Tolerancia a fallos | Transferir el control al procesador de backup si el controlador detecta falla en el principal. | (H, H) |[cite: 3]
| Usabilidad | Cancelación de operaciones | Permitir a los usuarios cancelar una operación en curso en menos de 1 segundo para minimizar errores. | (M, L) |[cite: 3]
| Escalabilidad | Soporte de dispositivos | Gestionar despliegues masivos de hasta 1000 dispositivos sin degradar tiempos de procesamiento. | (H, M) |[cite: 3]

### 4.3 Conclusiones de la Integración
La herramienta demostró una excelente capacidad para aplicar la teoría arquitectónica de forma contextual. En la primera fase (Tarea 2), detectó con éxito la falta de métricas (Medidas de Respuesta) y tácticas formales en todos los casos provistos, inyectando justificaciones técnicas válidas (ej. táctica *Ping-Echo* para Disponibilidad o latencias de *10.000 eventos/s* para Escalabilidad) y forzando la toma de decisiones. En la segunda fase (Tarea 3), logró abstraer esos mismos requerimientos extendidos y mapearlos en la matriz de un Árbol de Utilidad, asignando prioridades lógicas de Negocio y Riesgo de forma coherente y lista para la revisión de stakeholders.