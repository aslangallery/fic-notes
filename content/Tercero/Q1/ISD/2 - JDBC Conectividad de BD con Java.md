---
Name: 2 - JDBC Conectividad de BD con Java
tags:
  - teoría
asignatura: ISD
---
***[[Internet y Sistemas Distribuidos]]***

**INTRODUCCIÓN A JDBC**
****
> JDBC se encarga de acceder a BD relacionales.

El desarrollador trabaja contra los paquetes `java.sql` y `javax.sql`, los cuales contienen interfaces y algunas clases concretas.

Para poder conectarse a la BD y lanzar consultas, es necesario tener un driver adecuado, es decir, un fichero `.jar` que contiene la implementación de las interfaces de las APIs. Nuestro código nunca depende del driver, dado que siempre trabaja con los paquetes `java.sql` y `javax.sql`.

***Ejecución de sentencias***
1. *Interfaz Connection*
	El método `getConnection()` permite obtener conexión a la BD a través de una URL o un ID y una contraseña.
	```java
	Connection connection = ConnectionManager.getConnection()
	String queryString = "SELECT movieId, title, runtime FROM TutMovie";
	PreparedStatement preparedStatement = connection.prepareStatement(queryString);
	```
2. *Interfaz PreparedStatement*
	Contiene la consulta SQL parametrizada. Permite lanzar consultas de actualización (`executeUpdate`) o de lectura (`executeQuery`). El driver se encarga de formatear los datos.
3. *SQL Exception*
	Los métodos lanzan esta sentencia ante cualquier error.
4. *Interfaz ResultSet*
	Representa todas las filas que concuerdan con la búsqueda.
	```java
	while (resultSet.next()) {
		String movieIdentifier = resultSet.getString(1);
		String title = resultSet.getString(2);
		short runtime = resultSet.getShort(3);
		
		System.out.println(
		"movieIdentifier = " + movieIdentifier 
		+ " | title = " + title 
		+ " | runtime = " + runtime
		);
	}
	```

***Liberación de recursos***
Para abrir y cerrar conexiones, los ejemplos anteriores usan `try-with-resources`. 
En caso de utilizar un `try` normal, habría que cerrar la conexión manualmente, redefiniendo `finalize` en la interfaz `Connection`.
```java
Connection connection = null;
try { 
	connection = ConnectionManager.getConnection(); 
	...
} catch (Exception e) { 
	e.printStackTrace(System.err);
 } finally {
	try {
		if (connection != null) { 
			connection.close(); 
		} 
	} catch (Exception e) {
		e.printStackTrace(System.err); 
	} 
}
```

**TIPOS DE SQL Y JAVA**
****
***Correspondencia de tipos***

| Tipo de Java         | Tipo de SQL             |
| -------------------- | ----------------------- |
| boolean              | BIT                     |
| byte                 | TINYINT                 |
| short                | SMALLINT                |
| int                  | INTEGER                 |
| long                 | BIGINT                  |
| float                | REAL                    |
| double               | DOUBLE                  |
| java.math.BigDecimal | NUMERIC                 |
| String               | VARCHAR/LONGVARCHAR     |
| byte[]               | VARBINARY/LONGVARBINARY |
| java.sql.Date        | DATE                    |
| java.sql.Time        | TIME                    |
| java.sql.Timestamp   | TIMESTAMP               |


**CONEXIONES**
****
***DataSources***
Interfaz que dispone del método `getConnection`. Se utiliza para pedir una conexión sin especificar URL, usuario o contraseña.

Se utiliza en servidores de aplicaciones y algunos frameworks, utilizando ficheros de configuración para indicar la URL, usuario y contraseña. Para implementarlo se utiliza `DriverManager.getConnection`.

***Pool de conexiones***
Para evitar cuellos de botella a la hora de recibir peticiones HTTP, se utiliza un pool de conexiones. Consiste en un conjunto de conexiones preestablecidas y listas para uso.
Se trabaja con la interfaz `DataSource`.
1. *ConnectionPool*
	Pide "n" conexiones a la BD con `getConnection` y se almacenan en una lista. Consta de los siguientes métodos:
	- `getConnection`: devuelve `ConnectionProxy` si queda alguno libre, sino deja durmiendo al thread que llama.
	- `releaseConnection`: devuelve la conexión a la lista y avisa a los threads que la esperan.
2. *ConnectionProxy*
	Proxy de conexión real que tiene los siguientes métodos:
	- `close`: libera la conexión.
	- `finalize`: lo llama si no se ha llamado ya a `close`.

***Caídas de la BD***
Si se cae la BD, todas las conexiones pool se invalidan.
Para comprobar que la conexión está viva, se usan 2 trucos:
1. Utilizar alguna función de la API del driver.
2. Lanzar una consulta poco costosa a la BD y comprobar que no se produce una `SQLException`.

**TRANSACCIONES**
****
>Secuencia de operaciones de BD que se ejecutan como una unidad atómica e indivisible.

Todas las operaciones dentro de una transacción se completan con éxito o se deshacen por completo, de modo que la BD se mantiene en un estado coherente y consistente. Se basa en las propiedades ACID.

Por defecto, una conexión se crea en `auto-commit` (cada consulta se ejecuta en su propia transacción). Para ejecutar varias en una sola, se hace el `commit` o `rollback` necesario cuando toque.

***Niveles de aislamiento***
1. *TRANSACTION_NONE*: transacciones no soportadas.
2. *TRANSACTION_READ_UNCOMMITED*: pueden ocurrir dirty reads, non-repeatable reads y phantom reads.
3. *TRANSACTION_READ_COMMITED*: pueden ocurrir non-repeatable reads y phantom reads.
4. *TRANSACTION_REPEATABLE_READ*: pueden ocurrir phantom reads.
5. *TRANSACTION_SERIALIZABLE*: elimina todos los problemas de concurrencia.

