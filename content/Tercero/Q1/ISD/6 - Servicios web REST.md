---
Name: 6 - Servicios web REST
tags:
  - teoría
asignatura: ISD
---
***[[Internet y Sistemas Distribuidos]]***

**DESCRIPCIÓN DEL CASO DE ESTUDIO**
****
***Diseño por capas***
1. *Interfaz de usuario*: lógica de la aplicación cliente. No depende de la tecnología de acceso a servicio.
2. *Acceso a servicio*: API que oculta la tecnología usada para acceder a los servicios.
3. *Servicios*: implementación de los servicios que delega en la capa modelo.
4. *Modelo*: lógica de la aplicación. No depende de la tecnología de implementación de los servicios.

***Capa servicios***
Utiliza objetos alternativos ya que no necesita algunas cosas de la BD. Para ello se crea `ServiceMovieDto` y `ServiceSaleDto`.

>Un DTO (Data Transfer Object) es un objeto que se usa para transferir datos entre aplicaciones. Sirve para encapsular las diferencias con los objetos internos.

***Capa de acceso a servicios***
- `ClientMovieService`: fachada con una interfaz para cada operación del servicio.
- `ClientMovieServiceFactory`: factoría que permite construir una instancia de `ClientMovieService` sin que el llamador necesite conocer la clase de la interfaz.
- [i] Sirven para que el cliente acceda a distintas implementaciones del servicio sin tener que recompilarlo.

***Capa interfaz de usuario***
En `MovieServiceClient` definimos la interfaz por terminal con la que va a interactuar el usuario.
```
#Añadir una película:
MovieServiceClient -a <title> <hours> <minutes> <description> <price>

# Borrar una película:
MovieServiceClient -r <movieId>

# Actualizar una película:
MovieServiceClient -u <movieId> <title> <hours> <minutes> <description> <price>

# Buscar una película por palabras clave en el título:
MovieServiceClient -f <keywords>  

# Comprar una película:
MovieServiceClient -b <movieId> <userId> <creditCardNumber>

# Ver una película:
MovieServiceClient -g <saleId> 
```

**INTRODUCCIÓN A LOS SERVICIOS WEB REST**
****
>REST (REpresentational State Transfer) es un estilo arquitectónico que sirve para construir aplicaciones distribuidas basado en las características de la Web.

Los servicios REST que siguen fielmente todos los requisitos se llaman RESTful.

Es independiente de la tecnología pero se suele implementar usando HTTP y JSON/XML.

***HTTP***
>HTTP (HyperText Transfer Protocol) es el protocolo cliente-servidor utilizado en Web. Se utiliza para transferir todo tipo de contenido, desde páginas hasta imágenes. Sigue un esquema petición/respuesta.

1. *Peticiones HTTP*
	Utiliza las URL como identificadores globales de recursos. Una petición HTTP está formada por:
	- *URL*: identifica al recurso sobre el que se actúa.
	- *Método de acceso*: `GET`, `POST`, `PUT`, etc. Especifican qué acción se realiza.
	- *Cabeceras*: metainformación de la petición.
	- *Cuerpo*: algunos métodos tienen un mensaje con cuerpo.
2. *Métodos de acceso*
	- `GET`: solicita una representación del recurso solicitado. Es seguro y de solo lectura. No hay que esperar cambios en el servidor ni produce cambios. Se pueden realizar consultas utilizando el `&` para separar los pares campo=valor.
	- `PUT`: carga un recurso en el servidor (no tenía por qué existir previamente). No es seguro.
	- `DELETE`: elimina un recurso. Tampoco es seguro.
	- `POST`: envía datos a un recurso para que los procese. Tampoco es seguro.
	
	- [i] La principal diferencia entre `PUT` y `POST` es que `PUT` es idempotente (el sistema no se ve afectado por la repetición de la misma solicitud y el estado es el mismo que después de una sola solicitud). Dos peticiones `POST` idénticas de creación de usuario crearán dos usuarios.
3. *Respuestas HTTP*
	Las respuestas HTTP contienen:
	- *Códigos de error*: `200 OK`, `201 CREATED`, `400 BAD REQUEST`, `403 FORBIDDEN`, `404 NOT FOUND`, `500 INTERNAL ERROR`, etc.
	- *Cabeceras*: metainformación de la respuesta.
	- *Cuerpo del mensaje*: a veces viene vacío.

***SERVICIOS REST***
Tienen una estructura muy similar a la web. Lo que los diferencia es que se accede a información estructurada en lugar de a páginas HTML. Los servicios REST se consideran autónomos entre sí y pueden referenciarse mediante links.

1. *Servicios stateless*
	Los clientes invocan URLs para acceder a los recursos. El servidor no guarda información de estado para cada cliente. HTTP es un servicio sin estado. Cada petición de un cliente debe contener toda la información necesaria para que el servidor la responda.
	
	Las ventajas e inconvenientes de estos servicios son:
	- [p] Facilidad para replicar el servicio en múltiples máquinas mediante un balanceador de carga.
	- [p] El servidor no necesita reservar recursos para cada sesión.
	- [p] Mejora la escalabilidad.
	- [c] A veces es necesario enviar información adicional en las peticiones.
2. *Recursos y representaciones*
	Las aplicaciones REST están compuestas por recursos (entidades persistentes), Pueden ser colecciones o individuales. Cada recurso tiene asociado un ID único y global (URL).
	
	Al invocar la URL mediante `GET`, se obtiene una representación del recurso. Los recursos de tipo colección suelen devolver una lista de los elementos con información resumida y enlaces a la información completa. Los recursos individuales suelen devolver datos del elemento.
3. *Interfaces uniformes*
	Los servicios REST tienen una serie de operaciones que siempre deben ser las mismas y funcionan igual en todos los servicios. 
	 
	Se utiliza HTTP:
	- `GET`: acceso a representaciones.
	- `PUT`: reemplaza la representación o crea recursos individuales.
	- `DELETE`: borra un recurso.
	- `POST`: crea un recurso en una colección. También se usa para otras operaciones.
	
	Los códigos de respuesta también son los de HTTP:
	- *Permanentes*: 400, 403, 410...
	- *Temporales*: 404, 500...

- [n] El uso de interfaces uniformes permite que haya intermediarios entre el cliente y el servicio. Por ejemplo, los servidores caché internos, que sirven una copia del recurso solicitado si la tienen y, si no, invocan al sitio web/servicio real.     

**DISEÑO E IMPLEMENTACIÓN DE UN SERVICIO WEB REST**
****
***Protocolo REST***
1. *Recursos*
	- `/movies` es un recurso colección:
		- `POST` añade una nueva película.
		- `GET` lista todas las películas.
	- `/movies/id` es un recurso individual:
		- `PUT` modifica la película.
		- `DELETE` borra la película.
	- `/sales` es un recurso colección:
		- `POST` añade una nueva venta.
	- `/sales/id` es un recurso individual:
		- `GET` obtiene información de la venta.
2. *Errores*
	- `400 BAD REQUEST` ⭢ `InputValidationException`
	- `404 NOT FOUND` ⭢ `InstanceNotFoundException`
	- `410 GONE` ⭢ `SaleExpirationException`
	- `403 FORBIDDEN` ⭢ `MovieNotRemovableException`
	- `500 INTERNAL ERROR`

***Overloaded Post***
Se utiliza cuando se manejan operaciones que no encajan con las operaciones básicas CRUD. Por ejemplo, podemos marcar una película como destacada así:
```java
POST http://XXX/ws-movies-service/movies/{id}/highlight
```

***Aplicaciones web Jakarta EE***
En Jakarta EE las aplicaciones web se instalan en servidores de aplicaciones. Cualquier aplicación Jakarta EE puede instalarse en un servidor y se distribuyen en `.war`.

1. *Servlets*
	Clase asociada a una o varias URLs. Extiende `HttpServlet` y redefine los métodos `doXXX`. Cuando el server recibe una petición, sobre ella invoca el servlet. Este se encarga de:
	- Acceder al contenido de la petición.
	- Ejecutar el código que le permite obtener la respuesta.
	- Codifica la respuesta en el formato deseado.
	
	Es un modelo multithread ⭢ cada vez que el servidor de aplicaciones recibe una petición dirigida a un servlet, la petición se procesa en un thread independiente.
	
	Clase `ServletRequest`:
	- `getParameter`: permite obtener el valor de un parámetro univaluado.
	- `getParameterValues`: permite obtener el valor de un parámetro multivaluado.
	- `getInputStream`: permite leer el cuerpo de la petición.
	
	Clase `HttpServerRequest`:
	- `getPathInfo`: fragmento del path de la petición a partir de la URL asociada al Servlet (siempre empieza por "/"). Por ejemplo, `/movies/123` ⭢ `/123`.
	- `getRequestURL`: devuelve la URL completa que utilizó el cliente para realizar la petición sin incluir parámetros.
	
	Clase `ServletResponse`:
	- `setContentType`: especifica el contenido de la respuesta.
	- `getOutputStream`: permite escribir el cuerpo de la respuesta.
	
	Clase `HttpServletResponse`:
	- `setStatus`: especifica el código de estado de la respuesta.
	- `setHeader`: añade una cabecera con el nombre y el valor especificados a la respuesta.
	
	Clase `ServletUtils`:
	- `writeServiceResponse`: escribe una respuesta HTTP.
	- `normalizePath`: recibe un String y devuelve otro String igual al recibido quitándole el caracter "/" en caso de que finalice con él.
	- `getMandatoryParameter` o `getMandatoryParameterAsLong`: obtiene el valor del parámetro o lanza `InputValidationException` si no viene en la petición.
	- `checkEmptyPath`: comprueba si el path de una petición HTTP es vacío, nulo o está compuesto solo por "/", en caso negativo lanza `InputValidationExcp`.
	- `getIdFromPath`: obtiene un identificador de tipo Long a partir del path de una petición HTTP que siga el formato "/". En caso de no poder obtenerlo, lanza `InputValidationException`.
2. *Empaquetamiento de una aplicación web*
	```java
	jar cvf aplicacionWeb.war directorio
	```

***HttpClient***
>Framework que se usa en la capa de acceso a servicios. Dentro de este usaremos la Fluent API.

La clase Request representa una petición HTTP:
- `bodyStream`: permite asignar un contenido cualquiera al cuerpo de la petición.
- `execute`: envía la petición y devuelve un objeto Response que representa la respuesta a la petición.

***Validación de documentos***
Los clientes y el servicio comprueban que el JSON está bien formado con Jackson, pero no comprueban que el documento es válido.