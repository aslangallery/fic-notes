---
Name: 1 - Introducción al desarrollo con frameworks modernos
tags:
  - teoría
asignatura: PA
---
***[[Programación Avanzada]]***

**ARQUITECTURAS TÍPICAS DE APLICACIONES**
****
***Arquitectura en capas***
1. *Arquitectura de referencia*
	- *Backend*: compuesto por capa modelo, subdivido en capa de acceso a datos y capa lógica de negocio, y una capa de servicios.
	- *Frontend*: compuesto por capa de acceso a servicios y capa interfaz de usuario.
2. *Arquitectura de aplicación nativa*
	- *Backend*: capa modelo y servicio.
	- *Frontend*: capa de acceso a servicios y capa IU.

***Aplicaciones web SPA***
> Single Page Application es una aplicación web formada por un frontend en JS que se ejecuta en el navegador y un backend, normalmente implementado con servicios REST y JSON y una capa modelo a la que accede el frontend.

**ENTORNOS DE DESARROLLOS MODERNOS**
****
***Entornos para backend y aplicaciones web del lado del servidor***
- Jakarta EE / Spring
- .NET
- Ruby on Rails
- Frameworks de PHP
- Frameworks de Node

***Entornos para frontend en aplicaciones web SPA***
- Angular
- React
- Vue.js

***Entorno para aplicaciones nativas desktop y mobile***
- SDK nativos
- Soluciones cross-platform (Flutter, React)

***Entornos usados en la asignatura***
1. *Backend*
	Varios frameworks de Spring.
2. *Acceso a BD*
	Spring, Java Persistence API (JPA).
3. *Frontend*
	Varios frameworks de React.

**VISIÓN GLOBAL DEL EJEMPLO**
****
***ENTIDADES***
> Una entidad es un objeto o concepto del mundo real que se puede distinguir claramente y del cual se puede recopilar información.

Se implementan con MySQL y se mapean mediante anotaciones de JPA. Cada entidad es una clase y se mapea a una tabla donde cada atributo o propiedad es una columna. Las relaciones se mapean de forma natural en vez de utilizar claves foráneas. Por ejemplo, un pedido tendrá asociado un usuario, no el atributo `userId`.

***DAOs***
> Un DAO (Data Access Object) es un patrón de diseño utilizado para proporcionar abstracción y encapsulamiento al acceso a fuentes de datos.

Al hacerlo con Spring, disponemos de los métodos CRUD del `CrudRepository` implementados automáticamente. Es capaz de implementar métodos de búsqueda mediante ciertas convenciones de nombrado. 

***LÓGICA DE NEGOCIO***
> Seguiremos un enfoque declarativo, describe el resultado deseado y es el sistema el encargado de determinar la mejor forma de hacerlo.

Cada método realizará una transacción gracias a la anotación a nivel de clase `@Transactional`. Si la ejecución no lanza excepciones o lanza una checked, se realiza un commit. Si lanza una `RuntimeException` o `Error` se realiza un rollback.

***SERVICIOS***
Utilizamos servicios web REST. La anotación `@GetMapping` devolverá directamente el JSON con los atributos del DTO correspondiente.

***ACCESO A SERVICIOS***
Se hace una función de JavaScript para cada caso de uso que ofrece la capa servicios.
Por ejemplo:
```javascript
export const findOrder = (orderId, onSuccess) => appFetch(`/shopping/orders/${orderId}`, config('GET', null), onSuccess);
```

Esta función se invoca desde la UI.
```javascript
backend.shoppingService.findOrder(orderId, order => {
	//procesar order
})
```

***UI***
> Sigue un enfoque orientado a componentes. Se implementa la interfaz como un conjunto de componentes, que son pequeños fragmentos HTML, que respondan a eventos del usuario.

1. *Ventajas*
	- [p] Se basa en divide y vencerás.
	- [p] Los componentes son reusables, ya sea en la propia aplicación o en otras.
2. *Componentes*
	![[Pasted image 20250523165355.png]]

