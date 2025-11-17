---
Name: 9 - WebForms
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**INTRODUCCIÓN**
****
>Modelo de eventos de ASP permite añadir controles a un WebForm y decidir a qué eventos se quiere responder.

Cada evento se gestiona en un método y el código permanece claro y ordenado.

**MODELADO DE EVENTOS**
****
***¿Cómo funciona?***
1. Cuando la página se ejecuta por primera vez, ASP crea objetos, ejecuta código de inicialización, se renderiza a HTML y se envía al cliente. Los objetos se liberan.
2. El usuario realiza una acción que dispara un `postback`. La página se envía con todos los datos del formulario (`submit`).
3. ASP intercepta la página y crea de nuevo los objetos.
4. ASP comprueba qué operación se disparó y lanza los eventos. En este punto se realizará alguna operación del lado del servidor y se actualizarán los controles.
5. La página modificada se renderiza a HTML y se devuelve al cliente. Los objetos se liberan de la memoria del servidor.

**POSTBACKS AUTOMÁTICOS**
****
Los controles ASP.NET proporcionan características de *postback automático*:
- Permite disparar un `postback` cuando un usuario hace click en un checkbox, cambia el texto en un textbox y se mueve a otro campo...
- Emula el modelo de una aplicación de escritorio aunque en n.º de eventos sea menor.

***¿Cómo funcionan?***
1. Establecer la propiedad **AutoPostBack** a `true`, lo que asegura un rendimiento óptimo si no se necesita reaccionar a un evento.
2. ASP añade JS a la página HTML renderizada llamada **`_doPostBack()`**
3. ASP añade 2 campos de entrada ocultos que la función anterior usa para pasar información de nuevo al servidor:
	- **ID de control** -> que lanzó el evento.
	- Información adicional.
4. `_doPostBack()` establece estos valores con la información apropiada sobre el evento y reenvía el formulario.
5. La característica de postback automático está en Web Controls.

**VIEW STATE**
****
>Mecanismo que implementa ASP para mantener el estado de controles entre distintas llamadas al servidor.

ASP examina todas las propiedades de todos los controles de la página antes de que esta sea renderizada a HTML y enviada. Si alguno cambió su estado inicial, se anota a una colección nombre/valor. Esta información se serializa y codifica en base 64 y se inserta en la sección `<form>` como un campo oculto.

La próxima vez que la página se envía al servidor:
1. ASP carga la página en base a sus valores iniciales.
2. Deserializar View State y actualizar los controles. Devuelve a la página al estado anterior.
3. Actualiza la página de acuerdo a los datos enviados desde el cliente.
4. Se gestionan los eventos.

***Ventajas***
- Escalabilidad -> los recursos del servidor se liberan tras cada petición.

***Desventajas***
- Se incrementa el tamaño de la página.

**ETAPAS DE PROCESADO**
****
1. ***Inicialización de la página***
	ASP genera controles definidos con tags en `.aspx`. Si no es la primera vez que se solicita la página, ASP deserializa la información del View State y la aplica a los controles. Se lanza `Page.Init`.
2. ***Inicialización del código de usuario***
	Se lanza `Page.Load` (siempre, independientemente de si la página se solicita por primera vez o si es un postback). Para diferenciar la primera vez que se carga la página, ASP tiene la propiedad `IsPostBack`.
3. ***Validación***
	ASP incluye controles que validan otros controles de entrada del usuario y que muestran mensajes de error. Los eventos de validación se lanzan antes que otros tipos de eventos, aunque no es necesario responder a ellos. Basta con consultar la propiedad `Page.IsValid`.
4. ***Gestión de eventos***
	La página está completamente cargada y validada. ASP dispara ahora todos los eventos que hayan ocurrido desde el último postback. Estos pueden ser:
	- *Eventos de respuesta inmediata*: clic en submit u otro botón de postback.
	- *Eventos de cambios*: cambiar texto en una caja de texto. Se disparan una vez que dispararán la próxima vez que se envíe la página, salvo que se haya establecido `AutoPostBack`.
5. ***Cleanup***
	Después de que la página haya sido renderizada, se lanza `Page.Unload`.

**FLUJO DE PÁGINA**
****
>Los eventos de página son gestionados automáticamente siempre que `AutoEventWireup = true`.

Hay que respetar los nombres predefinidos. Necesario indicar los eventos de controles y el nombre en `.aspx` debe coincidir con el de `aspx.cs`.

**SYSTEM.WEB.UI.PAGE**
****
>WebForms son instancias de `System.Web.UI.Page`.

Algunas propiedades interesantes son:
1. ***Session y Application***
	- *Session*: instancia de `System.Web.SessionState.HttpSessionState`. Pensado para almacenar datos específicos del usuario que necesitan mantenerse entre solicitudes a distintas páginas. Almacena conjuntos de pares nombre/valor.
	- *Application*: instancia de `System.Web.HttpSessionState`. Pensado para almacenar datos globales a la aplicación. Almacena conjunto de pares nombre/valor.
2. ***Request***
	Instancia de `System.Web.HttpRequest`. Representa valores y propiedades de la solicitud HTTP que causó que la página se cargara.
	Propiedades interesantes:
	- *Browser*: permite consultar características del navegador.
	- *Cookies*: obtiene la colección de cookies enviadas con esta solicitud.
	- *QueryString*: proporciona parámetros que se pasaron con la consulta.
	- *URL y URLReferrer*: proporciona el Uri que representa la dirección actual de la página y la página de la que viene el usuario.
	- *UserLanguages*: proporciona un array de strings ordenado que lista las preferencias de lenguaje del cliente.
3. ***Response***
	Instancia de `System.Web.HttpResponse`. Representa la respuesta del servidor web a una solicitud del cliente. 
	Propiedades y métodos interesantes:
	- *Cookies*: colección de cookies enviadas con la respuesta.
	- *Redirect()*: indica al navegador que solicite otra URL.
4. ***Server***
	Instancia de `System.Web.HttpServerUtility`.
	Métodos interesantes:
	- *HtmlEncode() y HtmlDecode()*: convierte string normal en otro con caracteres HTML válidos.
	- *UrlEncode() y UrlDecode()*: convierte string normal en otro con caracteres válidos en una URL.
	- *MapPath()*: devuelve ruta física correspondiente a una ruta virtual en el servidor.
	- *Transfer()*: transfiere ejecución a otra página web en la misma aplicación web.
5. ***Trace***
	Instancia de `System.Web.TraceContext`. Permite escribir información en un log a nivel de página.
	Se puede habilitar desde código con `Trace.IsEnabled = true` o usando el atributo `Trace` en la directiva `Page`.
	
	Los mensajes se muestran en el orden en el que fueron generados. 
	Se pueden escribir mensajes en la traza con los métodos:
	- *Trace.Write()*: mensajes informativos.
	- *Trace.Warn()*: mensajes de advertencia.
	Si la traza se deshabilita, los métodos se ignoran.
	
	Opciones de configuración de traza:
	- `enabled`
	- `requestLimit`: limita almacenamiento de las trazas a un n.º específico.
	- `pageOutput`: muestra información de traza en la propia página. 
	- `traceMode`
	- `localOnly`