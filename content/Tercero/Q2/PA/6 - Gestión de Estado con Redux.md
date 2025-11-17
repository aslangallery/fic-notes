---
Name: 6 - Gestión de Estado con Redux
tags:
  - teoría
asignatura: PA
---
***[[Programación Avanzada]]***

**INTRODUCCIÓN A REDUX**
****
> Librería de JS que utiliza una arquitectura funcional y permite gestionar el estado de una aplicación de forma centralizada y modular. Permite recoger la mayor parte del estado y de la lógica de los componentes.

Funciona de forma similar al patrón Provider-Consumer. Los componentes reciben notificaciones cuando cambia el estado. Además permite que el estado que solo se utiliza en un componente sea gestionado por él mismo.

***CONCEPTOS PRINCIPALES***
El estado de la aplicación se almacena en un objeto llamado `store`. El estado tiene propiedades, que son las ramas del árbol de objetos que mantiene `store`.

1. *Acciones*
	Cuando queremos modificar el estado, se envía un objeto llamado `action` al `store`. Se hace mediante la función `store.dispatch(action)`. La acción contiene la propiedad `type`, que indica el tipo y otras propiedades adicionales.
2. *Reductor*
	Encargado de producir un nuevo estado ante una acción. Se ejecuta al invocar a  `store.dispatch(action)`. Devuelve el nuevo estado a partir de la acción y del estado anterior.
	
	Debe ser una función pura, lo que significa que siempre debe devolver el mismo resultado al pasarle los mismos parámetros.
	
	El estado es inmutable. El reductor devuelve un nuevo objeto, no modifica el estado anterior. Esto permite reducir el número de renderizaciones en el Virtual DOM.
3. *Notificaciones*
	Los componentes reciben notificaciones cuando el reductor produce un nuevo estado. Se pueden suscribir con la función `store.subscribe` y leen el estado con `store.getState`. Reciben la parte del estado que les interesa mediante propiedades o hooks.
	Funciona de forma muy similar al patrón Observador.
