---
Name: 2 - Arquitectura no distribuidas
tags:
  - teoría
asignatura: AS
---
***[[Arquitectura Software]]***

**ARQUITECTURA EN CAPAS**
****
Busca agrupar los distintos componentes del sistema alrededor de funcionalidades concretas. Tendremos distintos grupos y cada uno tendrá como finalidad la implementación de una responsabilidad dentro del sistema.

A cada uno de estos grupos se le llaman **capas**.

Todas las peticiones del sistema van a entrar por una de las capas, cada una de ellas solo se va a comunicar con la capa adyacente. Cada petición recorrerá todas las capas, de forma que todas las responsabilidades son necesarias para atender a la petición. Una vez recorra todas las capas, realizará el camino inverso para volver a salir y dar la respuesta por la capa inicial.

- **Ventajas**
	1. Permite añadir cambios fácilmente.
	2. Fácil de mantener.
	3. Las capas se pueden reutilizar para construir nuevos sistemas.
	4. Aumenta la **fiabilidad** al poder implementar mecanismos de seguridad específicos para cada capa.
	5. Favorece la **portabilidad** y el soporte **multi-plataforma**.

- **Inconvenientes**
	1. Necesitamos incrementar la **seguridad**.
	2. Limitación estructural en el **rendimiento**.

- **Representación modelo C4**
	Como es una arquitectura no distribuida, solo tenemos **un contenedor**.
	El cliente solo se comunica con una de las capas, la más exterior.
	![[Pasted image 20240523162611.png]]

- **Aplicable en**
	1. Cuando se construyen nuevos sistemas empleando sistemas ya existentes.
	2. Cuando se precisa dividir las responsabilidades entre varios equipos de desarrollo (uno por capa).
	3. Cuando se requieren varios niveles de seguridad (uno por capa).


**ARQUITECTURA PIPE&FILTER O PIPELINE**
****
Tiene una estructura muy parecida a la arquitectura en capas. En este caso cada capa se llama **filtro**, cada uno agrupado según las responsabilidades. Lo que se diferencia de la arquitectura en capas es que las peticiones entran por un filtro, recorre todos los filtros y sale por el último, evitando que tenga que hacer el camino de vuelta.

- **Ventajas**
	1. Mantiene las ventajas de la arquitectura por capas.
	2. No tiene limitación estructural.
	3. Por cada filtro solo pasa una vez la petición, por lo que dejará disponible al filtro para recibir más peticiones en menor tiempo.
	4. Se mejora el **rendimiento** y se introduce **paralelismo**.

- **Inconvenientes**
	- Al estar la entrada y salida en puntos diferentes, no podremos conseguir que el cliente conozca solo un punto de entrada. Esta es una de las razones por las que la arquitectura no se suele utilizar para sistemas interactivos.

- **Representación modelo C4**
	Destacar la gran diferencia que hay, a nivel de contexto, con la arquitectura en capas.
	![[Pasted image 20240523164154.png]]

- **Aplicable en**
	1. Sistemas asociados al procesamiento de información, donde es posible formalizar una serie de etapas en el procesado de cada petición para producir la salida deseada.


**ARQUITECTURA EN REPOSITORIO**
****
Propone que para los **intercambios de información** que habrá entre los componentes del sistema, existirá un componente/s que **centralicen** esa información.

Los componentes van a tener que comunicarse mucha información, con mucha frecuencia y comunicarla muchos a muchos. Estos componentes permanecerán independientes, sin conocerse, pero existe un conjunto central que recibe toda esta información.

Su objetivo principal es que los elementos del sistema se **desacoplen** lo máximo posible, conociendo solo al elemento centralizado, para que le puedan preguntar si hay información nueva.

- **Ventajas**
	1. El componente solo mandará 1 vez la información al elemento central y los demás podrán consultar dicha información con la temporalidad que precisen.
	2. El almacenamiento de información puede no ser persistente.

- **Inconvenientes**
	1. Se confía toda la comunicación del sistema al componente central.
	2. Si en algún momento falla, no se producirán comunicaciones.

- **Representación C4**
	![[Pasted image 20240523171417.png]]
	En el contenedor tenemos un cliente externo (gris). El resto de clientes van a poder especializarse, por ejemplo el de la izquierda va a enviar datos y leerlos. Pero hay uno que solo envía y otro que solo lee. Por ello, a veces se habla de productores y consumidores, en vez de clientes.
	- [i] Solo se comunican con el repositorio. Aquí el repositorio también se comunica con el almacenamiento cuando quiere guardar información.

- **Aplicable en**
	1. Sistemas en los que se precisa intercambiar una gran cantidad de información.
	2. Sistemas en los que la aparición o modificación de información en el repositorio funciona como disparador de acciones en los diferentes componentes.
	3. Casos en los que los componentes del sistema se especializan en solo producir o en solo consumir información.

**ARQUITECTURA CLIENTE-SERVIDOR**
****
También conocida como arquitectura orientada a servicios o basada en microservicios. Se basa en dividir las funcionalidades como **servicios** independientes que los clientes necesitarán en un momento.

Es un conjunto de componentes, cada uno con una **funcionalidad concreta**. Es importante que sean **independientes** entre ellas. 

También existe un intermediario (**directorio**) entre los servicios y los clientes, que busca el menor acoplamiento posible, aunque a veces está implementado en los clientes. Su trabajo es saber qué servicios están disponibles y, cuando un cliente necesite algún servicio, contactará primero con el directorio.

Si algún servicio es más pesado/accedido, podremos tener varias instancias del mismo.

- **Ventajas**
	1. Podemos desarrollar cada servicio independientemente.
	2. Mejor **rendimiento** (cambios independientes).
	3. Puede ser reutilizable.
	4. Podremos tener el servicio disponible sin tener todos los servicios desarrollados.
	5. Ganamos en **robustez** (servicios independientes entre sí).

- **Inconvenientes**
	1. El **directorio** es un punto único de fallo, todas las peticiones pasan por él.
	2. El repositorio intermediario limita el **rendimiento**.

- **Representación C4**
	Para el contenedor tenemos que recordar que estamos ante una arquitectura cliente-servidor NO distribuida, por lo que la *boundary* incluye elementos físicos y lógicos, ya que todos los componentes están desplegados en la misma máquina.
	![[Pasted image 20240523174203.png]]
	![[Pasted image 20240523174214.png]]

- **Aplicable en**
	1. Sistemas que se construyen de manera incremental, ya que no todos los servicios tienen que estar acabados para poder ofertar los que sí que están a los clientes.
	2. Cuando la heterogeneidad de los servicios a ofertar no encaja con otras arquitecturas.
