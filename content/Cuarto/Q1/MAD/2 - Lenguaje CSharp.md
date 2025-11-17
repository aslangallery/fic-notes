---
Name: 2 - Lenguaje CSharp
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**INTRODUCCIÓN A C#**
****
>Lenguaje de programación orientado a objetos que se puede utilizar para desarrollar aplicaciones en .NET. El objetivo es reducir el tiempo de desarrollo, evitando detalles de programación de bajo nivel.

***Convenciones***
Los métodos y nombres de las clases se ponen en PascalCase. Los atributos se ponen en camelCase. C# es sensible a mayúsculas.

```csharp
using System;

namespace Es.Udc.DotNet.CSharpTutorial{
	class HelloWorld {
		public static void Main(){
			Console.WriteLine("Hello World!");
		}
	}
}
```

***Jerarquía de clases***
Funciona igual que Java, solo admite herencia simple, pero permite la implementación de múltiples interfaces.

*System.Object*
```csharp
public class Object { 
	public Object(); 
	
	public virtual bool Equals(object obj); 
	public static bool Equals(object objA, object objB); 
	public virtual int GetHashCode(); 
	public Type GetType(); 
	protected object MemberwiseClone();
	public static bool ReferenceEquals(object objA, object objB);
	public virtual string ToString();
}

/*cualquier clase puede sobreescribirlos*/
public override bool Equals(object obj) {
	Car car = (Car) obj;

	return (this.Doors == car.Doors) && (this.Wheels == car.Wheels);
}
```

**COMPILADOR DE C#**
****
Se encuentra en `C:\windows\Microsoft.NET\Framework\vn.n.nnn\csc.exe`.
La sintaxis para utilizarlo es `csc [options] [file1.cs, file2.cs, ..., fileN.cs]`.

***Opciones***
- `/out:outputFile`: permite cambiar el nombre del archivo al compilarlo.
- `/target:winexe`: permite generar una aplicación de Windows sin abrir una consola.
- `/target:library`: permite generar una librería DDL.
- `/reference:libraryFile`: permite agregar referencias a librerías necesarias para la compilación.
- `main:classFile`: permite especificar cuál es el Main principal de la aplicación.

**NAMESPACES**
***
>Jerarquía que se utiliza para agrupar y organizar el código para evitar conflictos de nomenclatura.

***Uso de Namespaces***
Pueden anidarse para mayor semántica y se puede hacer referencia a ellos con `Using`.

```csharp
namespace Worker.Tech {

	class Engineer {
	
		public static string SayHello(){
			return "Hi! I am the engineer.";
		}
		
	}
}

----------

using Worker;

namespace Work {

	class Order {
		
		public string PresentWorkers(){
			Tech.Engineer.SayHello();
		}

	}
}
```

***Alias***
Se puede simplificar el uso de namespaces mediante alias.
```csharp
using WE = Worker.Tech.Engineer;

namespace Work {

	class Order {
		
		public string PresentWorkers(){
			WE.SayHello();
		}

	}
}
```

**SISTEMA DE TIPOS UNIFICADOS**
****
***Paso por valor***
Se pasan por valor los datos de tipo:
- Tipos primitivos.
- Enumeraciones.
- Structs.

***Paso por referencia***
Se pasan por referencia los datos de tipo:
- String.
- Object.
- Clases.
- Interfaces.
- Arrays.

***Boxing y unboxing***
Los datos que se pueden pasar por valor se pueden convertir en datos que se pueden pasar por referencia.

```csharp

// boxing
int i = 3;
object obj = i;


// unboxing
int value = (int) obj;
```

**TIPOS PREDEFINIDOS**
****
Los tipos básicos son alias a tipos que proporciona el propio SO. Estos son:
- *Reference*: string y object.
- *Signed*: sbyte, short, int y long.
- *Unsigned*: byte, ushort, uint, ulong.
- *Character*: char.
- *Floating point*: float, double, decimal.
- *Logical*: bool.

***Sufijos y problemas con métodos del mismo nombre***
Si hay dos métodos que se llaman igual, pero reciben un parámetro de distinto tipo, por defecto se llama al valor más pequeño que permite almacenar el número.

```csharp
public static void M0 (uint x){}
public static void M1 (int x){} // Foo(1) llamaría a este método
public static void M2 (long x){}
```

Para evitar esto se pueden utilizar los sufijos:
- `L`: long, ulong.
- `U`: int, uint.
- `UL`: ulong.
- `F`: float.
- `D`: double.
- `M`: decimal.

Ahora una llamada `Foo(1L)` llamaría al método `M2`.

**OPERADORES DE C#**
****
Las operaciones son muy similares al de resto de lenguajes:
- *Aritméticos*: +, -, *, /, %.
- *Asignación*: +=, -=, *=, /=, %=, ++, --.
- *Comparación*: ==, !=, <, >, <=, >=.
- *Lógicos*: &, &&, |, ||, ~, !, ^.

***Información sobre tipos***
El operador `typeof` permite conocer el tipo de una variable.
```csharp
Type t = typeof(string);
Console.WriteLine(t); /*salida: System.String*/
```

El operador `is` verifica si un objeto es de un tipo específico.
```csharp
object obj = 123;
if(obj is int){
	Console.WriteLine("obj es un int.")
}
```

El operador `as` permite realizar un casting. Si falla, en lugar de devolver una excepción, devuelve `null`.
```csharp
object obj = "texto"; 
string str = obj as string;
if (str != null) {
	Console.WriteLine($"obj fue casteado a string: {str}"); // salida: obj fue casteado a string: texto
}
```

**ENTRADA Y SALIDA POR CONSOLA**
****
***Entrada***
Para introducir valores por consola se pueden usar:
- `Console.Read()`: lee un caracter y guarda el Unicode.
- `Console.ReadLine()`: lee una línea.
- `Console.ReadKey()`: lee información de la tecla que se presiona.

***Salida***
Para mostrar valores por consola se puede usar:
- `Console.Write()`: escribe sin salto de línea.
- `Console.WriteLine()`: escribe y añade al final un salto de línea.

***Formatos de salida***
- `C`: moneda.
- `D`: decimal.
- `F`: punto fijo.
- `P`: porcentaje.
- `H`: hexadecimal.

**SENTENCIAS**
****
***Sentencias condicionales***
- Sentencia `if-else`
	```csharp
	int num = 6;
	
	if (if num < 10) {
		Console.WriteLine("Es menor");
	} else {
		Console.WriteLine("Es mayor");
	}
	```
- Sentencia `switch`
	```csharp
	int num = 6;
	int x = 0;
	
	switch (num) {
		case 1: x += 1; break;
		case 6: x += 6; break;
		default: x += 10; break;
	}
	```

***Sentencias iterativas***
- Sentencia `while`
	```csharp
	while (1 == 1) {
		Console.WriteLine("Loop");
	}
	```
- Sentencia `do-while`
	```csharp
	do {
		Console.WriteLine("Loop");
	} while (1 == 1);
	```
- Sentencia `for`
	```csharp
	for (int i=0; i<10; i++) {
		Console.WriteLine("i = " + (i + 1));
	}
	```
- Sentencia `foreach`
	```csharp
	String[] days = {"Monday", "Tuesday", "Wednesday", "Thursday", "Friday"};
	
	foreach (string day in days) {
		Console.WriteLine(day);
	}
	```

**CLASES EN C#**
****
***Constructores***
Todas las clases tienen uno, pero no es obligatorio definirlo. El constructor por defecto no contiene parámetros y lo genera el compilador.

Para llamar a otro constructor de la clase padre se puede usar el inicializador `base`.
Otro inicializador es `this`, que permite llamar a constructores de la misma clase para realizar sobrecargas.

```csharp
using System;

class SuperClass
{
    private string argument1;

    // Constructor sin parámetros, llama al constructor con parámetro
    public SuperClass() : this("<Default arg1>") { }

    // Constructor con un parámetro
    public SuperClass(string arg1)
    {
        this.argument1 = arg1;
        Console.WriteLine("Arguments: " + argument1);
    }

    public static void Main()
    {
        // Creando una instancia de SuperClass
        SuperClass sc = new SuperClass(); 
        // Muestra: Arguments: <Default arg1>

        // Crear una instancia de ChildClass sin parámetros
        ChildClass cc = new ChildClass(); 
        // Muestra: Arguments: <Default arg2>

        // Crear una instancia de ChildClass con dos parámetros
        ChildClass cc2 = new ChildClass("1", "2");
        // Muestra: Arguments: 1
        // Muestra: Arguments: 2

        Console.ReadLine();
    }
}

class ChildClass : SuperClass
{
    private string argument2;

    // Constructor sin parámetros, llama al constructor con un parámetro
    public ChildClass() : this("<Default arg2>") { }

    // Constructor con un parámetro
    public ChildClass(string arg2)
    {
        this.argument2 = arg2;
        Console.WriteLine("Arguments: " + argument2);
    }

    // Constructor con dos parámetros, llama al constructor de la clase base
    public ChildClass(string arg1, string arg2)
        : base(arg1)  // Llama al constructor de SuperClass con el primer argumento
    {
        this.argument2 = arg2;
        Console.WriteLine("Arguments: " + argument2);
    }
}
```

***Constructores estáticos***
Se puede inicializar código antes de crear la instancia de la clase, permite acceder a cualquier miembro estático de la clase. 
Solo puede haber uno por clase y no permiten parámetros.

```csharp
using System;
using System.IO;

public class LogManager
{
    private static FileStream logFile;

    static LogManager()
    {
        logFile = File.Open("logFile.dat", FileMode.Create);
    }

    public LogManager()
    {
        StreamWriter writer = new StreamWriter(logFile);
        writer.WriteLine(System.DateTime.Now.ToString() + " Instance created");
        writer.Flush();
    }
}
```

***Destructores***
Métodos que el recolector de basura llama automáticamente cuando ya no existen referencias a ese objeto. No pueden heredarse.
```csharp
~LogManager()
{
	if (logFile != null)
	{
		logFile.Close(); // Cierra el archivo
		Console.WriteLine("logFile cerrado en el destructor.");
	}
}
```

***Campos constantes***
Se evalúan en tiempo de compilación y se definen con `const`, siendo estáticas por defecto. Tienen que inicializarse al ser declaradas.
```csharp
public const int NUMBER = 1;
```
Se puede acceder a ellas desde código externo, por ejemplo `NumberClass.NUMBER`.

***Campos de solo lectura***
Se inicializan en tiempo de ejecución, por lo que pueden ser tanto estáticas como inicializadas en un constructor. Una vez inicializadas, no pueden modificarse.
```csharp
public readonly int NUMBER;
public static readonly int NUMBER2 = 2;

public NumberClass () {
	NUMBER = 1;
}
```

***Propiedades***
En .NET se pueden encapsular la declaración de los atributos, asó como los `getter` y `setter`, mediante un mecanismo llamado `properties`.
```csharp
public class Person
{
    private string name;  // property compleja

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int Age { get; set; }  // property simple
}
```

Se puede acceder a ellas como si fueran un campo público, sin necesidad de usar el `getter` o el `setter`, por ejemplo `person1.Age = 20`.

Pueden construirse `properties` de solo lectura usando `private`.

***Métodos***
Por defecto son `sealed`, no se pueden sobrescribir. Para poder redefinir un método debe ser `virtual` y en la definición se debe invocar a `override`. 

```csharp
public class Product {

	public virtual String GetRef() {
		return barCode;
	}
	
}

public class Book : Product {

	public override String GetRef() {
		return ISBN;
	}

}
```

***Clases abstractas***
>Fuerza a que se deriven en clases hijas para permitir su instanciación.

```csharp
public abstract class Animal
{
    public abstract void MakeSound();
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof!");
    }
}
```

***Clase sealed***
> Aquella que no permite ser extendida

```csharp
public sealed class Car
{
    public string Make { get; set; }
    public string Model { get; set; }

    public void Start()
    {
        Console.WriteLine("Car started.");
    }
}
```

**ESTRUCTURAS**
****
***Structs***
> Tipo especial de clases mucho más ligeras. Son un tipo de dato que se pasan por valor, no permiten herencia pero sí pueden implementar interfaces.

```csharp
public struct Point {

	public int x, y;
	
	public Point(int x, int y) {
		this.x = x;
		this.y = y;
	}
}
```

***Enums***
> Formados por constantes públicas y estáticas. Por defecto son de tipo `int` pero se puede cambiar por cualquier otro tipo básico que permita representar un entero. 
> Se les puede dar un valor, pero si no se hace el compilador les da valores incrementales.

```csharp
enum Semaphore {RED, GREEN, YELLOW}

public enum FontSize : int {
	SMALL: 12,
	NORMAL: 16,
	HUGE: 20
}
```

**INTERFACES**
****
Definen métodos, propiedades o eventos. No permiten campos, operadores, constructores o destructores. Las cosas definidas deben ser `public abstract` y ser implementadas en todas las implementaciones de la interfaz.

Proporcionan polimorfismo, ya que múltiples clases o estructuras pueden implementar la misma interfaz.

```csharp
public interface ILimpiador {
	void Barrer();
	void Fregar();
}

public class Limpiador : ILimpiador {

	public void Barrer() {
		Console.WriteLine("Barriendo...");
	}

	public void Fregar() {
		Console.WriteLine("Fregando...");
	}
}
```

**MODIFICADORES DE ACCESO**
****
***Tipos de accesibilidad***
- *private*: solo se puede acceder desde la misma clase o estructura.
- *protected*: solo se puede acceder desde la misma clase, estructura o derivados.
- *internal*: puede acceder a él cualquier elemento del mismo ensamblado.
- *protected internal*: puede acceder a él cualquier elemento del mismo ensamblado o derivados.
- *public*: puede acceder a él cualquier elemento del mismo ensamblado o con referencias a él.

**EXCEPCIONES EN C#**
****
> Mecanismo que permite gestionar errores inesperados.

No pueden ser ignoradas. No es obligatorio gestionarlas en el punto donde ocurren, pueden llevarse a capas superiores. Existen excepciones estándar ya definidas.

En C# no existen `throws`, por lo que se deben documentar con `xml`. Las excepciones son instancias de `System.Exception` que se pueden lanzar con `throw` para ser capturadas.

***Captura de excepciones***
Se utiliza `try-catch-finally`.
```csharp
public class Example
{
    public void Divide(int a, int b)
    {
        try
        {
            int result = a / b;
            Console.WriteLine("Resultado: " + result);
        }
        catch (DivideByZeroException)
        {
            Console.WriteLine("Error: No se puede dividir por cero.");
        }
        finally
        {
            Console.WriteLine("Bloque finally ejecutado.");
        }
    }
}
```

**SENTENCIA USING**
****
> Permite crear una instancia, emplearla y asegurar que tras ello se llama a `Dispose()`.

```csharp
public class MyResource : IDisposable 
{ 
	public void MyResource() { 
		// Acquire valuable resource 
	} 
	public void Dispose() { 
		// Release valuable resource 
	}
	public void DoSomething() { 
		// Do something 
	} 
} 

<<...>> 

static void Main() 
{ 
	using (MyResource r = new MyResource()) 
	{ 
		r.DoSomething(); 
	} // r.Dispose() is called 
}
```

**GENERICS**
****
> Característica del CLR que permite que las clases, structs, interfaces y métodos tengan parámetros de tipo genérico para los tipos de datos que almacenan y manipulan.

```csharp
public class Box<T>
{
    private T content;

    public void Add(T item)
    {
        content = item;
    }

    public T Get()
    {
        return content;
    }
}

// Ejemplo de uso:
// Box<string> stringBox = new Box<string>();
// stringBox.Add("Hello, World!");
// string result = stringBox.Get(); // result = "Hello, World!"
```

***Ventajas***
Se deben utilizar porque:
- Comprueban los tipos en tiempo de compilación.
- Tienen muy buen rendimiento.
- Reducen la complejidad del código.
