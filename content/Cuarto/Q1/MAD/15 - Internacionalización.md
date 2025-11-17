---
Name: 15 - Internacionalización
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**INTRODUCCIÓN**
****
>Proceso de diseñar un producto de forma genérica para facilitar el proceso de localización sin necesidad de modificar el componente central.

Separar desde un principio el código fuente de la información sobre el idioma y las peculiaridades culturales.

***Estándares y tipos de cultura***
- *Cultura neutral*: indica el idioma, no incluye información sobre el país o región.
- *Cultura específica*: indica el idioma y el país/región (fr - FR).
- *Cultura invariante*: raíz de tomas las demás culturas. Útil para casos en los que sea necesario almacenar información de forma independiente de culturas específicas. Se hace mediante `CultureInfo.InvariantCulture`.

***CultureInfo***
Proporciona información sobre una cultura concreta. Permite acceder a objetos concretos de la cultura con operaciones específicas para ellos.

Contiene las propiedades:
- `CurrentCulture`: determina los resultados que dependen del país y región, y su valor es una cultura específica.
- `CultureUICulture`: determina el idioma de las cadenas de texto de la UI y su valor puede ser simplemente el idioma.

**ARCHIVOS DE RECURSOS**
****
Almacenan diferentes tipos de recursos, como cadenas de texto o imágenes. Son archivos XML con extensión `.resx` formados por pares clave-valor. La clave no distingue mayúsculas y minúsculas. Debe haber un archivo por idioma/región.

Pueden ser:
- Locales.
- Globales.

Se pueden manejar cómodamente mediante la extensión `ResXResourceManager`.

**ARCHIVOS DE RECURSOS LOCALES**
****
Sirven para almacenar recursos de páginas específicas y se almacenan en `App_LocalResources`. Siguen el formato `<NombreWebForm>.aspx.<codcult>.resx`. No se especifica ningún código de cultura, este será el archivo por defecto.

***Creación de archivos de recursos locales***
Se pueden crear desde la vista de diseño o desde el explorador de soluciones. Esto añadirá, por ejemplo, `meta:resourcekey="Label1Resource1` a `Label1`.

***Acceso a archivos de recursos locales***
1. *Localización implícita*
	Se hace mediante `meta:resourcekey` y se sigue el formato `Resource_key.Property`. 
2. *Localización explícita*
	Se utiliza `<%$ Resources: Class, Resource_ID %>` para asociar cada propiedad de un control con un nombre en el archivo de recursos. Poner `Class` es opcional y si se omite se usa el archivo de recursos local asociado a la página.
3. *Mediante programación*
	Con el método `GetLocalResourceObject(string resourceKey)`.
	```csharp
	protected void Page_Load(object sender, EventArgs e){
		Button1.Text = GetLocalResourceObject("Button1_Texto").ToString(); 
	}
	```
	Se puede activar la generación de código y acceder a los valores mediante propiedades estáticas.

**ARCHIVOS DE RECURSOS GLOBALES**
****
Sirven para almacenar recursos presentes en varias páginas del sitio web. Se almacenan en `App_GlobalResources`, que debe estar en el directorio raíz. Los ficheros siguen el formato `<nombre>.<codcult>.resx`.

***Creación de archivos de recursos globales***
No se pueden generar automáticamente. Se hace de forma muy similar a los locales, pero desde la carpeta raíz.

***Acceso a recursos globales***
1. *Localización explícita*
	Igual que locales, pero `Class` obligatorio. Indica nombre del archivo de recursos globales.
2. *Mediante programación*
	Se puede hacer con:
	- Método genérico
		```csharp
		Button1.Text = GetGlobalResourceObject("Resource_Global", "Cancel").ToString();
		```
	- Propiedades estáticas
		```csharp
		Button1.Text = Resources.Resource_Global.Cancel;
		```

***asp:Localize***
Para incluir bloques de texto se puede usar `asp:Localize`. No genera etiquetas extra HTML y es editable en la vista de diseño.

```csharp
<h3>
	<asp:Localize runat=server ID="LocWelcMess"
	Text="Welcome"
	meta:resourcekey="WelcomeMessage"/>
</h3>

// Renderiza
<h3>Welcome</h3>
```

**CULTURAS**
****
La cultura se puede indicar de 3 formas:
1. ***Mediante programación***
	Se puede indicar en el `web.config`.
	```csharp
	<configuration>
		<system.web>
			<globalization
			uiCulture="..."
			culture="..." />
		</system.web>
	</configuration>
	```
2. ***A nivel de página***
	Se puede indicar con la directiva `Page`
	```csharp
	<%@Page UICulture="..." Culture="..." %>
	```
3. ***Mediante programación***
	Hay que cambiar el comportamiento por defecto de la cultura de la BBDD con el método `InitializeCulture()`.
	```csharp
	using System.Globalization;
	using System.Threading;
	
	...
	
	public partial class MainPage : System.Web.UI.Page
	{
	    ...
	
	    protected override void InitializeCulture()
	    {
	        // establecer la cultura en inglés (EE. UU.)
	        CultureInfo cultureInfo = new CultureInfo("en-US");
	        Thread.CurrentThread.CurrentCulture = cultureInfo;
	        Thread.CurrentThread.CurrentUICulture = cultureInfo;
	    }
	
	    ...
	}
	```
	Se tiene que hacer esto en todas las páginas. No se puede hacer sobre la página maestra porque no hereda de `System.Web.UI.Page`, por tanto no se puede sobrescribir el método. La solución es crear una clase hija que lo sobrescriba y que todas las páginas hereden de ella.
	```csharp
	using System.Threading;
	using System.Globalization;
	
	namespace Es.Udc.DotNet.MyApp.HTTP.Session
	{
	    public class SpecificCulturePage : Page
	    {
	        protected override void InitializeCulture()
	        {
	            // leer el idioma y el país (podría ser desde una base de datos)
	            String language = "es"; // idioma español
	            String country = "ES";  // país España
	            String culture = language + "-" + country;
	
	            // establecer la cultura específica
	            CultureInfo cultureInfo = CultureInfo.CreateSpecificCulture(culture);
	            Thread.CurrentThread.CurrentCulture = cultureInfo;
	            Thread.CurrentThread.CurrentUICulture = cultureInfo;
	        }
	    }
	}
	
	----------
	
	using System.Threading;
	using System.Globalization;
	using Es.Udc.DotNet.MyApp.HTTP.Session;
	
	namespace Es.Udc.DotNet.MyApp.Pages
	{
	    public partial class APage : SpecificCulturePage
	    {
	        // lógica adicional aquí
	        ...
	    }
	}
	```
	