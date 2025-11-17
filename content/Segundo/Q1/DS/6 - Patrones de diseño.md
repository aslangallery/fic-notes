---
Name: 6 - Patrones de diseño
tags:
  - teoría
asignatura: DS
---
***[[Diseño de Software]]***

***INTRODUCCIÓN A LOS PATRONES DE DISEÑO***
Un **patrón de diseño** es una solución general y reusable a un problema recurrente dentro de un determinado contexto. Típicamente muestran relaciones e interacciones entre clases y objetos.

**TIPOS DE PATRONES**
- **Patrones arquitectónicos**: organización estructural fundamental del software.
- **Patrones de diseño**: refine componentes y sus relaciones. Sus tipos son:
	- **Patrones creacionales**: crear instancias de objetos para hacer programas más flexibles y generales.
	- **Patrones estructurales**: cómo las clases pueden formar estructuras.
	- **Patrones de comportamiento**: aspectos relacionados con la comunicación entre objetos.
- **Antipatrones**: soluciones recurrentes negativas, nos permite identificar las malas soluciones a tiempo.

***PATRONES FUNDAMENTALES***
**PRINCIPIO "FAVORECE LA INMUTABILIDAD"**
Las clases deben ser inmutables, a no ser que haya una buena razón para hacerlas inmutables.
- Son objetos simples ya que no pueden cambiar de estado. Para asegurar que las invariantes se cumplen, solo hay que realizar los constructores correctamente.

**PATRÓN INMUTABLE**
Se encarga de diseñar clases inmutables. Reglas para hacer una clase inmutable:
1. No incluir métodos que modifiquen el estado del objeto: set...
2. Asegurarse de que la clase no puede ser extendida.
3. Declarar todos los atributos como *final*.
4. Declarar todos los atributos como *private*.
5. Evitar el acceso a componentes internos mutables.
```
public final class Punto{ //clase definida como final
	private final int x; //atributos privados y finales
	private final int y;
	
	public Punto(int x, int y){
		this.x = x;
		this.y = y;
	}
	
	public int getX(){ //método que no devuelve referencia a objetos mutables
		return x;
	}
	
	public int getY(){
		return y;
	}
	
	//Método que simula alterar el estado del objeto pero que en realidad devuelve una nueva instancia de dicho objeto.
	public Punto mover(int x, int y){
		return new Punto(this.x + x, this.y + y);
	}
}
```

**PATRÓN INSTANCIA ÚNICA**
Se utiliza para asegurarnos de que una clase tiene solo una instancia y proporcionar un punto global de acceso a ella. Problemas con el patrón:
1. Ofrecen un punto global de acceso a ella.
2. Llevan un estado con ellos que dura tanto como la ejecución del programa.
Estos problemas se minimizan si los *singleton* no guardan ningún estado.

***DISEÑOS ADAPTABLES A LOS CAMBIOS***
**PRINCIPIO "ENCAPSULA LO QUE VARÍA"**
Identifica los aspectos de tu aplicación que varían y los separa de aquellos que permanecen estables.

**PATRÓN ESTRATEGIA**
Patrón de comportamiento que se utiliza para definir una familia de algoritmos, encapsularlos y hacerlos intercambiables.
Problemas:
- Los algoritmos tienen que compartir interfaz, lo que obliga a pasar datos innecesarios.
- Hay muchas alternativas.

**PATRÓN ESTADO**
Modifica la conducta del objeto a cambiar su estado con transiciones claras. Permite añadir nuevos estados fácilmente.
Problemas:
1. Código menos compacto que la solución basada en sentencias condicionales.

**PATRÓN ESTRATEGIA VS PATRÓN ESTADO**
**Similitudes**
- La estructura de las clases es la misma.
- El funcionamiento es similar, un contexto delega en una estrategia/estado.

**Diferencias**
- El problema que tratan de resolver es distinto.
- El funcionamiento dinámico cambia:
	- Las estrategias son independientes entre sí y no interactúan entre ellas.
	- Los estados se conocen entre sí porque tienen que establecer transiciones entre ellos.

***PATRONES Y COLECCIONES DE OBJETOS***
**PATRÓN COMPOSICIÓN**
Es un patrón estructural que se utiliza para componer objetos en estructuras de árbol que representan jerarquías TODO-PARTE. Permite tratar uniformemente a los objetos y a las composiciones. La clave es una clase abstracta que representa al mismo tiempo a los elementos primitivos y a sus contenedores. Es un buen ejemplo de utilización conjunta de herencia y composición.

**PATRÓN ITERADOR**
Proporciona un modo de acceder secuencialmente a los elementos de un objeto agregado (colección de objetos) sin exponer su representación interna. Permite implementar varios recorridos con una interfaz uniforme para recorrer diferentes estructuras.
- **Clases anidadas**: clases definidas dentro de otra clase.
	- Clases anidadas estáticas.
	- Clases internas (no estáticas): tienen acceso a los elementos de la clase externa aunque hayan sido declarados privados.
- **Iteradores fail-fast**: abortan la operación cuanto antes.
- **Iteradores fail-safe**: evitan abortar, crean un clon de la colección y si hay modificación, la copia sigue intacta.

***DISEÑOS DÉBILMENTE ACOPLADOS***
**PRINCIPIO DE BAJO ACOPLAMIENTO**
Busca diseños entre objetos independientes para código más robusto y adaptable.
- **Acoplamiento**: medida de la dependencia que tiene una determinada clase de otras clases del sistema.

**PATRÓN OBSERVADOR**
Permite definir una dependencia "uno a muchos" entre objetos de tal forma que cuando el objeto cambie de estado, todos sus objetos dependientes sean notificados y actualizados automáticamente. Se pueden añadir observadores en cualquier momento y permite que los observadores y los observados varíen independientemente. Usa la clase *Observable*.
**Problemas**:
1. Observadores independientes pueden dar problemas al actualizar.
2. Comunicación observado-observador simple fuerza a decidir que ha cambiado en el observado.
**Modelos**:
- Modelo pull: sujeto manda notificación mínima y observadores deben descubrir qué cambió -> Ineficiente.
- Modelo push: sujeto manda info. detallada -> Más eficiente -> Crea dependencias

**PATRÓN ADAPTADOR**
Convierte el interfaz de una clase en otro esperado por los clientes. Permite que cooperen clases con interfaces compatibles. El patrón asume que no se pueden hacer cambios ni en el interfaz que esperan los clientes ni en el interfaz que provee la clase servidora. 

**PRINCIPIO DEL MÍNIMO CONOCIMIENTO (LEY DE DEMÉTER)**

>  **Ley de Deméter**
> "No hables con extraños... solo con tus amigos inmediatos"

Un objeto debe saber lo menos posible acerca de la estructura de otros objetos. En la OO, dentro de un método de un objeto sólo pueden mandar mensajes al propio objeto y lo que hay dentro de él. Menor acoplamiento y mayor reparto de responsabilidades.

**PRINCIPIO "TELL, DON'T ASK"**
Un código procedimental pide información y luego toma decisiones. El código OO dice a los objetos qué hacer. Mejora la abstracción y reduce el acoplamiento. Enviar instrucciones a un objeto es mejor que consultarlo para realizar acciones.
**Inconvenientes**: puede haber conflicto con el **Principio de Responsabilidad Única** si objetos se vuelven grandes.

**PATRÓN FACHADA**
Provee de un interfaz unificado para un conjunto de clases en un subsistema. La fachada actuaría como un interfaz de alto nivel que hará que el subsistema sea más fácil de utilizar. Se divide en:
- **Fachada**: conoce qué clases son las responsables de una determinada petición y delega dicha petición sobre esas clases.
- **Clases del subsistema**: implementan las funcionalidades del subsistema y reciben las peticiones desde la fachada, aunque desconocen su existencia.

***PATRONES CREACIONALES Y OTROS PATRONES Y PRINCIPIOS***
**PATRÓN MÉTODO FACTORÍA**
Define un interfaz para crear un objeto pero permitiendo que las subclases decidan qué objeto hay que crear.
Características:
- Después de una instrucción *new* siempre tiene que ir una clase concreta, no puede ir una clase abstracta o un interfaz.
- Incumpliría el Principio de Inversión de la Dependencia al trabajar con una clase concreta y no una clase abstracta.
- Si queremos abstraer el proceso de creación de los objetos y delegarlo en las subclases, necesitamos crear objetos dentro de métodos especiales denominados *métodos factoría*.

**PATRÓN CONSTRUCTOR**
Separa la construcción de un objeto complejo de su representación, de forma que un mismo proceso de construcción pueda crear diferentes representaciones.
- **Antipatrón constructor telescópico**: un constructor telescópico ocurre en aquellas clases que sobrecargan el constructor para permitir distintas formas de crearlas. Cada nueva versión del constructor añade nuevos parámetros a la anterior estirando los parámetros del constructor como un telescopio pirata.

**PATRÓN DECORADOR**
Asigna responsabilidades adicionales a un objeto dinámicamente, proporcionando una alternativa flexible a la herencia para extender la funcionalidad.

**PATRÓN COMPOSICIÓN VS PATRÓN DECORADOR**
- **Similitudes**: estructura de clases prácticamente la misma, funcionamiento similar (una composición/decorador delega en un componente, que desconoce, el cálculo de una determinada operación y añade algo de su parte).
- **Diferencias**:
	- Tratan de resolver problemas distintos:
		- Composición: tratar de hacer estructuras en forma de árbol en las que una determinada operación se delega a las superclases.
		- Decorador: añadir responsabilidades a un objeto dinámicamente.
	- Funcionamiento dinámico distinto:
		- Composición: alberga múltiples componentes a los que delega una información y agrega los resultados.
		- Decorador: decora a un único componente con una nueva responsabilidad cada vez, que se añade al comportamiento del objeto decorado.

**MÉTODO PLANTILLA**
Define el esqueleto de un algoritmo en una operación pero difiriendo algunos de los pasos a las subclases.
- Las subclases pueden cambiar ciertos aspectos del algoritmo, sin cambiar su estructura general.
- Debe su nombre a que utilizarlo es como cubrir una plantilla o un formulario.

**PATRÓN MÉTODO PLANTILLA VS PATRÓN ESTRATEGIA**
- **Similitudes**:
	- Se utilizan para implementar distintos tipos de algoritmos similares.
- **Diferencias**:
	- Estrategia: utiliza la delegación para escoger entre las distintas variantes de un algoritmo (los algoritmos persiguen el mismo objetivo pero pueden ser diferentes)
	- Método Plantilla: utiliza la herencia para variar determinadas partes de un algoritmo dado (no se puede variar el esquema general del algoritmo).

**PRINCIPIO DE HOLLYWOOD**
Consiste en desacoplar elementos de alto nivel y de bajo nivel que pueden trabajar juntos. El elemento de alto nivel mantiene el control y es el encargado de llamar a los elementos de bajo nivel cuando lo considera necesario. Los elementos de bajo nivel nunca llaman a los de alto nivel directamente. Es una forma de *inversión del control* como la **inversión de la dependencia**.

**PRINCIPIO DRY "DON'T REPEAT YOURSELF"**
Cada pieza de conocimiento debe tener una representación única, inequívoca y autorizada dentro de un sistema.

**PRINCIPIO KISS "KEEP IT SIMPLE, STUPID"**
La simplicidad debe ser mantenida como un objetivo clave del diseño y cualquier complejidad **innecesaria** debe ser evitada.

**PRINCIPIO YAGNI "YOU AREN'T GONNA NEED IT"**
Implementar los cambios cuando realmente se necesitan, no cuando sólo se prevea que se necesitarán.

**SOBRE-INGENIERÍA**
Hacer el código más flexible o sofisticado de lo necesario. Se hace para anticipar futuras necesidades, lo cual es arriesgado. Si las predicciones no son correctas, perdemos un tiempo valioso en hacer código innecesario.

**INFRA-INGENIERÍA**
Se refiere a código pobremente diseñado y es el problema más común del diseño. Los diseños incorrectos surgen por diversas causas.