---
Name: 4 - Capa Servicios REST
tags:
  - teoría
asignatura: PA
---
***[[Programación Avanzada]]***

**DESARROLLO DE CONTROLADORES**
****
Se define un controlador por cada servicio local. Reciben peticiones HTTP e invocan un método de un servicio local. Internamente spring-web utiliza la API `Servlet`.

Las ventajas son:
- [p] Mayor nivel de abstracción.
- [p] Los controladores tienen la misma cantidad de métodos públicos que el servicio local.
- [p] Se pueden usar anotaciones para manejar peticiones HTTP.
- [p] La conversión de JSON a DTO es automática.

***ESQUEMA***
![[Pasted image 20250524124819.png]]

***USERCONTROLLER***

| Recurso                      | Método | Entrada                                                 | Salida                      |
| ---------------------------- | ------ | ------------------------------------------------------- | --------------------------- |
| /users/signUp                | POST   | UserDto [body]                                          | AuthenticatedUserDto [body] |
| /users/login                 | POST   | LoginParamsDto [body]                                   | AuthenticatedUserDto [body] |
| /users/loginFromServiceToken | POST   | userId [jwt]                                            | AuthenticatedUserDto [body] |
| /users/{id}                  | PUT    | userId [jwt], id [path], UserDto [body]                 | UserDto [body]              |
| /users/{id}/changePassword   | POST   | userId [jwt], id [path], ChangePasswordParamsDto [body] |                             |

En las URL 1, 2 y 3 se usa overloading POST porque no corresponden a operaciones CRUD. El `AuthenticatedUserDto` incluye los datos del perfil de usuario, excepto la contraseña y el carrito para que sean cacheados por el frontend. El `userId` se pasa de forma segura mediante un JSON Web Token (JWT).

***CATALOGCONTROLLER***

| Recurso                | Método | Entrada                                            | Salida                                  |
| ---------------------- | ------ | -------------------------------------------------- | --------------------------------------- |
| /catalog/categories    | GET    |                                                    | List< CategoryDto >, [body]             |
| /catalog/products/{id} | GET    | id [path]                                          | ProductDto [body]                       |
| /catalog/products      | GET    | categoryId [param], keywords [param], page [param] | BlockDto<br>< ProductSummaryDto> [body] |

***SHOPPINGCONTROLLER***

| Recurso                                                                 | Método | Entrada                                                                             | Salida                                |
| ----------------------------------------------------------------------- | ------ | ----------------------------------------------------------------------------------- | ------------------------------------- |
| /shopping/shoppingcarts/{shoppingCartId}/addToShoppingCart              | POST   | userId [jwt], shoppingCartId [path], AddToShoppingCartParamsDto [body               | ShoppingCartDto [body]                |
| /shopping/shoppingcarts/{shoppingCartId}/updateShoppingCartItemQuantity | POST   | userId [jwt], shoppingCartId [path], UpdateShoppingCartItemQuantityParamsDto [body] | ShoppingCartDto [body]                |
| /shopping/shoppingcarts/{shoppingCartId}/removeShoppingCartItem         | POST   | userId [jwt], shoppingCartId [path], RemoveShoppingCartItemParamsDto [body]         | ShoppingCartDto [body]                |
| /shopping/shoppingcarts/{shoppingCartId}/buy                            | POST   | userId [jwt], shoppingCartId [path], BuyParamsDto [body]                            | Long [body]                           |
| /shopping/orders/{orderId}                                              | GET    | userId [jwt], orderId [path]                                                        | OrderDto [body]                       |
| /shopping/orders                                                        | GET    | userId [jwt], page [param]                                                          | BlockDto<br>< OrderSummaryDto> [body] |

En las URL 1, 2, 3 y 4 se usa overloading POST porque no corresponden a operaciones CRUD. `ShoppingCartDTO` es la nueva representación del carrito, que será cacheada por el frontend. El `userID` se pasa de forma segura mediante un JSON Web Token (JWT).

***MAPPING DE PETICIONES HTTP***
Con la anotación `@RequestMapping`, se indica que las peticiones dirigidas a un path que comience por ese valor son procesadas por el controlador correspondiente. 

Con `@{Get, Post, Put, Delete}Mapping` se especifica en el método qué tipo de petición HTTP es. El path indicado mapea la petición con el método correspondiente. Se puede capturar una parte del path entre llaves como sucede con `orderId`. Se inyectará en un parámetro del método, nombrado igual, con `@PathVariable`.

***CONVERSIÓN DE JSON A DTO***
Para convertir de JSON a DTO o viceversa se usa Jackson. Los nombres de los campos del JSON tienen que ser iguales a los `getter` y `setter` de los DTOs. 


**VALIDACIÓN BÁSICA DE DATOS**
****
Se usan spring-web y la API estándar de validaciones de Java para realizar validaciones básicas de los datos recibidos en las peticiones HTTP.

Esto evita mucha lógica en la capa modelo. Simplifica el código y no es ningún problema porque la capa modelo solo es accesible a través de la capa de servicios REST.

Los datos pueden venir de:
- Variables en el path.
- Parámetros HTTP.
- Representaciones en el cuerpo de la petición.

***FUNCIONAMIENTO***
Spring-web valida conversiones de valores y la obligatoriedad de variables del path o de parámetros HTTP en base a los tipos Java de los parámetros correspondientes al hacer el mapeo.

1. *Ejemplo*
	`POST https://.../shopping/shoppingarts/abc/buy`
	Devuelve un error 400 porque `abc` no es un valor convertible a `Long`.
	Si la conversión no se puede hacer o falta un parámetro obligatorio, se lanza un error 400 con un JSON en la respuesta significativo. No se presentan problemas para el usuario, ya que no teclea manualmente el ID del carrito.
2. *Validaciones más refinadas*
	En los casos de peticiones POST/PUT, se deben pasar los datos en el cuerpo de las peticiones. Se mapean a un DTO y se especifican reglas de validación mediante la API Bean Validation.

**GESTIÓN DE EXCEPCIONES**
****
Spring-web ofrece varias formas para que un método de un controlador devuelva un código de respuesta y una representación de este en el cuerpo:
- *Si no devuelve excepción*: 200 OK y el valor en el cuerpo de la respuesta.
- *Si devuelve una excepción*: código de la respuesta e información sobre el error.

***EXCEPCIONES***
Se declaran en el `throws` y se usan anotaciones para saber cómo tratar cada excepción.
Por cada excepción se define un método que la trata con las anotaciones.
- `@ExceptionHandler`: spring-web invoca al método cuando un controlador lance una excepción correspondiente.
- `@ResponseStatus`: código de respuesta.
- `@ResponseBody`: representación del contenido del cuerpo.
- `@ControllerAdvice`: permite tratar las excepciones comunes a varios controladores en una única clase. Es un bean y permite inyección de dependencias.

**INTERNACIONALIZACIÓN DE MENSAJES**
****
***MENSAJES DE ERROR***
Deben ir en el idioma que el usuario tenga seleccionado. La emisión de mensajes en un idioma concreto se denomina `i18n` o internacionalización. Se hace mediante locales, una abstracción de idioma, país y variante.
1. *Internacionalización en Java*
	Se puede usar `java.util.Locale`.
	Solo se tratan los mensajes y se dejan para el frontend el resto de aspectos. Spring permite hacerlo mediante ficheros properties con el formato clave = mensaje en el idioma x. En PA Shop los mensajes se guardan en `backend/src/main/resources`.
	Se usa el bean `MessageSource` para recuperar el mensaje a partir de su clave.
2. *Peticiones del navegador*
	Pueden enviar en las peticiones HTTP la cabecera `Accept-Language`. Es una lista priorizada de los locales que prefiere el usuario.
3. *Mensajes en otros idiomas*
	Algunos idiomas no están disponibles en la implementación de la API de Bean Validation y se deben definir en, por ejempo, `messages_gl.properties`.

**AUTENTICACIÓN, ACCESO Y CONTROL DE ACCESO**
****
***PASO SEGURO DEL ID DEL USUARIO***
Algunas peticiones necesitan el ID del usuario. Es necesario poder enviarlo de forma segura del frontend al backend. 

![[Pasted image 20250524134450.png]]

Autenticarse consiste en introducir usuario y contraseña. Si es correcto, devuelve una lista de caracteres (token), que se devuelve al backend y autoriza al usuario para hacer peticiones. Este incluye el id del usuario.

Todas las peticiones que requieran el id del usuario tienen que incluir el token. Cuando se hace una petición se hace el control de acceso. Consiste en validar el token y comprobar que el usuario tiene permitido realizar esa petición.

***JWT (JSON WEB TOKEN)***
Para generar los token se usan JSON Web Token. Define un formato que permite transmitir información en JSON entre dos partes de forma segura.

Los token están formados por `cabecera.cuerpo.firma`:
- *Cabecera*: tipo de token y algoritmo de firma usado codificado en base64url.
- *Cuerpo*: información sobre una entidad codificado en base64url.
- *Firma*: algoritmo de firma para la cabecera y el cuerpo en conjunto en base64url.

1. *Autenticación*
	Para autorizar a un usuario se recupera a partir del `userName`. Se cifra la contraseña introducida y se comprueba si coincide con `user.getPassword()`.
2. *Autorización*
	Se genera un token firmado con un algoritmo de firma simétrico. En el cuerpo del token se incluyen la fecha de expiración del token, el ID del usuario y el rol del usuario.
3. *Control de acceso* 
	Se maneja con Spring Security. Permite realizarlo de forma declarativa mediante anotaciones, ficheros de configuración o código. Se hace en la clase `SecurityConfig`.
	- *Otras formas de control de acceso*
		Hay parte del control de acceso que no se hace en base a roles. Por ejemplo, en la búsqueda de un pedido, se comprueba que el pedido además de ser de un usuario rol `USER`, pertenezca a la persona que lo busca.
