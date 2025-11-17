---
Name: 10 - Controles del servidor
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**TIPOS DE CONTROLADORES**
****
>Se ejecutan en el lado del servidor. Poseen el atributo `runat = "server"`
>Pueden mantener su estado entre PostBacks haciendo uso de ViewState.

Existen dos tipos de controladores: 
1. ***HTML Controls***
	Se corresponden con elementos HTML estándar. Los elementos no son accesibles desde código del lado del servidor, desde `CodeBehind` no es posible acceder a sus propiedades. 
	ASP permite convertir elementos existentes en HTML en controles de servidor, simplemente añadiendo los atributos `runat = "server"` e `id`. A partir de ese momento, es posible acceder al control desde el lado del servidor. 
2. ***Web Controls***
	Accesibles desde el lado del servidor. Poseen mayor funcionalidad. 
	Son del tipo `<asp:nombre_control>` y no tienen relación 1:1 con elementos HTML, un web control puede generar varios elementos elementos HTML al renderizar.
	
	Además de los atributos `id` y `runat`, otras propiedades comunes son:
	- *CssClass*: define atributo HTML class. Apunta a una clase CSS definida.
	- *Enabled*: determina si el usuario puede interactuar con el control en el navegador. 
	- *TabIndex*: fija el atributo HTML que determina el orden en el que los usuarios pueden moverse a través de los controles pulsando la tecla `Tab`.
	- *ToolTip*: se renderiza como un atributo `title` en el HTML.
	- *Visible*: determina si el control se envía o no al navegador.

**PROPIEDADES DE SYSTEM.OBJECT.CONTROL**
****
- ***ClientID***: identificador único creado por ASP cuando se instancia la página.
- ***Controls***: colección de controles.
- ***EnableViewState***: indica si el control debería mantener su estado entre peticiones.
- ***ID***: id del control, nombre a través del cual se puede acceder al control desde código.
- ***Page***: referencia a la página que contiene el control.
- ***Parent***. referencia al "padre" del control.
- ***Visible***: indica si el control debe renderizarse.

**MÉTODOS DE SYSTEM.OBJECT.CONTROL**
****
- ***DataBind()***: enlaza el control con un DataSource.
- ***FindControl()***: busca un control con un nombre determinado.
- ***HasControls()***: verifica si tiene "hijos".
- ***RenderControl()***: genera HTML de salida para el control.

**WEB CONTROLS DE VALIDACIÓN**
****
>Elementos ocultos que validan las entradas de datos contra algún patrón.
- *Validación lado cliente*: permite avisar al usuario antes de enviar los datos al servidor. Implica generar código del lado del cliente con JS. Los controles de validación de ASP lo hacen automáticamente.
- *Validación lado servidor*: independientemente de si los datos han sido validados en el lado del cliente o no, ASP repite la validación en el servidor.

ASP proporciona 6 controles:
- ***RequiredFieldValidator***: comprueba que el control que tiene que validar no está vacío en el momento de enviar el formulario.
- ***RangeValidator***: valor dentro de un rango de tipos.
- ***CompareValidator***: valida contra un valor constante o contra otro control.
- ***RegularExpressionValidator***: valida contra un patrón o expresión regular.
- ***CustomValidator***: permite definir una validación personalizada desde el lado del cliente y su correspondiente validación del lado del servidor.
- ***ValidationSummary***: no es un validador en sí, muestra de forma agrupada los mensajes de error generados por otros controles.

La validación tiene lugar después de que se cargue la página, pero antes de que sucedan otros eventos. En el manejador del evento, antes de ejecutar el código, se puede comprobar si la página es válida con la propiedad `Page.IsValid`.

**CLASE BASEVALIDATOR**
****
Controles de validación se encuentran en `System.Web.UI.Controls` y extienden de `BaseValidator`, que define la funcionalidad básica de un control de validación:
- ***ControlToValidate***: indica el control a validar.
- ***Display***: indica cómo se muestra el mensaje de error.
	1. *Static*: espacio para mostrar el mensaje se reserva.
	2. *Dynamic*: página cambia dinámicamente para mostrar el mensaje de error.
- ***EnableClientScript***: especifica si se realiza validación en el lado del cliente.
- ***Enabled***: permite habilitar/deshabilitar el validador.
- ***ErrorMessage***: cadena que se muestra en el resumen de errores utilizado por el control `ValidationSummary`.
- ***Text***: cadena que se muestra en el control de validación si esta falla.
- ***IsValid***: determina si el valor del control asociado es válido.
- ***SetFocusOnError***: si es `true`, el navegador cambia el foco al control que falló en la validación.
- ***ValidationGroup***: permite agrupar validadores y realizar validación por grupos.
- ***Validate()***: efectúa la validación y actualiza el valor de `IsValid`. La página web llama a este método cuando una página realiza un postback debido a un control con `CausesValidation = "true"`.