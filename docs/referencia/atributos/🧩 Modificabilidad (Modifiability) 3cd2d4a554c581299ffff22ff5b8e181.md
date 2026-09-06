# 🧩 Modificabilidad (Modifiability)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 8 — Modifiability.
> 

# Escenario General de Modificabilidad

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | El agente que provoca el cambio. La mayoría son actores humanos, pero puede ser el propio sistema si aprende o se automodifica | Usuario final, desarrollador, administrador del sistema, dueño de la línea de producto, el propio sistema |
| Estímulo | El cambio que el sistema necesita acomodar (corregir un defecto también cuenta como cambio) | Directiva de agregar/borrar/modificar funcionalidad, o cambiar un atributo de calidad, capacidad, plataforma o tecnología · Directiva de agregar un nuevo producto a una línea de productos · Directiva de cambiar la ubicación de un servicio |
| Artefactos | Los artefactos que se modifican | Código, datos, interfaces, componentes, recursos, casos de prueba, configuraciones, documentación |
| Ambiente | El momento/etapa en que se hace el cambio | Runtime, tiempo de compilación, tiempo de build, tiempo de inicio, tiempo de diseño |
| Respuesta | Hacer el cambio e incorporarlo al sistema | Hacer la modificación · Testear la modificación · Desplegar la modificación · Automodificarse |
| Medida de Respuesta | Los recursos que se gastaron en hacer el cambio | Costo en términos de: número/tamaño/complejidad de artefactos afectados, esfuerzo, tiempo transcurrido, dinero · Grado en que la modificación afecta otras funciones/atributos de calidad · Nuevos defectos introducidos · Cuánto tardó el sistema en adaptarse |

**Ejemplo concreto (del libro):** un desarrollador quiere cambiar la interfaz de usuario; el cambio se hace en el código en tiempo de diseño, tarda menos de 3 horas en hacerse y testearse, y no genera efectos secundarios.

# "Sabores" específicos de Modificabilidad

- **Escalabilidad**: acomodar más carga. **Horizontal** (scaling out: sumar más nodos, en la nube = *elasticidad*) vs. **vertical** (scaling up: sumar más recursos a una unidad física).
- **Variabilidad**: soportar la producción de variantes preplanificadas de un sistema (clave en líneas de producto).
- **Portabilidad**: facilidad de mover el software a otra plataforma (minimizando y aislando dependencias de plataforma).
- **Independencia de ubicación**: dos partes distribuidas pueden interactuar sin conocer de antemano (o pudiendo cambiar) su ubicación física/lógica.

# Los 4 parámetros que determinan el costo de un cambio

**Acoplamiento** (coupling, cuanto más bajo mejor), **cohesión** (cuanto más alta mejor), **tamaño del módulo** (cuanto más chico mejor) y **momento de binding** (cuanto más tardío, mejor — pero cuesta más prepararlo).

# Tácticas para Modificabilidad

## Aumentar Cohesión

- **Split module**: dividir un módulo con responsabilidades no cohesivas en varios módulos más cohesivos.
- **Redistribute responsibilities**: agrupar responsabilidades similares que están dispersas en varios módulos.

## Reducir Acoplamiento

- **Encapsulate / Use an intermediary / Abstract common services**: las mismas tácticas que en Integrabilidad (Cap. 7) — reducir dependencias es el objetivo compartido.
- **Restrict dependencies**: limitar con qué módulos puede interactuar un módulo dado (visibilidad + autorización); típico en arquitecturas en capas.

## Diferir el Binding (bindear lo más tarde posible que sea rentable)

- **En compilación/build**: component replacement, compile-time parameterization, aspects.
- **En despliegue/inicio**: configuration-time binding, resource files.
- **En runtime**: discovery, interpret parameters, shared repositories, polymorphism.

# Patrones para Modificabilidad

- **Client-server**: bajo acoplamiento cliente–servidor; fácil de escalar; riesgo de latencia de red y de requerir seguridad extra en la comunicación.
- **Plug-in (microkernel)**: núcleo mínimo + plug-ins que agregan funcionalidad vía interfaces fijas; permite evolución independiente pero facilita vulnerabilidades si los plug-ins son de terceros.
- **Layers (capas)**: división en capas con relación de uso unidireccional; una capa solo usa las de más abajo. Beneficio: cambios en capas bajas no afectan a las de arriba (si la interfaz no cambia). Riesgo: overhead de performance y "layer bridging" mal usado rompe la modificabilidad.
- **Publish-subscribe**: comunicación asíncrona por eventos/tópicos, publicadores y suscriptores desacoplados. Riesgo: performance, determinismo y testeabilidad más difíciles de razonar.

# Cómo reconocer Modificabilidad en un escenario

Aparece cuando el estímulo es un **pedido de cambio** (agregar, borrar o modificar funcionalidad, plataforma o atributo) y lo que importa es el costo/tiempo/riesgo de hacer ese cambio sobre el sistema propio. Se diferencia de Integrabilidad en que acá el foco es el cambio en general (no específicamente sumar/combinar componentes externos) y de Desplegabilidad en que acá el foco es hacer el cambio, no llevarlo a producción.