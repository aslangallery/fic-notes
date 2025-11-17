---
Name: 11 - Configuración Aplicaciones ASP
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**GLOBAL.ASAX**
****
Permite escribir manejadores que reaccionan a eventos globales de la aplicación. El código se ejecuta automáticamente en respuesta a eventos. Sólo un archivo `Global.asax` por aplicación (opcional) y debe residir en el directorio raíz.

**EVENTOS DE LA APLICACIÓN**
****
Existen dos tipos de eventos:
- ***Ocurren siempre para cada request***
	1. *Application_BeginRequest()*: se llama al comienzo de cada request.
	2. *Application_AuthenticateRequest()*: justo antes de que se realice la autenticación.
	3. *Application_AuthorizeRequest()*: después de que el usuario se autentique, se determinan sus permisos.
	4. *Application_ResolveRequestCache()*: se usa en conjunción con caché de salida. El HTML renderizado de un WF se reutiliza, sin ejecutar de nuevo su código. Sin embargo, el manejador de eventos sí que se ejecuta.
	5. En este punto la request es gestionada por el manejador apropiado.
	6. *Application_AcquireRequestState()*: se llama justo antes de que se recupere información específica de la sesión y se use para rellenar la colección Session.
	7. *Application_PreRequestHandlerExecute()*: se llama antes de que el manejador HTTP apropiado ejecute la request.
	8. En este punto el manejador ejecuta la request. 
	9. *Application_PostRequestHandlerExecute()*: se llama justo después de que la request sea manejada.
	10. *Application_ReleaseRequestState()*: se llama cuando la información de la sesión está a punto de ser serializada para que esté disponible para la próxima solicitud.
	11. *Application_UpdateRequestCache()*: se llama justo antes de que se añada información a la caché de salida. 
	12. *Application_EndRequest*: se llama al final de la request, justo antes de que los objetos sean liberados.
	
- ***Ocurren sólo bajo ciertas condiciones***
	1. *Application_Start()*: se invoca cuando la aplicación arranca por 1ª vez y se crea el dominio de aplicación. Lugar útil para proporcionar código de inicialización para la aplicación.
	2. *Session_Start()*: se invoca cada vez que se inicia una nueva sesión. Se utiliza para inicializar información específica del usuario.
	3. *Application_Error()*: se invoca si ocurre una excepción no controlada.
	4. *Session_End()*: se invoca si termina la sesión del usuario. Una sesión finaliza cuando el código la libera explícitamente o cuando expira después de que no se hayan recibido requests en un período de tiempo.
	5. *Application_End()*: se llama justo antes de que la aplicación termine. Puede ocurrir cuando se está reiniciando el IIS o porque la aplicación está cambiando a un nuevo dominio de aplicación .
	6. *Application_Disposed():* se invoca poco después de que la aplicación se haya apagado. El recolector de basura libera la memoria.

**ASP.NET CONFIGURACIÓN**
****
Se realiza mediante archivos XML. Se pueden modificar en cualquier momento sin reciclar la aplicación.

***`machine.config`***
Define secciones soportadas en ficheros de configuración, configura el proceso ASP.NET, registra proveedores... En el mismo directorio se ubica `web.config`, que contiene configuración adicional. Todas las aplicaciones web en la máquina heredan la configuración de los dos ficheros. Algunos de estos aspectos de configuración no aplican cuando se realiza el deploy a un servidor web IIS, que tiene su propio fichero de configuración: `ApplicationHost.config`.

***`web.config`***
ASP usa herencia de configuración, de modo que cada subdirectorio adquiere la configuración de su directorio padre. Existen los siguientes elementos:
- `<location>`: extensión que permite especificar más de un grupo de opciones de configuración en el mismo fichero de configuración. Con `path` se especifica el subdirectorio o archivo al que se le aplican las opciones de configuración.
- `<system.web>`: contiene opciones de configuración específicas de ASP (seguridad, gestión de estados...). Algunos de sus elementos son `authentication`, `authorization`, etc.
- `<appSettings>`: define pares clave-valor. Permite actualizar configuración sin necesidad de recompilar. Útil para rutas de archivos, URL de servicios web...
- `<connectionStrings>`: define cadenas de conexión a BBDD. Se accede con `ConfigurationManager.ConnectionStrings`.

**PÁGINAS DE ERROR**
****
La ejecución de una aplicación web puede originar excepciones:
- *Controladas*: las gestiona el código de usuario (`IncorrectPasswordException`).
- *No controladas*: originadas por algún tipo de error interno (BBDD). Encapsuladas como excepciones `InternalErrorException`.

ASP permite definir una página a la que se redirecciona en caso de ocurrir una excepción no controlada. Se puede definir en 2 niveles:
- *Página*: atributo `PageError`
	```XML
	<%@ Page Language="C#" CodeBehind="Register.aspx.cs"
		Inherits="Es.Udc.DotNet.MiniPortal.Web.Pages.User.Register"
		PageError="InternalError.aspx" %>
	```
- *Aplicación*: sección `customErrors` de `web.config`
	```XML
	<customErrors mode="RemoteOnly"
				defaultRedirect="InternalError.aspx">
	</customErrors>
	```
	Opciones del atributo `mode`:
	- `On`: habilita errores personalizados. Si no se especifica `defaultRedirect`, los usuarios verán un error genérico.
	- `Off`: deshabilita errores personalizados. Permite mostrar errores detallados estándar.
	- `RemoteOnly`: especifica que los errores personalizados sólo deben mostrarse en los clientes remotos. Accediendo desde el servidor en local, se muestran los errores de ASP.
