---
Name: 2 - Capa Acceso a Datos
tags:
  - teoría
asignatura: PA
---
***[[Programación Avanzada]]***

**INTRODUCCIÓN A JPA**
***
***JPA E HIBERNATE***
> JPA son las siglas de Java Persistence API. Es una API de Java que funciona como mapeador objeto-relacional, coge entidades y las mapea a la base de datos.
   El paquete principal que usa es `jakarta.persistence`.

Usaremos Hibernate, una implementación de JPA. Por dentro utiliza JDBC.

Proporciona:
- Anotaciones que permiten mapear entidades a una base de datos relacional.
- Una API para interactuar con la BBDD.

***SPRING DATA***
> Librería que permite implementar ágilmente DAOs sobre diferentes fuentes de datos, como bases de datos, relaciones o NoSQL. En Spring se suelen llamar repositorios a los DAOs.

***CREACIÓN DE TABLAS CON HIBERNATE***
Hibernate permite crear automáticamente las tablas. Sobre las entidades se pueden utilizar anotaciones para indicar a qué tablas se mapean.
También permite adaptarse a las tablas que ya existen. 

1. *Tipos de mapeo*
	- Con anotaciones.
	- En un fichero de configuración.
	- Combinando las dos anteriores.
2. *Creación de tablas*
	En PA Shop las tablas se crean en `backend/src/sql/1-MySQLCreateTables.sql`.

**SPRING BOOT**
****
> Spring Boot es un proyecto de Spring, un framework que simplifica el desarrollo de las distintas capas de backend.

Permite principalmente:
- Creación de starter poms.
- Reducción de la cantidad de configuración de las librerías 
	(`application.{properties.yml}`).

***STARTER POMS***
Los frameworks de Spring que se usen y otras librerías se declaran en ficheros XML. Todas las versiones que se indican deben ser compatibles entre sí. Spring Boot permite automatizar esto. 

**MODELADO DE ENTIDADES**
****
Una clase entidad se crea al anotarlo con `@Entity`. Necesita un constructor sin argumentos `public` o `protected` y no puede ser `final`. Necesita un atributo que actúa como clave primaria y permite relaciones de herencia o asociación con otras entidades.

***ESTADO PERSISTENTE***
Las entidades Hibernate tienen estado persistente. Estos son el conjunto de atributos de la clase que se mapearán en columnas. Son de tipos Java que tengan equivalencias en SQL.

***ACCESO AL ESTADO***
El mapeador objeto-relacional puede acceder al estado persistente o, lo que es lo mismo, a las propias tablas de dos formas.
1. *Tipo de acceso campo*: se accede a través de los atributos.
2. *Tipo de acceso propiedad*: se accede mediante `getter` y `setter`.

***ANOTACIONES DE HIBERNATE***
Para mapear mediante anotaciones se usa:
- `@Table`: por defecto una entidad se mapea a una tabla con el mismo nombre simple de la clase. Si queremos un nombre distinto debemos usar esta anotación.
- `@Column`: cada atributo se mapea a una columna con el mismo nombre. Cuando queremos un nombre distinto debemos usar esta anotación.
- `@Id`: especifica el atributo clave primaria. Sólo sirve para claves simples, aunque existen anotaciones para claves compuestas.
- `@GeneratedValue`: se puede usar con `@Id` si la clave es numérica y queremos que se genere automáticamente. `GenerationType.IDENTITY` asume que la tabla usa columnas contador. Si se usase una secuencia usaríamos `GenerationType.SEQUENCE` con `@SequenceGenerator`.

***PATRÓN DOMAIN MODEL***
> Las entidades a veces contienen métodos de negocio que pueden facilitar la implementación de capas superiores. Esto se conoce como patrón Domain Model.

Si uno de estos métodos empieza por `get` o `is`, debemos decirle al mapeador que no es una entidad persistente. Esto se hace con `@Transient`.

**MODELADO DE RELACIONES**
****
Se pueden modelar relaciones 1:1, 1:N o M:N. Pueden ser unidireccionales o bidireccionales, según el sentido de la navegación. Si solo podemos navegar de una clase a otra, es unidireccional. Si podemos navegar entre ambas es bidireccional.

Un truco que se suele usar con las relaciones M:N es utilizar relaciones 1:N con una entidad intermedia. No se considera buena práctica tener relaciones M:N.

Las entidades en una relación actúan como:
- *Propietario*: entidad que contiene la clave foránea.
- *Inverso*: en las relaciones bidireccionales es el otro lado. En las relaciones unidireccionales no existe el inverso.

***RELACIONES 1:N***
Se anotan los `getters` que permiten navegar de una entidad a otra con `@ManyToOne` en el atributo con cardinalidad N y `@OneToMany` en el atributo con cardinalidad 1.
Se suele usar `Set` en este último atributo para devolver una colección de entidades.

En relaciones bidireccionales se utiliza `mappedBy` en el lado inverso. Especifica el atributo del otro lado de la relación (nombre del método `getter` sin `get` y la primera letra en minúscula: `getItem -> item`).

La anotación `@JoinColumn` se coloca sobre el `getter` de la entidad propietaria de la relación. En una relación 1:N, el lado propietario siempre es N, y en las 1:1 el lado 1, ya que el otro siempre es 0..1. Especifica el nombre de la columna que actúa como clave foránea.

En la anotación `@ManyToOne`, la anotación `optional=false` indica que ese método nunca puede devolver nulos. En caso de `optional=true` indicaría cardinalidad 0..1.

`@OneToMany` no tiene el parámetro `optional`. No tendría sentido ya que es normal que a veces no existan elementos del lado N. No se devolvería nulo, sino conjunto vacío.

***RELACIONES 1:1***
Es idéntico a las relaciones 1:N, pero usando `@OneToOne` en ambos lados. Permite también especificar `optional`.

***RELACIONES UNIDIRECCIONALES***
Cuando se modela una relación como unidireccional, solo se anota el `get` del lado propietario, ya que no tienen lado inverso. No tendría sentido hacer `category.getProducts()` ya que podría devolver muchos productos.

![[Pasted image 20250523202432.png]]

***POLÍTICAS DE RECUPERACIÓN***
1. *Política eager*
	> Para cada datos objeto se recuperan todos los datos.
	
	JPA sigue esta política si una entidad tiene una relación `@xxxToOne` con otra.
	Al recuperar una entidad, también se recuperan las relacionadas si no estaban cargadas en memoria.
2. *Política lazy*
	> Para evitar sobrecargas de memoria.
	
	Consiste en inicializar con un proxy las entidades correspondientes a los atributos anotados con `@xxxToOne`. Esto hace que inicialmente solo contengan la clave.
	Cuando se invoca una operación sobre el proxy diferente a `getId`, este lanza una consulta para recuperar el valor del resto de atributos.
	JPA usa por defecto la política lazy para relaciones `@xxxToMany`. Cuando se itera o se invoca una operación sobre la colección de valores se lanza una consulta para recuperar valores de las instancias relacionadas.

**DESARROLLO DE DAOs**
****
Spring Data permite desarrollar DAOs para diferentes fuentes de datos. Las interfaces no revelan ningún detalle de implementación. En la mayoría de casos, Spring Data implementa los DAO en tiempo de ejecución.

***OPERACIONES CRUD***
Todos los DAO extienden de `CrudRepository`. Define operaciones de `Create`, `Read`, `Update` y `Delete`. Sin embargo, algunos DAOs pueden necesitar extender de otras interfaces.

***OPERACIONES DE BÚSQUEDA***
Se pueden definir métodos de búsqueda con convenciones de nombrado. Se usan los prefijos `findBy` y `existsBy`. Buscan por el primer parámetro que reciben en notación CamelCase. Al usar `findBy`, se puede devolver un `Optional` si se sabe que puede devolver ninguna o una instancia como mucho.

- *Paginación de búsquedas*
	Los métodos reciben un parámetro `Pageable`, que especifica el rango de resultados que puede sacar de la base de datos. Además devuelven los datos con un valor de tipo `Slice` que indica si todavía hay más datos.
	
	Es importante que la paginación siga un tipo de orden. La implementación por debajo utiliza la API de JDBC para informar a la BBDD que solo devuelva las filas que se encuentran en `page*size` y `page*size+size-1`.

***JPQL***
Cuando las convenciones de nombrado no son viables, podemos usar JPQL. Es un lenguaje declarativo que permite lanzar consultas en JPA. También se puede hacer mediante la API criteria.
1. *Sintaxis*
	Es parecido a SQL. No utiliza nombre de tablas y columnas, sino entidades y atributos.
	```sql
	select u from User u where u.userName=:userName
	
	-- :userName es un parámetro nombrado. Funciona similar a `?` en SQL. JPA permite sustituirlo por un valor antes de lanzar la consulta. 
	```
	Al lanzar una consulta:
	1. Busca el nombre de la tabla y de las columnas a las que referencian los atributos.
	2. Traduce JPQL a SQL.
	3. Ejecuta la consulta mediante JDBC.
	4. Recupera las filas y las devuelve como instancias de entidades.
	
2. *Querys*
	Spring Data JPA permite lanzar consultas de forma muy simple utilizando `@Query`. Se utiliza `?i` en ligar de parámetros nombrados, siendo `i` el parámetro número `i` el método. Empieza a contar en 1, no en 0.
	
	Esta notación no es válida en consultas dinámicas (búsqueda de palabras clave). En esos casos es necesario:
	1. Definir estas operaciones en una interfaz adicional, `CustomizedProductDao`.
	2. Implementar la consulta en `CustomizedProductDaoImpl` utilizando la API de JPA para lanzar la consulta.
	3. `ProductDao` extiende `CustomizedProductDao`.