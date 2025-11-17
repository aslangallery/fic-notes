---
Name: 3 - Capa Lógica de Negocio
tags:
  - teoría
asignatura: PA
---
***[[Programación Avanzada]]***

**SERVICIOS DE PA SHOP**
****
PA Shop ofrece 14 casos de uso agrupados en 3 servicios locales siguiendo el patrón fachada:
- *UserService*: registro de usuarios.
- *CatalogService*: búsqueda de productos.
- *ShoppingService*: gestión del carrito, compra y visualización de pedidos.

***USERSERVICE***
![[Pasted image 20250524110550.png]]

***CATALOGSERVICE***
![[Pasted image 20250524110609.png]]

***SHOPPINGSERVICE***
![[Pasted image 20250524110631.png]]

***PERMISSIONCHECKER***
> Permite verificar permisos de acceso.
   Por ejemplo, permite que un usuario solo pueda ver su propio carrito.


**INYECCIÓN DE DEPENDENCIAS**
****
> El principio de inyección de dependencias consiste en que los componentes de un sistema no crean directamente las otras partes de las que dependen, sino que se les pasan desde el exterior.

***INYECCIÓN DE DEPENDENCIAS EN SPRING***
1. *Uso de anotaciones*
	- `@Repository`: clases DAO en caso de que los DAOs no sean implementados por Spring Data.
	- `@Service`: servicio local.
	- `@Autowired`: inyecta un bean que implementa la interfaz del atributo. Si más de un bean implementa esa interfaz, lanza una `NoUniqueBeanDefinitionException`.
2. *Beans*
	> Singleton que forma una instancia única de cada servicio local o DAO.
	
	Al arrancar la aplicación, el contenedor de Spring busca las clases anotadas con `@Repository` o `@Service` y crea una instancia de ellas. Para cada atributo anotado con `@Autowired` crea un bean.

**TRANSACCIONALIDAD EN SPRING**
****
Spring permite usar un enfoque declarativo para implementar la transaccionalidad de los métodos de los servicios locales. En lugar de escribir manualmente el código para gestionar las transacciones, se puede hacer con anotaciones.

***ANOTACIÓN @TRANSACTIONAL***
> Permite especificar la transaccionalidad.

Puede usarse:
- *A nivel de clase*: se aplica a todos los métodos públicos.
- *A nivel de método*: sobrescribe la transaccionalidad indicada a nivel de clase.

1. *Uso de la anotación*
	Al invocar un método anotado con `@Transactional` dentro de una transacción, el método se une a ella. Si se hace desde fuera, se crea una nueva transacción.
	Las operaciones de los DAOs y servicios se unen a la transacción. Cuando esta termina puede lanzar:
	- *Commit*: si ha terminado bien o ha lanzado una excepción checked.
	- *Rollback*: si ha terminado con un `Error` o con `RuntimeException`.
2. *Operaciones de lectura*
	Se puede especificar `readOnly = true` para realizar optimizaciones.

***SISTEMA DE CACHÉ***
> Almacena en memoria objetos recuperados de la BD durante la ejecución de consultas.

Si una entidad ha sido recuperada previamente de la BD y está almacenada en caché, las consultas posteriores que soliciten la misma entidad no necesitarán acceder a la BD nuevamente. La entidad será recuperada directamente desde caché, lo que puede mejorar significativamente el rendimiento de la aplicación al reducir la cantidad de consultas a la BD.

1. *Entidades estado dirty*
	Si dentro de una transacción se modifica una entidad en memoria, se dice que está en estado dirty hasta que se actualiza en BD. Sin embargo, si se hace desde dentro de una transacción, Hibernate se encarga de actualizar la BD antes de terminar la transacción. No es necesario llamar a `save` del DAO.
	```java
	@Override
	public User updateProfile(Long id, String firstName, String lastName, String email) throws InstanceNotFoundException {
	    // Recuperación del usuario
	    User user = permissionChecker.checkUser(id);
    
	    // Actualización del perfil del usuario
	    user.setFirstName(firstName);
	    user.setLastName(lastName);
	    user.setEmail(email);
    
	    // La transacción se completará automáticamente al finalizar el método
	    // Hibernate detectará que la entidad está en estado "dirty" y lanzará una sentencia de actualización en BD antes de finalizar la transacción
    
	    return user;
	}
	```
2. *Caché de primer nivel*
	Caché interna para almacenar en memoria las entidades recuperadas durante una transacción. Se asocia directamente con una instancia de la sesión de Hibernate y se limpia al finalizar la sesión o cuando se elimina explícitamente.
	
	No se comparte entre transacciones.
	```java
	// Ejemplo 2: Recuperar un producto y luego obtener su categoría
	public void example2(Long productId) {
	    // Recuperar el producto de la base de datos
	    Product product = productDao.findById(productId).get(); // Acceso a la base de datos (SELECT)
	    
	    // Obtener la categoría del producto
	    Category category = product.getCategory(); // Proxy inicializado al obtener la categoría del producto
	    
	    // Acceder a la ID y el nombre de la categoría
	    Long categoryId = category.getId(); // Acceso a la base de datos (inicialización del proxy)
	    String categoryName = category.getName(); // No provoca acceso a la base de datos (el proxy ya está inicializado)
	    
	    // Recuperar la misma categoría de la base de datos
	    // Como la entidad se recuperó previamente en esta sesión de Hibernate, se recupera de la caché de primer nivel
	    category = categoryDao.findById(categoryId).get(); // No provoca acceso a la base de datos (está en caché)
	}
	```
3. *Caché de nivel 2*
	Permite cachear entidades y resultados de consultas a nivel de servicio. Se puede activar en la configuración. Es la memoria que JPA pone a disposición de la aplicación para guardar objetos de uso frecuente y conseguir una mejora en el rendimiento.

**OPTIMISTIC LOCKING**
****
Cuando hay mucha carga, no es viable utilizar siempre un nivel de aislamiento `SERIALIZABLE`, es recomendable que sea `READ_COMMITED`. Si es mayor que este último, habrá demasiados bloqueos en las filas afectadas por una transacción para garantizar aislamiento.

Hibernate para ello asume que siempre se trabaja con un nivel de aislamiento menor a `READ_COMMITED`.

***CONSECUENCIAS DE UTILIZAR NIVELES INFERIORES***
- *Phantom reads*
	1. Una transacción realiza una consulta.
	2. Otra transacción inserta o elimina filas relevantes para la consulta.
	3. La primera transacción ejecuta la consulta de nuevo, obteniendo resultados diferentes.
- *Non repeatable reads*
	1. Una transacción realiza una consulta.
	2. Antes de que finalice, otra transacción realiza modificaciones relevantes.
	3. La primera transacción obtiene diferentes resultados para la consulta.
- *Second lost update*
	1. Dos transacciones leen los mismos datos, realizan cambios e intentan escribirlos en la BD.
	2. Si la segunda transacción sobrescribe los cambios de la primera antes de que esta haya confirmado sus cambios, se produce una pérdida de la actualización.

En el optimistic locking, en lugar de bloquear recursos de forma exclusiva mientras se realiza una operación, se permite que múltiples transacciones accedan y modifiquen los datos al mismo tiempo. Sin embargo, antes de confirmar los cambios, el sistema verifica si otros usuarios han modificado los mismos datos.

Permite evitar problemas de second lost update en un nivel de aislamiento `READ_COMMITED`. En JPA se puede hacer añadiendo un atributo numérico anotado con `@Version` y añadiéndolo a la tabla.

El valor lo proporciona el propio mapeador objeto-relacional. Cuando se hace una inserción de una entidad, se le da el valor 0. Cada vez que se actualiza una instancia de la entidad, se incrementa el valor de la versión en 1.

Internamente, `PreparedStatement.executeUpdate` devuelve el número de filas afectadas por la consulta. Si esto devuelve 0 filas (no coincide el número de versión), se lanza una `RuntimeException` y se hace rollback.

***VENTAJAS***
- [p] La estrategia es escalable porque no se realizan bloqueos sobre las filas que se leen en la transacción.
- [p] La excepción puede subir a capas superiores e informar al usuario de que no se ejecutó su operación. 

**PRUEBAS AUTOMATIZADAS**
*****
Cada caso de prueba se ejecuta en un método anotado con `@Test`. Los casos de prueba son independientes entre sí. Se pueden crear DAOs en las clases de prueba cuando sea necesario.

En el ejemplo se usan dos BD:
- *pa*: para probar la aplicación como desarrollador.
- *patest*: para ejecutar los tests.

***TESTS CON SPRING***
Gracias a la anotación `@SpringBootTest` podemos:
- Ejecutar las pruebas con JUnit5.
- Utilizar las características de Spring Boot.
- Inyectar beans en las clases de prueba.

1. *Estructura*
	```java
	@SpringBootTest
	@ActiveProfiles("test")
	@Transactional
	public class <<Service>>Test {
	    @Autowired
	    private <<Service>> service;
	    
	    @Autowired
	    private <<Dao>> dao;
	    
	    @Test
	    public void test<<UseCase>>() throws ... {
	        ...
	        assertXxx(...);
	    }
	    ...
	}
	```
	Se pueden tener múltiples perfiles con Spring. Para ello basta con crear el fichero `application-(perfil).{yml, properties}` asociado. La anotación `@ActivateProfile` permite cargar el perfil test definido en los archivos de configuración. Este perfil utiliza la BD `patest`. Al ejecutar el backend, se puede especificar el perfil que se considere.
	
	Con `@Transactional` a nivel de clase se asegura que cada caso de prueba vaya en una transacción junto a los DAOs y servicios que invoque. Por defecto la librería termina todos los métodos con rollback. 
2. *Equals y Hashcode*
	Existen muchas estrategias para redefinirlos. La más sencilla es que como cada prueba se ejecuta en una sola transacción y dentro de la transacción nunca se cargará más de una instancia de la misma entidad, podemos usar la igualdad referencial. Nunca habrá dos instancias duplicadas en memoria, por lo que no es necesario redefinir estos dos métodos.
