---
Name: 3 - Propiedades básicas OO
tags:
  - teoría
asignatura: DS
---
***[[Diseño de Software]]***

***ABSTRACCIÓN, ENCAPSULAMIENTO Y MODULARIDAD***
- **Abstracción**: representación de las características fundamentales de algo sin incluir antecedentes o detalles irrelevantes.
- **Encapsulamiento**: ocultar al exterior detalles de la implementación que no es necesario conocer para su uso.
- **Modularidad**: propiedad que tiene un sistema que ha sido descompuesto en un conjunto de pares o módulos que sean cohesivos y débilmente acoplados. La modularidad se organiza de forma lógica en clases y paquetes y de forma física mediante ficheros y directorios.

Los paquetes son unidades lógicas de agrupación de clases. Las clases que no son públicas solo son visibles dentro del mismo paquete. El nombre de una clase es siempre el nombre de su paquete + un punto + el nombre de la clase.

**Formas de importación**:
- Importar una clase concreta: `import graficos.Circulo;`
- Importar todas las clases públicas de un paquete:  `import graficos.*;`
No es necesario incluir la sentencia **import** para dar privilegios de acceso a un paquete a otro.

***COMPOSICIÓN Y HERENCIA***
Una jerarquía es una clasificación de las abstracciones y existen dos tipos:
- **Composición**: define relaciones TIENE_UN y ocurre cuando un elemento contiene otros elementos.
- **Herencia**: relación ES_UN entre clases, en las que una clase hereda la estructura y el comportamiento definidos en una o más clases. Una subclase hereda de una o más superclase y aumenta o redefine la estructura y el comportamiento de dichas superclases. Se hace con la clausula **extends**.
	- **Herencia simple**: cada clase tiene, como mucho, un ancestro.

***CLASES ABSTRACTAS***
Son clases que no pueden ser instanciada y que está destinada a ser extendida por herencia. En Java se crea anteponiendo *abstract* a *class*.

- **Método abstracto**: método en el que solo se define su interfaz pero no su implementación. Un método abstracto solo puede pertenecer a una clase abstracta, pero una clase abstracta puede tener métodos **NO abstractos**. Las subclases de una clase abstracta deben implementar los métodos abstractos o declararse como abstractas.

***INTERFACES***
Son clases abstractas "puras" que definen un protocolo de conducto pero no cómo debe implementarse dicha conducta.
- Se definen con la palabra *interface*.
- Contienen las cabeceras de métodos que son implícitamente *public* y *abstract*.
- Pueden contener atributos pero serán implícitamente públicos, estáticos y finales.
- Una interfaz puede heredar de otros interfaces, pero nunca de una clase.

Implementar un interfaz consiste en darle una implementación concreta a todos y cada uno de los métodos que contiene.

**USOS DE LOS INTERFACES**
1. Revelar el interfaz de programación de un objeto sin revelar su clase.
2. Capturar las similitudes entre clases no relacionadas.
3. Definir nuevos tipos de datos.

Los tipos enumerados en Java pueden definir métodos abstractos siempre y cuando se dé una implementación a dichos métodos dentro de cada una de las constantes.
```
public enum Animal{
	CAT{
		public String makeNoise(){
			return "MEOW!";
		}
	}
	DOG{
		public String makeNoise(){
			return "WOOF!";
		}
	}
	public abstract String makeNoise();
}
```
De la misma forma, un tipo enumerado puede implementar una interfaz
```
interface CanMakeNoise{
	String makeNoise();
}

public enum Animal{
	CAT{
		public String makeNoise(){
			return "MEOW!";
		}
	}
	DOG{
		public String makeNoise(){
			return "WOOF!";
		}
	}
}
```


***HERENCIA VS COMPOSICIÓN***
**VENTAJAS HERENCIA**
- Lleva implícito el principio de sustitución.
- El código a escribir es menor.
- Permite el acceso a elementos protegidos.
- Las relaciones entre clases son más públicas.

**VENTAJAS COMPOSICIÓN**
- La composición indica con claridad cuales son las operaciones que podemos utilizar en la nueva estructura de datos definida: la composición permite no heredar métodos no deseados.
- La composición es más fácil de modificar.
- La composición no implica sustitución.
- La composición es una solución más general.


***POLIMORFISMO***
Capacidad de un objeto de pertenecer a más de una clase y de una función de ser aplicada sobre parámetros de distintas clases.
- **Coacción**: operación semántica por el cual se convierte un argumento al tipo por una función para evitar que se produzca un error de tipos.
- **Sobrecarga *(overloading)***: consiste en utilizar el mismo nombre para denotar métodos distintos diferenciándolos por el n.º o tipo de los parámetros. 
- **Sobreescritura**: ocurre cuando una clase hija le da una implementación más específica que la clase padre.
```
public class ClasePadre(){
	public void metodoX (int i, int j){
		System.out.println("Enteros " + i + "y" + j);
	}
}

public class ClaseHija extends ClasePadre {
	public void metodoX (String c){
		System.out.println("Cadena " + c);
	}
}
```
En este caso, hay 2 métodos: uno que acepta dos enteros y otro que acepta un String.

```
public class ClasePadre(){
	public int metodoX (int i, int j){
		return i + j;
	}
}

public class ClaseHija extends ClasePadre {
	public int metodoX (int i, int j){
		return i * j;
	}
	public static void main (String[] args){
		ClaseHija ch = new ClaseHija();
		System.out.println(ch.metodoX(5, 2)); //¿7, 10, no compila? -> 10
	}
}
```
Aquí se está haciendo una sobreescritura de métodos, no una sobrecarga. 

- **Sobreescritura de refinamiento**: los métodos de la superclase son accedidos desde la subclase usando la palabra clave *super*.
```
class Dog extends Animal{
	public void display(){
		super.display(); //llamada a la superclase
		System.out.println("I am a dog");
	}
}

class Main{
	public static void main(String[] args){
		Dog dog = new Dog();
		dog.display(); //I am an animal .. I am a dog
	}
}
```

**POLIMORFISMO DE INCLUSIÓN**
Polimorfismo que ocurre a través de las relaciones de herencia y mediante el cual una instancia de una subclase es también una instancia de sus superclases. 
```
class Polimorfismo {
	public static void main(String[] args){
		Perro p = new Perro(); //Perro hereda de Animal
		if(p instanceof Perro) System.out.println("p es Perro");
		if(p instanceof Animal) System.out.println("p es Animal");
		Animal a = new Perro();
		if(a instanceof Perro) System.out.println("a es Perro");
		if(a instanceof Animal) System.out.println("a es Animal");
	}
}
```
**Resultado:** "p es Perro", "p es Animal", "a es Perro" y "a es Animal".

**POLIMORFISMO PARAMÉTRICO (GENERACIDAD)**
Consiste en que una clase tiene uno o varios parámetros genéricos definidos. En la instanciación de dicha clase, se especificará qué valores concretos tienen dichos parámetros genéricos.
- **Clase genérica**: incluye un parámetro de tipo (T) que representa a un objeto cualquiera cuyo tipo real será especificado a la hora de crear instancias de dicha clase.
- **Parámetros de tipo**: pueden representar a cualquier tipo que no sea un tipo primitivo (clase, interfaz, array...). Por convención se representan con una sola letra: T(Tipo), K(Clave), E(Elemento), V(Valor), N(Número).
- **Tipos vinculados**: parámetro que tiene un límite superior. Solo puede ser instancia de de cualquier subclase de la clase límite, o una instancia de la propia clase límite.
- **Comodín**: tipo especial de parámetros que representa a un tipo desconocido en la declaración de tipos genéricos. <"?">

***TIPIFICACIÓN***
Un tipo es una caracterización precisa de las propiedades estructurales y de comportamiento que comparten una serie de entidades.
- **Objetivo principal**: noción de congruencia

**TIPADO ESTÁTICO O DINÁMICO**
- **Tipado estático**: la comprobación de tipos se hace en tiempo de compilación.
- **Tipado dinámico**: la comprobación de tipos se hace en tiempo de ejecución.
					**JAVA USA TIPADO ESTÁTICO**

**TIPADO FUERTE O DÉBIL**
- **Tipado fuerte**: las reglas de tipos son estrictas.
- **Tipado débil**: las reglas de tipos son más flexibles.
					**JAVA USA TIPADO FUERTE**

```
class Animal{} //no tiene método ladra()
class Perro extends Animal{
	public void ladra() { System.out.println("Guau"); }
	public static void main (String[] args){
		Animal unAnimal = new Perro();
		unAnimal.ladra();
	}
}
```
Este código da error de compilación ya que el método ladra() no está implementado en la clase Animal ni en ninguna de sus superclases.

**TIPADO DE PATO (*Duck Typing*)**
La comprobación de tipos se encarga de comprobar que el objeto en cuestión tiene los aspectos deseados y no de qué tipo se trata.


***LIGADURA DINÁMICA***
Proceso que se encarga de ligar o relacionar la llamada a un método con el código que se ejecuta finalmente.
- **Ligadura estática**: realizar la ligadura en tiempo de compilación según el tipo declarado del objeto al que se manda el mensaje. En Java se usa en métodos privados o finales, que no se pueden sobrescribir.
- **Ligadura dinámica**: realizar la ligadura en tiempo de ejecución siendo la forma dinámica del objeto la que determina la versión del método a ejecutar. Se utiliza en métodos que no son ni privados ni final.

**ESENCIA DEL DISEÑO OO**
Usar las propiedades típicas de la OO (**herencia, polimorfismo, ligadura dinámica**) conjuntamente para obtener las ventajas que ofrece la OO: **flexibilidad, escalabilidad, extensibilidad, abstracción...**

```
abstract class Animal{
	public void hazRuido() { System.out.println("No hago ruido"); }
}
class Perro extends Animal{
	public void hazRuido() { ladra(); }
	public void ladra() { System.out.println("Guau"); }
}
class Pez extends Animal { //no redefine hazRuido() }

Animal a1 = new Perro();
Animal a2 = new Pez();
a1.hazRuido(); //"Guau"
a2.hazRuido(); //"No hago ruido"
```
```
class Padre{
	public static void metodoEstatico(){
		System.out.println("Padre");
	}
}
class Hijo {
	public static void metodoEstatico(){
		System.out.println("Hijo");
	}
}
class Estaticos{
	public static void main(String[] args){
		Padre p = new Hijo();
		p.metodoEstatico(); //"Padre" los métodos estáticos tienen ligadura estática
	}
}

class Padre{
	public final void metodoFinal(){
		System.out.println("Padre");
	}
}
class Hijo extends Padre{
	public final void metodoFinal(){
		System.out.println("Hijo");
	}
}
class Finales{
	public static void main(String[] args){
		Padre p = new Hijo();
		p.metodoFinal(); //"Error de compilación": los métodos finales no pueden ser sobrescritos.
	}
}
```
