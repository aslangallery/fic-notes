---
Name: 12 - Gestión de estado
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**LADO CLIENTE**
****
1. ***Parámetros incluidos en la URL (`<a href="...">`)***
	- Query string
	- Visibilidad elevada.
2. ***Elementos de formularios ocultos (`input type="hidden"`)***
	-  ViewState almacena el estado de los controles entre una petición y la siguiente. 
	- Todos los controles tiene la propiedad `EnableViewState` activa por defecto.
	- Es posible añadir datos propios al ViewState y se materializa como un campo oculto en el HTML de salida.
3. ***Cookies***
	- Almacena datos en el navegador del cliente. 
	- Tamaño máximo 4kb
	- N.º alrededor de 300
	- Cookies por sitio web alrededor de 20.
	- Usuario puede bloquearlas.
	- Propiedades:
		1. *Domain*: servidor del que se descargó la cookie.
		2. *Expires*: fecha en la que el navegador borrará la cookie.
		3. *Name*: nombre de la cookie.
		4. *Value*: contenido de la cookie.

**LADO SERVIDOR**
****
1. ***Variables de aplicación***
	- Compartidas entre todas las sesiones y usuarios.
	- Estado de la aplicación se almacena en una instancia de `HttpApplicationState`.
	- Accesible a través de `Page.Application`
	- Usar en modo lectura.
	- Se inicializa a través de un fichero `Global.asax`.
2. ***Variables de sesión***
	- Accesibles sólo al propietario de la sesión.
	- Requiere envío de `SessionId`.
	- Una sesión es un contexto en el que un usuario se comunica con un servidor a través de múltiples peticiones HTTP.
	- Tiene problemas, HTTP no está orientado a estados ni a sesiones.
	- El concepto de sesión está manejado a nivel de programación y el estado de la aplicación se almacena en una instancia de `HttpSessionState`.
	- Es accesible a través de `Page.Session`.
		1. *Identificador de sesión*: cadena ASCII de 120 bits. Puede almacenarse en una cookie no persistente generada automáticamente.
		2. Opcionalmente puede gestionarse a través de la propia URL. No requiere cambios en el código de la aplicación. Los links relativos siguen funcionando y se redirecciona mediante `Response.ApplyAppPathModifier`. Genera URLs.
		3. El comportamiento puede establecerse a nivel de aplicación con el atributo `cookieless`:
			- `True` o `UseUri`: sessionId incluido en la URL.
			- `False` o `UseCookies`: sessionId incluido en una cookie.
			- `AutoDetect`: cookies se utilizan si el navegador del cliente las permite.
	
	El estado de la sesión puede almacenarse:
	- `In-process`: en memoria, en el proceso ASP.
	- `Out-of-process`: en un servidor de estado ASP, en una BBDD SQL Server. Proporciona fiabilidad y escalabilidad, sobrevive a caídas del proceso ASP.
	
	Propiedades del objeto Session:
	- `Count`: n.º de pares (clave, valor) almacenados.
	- `Keys`: conjunto de claves almacenadas en la sesión.
	- `IsNewSession`: indica si la sesión se ha creado durante la carga de la página actual.
	- `SessionID`: identificador de sesión.
	- `Timeout`: n.º máximo de minutos durante los que la sesión puede permanecer inactiva antes de ser eliminada. 