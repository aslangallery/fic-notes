---
Name: 5 - Casos de uso
tags:
  - teoría
asignatura: ER
---
***[[Ingeniería Requisitos]]***

**INTRODUCCIÓN A LOS CASOS DE USO**
****
Los sistemas software se desarrollan para servir a los usuarios. Los distintos usuarios perciben el software de formas diferentes. Por ejemplo, en el caso del torno del metro, no es lo mismo un usuario que quiere usar el metro, cuyo objetivo es introducir la moneda y pasar; que un responsable del metro que quiere ver cuánto dinero ha recaudado y cuántas personas han pasado.

Los usuarios comprenden mucho mejor el sistema si se describen desde una perspectiva funcional. Sin embargo, realizar muchas funcionalidades puede ser malo, ya que puede implicar trabajo innecesario que puede haber que eliminar.

Los casos de uso permiten entender mejor qué quiere el usuario e implementar el sistema desde una perspectiva funcional.

**DEFINICIÓN DE CASO DE USO**
****
Un usuario no es lo mismo que un actor. Un usuario puede tomar diferentes roles para llevar a cabo diferentes acciones. Los usuarios pueden ser personas físicas o sistemas.

Los casos de uso y los escenarios son las formas de reproducir cómo los usuarios utilizan el sistema para realizar tareas. Los casos de uso suelen ayudar a contemplar escenarios que pueden pasar desapercibidos.

- **Escenario**
	Un escenario es una descripción detallada, en lenguaje natural, de la secuencia de pasos que sigue un actor con el sistema para alcanzar un objetivo. No es lo mismo que un caso de uso.

- **Caso de uso**
	Un caso de uso es una actividad que un actor puede llevar a cabo con el sistema para obtener un resultado. Engloba uno o más escenarios.

	Permiten:
	1. Obtener requisitos fácilmente.
	2. Crear tests.
	3. Elaborar manuales de usuario y documentación.

	Para identificar los casos de uso, primero se deben identificar a los actores. Una vez se listan, se pueden enumerar las funcionalidades que ofrece el sistema y definir las interacciones con los actores. 

	Después, para ver los escenarios se puede partir de un escenario general e identificar el resto, viendo qué actores están involucrados en cada uno.

	Además se deben identificar los eventos externos a los que tiene que responder el sistema y relacionaros con los actores y casos de uso específicos. Para ello se usa el diagrama de contexto.

	Los casos de uso contienen:
	- Nombre breve y descriptivo.
	- Descripción breve.
	- Condición de inicio.
	- Precondiciones.
	- Postcondiciones.
	- Secuencia de pasos que muestra las interacciones entre el actor y el sistema.
	- Excepciones.

	Los casos de uso se pueden validar mediante:
	- Pruebas de aceptación.
	- Representaciones.
	- Revisiones con el cliente.
	- Mockups o prototipos.

	Deben evitarse:
	- [c] Demasiados casos de uso.
	- [c] Casos de uso muy complejos.
	- [c] Casos de uso que incluyen decisiones de diseño.
	- [c] Casos de uso que los usuarios no entiendan. 

**UML PARA CASOS DE USO**
****
Un diagrama de casos de uso captura todos los casos de uso y actores de un sistema y los relaciona.

Los actores se representan con un muñeco y los casos de uso dentro de una elipsis. Se deben nombrar con infinitivos y un ID. El sistema se representa como un rectángulo que rodea los elementos necesarios.

- **Relaciones**
	Las relaciones se representan con una línea continua. Las generalizaciones se representan con una flecha acabada en un triángulo vacío.

	Las relaciones extends son aquellas en las que el caso de uso extiene un comportamiento de otro cuando se cumplen ciertas condiciones.
	![[Pasted image 20240526205021.png]]

	Las relaciones include son aquellas en las que el caso de uso depende del caso base para que pueda ser ejecutado.
	![[Pasted image 20240526205120.png]]

- **Diagrama de actividad**
	Los diagramas de actividad muestran los pasos que hay que seguir en ciertas acciones del sistema. Pueden englobar a más de un caso de uso.
	![[Pasted image 20240526205229.png]]
