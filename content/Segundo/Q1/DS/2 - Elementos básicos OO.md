---
Name: 2 - Elementos básicos OO
tags:
  - teoría
asignatura: DS
---
***[[Diseño de Software]]***

- **Especificador de acceso**: definen desde dónde se puede acceder a un atributo
- **Modificadores**: definen atributos de clase y atributos constantes.
- **Valor inicial**: los atributos se inicializan automáticamente, pero se pueden definir si es necesario.

***ESPECIFICADORES DE ACCESO***
- **Público (public)**: visible para todas las clases.
- **Paquete (no se indica especificador)**: visible para todas las clases que se sitúan en el mismo paquete.
- **Protegido (protected)**: visible para las subclases y clases del mismo paquete.
- **Privado (private)**: atributos sólo visibles dentro de la propia clase.

Los atributos deben declararse privados y su acceso sólo debe ser posible a través de métodos públicos de lectura/escritura.
- **Control de acceso**.
- **Abstracción de la implementación**.
- **Limitar la propagación de modificaciones**.

***EJEMPLO***
```
Cliente cJuan = new Cliente ("Juan", 1000); //Cliente con mil euros
Cuenta c = cJuan.getCuenta(); //Obtenemos referencia a objeto privado
c.retirada(1000); //Como el objeto es mutable, lo modificamos

//Juan se queda compuesto y sin mil euros
System.out.println("Saldo de Juan = " + cJuan.getCuenta().getBalance());
```
- **Solución**: uso de clones
```
class Cuenta { 
	... 
	// Método constructor de copia
	public Cuenta (Cuenta c) { 
		this.balance = c.balance; 
	} 

class Cliente { 
	... 
	public Cuenta getCuenta() { 
		return new Cuenta (cuenta); 
	} 
}
```
- **Solución**: objetos inmutables
```
class Cuenta { 
	// Atributos 
	private int balance; 
	
	// Métodos constructores 
	public Cuenta (int cantidad) { 
		balance = cantidad; 
	} 
	// Métodos de acceso 
	public int getBalance() { 
		return balance; 
	} 
}
```

***PRINCIPIO DE DISEÑO***
Favorece la inmutabilidad sobre la mutabilidad en el desarrollo de objetos.

**¡¡IMPORTANTE!!**: String, BigDecimal... son INMUTABLES.

***MODIFICADORES DE ATRIBUTOS***
- **Atributos estáticos (static)**: los atributos pertenecen a la clase y no a una instancia en particular, pueden ser modificados sin que exista una instancia creada de la clase.
	-  **EJEMPLO**
	```
	public class Clase { 
		public static int i; 
		public static void main(String[] args) { 
			Clase c1 = new Clase(); 
			Clase c2 = new Clase(); 
			c1.i = 5; 
			c2.i = 10; 
			System.out.println("c1.i = " + c1.i); //10
			System.out.println("c2.i = " + c2.i); //10
			} 
		}
	```
- **Atributos finales (final)**: atributos constantes. Una vez asignado un valor, no es posible cambiarlo.

***DEFINICIÓN DE MÉTODOS***

- **Mensajes**: petición enviada a un objeto para que realice a una acción y que va acompañada de información adicional.
- **Métodos**: ante la llamada de un mensaje, ejecuto un método.

**TODOS LOS PARÁMETROS EN JAVA SE PASAN POR VALOR**
```
public class Parametros { 
	public static void manipularCuenta(Cuenta c1) { 
		c1.ingreso(500); 
		Cuenta c2 = new Cuenta(500); 
		c1 = c2; 
	} 
	public static void main(String[] args) { 
		Cuenta c = new Cuenta(1000); 
		manipularCuenta(c); 
		System.out.println("Saldo = " + c.getBalance()); //1500
	} 
}
```

***MODIFICADORES DE MÉTODOS***
- **Static**
	- Métodos que no se ejecutan sobre una instancia de una clase, sino sobre la clase en sí.
	- Al no pertenecer a una instancia, no pueden acceder a atributos y métodos de instancia ya que no existe el puntero **this**.
```
		public class Estaticos { 
			private int valor; 
			public static void metodoEstatico() { 
			//this.valor; //ERROR DE COMPILACIÓN
			Estaticos e = new Estaticos(); 
			e.valor = 5; // Acceso permitido 
			} 
		}
```
- **Abstract**: 
	- Métodos no definidos destinados a ser implementados por una subclase.
- **Final**: evita que un método sea sobrescrito. SI una clase es final, todos sus métodos son final.

***CONSTRUCTORES***
Son los métodos que se utilizan para crear e inicializar instancias.
- Deben llevar el mismo nombre de la clase.
- No deben devolver ningún valor.
- Se llaman usando el operador **new**.
Un método puede tener varios constructores, siempre que su número o tipo de parámetros sea distinto.

***TIPOS ENUMERADOS***
Es un tipo de datos que consiste en un conjunto de valores con nombre llamados elementos que se comportan como constantes en el lenguaje.
En un principio, Java no incluía tipos enumerados, por lo que se definían como constantes. Al ser clases, los enumerados aceptan que se le definan atributos, métodos y constructores.

Algunas ventajas de los tipos enumerados son:
- **Igualdad**: pueden hacerse comparaciones de igualdad usando == y equals.
- **Orden**: 
	- Implementan el interfaz *Comparable*.
	- El método *ordinal* devuelve el valor ordinal de cada enumerado.
	- El método *values* devuelve un array de enumerados.
- **Enumerados y Strings**:
	- Sobreescriben el método *toString*.
	- Tienen un método *valueOf* que hace lo contrario.

***REGISTROS***
Un registro es un tipo de clase restringida que se crea de forma sencilla y eficiente para actuar como portadora de datos.
Se definen con la palabra clave *record* especificando un nombre para el mismo y unos parámetros que determinan su estado.

Un registro añade **automáticamente**:
- Un **atributo privado** y **final** para cada componente del estado.
- Un **método público de acceso a dicho atributo** con el mismo nombre.
- Un **constructor público** al cual se le pasan los atributos especificados.
- Implementaciones para los métodos **equals** y **hashCode**.
- Implementación de **toString**.

Es posible añadir a los registros **atributos estáticos** y **métodos estáticos**.

Restricciones de los registros:
- No pueden extender a ninguna clase.
- No pueden declarar atributos de instancia a parte de los incluidos en su declaración.
- No pueden declararse abstractos.
- **SON INMUTABLES SUPERFICIALMENTE**.