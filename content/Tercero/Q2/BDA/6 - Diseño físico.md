---
Name: 6 - Diseño físico
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**VISIÓN LÓGICA Y FÍSICA DE LOS DATOS**
****
- **Arquitectura ANSI/PARK**
	Conceptualización de la arquitectura de una base de datos que se basa en la separación en 3 niveles.
	![[ansi-spark.png]]

- **Niveles de diseño**
	Se suelen diferenciar los niveles físico y lógico en una base de datos.

	1. *Nivel lógico*
		Está compuesto por:
		- Tablas, filas y columnas.
		- Vistas.
		- Espacios lógicos de almacenamiento.
		- Lenguaje SQL declarativo.
	2. *Nivel físico*
		Está formado por:
		- Espacios lógicos y físicos de almacenamiento de datos.
		- Ficheros, extensiones, bloques y organizaciones de ficheros.
		- Índices.

**ALMACENAMIENTO DE DATOS**
****
- **Zonas lógicas y físicas**
	Una zona lógica de almacenamiento de datos es la abstracción de una o más zonas físicas de almacenamiento de datos. Al crear una tabla, se especifica la zona lógica, no la zona física. También se llaman `tablespace` en Oracle, MySQL... O `filegroup` en MS SQL Server.

	Las zonas físicas generalmente son ficheros del SO. También pueden ser otros dispositivos o particiones, gestores de volumen propios como Oracle ASM...

	```sql
	--EN ORACLE
	create tablespace ts1
	add datafile '/u01/data/ts1.dbf'
	size 200m autoextend on;

	create table emp(
		empno number(3)
		...
	) tablespace ts1;

	--EN MS SQL SERVER
	alter database bd1 add filegroup fg1;
	alter database bd1 add file
	( name = fg01, 
	filename = 'C:\Data\fg01.ndf',
	size = 200mb, filegrowth = 5mb)
	to filegroup fg1;

	create table emp(
		empno number(3)
		...	
	) on fg1; 
	```

- **Tablespace en Oracle**
	En Oracle, un `tablespace` está formado por ficheros de datos. Crea algunos de forma automática:
	- *SYSTEM*: catálogo del sistema, tablas y vistas administrativas, objetos compilados...
	- *SYSAUX*: sirve como apoyo de `SYSTEM` para componentes de Oracle.
	- *UNDOTBS1*: para deshacer registros, rollbacks y consistencia de lectura.
	- *TEMP*: datos temporales usados en consultas.
	- *USERS*: objetos de usuarios.

- **Espacio asociado a un objeto**
	Algunos gestores, como Oracle, llaman segmento a los datos almacenados por un objeto como sus tablas e índices. Se almacenan dentro de la misma zona lógica, aunque pueden estar en varios ficheros.

	Los segmentos están formados por extensiones, que son un conjunto de bloques contiguos en un fichero. Sirven como unidades de asignación de espacio.

	Los bloques sirven para almacenar filas. El tamaño de un bloque es múltiplo del tamaño de bloque del sistema de ficheros. Puede ser de longitud fija o variable.
	![[Pasted image 20240512183922.png]]

- **Organización de tablas**
	Las tablas se suelen organizar como `heap tables`. Se añaden las filas en cualquier lugar donde haya espacio, por lo que las inserciones son muy eficientes. No están ordenadas las filas.
	- *Desventajas*
		- [c] Tras hacer borrados pueden quedar con huecos.
		- [c] Las búsquedas no son eficientes.
	- *Organizaciones*
		- [b] Tablas almacenadas como índices ordenadas por clave primaria.
		- [b] Clusters.


**ÍNDICES**
****
La selectividad de una consulta es inversamente proporcional al porcentaje de filas que devuelve. Una selectividad alta devuelve un porcentaje bajo de filas.

Es interesante acceder directamente a la tabla cuando hay baja selectividad. Se lee toda la zona de almacenamiento de forma secuencial. Cuando hay alta selectividad, muchas de las lecturas no dan resultado, por lo que es muy poco eficiente.

Un índice indica dónde se encuentran los datos, de forma similar a un índice de un libro. Ofrece un camino alternativo a la exploración secuencial de la tabla. 

Una **clave de indexación** es un atributo o conjunto de atributos que se utiliza para identificar de manera única cada fila en una tabla. Los índices almacenan entradas, formadas por valores clave de indexación, y la posición física que ocupan, el `rowid`. Suelen estar ordenados por clave de indexación.

- **Índices en árbol**
	Los índices más comunes son los que tienen forma de árbol B+. Se caracteriza por:
	- Es un árbol balanceado.
	- Los nodos hoja contienen las claves de indexación y el `rowid`. Están enlazados entre ellos.
	- Los valores de clave de los nodos intermedios también están en los nodos hoja.

- **Acceso a datos a través de índices**
	Con alta selectividad, el acceso es eficiente porque se accede al índice y este hace lecturas diferenciadas a pocas zonas de la tabla. Con baja selectividad, habría muchas lecturas y bajaría la eficiencia.

- **Creación y borrado de índices**
	El estándar SQL no contempla los índices. La sintaxis suele ser la misma en los diferentes SGBD.
	```sql
	create index i_emp_sal on emp(sal);
	create unique index i_dept_dname on dept(dname);
	drop index i_emp_sal;
	```

	Los gestores pueden crear índices automáticamente a partir de las restricciones. Por ejemplo, lo hacen al crear claves primarias y, algunos gestores, sobre las claves foráneas.

- **Ventajas**
	- [p] Aceleran los `select`, `delete`  y `update`.
	- [p] Evita acceder a zonas de disco que no contienen los datos que se buscan.
	- [p] Pueden evitar el acceso a la tabla cuando todos los datos necesarios están en el índice. Esta estrategia se llama `index only`.
	- [p] Pueden acelerar los datos ordenados por clave de indexación.
	- [p] Existen otros índices, a parte de los árboles B++, útiles para recuperar un porcentaje alto de filas.
- **Desventajas**
	- [c] Ocupan espacio en disco.
	- [c] Retardan las operaciones DML.
	- [c] No siempre son adecuados.
	- [c] Si el optimizador no utiliza un índice, este ralentizará las operaciones DML.

- **Tipos de índices**
	1. *Único*: la clave contiene valores únicos.
	2. *No único*: la clave admite duplicados.
	3. *Simple*: el índice está definido sobre una sola columna de la tabla.
	4. *Compuesto*: está definido sobre varias columnas de la tabla. La elección del orden de los campos de la clave de indexación es muy relevante para determinar qué tipo de consultas puede acelerar.
		```sql
		create unique index i_dept_name on dept(name); --ÚNICO
		create index i_emp_sal on emp(sal); --NO ÚNICO

		create index i_emp_sal on emp(sal); --SIMPLE
		create index i_emp_job_sal on emp(job, sal); --COMPUESTO
		```
	5. *Denso*: si existe una entrada por cada fila.
	6. *Escaso/Disperso*: no hay una entrada por cada fila, implica que debe ser agrupado.
	7. *No agrupado/Secundario*: los datos de la tabla no están ordenados por la clave de indexación. Sobre una tabla solo puede haber más de un índice secundario.
	8. *Agrupado*: la tabla está físicamente ordenada por la clave de indexación.
	9. *Predeterminado* (`INDEX_TYPE='NORMAL'`): utiliza un árbol B+.
	10. *De clave inversa* (`NORMAL/REV`): invierte la clave de indexación. Son útiles para valores que crecen/decrecen monótonamente, evitando contención sobre los mismos nodos hoja del índice. No sirven para realizar búsquedas por rango.
	11. *Basados en funciones* (`FUNCTION-BASED NORMAL`): no indexa columnas, sino expresiones sobre columnas. Permite optimizar búsquedas sobre las expresiones indexadas.
		```sql
		create index i_emp_mgr on emp(mgr); --PREDETERMINADO

		create index i_emp_mgr_rev on emp(mgr) reverse; --DE CLAVE INVERSA

		create index i_emp_saltot on emp(sal+comm); --BASADOS EN FUNCIONES
		create index i_emp_saltot on emp(sal desc); --BASADOS EN FUNCIONES
		```
	12. ***Índice bitmap***
		Índice con forma de árbol. Para cada valor de la clave de indexación, en lugar de `rowid`, almacena un bitmap con tantas posiciones como filas tiene la tabla. En cada posición se indica si la fila tiene (1) o no tiene (0) ese valor.
		![[Pasted image 20240512193712.png]]
		```sql
		create bitmap index i_tabla_color on tabla(color);
		```
		Es útil para columnas con baja cardinalidad (pocos valores diferentes) y que sufren pocas modificaciones. Permite optimizar consultas que usan predicados con AND y OR.
		![[Pasted image 20240512193927.png]]

	- **Selección de columnas a indexar**
		- *Columnas candidatas a aparecer en índices*:
			1. Aquellas que aparecen en una **clave primaria** o **restricción de unicidad**
			2. Las que aparecen en una **clave foránea**.
			3. Cualquier otra columna implicada en una condición de **join**.
			4. Columnas usadas para **filtrar** el resultado.
			5. Las que implican **ordenación** directa (`order by`) o indirecta (`group by`, `distinct`, algún tipo de join...)
			6. Las que permiten resolver consultas accediendo a un solo **índice**.
		- *Columnas candidatas a no aparecer en índices*:
			1. Aquellas que se **actualizan muy frecuentemente**.

**OTRAS ORGANIZACIONES E ÍNDICES**
****
![[Pasted image 20240512195136.png]]
