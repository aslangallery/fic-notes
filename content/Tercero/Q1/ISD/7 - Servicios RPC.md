---
Name: 7 - Servicios RPC
tags:
  - teoría
asignatura: ISD
---
***[[Internet y Sistemas Distribuidos]]***

**MODELO RPC**
****
>Modelo donde un proceso expone su funcionalidad como una serie de operaciones que pueden ser invocados desde cualquier aplicación en cualquier punto de la red. Se basa en la generación automática de código de creación y parseo.

***Funcionamiento***
El programador del server modela un conjunto de operaciones que pueden ser invocadas por los clientes. Se definen en un fichero que contiene:
- Nombre de la operación.
- Nombre y tipo de los parámetros de entrada.
- Nombre y tipo de los parámetros de salida.

Después ejecuta un compilador especial de framework y genera el esqueleto. Este contiene:
- Código fuente plantilla.
- Librerías que se ocupan de la comunicación con los clientes.

El programador del cliente compila el fichero de definición y genera un stub, que ofrece al programador las operaciones del fichero. Cada operación del stub se encarga de invocar la operación del servicio y obtener la respuesta. 

***Flujo de una aplicación remota***
1. App cliente invoca una operación stub con parámetros necesarios.
2. Stub genera un mensaje encapsulado con el nombre de la operación y los parámetros de entrada recibidos, envía el mensaje al server y espera respuesta.
3. El esqueleto comprueba qué operación se desea invocar, extrae los parámetros e invoca la operación correspondiente del código fuente plantilla o la implementación de la interfaz.
4. La implementación convierte los parámetros a los tipos adecuados.
5. La operación de la capa modelo se ejecuta y devuelve un resultado.
6. La implementación convierte el resultado de la capa modelo a los tipos con los que trabajan esas operaciones y lo devuelve.
7. El esqueleto genera un mensaje encapsulando el resultado recibido y envía el mensaje al cliente.
8. El stub extrae de la respuesta el resultado y se lo devuelve al cliente.
9. El cliente recibe la respuesta y continúa su ejecución.

**APACHE THRIFT**
****
>Framework que sigue el modelo RPC. Permite diseñar y construir servicios y clientes remotos interoperables entre diferentes lenguajes y plataformas. Soporta modelo síncrono y asíncrono. 

Cada capa utiliza servicios que le proporcionan las capas inferiores, que pueden estar implementados de diferentes maneras.
- *Librerías de transporte*: envía peticiones/respuestas por la red. Todas las implementaciones heredan de `TTransport`. Es extensible.
- *Librería de protocolos de serialización*: serialización y deseralización de la información que se envía en cada petición y respuesta.
- *Esqueleto y stub*
- *Código del cliente y servidor*
- *Librería de servidores*: proporciona clases que permiten implementar diferentes tipos de servidores según las necesidades concretas de la aplicación y las capacidades de cada lenguaje.

***Factorías***
Los servers Apache Thrift crean un nuevo objeto `TProtocol` para cada conexión de un cliente aceptada. Para ello se utiliza una implementación de `TProtocolFactory`.

***IDL***
>Interface Definition Language es un lenguaje que permite definir archivos `.thrift` donde se añaden servicios, operaciones y los tipos que usan estas. Todos los elementos deben tener nombre.

1. *Tipos*

| Palabra clave | Descripción                                                    | Tipo Java           |
| ------------- | -------------------------------------------------------------- | ------------------- |
| binary        | Array de bytes                                                 | java.nio.ByteBuffer |
| bool          | Booleano                                                       | boolean             |
| double        | Punto flotante de doble precisión                              | double              |
| i8(byte)      | Entero de 8 bits                                               | byte                |
| i16           | Entero de 16 bits                                              | short               |
| i32           | Entero de 32 bits                                              | int                 |
| i64           | Entero de 64 bits                                              | long                |
| string        | Cadena de caracteres                                           | java.lang.String    |
| void          | Para indicar que una operación de un servicio no devuelva nada | void                |
| list<>        | Lista ordenada de cero o más elementos                         | List<>              |
2. *Structs*
	Permite crear tipos definidos por el usuario que pueden usarse en los mismos lugares que los tipos base y contenedores. El compilador genera un tipo por cada struct.
	```java
	struct ThriftMovieDto {  
	    1: i64 movieId  
	    2: string title  
	    3: i16 runtime  
	    4: string description  
	    5: double price  
	}
	```
3. *Excepciones*
	Adopta el modelo de excepciones como su modelo abstracto para gestionar errores. Se definen igual que los struct pero usando la palabra reservada exception.
	```java
	exception ThriftInputValidationException {  
	    1: string message  
	}
	```
4. *Servicios*
	Define un conjunto de funciones relacionadas. Se declaran con la palabra reservada service.
	```java
	service ThriftMovieService {  
	   ThriftMovieDto addMovie(1: ThriftMovieDto movieDto) 
		   throws (1: ThriftInputValidationException e)  
	  
	   void updateMovie(1: ThriftMovieDto movieDto) 
		   throws (
			   1: ThriftInputValidationException e, 
			   2: ThriftInstanceNotFoundException ee
			)  
	  
	   void removeMovie(1: i64 movieId) 
		   throws (
			   1: ThriftInstanceNotFoundException e, 
			   2: ThriftMovieNotRemovableException ee
			)  
	  
	   list<ThriftMovieDto> findMovies(1: string keywords)  
	  
	   ThriftSaleDto buyMovie(1: i64 movieId, 2: string userId,
						    3: string creditCardNumber) 
			throws (
				1: ThriftInputValidationException e, 
				2: ThriftInstanceNotFoundException ee
			)  
	  
	   ThriftSaleDto findSale(1: i64 saleId) 
			throws (
				1: ThriftInstanceNotFoundException e, 
				2: ThriftSaleExpirationException ee
			)  
	}
	```

**OTRAS TECNOLOGÍAS Y COMPARACIÓN**
****
***SOAP***
>Protocolo que permite invocar servicios remotos intercambiando mensajes en formato XML.

Los mensajes se envían encapsulados en otros protocolos de nivel de aplicación.
Utiliza WSDL como lenguaje de definición de la interfaz del servicio. Permite especificar en XML la interfaz de un servicio.

***gRPC***
>Permite invocar servicios remotos usando un protocolo binario sobre HTTP/2 para intercambio de mensajes.

Utiliza Protocol Buffers como lenguaje de definición de la interfaz del servicio.
