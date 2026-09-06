# 🔌 Integrabilidad (Integrability)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 7 — Integrability.
> 

# Escenario General de Integrabilidad

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | De dónde viene el estímulo | Stakeholder de la misión/sistema · Marketplace de componentes · Proveedor del componente |
| Estímulo | Qué tipo de integración se describe | Agregar un componente nuevo · Integrar una nueva versión de un componente existente · Integrar componentes existentes de una forma nueva |
| Artefacto | Qué partes del sistema están involucradas | Sistema entero · Conjunto específico de componentes · Metadata de un componente · Configuración de un componente |
| Ambiente | En qué estado está el sistema cuando ocurre el estímulo | Desarrollo · Integración · Despliegue · Runtime |
| Respuesta | Cómo responde un sistema "integrable" al estímulo | Los cambios se completan/integran/testean/despliegan · Los componentes en la nueva configuración intercambian información correctamente (sintáctica y semánticamente) · Colaboran exitosamente · No violan límites de recursos |
| Medida de Respuesta | Cómo se mide la respuesta | Costo en términos de: número de componentes cambiados, % de código cambiado, líneas de código cambiadas, esfuerzo, dinero, tiempo calendario · Efectos sobre las medidas de respuesta de otros atributos de calidad (tradeoffs permitidos) |

**Ejemplo concreto (del libro):** un nuevo componente de filtrado de datos aparece en el marketplace de componentes; se integra y despliega en 1 mes, con no más de 1 persona-mes de esfuerzo.

# Concepto clave: "distancia" entre componentes

La dificultad de integración depende del **tamaño** (cantidad de dependencias potenciales) y la **distancia** (dificultad de resolver diferencias en cada dependencia) entre los elementos {Ci} y el sistema S. Tipos de distancia:

- **Sintáctica**: tipo/número de datos compartidos no coincide (ej. entero vs. flotante).
- **Semántica de datos**: mismo tipo de dato, pero interpretado distinto (ej. metros vs. pies).
- **Semántica de comportamiento**: desacuerdo sobre estados/modos del sistema (ej. quién inicia una interacción).
- **Temporal**: supuestos distintos sobre tiempos/tasas (ej. 10 Hz vs. 60 Hz).
- **De recursos**: supuestos distintos sobre recursos compartidos (memoria, ancho de banda, acceso exclusivo vs. compartido).

# Tácticas para Integrabilidad

Se agrupan en **limitar dependencias**, **adaptar** y **coordinar**.

## Limitar Dependencias

- **Encapsulate**: introducir una interfaz explícita y forzar que todo el acceso pase por ella (base de las demás tácticas de esta categoría).
- **Use an intermediary**: romper dependencias directas (ej. bus publish-subscribe, repositorio de datos compartido, discovery dinámico).
- **Restrict communication paths**: limitar con quién puede comunicarse un elemento (visibilidad + autorización); típico en SOA con un enterprise service bus.
- **Adhere to standards**: adoptar estándares (IEEE, ISO, OMG) o convenciones locales para reducir dependencias y distancia.
- **Abstract common services**: ocultar servicios similares detrás de una abstracción común, para que futuros componentes se integren con una sola interfaz.

## Adaptar

- **Discover**: catálogo de direcciones/servicios (discovery service) para localizar dinámicamente a quién integrar.
- **Tailor interface**: agregar u ocultar capacidades de una interfaz existente sin cambiar su API (ej. filtros que validan datos o traducen formatos).
- **Configure behavior**: permitir configurar el comportamiento de un componente en build, inicialización o runtime (ej. soportar distintas versiones de un estándar).

## Coordinar

- **Orchestrate**: mecanismo de control que coordina la invocación de servicios para que permanezcan ajenos entre sí (workflows, BPEL).
- **Manage resources**: un gestor de recursos intermedia el acceso a recursos computacionales compartidos (similar a restrict communication paths pero para recursos).

# Patrones para Integrabilidad

- **Wrappers / Bridges / Mediators**: las tres giran en torno a tailor interface. Wrapper: encapsula un componente y traduce su interfaz. Bridge: traduce entre "requires" de un componente y "provides" de otro, independiente de componentes específicos, definido en tiempo de construcción. Mediator: como el bridge pero con planificación en runtime, con mayor autonomía y rol de primera clase en la arquitectura.
- **Service-Oriented Architecture (SOA)**: componentes distribuidos que proveen/consumen servicios estándar (WSDL, SOAP), típicamente entre organizaciones distintas (vs. microservicios, que son de una sola organización).
- **Dynamic discovery**: aplica la táctica discover en runtime, permitiendo bindear consumidor y servicio concreto dinámicamente.

# Cómo reconocer Integrabilidad en un escenario

Aparece cuando el estímulo es la **necesidad de sumar o combinar componentes/sistemas** (nuevos o existentes) y lo que importa es el costo/riesgo de esa integración (no el cambio en sí de una funcionalidad, que sería Modificabilidad). Muy asociada a integraciones con **terceros** y a la palabra "distancia" sintáctica/semántica/temporal/de recursos.