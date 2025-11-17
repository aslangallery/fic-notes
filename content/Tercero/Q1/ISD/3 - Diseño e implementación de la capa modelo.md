---
Name: 3 - Diseño e implementación de la capa modelo
tags:
  - teoría
asignatura: ISD
---
***[[Internet y Sistemas Distribuidos]]***

**CAPA MODELO, PRINCIPIOS DE DISEÑO Y CONSIDERACIONES PREVIAS**
****
- [i] Nos centramos en el ejemplo de Movies

***Capa modelo***
Permite los siguientes casos de uso:
1. Añadir películas.
2. Actualizar películas.
3. Eliminar películas sin compra.
4. Buscar películas por clave.
5. Buscar películas por palabras clave del título.
6. Comprar películas.

Está formada por dos entidades:
```java
Película(ID, título, duración, descripción, precio, fecha_de_alta)
Venta(ID, ID_película, ID_usuario, fecha_de_expiración,
	  número_de_tarjeta, precio_película, URL_streaming, fecha_de_venta)
```

***Principios de diseño***
Ofrece una API con las siguientes características:
1. Permite invocar cada caso de uso de forma sencilla.
2. Oculta detalles de implementación. Tiene que poder modificarse sin afectar a la capa superior.

***Consideraciones previas***
1. *Errores lógicos*
	Son errores del usuario final. La capa modelo notifica esto lanzando excepciones de tipos checked (hijas de `Exception`). Se capturan en la interfaz de usuario y se muestran mensajes de error para que el usuario corrija los datos.
	- `InstanceNotFoundException`: hacer algo sobre un objeto que no existe.
	- `InputValidationException`: información de entrada en formato erróneo.
2. *Errores graves*
	Representan un error en la infraestructura, como caída de la BD, acceso a una tabla inexistente, etc. Se capturan con `RuntimeException` y se alerta al usuario.

**MODELADO DE ENTIDADES**
****
Se definen 2 entidades, cada una formada por atributos privados que modelan el estado, getters y setters. 

```java
public Movie(Long movieId, String title, short runtime, String
			 description, float price, LocalDateTime creationDate) { 
	this(movieId, title, runtime, description, price);
	this.creationDate = 
		(creationDate != null) ? creationDate.withNano(0) : null; 
} 

public void setCreationDate(LocalDateTime creationDate) {
	this.creationDate = 
		(creationDate != null) ? creationDate.withNano(0) : null; 
}
```

**DEFINICIÓN DE API MODELO**
****
***Pasos***
1. Agrupar los casos de uso de forma lógica.
2. Definir una interfaz por cada grupo. Cada caso de uso equivale a un método. Cada interfaz es una aplicación del patrón fachada (`MovieService`).

***Ventajas de la API modelo***
- Al definirse como una interfaz permite tener implementaciones alternativas.
- En una fase temprana de desarrollo podría presentar una implementación ficticia que no accede a la BD o que los métodos no hacen nada.
- Permite al resto del equipo de desarrollo encargarse de otras cosas.

**GESTIÓN DE LA PERSISTENCIA**
****
Para guardar películas y ventas se usa una BD en la que cada instancia se guarda en una fila de la tabla que corresponda.

Para representar los casos de uso puede resultar necesario tener operaciones CRUD (create, read, update, delete) u otras.

Para facilitar la implementación, se debe realizar una abstracción que permita gestionar la persistencia de cada entidad. Este patrón de diseño se llama **DAO** (Data Access Object). Conceptualmente es la capa de acceso a datos.

***Interfaz DAO***

| << Interface >> SqlMovieDao                                             |
| ----------------------------------------------------------------------- |
| create(connection: Connection, movie: Movie): Movie                     |
| find(connection: Connection, movieId: Long): Movie                      |
| findByKeywords(connection: Connection, keywords: String): List< Movie > |
| update(connection: Connection, movie: Movie): void                      |
| remove(connection: Connection, movieId: Long): void                     |
***Generación dinámica de claves***
Existen varias estrategias para la generación de claves numéricas:
- Mecanismos ofrecidos por las BD.
- Uso de algoritmos.

Las claves tipo String suelen ser concatenaciones de varios campos.
Cada BD suele tener un soporte diferente para la generación. En el caso del ejemplo, es mediante columnas contador (permite asignar automáticamente valores únicos y crecientes a estas columnas al insertar nuevos registros en una tabla).
```sql
CREATE TABLE Movie ( movieId BIGINT NOT NULL AUTO_INCREMENT,...);
```

**IMPLEMENTACIÓN DE LOS CASOS DE USO**
****
La implementación corresponde a la capa lógica de negocio. Debemos tener en cuenta:
- Gestión de transacciones.
- Obtención de referencias a `DataSource`.
- Obtención de referencias a DAOs.
- Obtención de referencias al propio servicio desde el cliente.

***Gestión de transacciones e implementación***
Para casos de uso que solo ejecuten una operación de lectura contra la BD:
- `autocommit`
- Nivel de aislamiento por defecto.

En el resto de casos:
1. Validar datos de entrada.
2. Empezar transacción serializable.
3. Comprobar que es posible ejecutarlo.
4. Si no es posible: `commit + checked`.
5. Implementar la capa lógica de negocio.
6. Si hay un error grave: `rollback`.
7. En cualquier otro caso: `commit`.

***DataSources***
Es necesario obtener una referencia a un DataSource:
- En las pruebas de integración de la capa modelo. 
- Cuando se ejecuta la capa modelo dentro de una aplicación/servicio web.

Para ello se crea la clase `DataSourceLocator`:
```java
public static final String MOVIE_DATA_SOURCE = "ws-javaexamples-ds";
dataSource = DataSourceLocator.getDataSource(MOVIE_DATA_SOURCE);  
```

***Referencias a DAOs***
Para poder tener referencia a un dato independientemente de su implementación, usamos el patrón factoría.

```java
movieDao = SqlMovieDaoFactory.getDao();  
saleDao = SqlSaleDaoFactory.getDao(); 
```

```java
public class SqlMovieDaoFactory {  
  
    private final static String CLASS_NAME_PARAMETER = "SqlMovieDaoFactory.className";  
    private static SqlMovieDao dao = null;  
  
    private SqlMovieDaoFactory() {  
    }  
    
    @SuppressWarnings("rawtypes")  
    private static SqlMovieDao getInstance() {  
        try {  
            String daoClassName = ConfigurationParametersManager  .getParameter(CLASS_NAME_PARAMETER);  
            Class daoClass = Class.forName(daoClassName);  
            return (SqlMovieDao) daoClass.getDeclaredConstructor().newInstance();  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }   
    }  
  
    public synchronized static SqlMovieDao getDao() {  
        if (dao == null) {  
            dao = getInstance();  
        }  
        return dao;  
    }  
}
```

Las factorías tratan a los DAOs como singletons (objetos sin estado que pueden ser usados por múltiples threads). 