---
Name: 4 - Pruebas de integración de la capa modelo
tags:
  - teoría
asignatura: ISD
---
***[[Internet y Sistemas Distribuidos]]***

**INTRODUCCIÓN A LAS PRUEBAS DE INTEGRACIÓN**
****
***Independencia entre casos de prueba***
Es recomendable que cada caso de prueba cumpla ciertos aspectos:
- Crea los datos que necesita.
- Invoca la operación que quiere testear.
- Realiza comprobaciones necesarias.
- Elimina datos generados.

Así se consigue que cada caso de prueba sea independiente. Para evitar redundancias definimos métodos privados.

***Uso de bases de datos***
Es una buena práctica tener varias bases de datos:
- Para probar en la fase de desarrollo (`ws`).
- Para las pruebas automatizadas (`wstest`).
- Para producción.

***JUnit5***
>Las pruebas de unidad se centran en verificar el correcto funcionamiento de unidades individuales de código. 

>Las pruebas de integración se centran en verificar que las unidades de código se combinan correctamente y funcionan adecuadamente como un conjunto más grande.

- [i] Se deben hacer test de integración.

1. *AssertEquals*
	Compara dos objetos utilizando `equals`. Por defecto compara si dos objetos apuntan al mismo sitio. Para que haga comparaciones por contenido, se redefine el método `equals` y el `hashcode`.
2. *Inicialización*
	En los test se usan las variables `movieService` y `saleDao`, que se deben definir antes de nada con la notación `@BeforeAll`.
	```java
	@BeforeAll  
	public static void init() {  
		DataSource dataSource = new SimpleDataSource();  
	    DataSourceLocator.addDataSource(MOVIE_DATA_SOURCE, dataSource);  
	    movieService = MovieServiceFactory.getService();  
	    saleDao = SqlSaleDaoFactory.getDao();  
	}
	```

**DATASOURCELOCATOR**
****
***DataSources***
`MovieService` debe invocar un `DataSource` antes de poder invocar al servicio de la capa modelo. Cuando la capa modelo se ejecuta dentro de un servidor de aplicaciones, es este quien proporciona una implementación de `DataSource` mediante un pool de conexiones.

Una aplicación instalada en un servidor de aplicaciones Java puede obtener referencias a objetos mediante la API *JNDI (Java Naming and Directory Interface)*.

***DataSourceLocator***
Proporciona un mecanismo para gestionar fuentes de datos de manera centralizada, permitiendo agregar fuentes de datos y obtenerlas por su nombre, ya sea desde un mapa en memoria o buscándolas en el entorno JNDI si no están en el mapa.