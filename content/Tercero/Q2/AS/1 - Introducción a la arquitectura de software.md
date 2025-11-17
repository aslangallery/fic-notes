---
Name: 1 - Introducción a la arquitectura de software
tags:
  - teoría
asignatura: AS
---
***[[Arquitectura Software]]***

**¿QUÉ ES LA ARQUITECTURA DE SOFTWARE**
****
Es la descomposición de un software en componentes o módulos y las interacciones entre ellos. Definir la arquitectura consiste en realizar la selección de elementos, como interactúan entre ellos y las restricciones que proporciona el contexto que satisface los requisitos no funcionales. Sirve como base para el diseño e implementación.

Para definirla se toman decisiones en función al impacto que tendrán en cuanto al coste de los cambios.

Los estilos de arquitectura determinan el uso de elementos y como se organizan. Sirven para comunicar a los desarrolladores, al igual que los patrones de diseño software.

**DISEÑO FRENTE A LA ARQUITECTURA**
****
El diseño:
- Viene de los requisitos funcionales.
- Se organiza mediante patrones de diseño.
- Pode utilizarse en diferentes arquitecturas.

La arquitectura:
- Viene de los requisitos no funcionales.
- Se organiza mediante estilos de arquitectura

**REPRESENTACIÓN**
****
Se suele representar la arquitectura de forma no funcional y heterogénea. El estándar, aunque no se suele seguir, es el IEEE 1471.

Las representaciones deben:
- Indicar qué es cada componente y qué hace.
- Mostrar las interacciones entre los componentes y la finalidad.
- Mantener coherencia entre componentes.
- Mantener coherencia entre iteraciones.

Se representa mediante diagramas. Antes se especificaban:
- Elementos de la arquitectura
    - Procesos: elementos que hacen cosas.
    - Datos: elementos que contienen información.
    - Elementos de conexión: llamadas, mensajes, contenidos, etc.
- Forma de la arquitectura
    - Propiedades: restricciones sobre los elementos.
    - Relaciones: restricciones sobre las interacciones.
- Justificación de la arquitectura
    - Motivación de las elecciones.
    - Evita derivaciones y errores.
    - Incluye aspectos económicos, de negocio, etc.

Se representa como una combinación de vistas:
- De datos o estática: flujo de datos.
- De proceso o dinámica: flujo de lógica.
- De despliegue o física: distribución física.

**MODELO C4**
****
Se representa la arquitectura como 4 vistas:
- Contexto: relación do sistema con otros.
- Contenedor: distribución física de componentes.
- Componente: relaciones entre componentes.
- Código: diagramas UML.

**¿QUÉ SON LOS REQUISITOS NO FUNCIONALES?**
****
Son las propiedades y restricciones del sistema, como la fiabilidad, tiempo de respuesta, almacenamiento... Pueden incluir restricciones de negocio o entorno. Afectan a todo el sistema.

Son igual de críticos que los requisitos funcionales. Si no se cumplen el software podría ser inútil. Pueden generar requisitos funcionales.
Son difíciles de concretar, pero si son imprecisos son muy difíciles de validar. Deben ser verificables.

Por ejemplo, no es lo mismo que un requisito no funcional sea "quiero que a mis usuarios les lleguen rápido los mensajes" que "quiero que a mis 100 usuarios les lleguen 20 mensajes en 5 segundos".

Tienen que ser consistentes, pero realistas. No existe algo perfecto que siempre pueda funcionar a la perfección y estar siempre disponible.

No tienen que ser compatibles entre sí. Por ejemplo, la seguridad y el rendimiento, Hacer algo más seguro implica un gasto de recursos y una ralentización del software.