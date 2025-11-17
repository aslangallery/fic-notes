---
Name: 1 - Introducción a .NET
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**DEFINICIÓN DE FRAMEWORK**
****
Plataforma que facilita el desarrollo de una forma determinada, que indica al programador cómo tiene que desarrollar.
Se diferencian de las librerías en que estas no obligan a trabajar de una forma concreta, sino que pueden ser utilizadas en cualquier lugar.

**FRAMEWORK .NET**
****
.NET es una plataforma de desarrollo y ejecución que se compone de:
- Un entorno de ejecución.
- Librerías.
- Lenguajes de programación.
- Compiladores.
- Herramientas de desarrollo.
- Documentación y guías de arquitectura.

**PLATAFORMA INDEPENDIENTE DEL LENGUAJE**
****
Permite programar en múltiples lenguajes, transformando el código fuente a Intermediate Language (IL). No es un lenguaje interpretado, hay que compilarlo antes de que pueda ser ejecutado.

El IL utiliza tipos de datos comunes soportados por todos los lenguajes que soporta el framework .NET. Se conoce como el Common Type System (CTS). Gracias a esto, utilizar un lenguaje u otro no condiciona la potencia. Todos los lenguajes tienen la misma capacidad de acceso a recursos a servicios.

**PLATAFORMA DE EJECUCIÓN INMEDIATA**
****
El Common Language Runtime (CLR) se ocupa de gestionar la ejecución de las aplicaciones .NET. Compila el código en el momento exacto en el que es necesario para ejecutar el programa. Esto se conoce como compilación Just-In-Time (JIT). La utiliza para pasar IL a lenguaje máquina.

No utiliza una máquina virtual para ejecutar las aplicaciones. La compilación genera un fichero ejecutable en formato Portable Executable (PE). Aunque se cambie el compilador, el código debería funcionar igualmente.

**CÓDIGO GESTIONADO**
****
Las aplicaciones .NET se consideran aplicaciones gestionadas, ya que no se ejecutan directamente sobre el SO, sino en el entorno del CLR. Se encarga del recolector de basura, la carga y verificación del código, etc.

***Metadata***
Se produce en tiempo de compilación y describe tipos, propiedades, firmas de métodos y operaciones. Esto permite que los componentes de .NET sean autodescriptivos y no se necesite información adicional para interactuar entre componentes escritos en otros lenguajes.

**OTRAS CARACTERÍSTICAS**
****
1. Orientación a objetos.
2. Desarrollo de aplicaciones empresariales.
3. Modelo de programación único para web, hardware, mobile, etc.
4. Tiene peor documentación que Java.

**¿CÓMO ESTÁ FORMADO .NET?**
****
- *.NET framework Redistributable Package*: contiene el CRL y librerías.
- *.NET framework SDK*: contiene a mayores compiladores, depuradores, etc.
- *.NET Compact Framework*: contiene el CLR y librerías, pero mucho más livianas.

**COMMON LANGUAGE RUNTIME**
****
El CLR, además de ejecutar aplicaciones, tiene las siguientes funciones:
- Recolecta basura.
- Aísla unas aplicaciones de otras.
- Aplica políticas de seguridad.
- Proporciona servicios de debug.
- Incluye control de versiones.
- Desacopla la aplicación del SO.

**CLASS LIBRARY**
****
.NET ofrece una serie de clases e interfaces organizados en namespaces de forma jerárquica. Un ejemplo sería `System.Collections`.

Los tipos son independientes del lenguaje que se esté usando. Todas las librerías están disponibles para todos los lenguajes. Se pueden extender librerías, ya que están totalmente orientadas a objetos y se dividen en 2 subgrupos.

***Base Class Library***
Pequeño conjunto de librerías que incluye funcionalidades básicas. 
Ej.: manejo de strings, acceso a BBDD, colecciones, etc.

***Framework Class Library***
Colección mucho más grande que incluye la BCL y funcionalidades mucho más específicas. Contiene LINQ, ADO.NET, ASP.NET y muchas más.

***Enterprise Library***
Librería a parte de las Class Library que contiene funcionalidades comunes en aplicaciones empresariales.
Ej.: manejo de excepciones, sistemas de caché, seguridad, etc.

**COMMON TYPE SYSTEM**
****
>Conjunto de reglas que tienen que seguir las definiciones de datos para que el CLR las acepte.

Define datos OO. La filosofía de .NET es que todo es un objeto y hereda de `System.Object`. 

**COMMON LANGUAGE SPECIFICATION**
****
>Conjunto mínimo de características que todos los lenguajes deben soportar para ajustarse al CLR.

Permite que todos los componentes que sigan esta especificación pueda interactuar entre sí independientemente del lenguaje en el que fueran escritos. Solo define los tipos y métodos visibles externamente para que estos sean accesibles desde cualquier lenguaje de programación.

**ASSEMBLIES**
****
>Unidad mínima de ejecución, instalación, distribución y versionado de .NET

Son ficheros con extensiones `.exe` o `.dll`.
Están formados por código IL y por el manifest (metadata del conjunto de archivos que conforman el assembly). Uno o varios assemblies forman una aplicación .NET.

***Tipos de assemblies***
- *Privados*: solo pueden ser usados por una aplicación.
- *Compartidos*: pueden ser usados por varias aplicaciones. Se instalan en la Global Assembly Cache.

**CLR HOSTING Y APPDOMAINS**
****
Para ejecutar una aplicación .NET es necesario un Runtime Host. Es un fragmento de código que carga el CLR en un proceso, carga y ejecuta la aplicación en los AppDomains.

***Definición de AppDomain***
> Proceso virtual que corre dentro del CLR, que funciona de forma similar a un proceso del SO, pero con la característica de que carios AppDomain pueden correr en el mismo proceso.

Proporcionan aislamiento entre aplicaciones, lo que permite:
- Detener una sola aplicación sin afectar al resto.
- Aislar las aplicaciones del código de las otras.
- Evitar que un fallo afecte al resto.

**ADO.NET**
****
>Mapeador objeto-relacional. Proporciona soporte para la mayoría de gestores de BBDD relacionales. Está formado por dos entornos.

***Entorno conectado***
Los usuarios están siempre directamente conectados a la fuente de datos. Esto permite un mejor control de la concurrencia y datos constantemente actualizados, pero a cambio consume muchos recursos y limita la escalabilidad. Los recursos se mantienen en el servidor hasta cerrar la conexión.

***Entorno desconectado***
Se copian datos o tablas en local, se trabaja con la copia y después se sincroniza con la fuente de datos. Es muy cómodo si los datos solo necesitan lecturas, pero al no estar sincronizados no siempre se puede usar.

**ASP.NET**
****
>Versión de .NET para trabajar con ASPs (Active Server Pages).

Se compone por:
- Formularios web.
- Controles.
- Servicios web.
