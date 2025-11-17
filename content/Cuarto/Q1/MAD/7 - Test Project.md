---
Name: 7 - Test Project
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**ESTRUCTURAS**
****
Las estructuras involucradas en el proceso de test se identifican mediante ciertos atributos:
- `[TestClass]`
- `[TestMethod]`
- `[ClassInitialize()]`
- `[ClassCleanup()]`
- `[TestInitialize()]`
- `[TestCleanup()]`
- `[ExpectedException(typeof(<Exception>))]`

**ASSERT**
****
>Permite establecer el estado de finalización de una prueba

***Opciones***
- `AreEqual` / `AreNotEqual`
- `AreSame` / `AreNotSame`
- `IsTrue` / `IsFalse`
- `IsNull` / `IsNotNull`
- `IsInstanceOfType` / `IsNotInstanceOfType`
- `Fail`
- `Inconclusive`

**PRUEBAS DE UNIDAD**
***
***Ciclo de vida***
1. *Creación de una instancia*
	`unit test runner`
2. *Test runner crea una sesión de test*
	Obtiene por reflexión los atributos de test.
3. *Se crea/recupera un test context*
	Esta instancia existe durante la totalidad de la sesión de test.
4. *Se llaman los métodos de inicialización del `assembly` y de la clase*
	`AssemblyInitialize` y `ClassInitialize`

***Para cada método de test, se sigue este procedimiento***
1. Se recupera una instancia de la sesión de test (test fixture).
2. Se llama al método de inicialización (`TestInitialize`).
3. El método que se desea probar se invoca.
4. Se llama al método `testCleanup`.

***Una vez ejecutados los tests***
1. Se llama al método `classCleanup`
2. Se llama al método `assemblyCleanup`

- [i] Si durante cualquier fase del ciclo de vida de un test se produce una excepción, el test termina con un fallo.

**TIPOS DE PRUEBAS**
****
>Prueban la mínima cantidad de código posible.

1. *Automáticas*
	Ejecución no requiere intervención ni configuración adicional.
2. *Completas*
	Cubren toda la funcionalidad importante, incluyendo métodos privados y protegidos.
3. *Repetibles*
	Ejecuciones con los mismos datos, devuelven los mismos resultados.
4. *Independientes*
	Cada test es independiente de los resultados de tests previos y no debe afectar a tests posteriores.
5. *Eficientes*
	Rendimiento suficientemente bueno como para soportar ejecución de múltiples pruebas simultáneas.

