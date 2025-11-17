---
Name: 5 - Enterprise Library
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**INTRODUCCIÓN**
****
>Conjunto de componentes de software reusables que resuelven problemas comunes.
>Código fuente que se puede descargar de forma gratuita para modificar, extender y utilizar como guía de arquitectura.

Enterprise Library está programada para .NET, pero no es parte de él.

**APPLICATION BLOCKS**
****
>Tipo de componente reusable. No son específicos a ningún tipo de aplicación o arquitectura.

***Tipos de application blocks***
1. *Catching Application Block*
	Mecanismos de caché para diferentes capas de aplicación.
2. *Cryptography Application Block*
	Hashing, cifrado simétrico.
3. *Data Access Application Block*
	Funcionalidad estándar de acceso a BBDD.
4. *Exception Handling Application Block*
	Definición de estrategias de procesado de excepciones.
5. *Logging Application Block*
	Funcionalidad de log.
6. *Policy Injection Application Block*
	Implementar políticas de intercepción que pueden usarse para agilizar la implementación de características comunes como el registro, el almacenamiento en caché, etc.
7. *Security Application Block*
	Autorización, caché de opciones de seguridad.
8. *Unity Application Block*
	Contenedor de DI extensible con soporte para inyección de constructor, propiedad y método, así como intercepción de tipo e instancia.
9. *Validation Application Block*
	Crear reglas de validación para los objetos de negocio.

**LOGGING APPLICATION BLOCK**
****
Permite generar eventos de log en diferentes destinos, sin necesidad de modificar el código. 

Posibles destinos:
- Visor de eventos.
- BBDD.
- Fichero XML.

Posibilita la configuración en tiempo de ejecución y filtrado de mensajes en base a la categoría.
>La configuración en tiempo de ejecución no optimiza recursos; permite modificar la configuración de registro sin reiniciar la aplicación. Facilita modificar la configuración del registro (niveles, oyentes, etc.) sin recompilar la aplicación. Esto permite adaptar el logging según las necesidades durante la ejecución, mejorando la flexibilidad y la capacidad de respuesta a problemas
