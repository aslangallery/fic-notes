---
Name: 7 - Frontend Tienda
tags:
  - teoría
asignatura: PA
---
***[[Programación Avanzada]]***

**ESTRUCTURA GENERAL DEL FRONTEND**
****
En la carpeta `src` tenemos:
- `backend`: capa de acceso a servicios.
- `i18n`: internacionalización.
- `modules`: capa UI.
- `store`: creación del `store`.
- `main.jsx`: creación de la aplicación

***DESARROLLO MULTIMODULAR***
Dentro de la carpeta `modules` tenemos cada uno de los módulos que conforman la UI correspondiente a los controladores con el mismo nombre. Además contiene otros dos módulos:
- `app`: layout de la UI.
- `common`: componentes reusables.

-  *Estructura de un módulo*
	Cada módulo es parte de la UI. Un módulo está formado por los componentes, ficheros de JS para acciones, reductores y selectores y un `index.js` que define lo que se exportará a otros módulos (acciones, reductores y selectores).

***REDUCTOR RAÍZ***
En `store/rootReducer.js` hay un reductor raíz que combina los reductores de los módulos.

```js
import {combineReducers} from 'redux';  
  
import app from '../modules/app';  
import users from '../modules/users';  
import catalog from '../modules/catalog';  
import shopping from '../modules/shopping';  
  
const rootReducer = combineReducers({  
    app: app.reducer,  
    users: users.reducer,  
    catalog: catalog.reducer,  
    shopping: shopping.reducer  
});  
  
export default rootReducer;
```

**LAYOUT Y ENRUTAMIENTO**
****
***LAYOUT***
El componente `App` se renderiza en `src/main.jsx`. Este componente define el layout de la interfaz.

***ENRUTAMIENTO***
Para poder navegar entre pantallas el frontend utiliza una librería de enrutamiento mediante enlaces llamada React Router. Cambia la URL del navegador para que sea consistente con la pantalla que se muestra.
1. *BrowserRouter*
	El componente envuelve a `App` en el main. Funciona de forma similar al `Provider` de Redux. De esta forma proporciona información para enrutar todos los componentes del árbol.
2. *Componente Link*
	Permite insertar un enlace a una pantalla. Funciona como la etiqueta 
	`<a href=...>` de HTML.
3. *Componentes Routes*
	Funciona como una especie de estructura `switch-case` con los paths que se dan al cambiar de pantallas.
	- *Cambios de pantalla*
		Cuando se produce un cambio de pantalla se renderiza el Route cuyo path concuerde mejor y se muestra el componente especificado en él.
	- *Parámetros*
		Los paths de Route pueden contener parámetros. 

**INTERNACIONALIZACIÓN**
****
Se usa React Intl. Proporciona ciertos componentes que permiten internacionalizar mensajes, cantidades numéricas y fechas.

La función `initReactIntl()` devuelve el locale del navegador y un objeto con los mensajes correspondientes al idioma. `IntlProvider` inyecta el locale a los componentes hijos.

Los ficheros de mensajes se encuentra en `scr/i18n/messages`. 

**ACCESO A SERVICIOS EN FRONTEND**
****
El ejemplo proporciona la función `appFetch` que recibe un path, el tipo de petición HTTP, una función que se ejecutará en caso de que la respuesta sea exitosa y una función que se ejecutará en caso de que sea errónea. Si recibe un error 500, esta función despliega una ventana de error.

Además envía el token del usuario si se había autenticado antes.

***EJEMPLOS***
```js
appFetch('/catalog/products?keywords=2001', config('GET'), result => doSomethingWithResult(result));

// HTTP/1.1 200 OK

/*
{ items: [ { id: 1, name: "2001: A Space Odyssey [Blu-ray]", categoryId: 1 } ], existMoreItems: false }
*/
```

```js
appFetch('/catalog/products?keywords=2001', config('GET'), result => doSomethingWithResult(result));

// HTTP/1.1 200 OK

/*
12
*/
```

```js
appFetch('/shopping/shoppingcarts/1/buy', config('POST', {postalAddress: "Rue del Percebe, 13", ...}), result => doSomethingWithResult(result), errors => doSomethingWithErrors(errors));

// HTTP/1.1 404 Not found

/*
{ globalError: "shopping cart is empty" }
*/
```

```js
appFetch('/shopping/shoppingcarts/1/buy', config('POST', {postalAddress: "Rue del Percebe, 13", ...}), result => doSomethingWithResult(result), errors => doSomethingWithErrors(errors));

// HTTP/1.1 400 Bad request

/*
{ fieldErrors: [ { fieldName: "postalCode", message: "size must be between 1 and 20" } ] }
*/
```

**BÚSQUEDA DE PRODUCTOS**
****
***UNCONTROLLED COMPONENTS***
> Componentes que implementan formularios que acceden a la entrada mediante el atributo `ref`. 

Los datos del formulario se guardan directamente en el DOM de la página. Para conocer los valores de los componentes, hay que consultar directamente el DOM.

- *Problemas*
	- [c] Parte del estado de la UI no se controla por React o Redux.
	- [c] Complica la implementación de funcionalidades adicionales.

***CONTROLLED COMPONENTS***
> Se implementan con otro estilo. Los datos del formulario se guardan en el estado del componente.

Los campos de entrada usan `value` y `onChange` para actualizar el estado.

- *Ventajas*
	- [p] Requieren algo más de cantidad de código.
	- [p] Facilitan añadir funcionalidades.
	- [p] Muestran los datos en mayúscula.

***PROPIEDADES***
Se usa la librería `prop-types` para especificar las propiedades que se pasan a los componentes.
React verifica en tiempo de ejecución que el componente recibe los valores según se especifiquen y lanza un warning en caso contrario.

***ACCIONES***
1. *Acciones asíncronas*
	Cuando se realizan peticiones al backend se realizan peticiones asíncronas. El código que realiza las peticiones no se bloquea mientras espera la respuesta.
	- *Comunicación con el reductor*
		1. Avisar al reductor del comienzo de la petición.
		2. Informar de que la petición terminó correctamente.
		3. Informar de que la petición terminó en error.
2. *HandleSubmit*
	Se usa para manejar la lógica de envío de datos después de que un usuario completa y envía un formulario. Permite que un action creator devuelva funciones en lugar de objetos.

**DETALLES DE PRODUCTOS**
****
***USE EFFECT***
> Hook que permite ejecutar código luego de que el componente se renderice.

Sirve para efectos secundarios como: 
- Pedir datos a back.
- Suscribirse a eventos.
- Modificar el DOM.
- Establecer timers.

```js
useEffect(() => {
  // Este es el EFECTO: lo que queremos que pase tras renderizar
  return () => {
    // Este es el CLEAN-UP: lo que queremos que pase antes de desmontar o antes del próximo efecto
  };
}, [dependencias]);
```
1. *Fases*
	1. *Montaje inicial*: se ejecuta el efecto una vez que el componente se ha montado.
	2. *Actualización*: si cambian las dependencias, primero se ejecuta `clean-up` anterior, luego el nuevo efecto.
	3. *Desmontaje*: cuando el componente se elimina del DOM, se ejecuta el `clean-up` final.
2. *Dependencias*
	Es el segundo argumento, un array que determina cuándo se ejecuta el efecto.
	
	| Dependencias | Qué pasa                                                               |
	| ------------ | ---------------------------------------------------------------------- |
	| No se pasa   | El efecto se ejecuta tras **cada render**.                             |
	| `[]` vacío   | Solo se ejecuta una vez al **montar el componente**.                   |
	| `[x, y]`     | Se ejecuta **solo cuando cambia alguno** de esos valores (`x`, `y`).\| |

**COMPRA DE PRODUCTOS**
****
***VALIDACIONES DEL LADO CLIENTE***
Se pueden usar de dos formas:
- *Enfoque declarativo*
	Los campos indican las restricciones que se deben cumplir. Algunas ya van implícitas con el tipo del campo.
	```js
	<input type='email' id=email>
	
	//Crea automáticamente una restricción para validar que el valor introducido sigue la sintaxis de un correo electrónico.
	
	<input type="text" id="name" required minlength="6" maxlength="20">
	
	<input type="number" id="age" min="18" max="100">
	```
	La ventaja es que no hay que programar, pero:
	- [c] No hay un estándar para los mensajes de error.
	- [c] Los mensajes se muestran de acuerdo al locale del navegador, no de la aplicación.

- *Enfoque programático*
	Permite especificar restricciones en los campos de entrada, pero el navegador no ejecuta las validaciones automáticamente. En su lugar, proporciona una API para ejecutar las validaciones y permite añadir mensajes de error con contenido propio. 
	Por temas de seguridad, el backend hace todas las validaciones de todas formas. Los errores se muestran con el componente `Errors`.

***GESTIÓN DE ERRORES DEVUELTOS POR EL BACKEND***
Los errores devueltos se mantienen en el estado local del componente. El usuario presiona el botón del formulario y se llama a `handleSubmit` y se ejecuta la acción. Si devuelve errores, se invoca a `backendErrors`.

Después se renderiza el componente de nuevo y se muestra el error. Si el usuario cierra el error, el componente se vuelve a renderizar.

**RECARGAS Y POLÍTICAS**
****
***RECARGAS***
> Ocurren cuando se modifica el código del frontend mientras se está en modo desarrollo.

Puede ocurrir:
- Se recarga solo un componente al editar un JSX.
- Se recarga todo el frontend al editar un fichero JS. Todas las variables pierden su valor.

1. *Recarga total*
	Si no se controla, los datos del usuario desaparecerán y el desarrollador tendría que volver a autenticarse. 
	El estado local de los componentes se pierde.
	Para poder hacer esto, el token JWT se guarda en el almacenamiento Web del navegador. Los navegadores para ello proporcionan la API `Storage`, que permite guardar pares clave-valor.
	- *Storage*
		1. `localStorage`: los datos no tienen fecha de expiración.
		2. `sessionStorage`: están vinculados a la web. Si se cierra, desaparecen.

***POLÍTICA***
El código frontend se descarga de un lugar distinto al código backend.
1. *Same-origin*
	El origen de la URL es la combinación de protocolo, host y puerto. Obliga a que un script descargado de un origen no pueda interactuar con scripts de otro origen.
2. *CORS*
	Para poder utilizar aplicaciones web SPA es necesario el mecanismo CORS (Cross-Origin Resource Sharing). Las respuestas del backend contienen cabeceras con los permisos que tendrán los scripts del navegador para poder interactuar con el backend.