---
Name: 5 - Principios de diseño
tags:
  - teoría
asignatura: DS
---
***[[Diseño de Software]]***

***CALIDAD EN EL DISEÑO***
- **Código maloliente**: indicación superficial que corresponde a un problema más profundo del sistema. Ejemplos de código maloliente pueden ser código duplicado, clases grandes/perezosas/de datos, envidia de las características, intimidad inapropiada...
- **Diseño maloliente**: síntoma de un diseño pobre. Es similar a un código maloliente pero a un nivel más alto. Ejemplos pueden ser rigidez, fragilidad, inmovilidad, viscosidad, complejidad innecesaria, repetición innecesaria, opacidad...
- **Principios de diseño**: guía que, cuando aplicas conjuntamente, hace más sencillo al programador desarrollar software que es fácil de mantener y de extender.

***PRINCIPIOS SOLID***
**PRINCIPIO DE RESPONSABILIDAD ÚNICA**
Cada principio debe tener una responsabilidad única que esté enteramente encapsulada en la clase. Todos los servicios que provee el objeto están estrechamente alineados con dicha responsabilidad.
- Una responsabilidad es un factor de cambio, si esta cambia, debe cambiar el código que la implementa.
- Si una clase implementa una única responsabilidad, entonces la clase **tiene una sola razón para cambiar**.
- Si una clase implementa más de una responsabilidad, entonces las responsabilidades están acopladas, el diseño será frágil.
- **Cohesión**: medida en que las funciones que incluye un objeto están relacionadas funcionalmente entre sí. Mide la cantidad de trabajo que un objeto es capaz de hacer. Se busca siempre una alta cohesión en los objetos y evitar las clases *Dios*, que lo hacen todo en un programa, dejando pocas responsabilidades al resto de clases.

**PRINCIPIO ABIERTO-CERRADO**
Las entidades software deberían ser abiertas para permitir su extensión, pero cerradas frente a la modificación. Se quiere ser capaz de cambiar lo que hace una clase sin tener que tocar el código de la clase.
- **Clases selladas**: restringen qué otras clases pueden extenderlas.

**PRINCIPIO DE SUSTITUCIÓN DE LISKOV**
Las clases derivadas deben ser sustituibles por sus clases base. Aquellos métodos que utilicen referencias a clases base, deben ser capaces de usar objetos de clases derivadas sin saberlo, es decir, si la subclase no usa la sobreescritura, una instancia de la subclase y una instancia de la superclase no se comportarán igual.
- Una subclase siempre puede ser pasada allá donde se requiera una clase base, pero eso no garantiza que la subclase sea un *subtipo conductual* de la clase base.
- **Diseño por contrato**: declaran pre y postcondiciones que se deben cumplir.
- **Principio de subcontratación**: precondición sustituible por una más débil y una postcondición por una más severa.

**PRINCIPIO DE SEGREGACIÓN DE INTERFACES**
Muchos interfaces específicos para cada cliente son mejores que un único interfaz de propósito general.

**PRINCIPIO DE INVERSIÓN DE LA DEPENDENCIA**
Depende de abstracciones y no de concreciones. Consiste en depender de interfaces o clases y funciones abstractas en vez de depender de clases y funciones concretas. Los **diseños procedimentales** presentan un esquema de dependencias de bajo nivel, que a su vez dependen de otros módulos de más bajo nivel, etc.
- **Leaky abstractions**: por mucho que intentemos abstraerla, la realidad siempre se acaba filtrando a través de la abstracción.
- **Inyección de dependencias**: paso de una dependencia a un objeto dependiente que podría usarlo, sin permitir que el objeto dependiente construya o encuentre dicho servicio.


***TIPOS DE HERENCIA***
**TIPOS ADECUADOS**
- **Herencia de especialización**: las subclases sobrescriben a la superclase para ofrecer una versión más especializada de sus métodos pero respetando las especificaciones del padre en los aspectos relevantes.
- **Herencia de especificación**: la clase padre define una conducta pero no la implementa sino que es implementada en las subclases.
- **Herencia de extensión**: la subclase no sobrescribe el código de la superclase, sino que añade código nuevo.

**TIPOS INADECUADOS**
- **Herencia de construcción**: la clase hija usa el comportamiento definido en la superclase pero no cumple una relación ES_UN con la clase padre. Alternativa: favorecer la composición sobre la herencia.
- **Herencia de variación**: dos o más clases tienen implementaciones similares pero no poseen ninguna relación jerárquica entre ellas. Arbitrariamente, una ejerce de superclase del resto y el resto hereda el código común. Solución: crear superclase con características comunes.
- **Herencia de limitación**: el comportamiento de la subclase es menor o más restrictivo que el comportamiento de la superclase. 
- **Herencia de combinación**: una clase hereda más de una clase (herencia múltiple).


