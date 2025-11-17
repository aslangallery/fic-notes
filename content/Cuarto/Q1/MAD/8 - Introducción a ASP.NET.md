---
Name: 8 - Introducción a ASP.NET
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**INTRODUCCIÓN**
****
>Evolución de ASP. Framework de programación Web dentro de .NET

Al codificar aplicaciones ASP.NET se tiene acceso a las clases del .NET Framework. Permite desarrollar aplicaciones Web con un modelo "similar" al utilizado para aplicaciones Windows. El componente fundamental de ASP.NET es el `WebForm`. 

**WEBSITES Y WEB PROJECTS**
****
Visual Studio ofrece dos modos de crear una aplicación Web con ASP.NET:
1. ***Web Project***
	Desarrollo basado en un proyecto. Archivo `.csproj` que almacena los archivos que pertenecen al proyecto y características de depuración (estructura similar a un proyecto de consola o ventanas). Cuando se ejecuta, se ejecuta todo el proyecto a un solo ensamblado antes de lanzar el proyecto.
	- [i] *Deploy* ⭢ ensamblado + archivos.aspx
	Utilizado para proyectos medianos o grandes.
2. ***WebSite***
	Desarrollo sin proyecto. Cada archivo en el directorio del sitio web forma parte de la aplicación. No se necesita compilar el código. ASP.NET compila el código la primera vez que se solicita a una página. 
	- [i] *Deploy* ⭢ archivos.aspc + código asociado
	Utilizado para proyectos muy pequeños.

**TIPOS DE ARCHIVOS EN UNA APLICACIÓN ASP.NET**
****
- `.aspx` ⭢ páginas Web ASP.NET. Contienen interfaz de usuario y código.
- `.ascx` ⭢ controles de usuario
- `.asmx` o `.svc` ⭢ servicios Web ASP.NET
- `web.config` ⭢ archivo de configuración de la aplicación Web.
- `global.asax` ⭢ permite definir variables globales y reaccionar a eventos de la aplicación (arranque de la aplicación, inicio de una sesión...). No se crea por defecto, si se necesita hay que añadirlo.
- `.cs` ⭢ código C# asociado a una página `.aspx` (code-behind).
- `.master` ⭢ páginas maestras.
- Otros componentes ⭢ imágenes, archivos XML, hojas de estilos...

**CONTROLES TOOL BOX**
****
1. `Standard` ⭢ controles ASP.NET
2. `Validation` ⭢ controles de validación
3. `HTML` ⭢ controles HTML estáticos (tradicionales)

**MODELO DE CÓDIGO**
****
2 modelos para codificar páginas Web:
1. ***Inline code***
	Código y etiquetas HTML se almacenan en un único archivo `.aspx`.
	Código se encierra en uno o más bloques `<script>`.
	Se puede hacer debug, utilizar IntelliSense...
	Sólo se utiliza en WebSite.
2. ***Code-Behind***
	Separa cada página en:
	- `.aspx` ⭢ contiene etiquetas HTML y controles ASP.NET
	- `.aspx.cs` ⭢ contiene el código fuente de la página.
	- `.aspx.designer.cs` ⭢ contiene código generado automáticamente.