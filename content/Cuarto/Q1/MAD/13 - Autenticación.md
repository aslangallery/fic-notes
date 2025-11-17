---
Name: 13 - Autenticación
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**AUTENTICACIÓN**
****
>Proceso mediante el cual se comprueba que el usuario final es quien dice ser.

El usuario especifica ciertas credenciales, cuya validez se puede comprobar consultando una lista de datos conocidos. Si las credenciales son correctas, se autentica al usuario.

Tiene 2 niveles:
- ***Internet Information Server (IIS)***
	1. *Anónimo*: servidor no realiza autenticación. Control de autenticación se delega en ASP.
	2. *Básica*: usuario y contraseña.
	3. *Digest*: envía hash de la contraseña.
	4. *Certificados digitales*: cliente necesita un certificado digital.
	5. *Integrada*: se usan las credenciales de la cuenta de Windows.
- ***ASP.NET***
	1. *None*: no se realiza autenticación. Acceso anónimo permitido.
	2. *Windows*: delega autenticación en IIS. Valor predeterminado.
	3. *Forms*: autenticación basada en formularios.
	4. *Passport*: autenticación a través de Web MS Passport.

***Autenticación basada en formularios***
Se indica a ASP.NET dónde se redirige al usuario en caso de que solicite acceso a un recurso protegido. Normalmente se hace a una página para introducir credenciales. Una vez puestas, se hace validación. Si son correctas, se genera un ticket de autenticación que se guarda en una cookie.

Se configura con el IIS en modo anónimo y se añade al `web.config`:
```csharp
<configuration>
  <system.web>
    <authentication mode="Forms">
      <forms
        name=".ASPXAUTH"
        loginUrl="/Authentication.aspx"
        timeout="30"
        path="/"
        defaultUrl="/MainPage.aspx"
        cookieless="AutoDetect" />
    </authentication>
  </system.web>
</configuration>
```

El usuario se puede validar mediante:
- El método `System.Web.Security.FormsAuthentication.Authenticate()` validando contra definiciones del `web.config`.
- Utilizando un método propio.
- Mediante la API `Membership`.

***Métodos de FormsAuthentication***
1. *RedirectFromLoginPage()*: marca al usuario como autenticado suponiendo que las credenciales ya han sido comprobadas. Crea un ticket en una cookie temporal o persistente, en función del valor de `createPersistentCookie`. Después, redirige a la página solicitada.
2. *SetAuthCookie()*: crea una cookie de autenticación y la añade a la response suponiendo que las credenciales ya han sido comprobadas, pero no redirige.
3. *GetAuthCookie()*: lo mismo que la anterior, pero no la añade a la response.
4. *SignOut()*: elimina el ticket.

**AUTORIZACIÓN**
****
> Proceso que consiste en determinar lo que el usuario puede o no puede hacer.

Se realiza una vez que se ha autenticado al usuario. También se conoce como "control de acceso".
Se pueden indicar:
- `*`: todos los usuarios.
- `?`: usuarios no autenticados.

```csharp
<configuration>
	<location path="Register.aspx">
	  <system.web>
	    <authorization>
	      <allow users="*" />
	    </authorization>
	  </system.web>
	</location>
</configuration>
```

***Políticas de acceso***
- *Restrictiva*: todo lo que no está permitido, está prohibido. Se deniega el acceso a todos los usuarios anónimos y se da acceso a los recursos que no necesitan autenticación.
- *Permisiva*: todo lo que no está prohibido, está permitido. Se permite el acceso a todos los recursos y se indican aquellos que necesitan autenticación.





