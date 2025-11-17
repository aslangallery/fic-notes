---
Name: 1 - Introducción al Desarrollo de Aplicaciones Empresariales
tags:
  - teoría
asignatura: ISD
---
***[[Internet y Sistemas Distribuidos]]***

**CARACTERÍSTICAS DE LAS APLICACIONES EMPRESARIALES**
****
>Una aplicación empresarial es aquella que aborda necesidades críticas.

Las aplicaciones hoy en día no funcionan de forma aislada y se tiene muy poco control sobre los servicios.

***Problemas***
- Necesita acceso a la red.
- Puede utilizar tecnologías diferentes a los servicios.
- Evolución entre aplicaciones es totalmente independiente.

***Requisitos***
- *Alta disponibilidad*: el usuario no debe percibir que el servicio no funciona.
- *Escalabilidad*: capacidad de soportar más usuarios o carga.
- *Transaccionalidad*: cumple con las propiedades ACID.
- *Seguridad*: los permisos se distribuyen según el tipo de usuario.

***Propiedades ACID***
1. *Atomicity*
	Capacidad de una transacción para ser tratada como una unidad completa e indivisible.
2. *Consistency*
	Capacidad de mantener la integridad de los datos y de la aplicación.
3. *Isolation*
	Capacidad de una operación para ejecutarse sin interferencias de otras operaciones concurrentes.
4. *Durability*
	Capacidad de que los resultados de una operación persistan, incluso en el caso de fallos del sistema.

**DISEÑO POR CAPAS**
****
>Una capa es un software que proporciona servicios a otro software a través de una interfaz o contrato de servicio.

La capa inferior es encargada de proporcionar el servicio y la superior de recibirlo.
Permite escalabilidad, tolerancia a fallos y facilita el mantenimiento debido a que sirve para independizar el software.

***Ventajas***
- Desarrolladores no necesitan conocer las tecnologías empleadas en otras capas.
- Se puede desarrollar software en paralelo.
- Facilita el mantenimiento, escalabilidad y tolerancia a fallos.

***Riesgos***
- Software complejo.
- Existen casos en los que el enfoque de las capas no es lo más óptimo.

**ARQUITECTURA BASADA EN CAPAS**
****
***Capa Modelo***
Implementa la lógica de negocio de todos los casos de uso de la aplicación de forma independiente de la interfaz gráfica. Se divide en dos subcapas:
- *Capa de acceso a datos*: accesos a BBDD y otras aplicaciones.
- *Capa lógica de negocio*: implementa los casos de uso de la capa modelo usando la capa de accesos a datos para leer y escribir en la BBDD.

***Capa Interfaz***
Se divide en dos:
- *Capa interfaz gráfica*: interfaz gráfica que permite a los usuarios utilizar las funcionalidades de la capa modelo.
- *Capa servicios*: interfaz orientada a que otros servicios utilicen las funcionalidades de la capa modelo a través de la subcapa de "acceso a servicios".

***Ejemplo: aplicación bancaria***
1. *Capa modelo*
	```
	saldoOrigen = LeerSaldo(idCuentaOrigen)
	si saldoOrigen > Importe {
		nuevoSaldoOrigen = sadoOrigen - importe
		EscribirSaldo(idCuentaOrigen, nuevoSaldoOrigen)
		saldoDestino = LeerSaldo(idCuentaDestino)
		nuevoSaldoDestino = saldoDestino + importe
		EscribirSaldo(idCuentaDestino, nuevoSaldoDestino)
	} sino lanzar error saldo insuficiente
	```
2. *Capa interfaz*
	Se encarga de comunicar la aplicación con otras fuentes, interactuar con los usuarios (capa interfaz gráfica) y ofrecer una API para que otras aplicaciones puedan interactuar con ella (capa servicios).

**DISTRIBUCIÓN DE LAS CAPAS**
****
1. ***Opción 1***
	![[Pasted image 20240916124415.png]]
	*Problemas*
	- Si se hacen cambios en la capa modelo, tenemos que reinstalar todo en las máquinas cliente.
	- No es seguro, se accede al código binario y a la BD desde el cliente.
	*Soluciones*
	-  Mover la capa modelo a un servidor intermedio, los clientes solo disponen de interfaz gráfica.

2. ***Opción 2***
	![[Pasted image 20240916124512.png]]
	Se añade una capa de acceso a servicios y una capa de servicios para poder comunicar el servidor. Se trata de una arquitectura en 3 capas (físicas). Aplicaciones standalone (aplicaciones de escritorio).
	
	*Ventajas*
	- Permiten utilizar parte de sus funcionalidades en local.
	*Problemas*
	- Al querer hacer un cambio en la interfaz gráfica tenemos que cambiarla en todos los clientes.
	*Soluciones*
	- Usar interfaces web. 

3. ***Opción 3***
	![[Pasted image 20240916124915.png]]
	Las aplicaciones web se instalan en un servidor de aplicaciones web.
	
	*Ventajas*
	- Los cambios que se hagan en la interfaz y en la capa modelo, solo requieren la reinstalación de la aplicación en el servidor de aplicaciones.
	- Crear clúster de máquinas con balanceador de carga (round robin).
	- Escalabilidad -> permite añadir más máquinas al clúster.
	- Disponibilidad -> si se cae una máquina del clúster, las otras siguen funcionando.

4. ***Opción 4***
	![[Pasted image 20240916125430.png]]
	Arquitectura en 4 capas.
	
	*Ventajas*
	- Capa de servicios se puede implementar utilizando tecnologías de desarrollo web dentro del servidor de aplicaciones web.
	- Permite que la interfaz y la capa modelo se implementen con tecnologías diferentes.
	- Permite que aplicaciones remotas utilicen la capa modelo.

5. ***Opción 5***
	![[Pasted image 20240916130653.png]]
	Distribución que utilizan las aplicaciones SPA (Single Page Application). Son aplicaciones web que funcionan en una página HTML inicial y utilizan JavaScript para cargar dinámicamente contenido y actualizar la interfaz de usuario en respuesta a las acciones del usuario, todo sin requerir carga completa de nuevas páginas desde el servidor.
	
	*Ventajas*
	- Los cambios en la interfaz solo requieren reinstalar la aplicación en el servidor web.
	- Los cambios en la capa modelo solo requieren reinstalar la aplicación en el servidor de aplicaciones.


**TECNOLOGÍAS JAVA**
****
- ***Java SE***: conjunto de especificaciones sobre cómo se debe comportar el compilador, la mv...
- ***Java ME***: conceptualmente como SE, pero para dispositivos con recursos limitados.
- ***Java/Jakarta EE***: versión enterprise de java, permite construir aplicaciones empresariales. 

**TECNOLOGÍAS POR CAPAS**
****
***Capa de acceso a datos***
**JDBC** (Java DataBase Conectivity) posibilita el acceso a bases de datos relacionales y permite lanzar consultas con Java.

***Capa lógica de negocio***
Sus funcionalidades son implementar mecanismos que simplifican la programación o que añaden seguridad y mecanismos de transacciones. Un ejemplo es **Spring**.

***Capa interfaz web***
Un ejemplo de API básica es **Servlets**.

***Capa servicios***
- *Servicios web REST*: se basa en principios y tecnologías que surgieron en torno a la web y sirve para diseñar aplicaciones web y servicios en línea.
- *Modelo RPC (Remote Procedure Call)*: generar automáticamente código que se encarga de abstraer al programador de los detalles de crear, modificar mensajes y enviarlos/recibirlos por la red. Un ejemplo es **Apache Thrift**.