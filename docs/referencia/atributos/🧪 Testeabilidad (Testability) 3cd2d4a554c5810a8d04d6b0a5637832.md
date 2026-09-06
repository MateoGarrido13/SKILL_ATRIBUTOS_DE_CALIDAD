# 🧪 Testeabilidad (Testability)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 12 — Testability.
> 

# Escenario General de Testeabilidad

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | Los casos de prueba pueden ejecutarlos una persona o una herramienta automatizada | Testers de unidad, de integración, de sistema, de aceptación, usuarios finales — corriendo tests manualmente o con herramientas automatizadas |
| Estímulo | Se inicia un test o conjunto de tests | Validar funciones del sistema · Validar atributos de calidad · Descubrir amenazas emergentes a la calidad |
| Ambiente | El testing ocurre en distintos eventos o hitos del ciclo de vida | El conjunto de tests se ejecuta por: la finalización de un incremento de código (clase, capa, servicio) · la integración completa de un subsistema · la implementación completa del sistema · el despliegue a producción · la entrega al cliente · un cronograma de testing |
| Artefactos | La porción del sistema que se testea y cualquier infraestructura de testing necesaria | Una unidad de código (módulo) · Componentes · Servicios · Subsistemas · El sistema entero · La infraestructura de testing |
| Respuesta | El sistema y su infraestructura de testing pueden controlarse para ejecutar los tests deseados, y los resultados pueden observarse | Ejecutar la suíte de tests y capturar resultados · Capturar la actividad que resultó en la falla · Controlar y monitorear el estado del sistema |
| Medida de Respuesta | Qué tan fácil el sistema bajo prueba "entrega" sus fallas o defectos | Esfuerzo para encontrar una falla o clase de fallas · Esfuerzo para alcanzar cierto % de cobertura del espacio de estados · Probabilidad de que el próximo test revele una falla · Tiempo para ejecutar los tests · Esfuerzo para detectar fallas · Tiempo para preparar la infraestructura de testing · Esfuerzo para llevar al sistema a un estado específico · Reducción de la exposición al riesgo: tamaño(pérdida) × probabilidad(pérdida) |

**Ejemplo concreto (del libro):** un desarrollador completa una unidad de código durante el desarrollo y ejecuta una secuencia de tests que da 85% de cobertura de caminos (path coverage) en 30 minutos.

# Tácticas para Testeabilidad

Dos categorías: **controlar y observar el estado del sistema** y **limitar la complejidad**.

## Controlar y Observar el Estado del Sistema

- **Specialized interfaces**: interfaces de testing dedicadas (get/set de variables clave, método que devuelve el estado completo, reset a un estado interno específico, activar logging/instrumentación verbose). Deben mantenerse separadas de las interfaces funcionales.
- **Record/playback**: registrar el estado que cruza una interfaz para poder "reproducir" el sistema y recrear una falla.
- **Localize state storage**: centralizar el estado en un solo lugar (ej. una máquina de estados) para poder arrancar el sistema en un estado arbitrario fácilmente.
- **Abstract data sources**: abstraer las fuentes de datos para poder sustituirlas fácilmente por datos de prueba (ej. apuntar a una base de test en vez de la real).
- **Sandbox**: aislar una instancia del sistema del mundo real para experimentar sin consecuencias permanentes; incluye virtualizar recursos como el reloj, la red o la batería (stubs, mocks, dependency injection).
- **Executable assertions**: aserciones codificadas (pre/post-condiciones, invariantes de clase) que señalan cuándo y dónde el programa entra en un estado defectuoso.

También se mencionan **component replacement** (cambiar la implementación por una versión testeable), **preprocessor macros** y **aspects** para inyectar reportes de estado.

## Limitar Complejidad

- **Limit structural complexity**: evitar dependencias cíclicas, aislar dependencias del entorno externo, reducir el acoplamiento en general (ej. limitar profundidad de herencia, cantidad de clases hijas, polimorfismo/llamadas dinámicas). Alta cohesión, bajo acoplamiento y separación de concerns (tácticas de Modificabilidad) también ayudan a la testeabilidad. Los patrones en capas facilitan testear capa por capa.
- **Limit nondeterminism**: reducir fuentes de comportamiento no determinista (ej. paralelismo no restringido), ya que un sistema no determinista es mucho más difícil de testear.

# Cómo reconocer Testeabilidad en un escenario

Aparece cuando el estímulo es la **ejecución de un test** (o la necesidad de descubrir/aislar una falla) y lo que importa es cuán fácil/barato es controlar el sistema hacia un estado específico y observar el resultado. Palabras clave: *cobertura, aserción, mock/stub, ambiente de testing, reproducir una falla*. No debe confundirse con Disponibilidad/Safety (que hablan de fallas en producción, no de encontrarlas antes).