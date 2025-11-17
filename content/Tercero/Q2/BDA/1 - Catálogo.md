---
Name: 1 - Catálogo
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**INTRODUCCIÓN AL CATÁLOGO**
****
Un sistema de información es el conjunto de tecnologías, procesos, aplicaciones de negocios y software disponibles para las personas dentro de una organización.

Antes estaban compuestos por ficheros y programas que los manejaban. La estructura y descripción de los datos estaba en el propio código de los programas que los manejaban.
- **Contras:**
	- Los programas podían contener información incompatible entre ellos.
	- Faltaba documentación sobre los datos, permisos de acceso a estos, procesos que los usaban, etc.

Esta metodología era poco útil y no era mantenible.
- **Surgen:**
	- _Diccionario de datos:_ lista organizada de los datos que se manejarán en el sistema, junto con sus definiciones, características y relaciones.
	- _Catálogo:_ conjunto de tablas y vistas especiales que contienen información sobre la estructura y el contenido de la base de datos. Almacena metadatos sobre los objetos e la base de datos.

**DICCIONARIO DE DATOS**
****
Un Diccionario de Datos (DD) es un repositorio centralizado de información que almacena metadatos, es decir, datos sobre los propios datos. Este contiene detalles sobre los datos, como su significado, tipos de datos, formatos, tamaños, restricciones, entre otros aspectos. Además, puede incluir información sobre otros elementos relacionados, como los programas que utilizan esos datos o los usuarios que tienen acceso a ellos.

La gestión de la información se realiza de manera centralizada, lo que significa que puede ser consultado y modificado por usuarios con los permisos adecuados. Esto mejora la autenticidad de la información y facilita la comunicación entre los usuarios.

- **Sirve para:**
	- Documentar el modelo de datos, incluyendo requisitos, especificaciones y diseño.
	- Generar informes sobre cualquier elemento del modelo de datos, incluidos los elementos relacionados.
	- Analizar el impacto de un cambio en el modelo de datos, como cambios en los tipos de datos o la inclusión de nuevos elementos.
	- Generar automáticamente la estructura de la BBDD.
	- Generar automáticamente la definición de datos en lenguajes de programación que acceden a esos datos, como clases Java.

**CATÁLOGO DEL SISTEMA**
****
Es un conjunto de tablas que almacenan metadatos sobre la base de datos y los exponen de forma amigable. Se suelen llamar `tablas o vistas del sistema` para diferenciarlas de las `tablas o vistas de usuario`.

Es creado y utilizado por el SGBD y es de solo lectura para los usuarios. Los usuarios hacen modificaciones en el catálogo al realizar `operaciones DDL`.

- **Consultas de usuario**
	Un usuario puede consultar el catálogo para saber las columnas de la tabla `emp`.
```sql
	select column_name, data_type
	from information_schema.columns
	where table_name='EMP';
```

- **Consultas del sistema**
```sql
	select *
	from emp
	where sal+comm>2000
	and deptno=10;
```
- Mediante el catálogo:
		- El SGBD comprueba que emp exista y sea una tabla o vista.
		- Comprueba que el usuario tiene permiso para hacer select.
		- Saca toda la lista de columnas.
		- Comprueba que las columnas que satisfacen el where existen 
		y son del tipo adecuado.
		- Comprueba si existen índices, por ejemplo, para optimizar la consulta.

- **Contenido del catálogo**
	El catálogo almacena datos de los esquemas conceptual, físico y externo.
	Contiene:
	- Definición de tablas y columnas.
	- Restricciones de integridad.
	- Definición de vistas.
	- Descripción de los espacios de almacenamiento.
	- Índices.
	- Información sobre usuarios, roles y privilegios.
	- Descripción de estructuras lógicas y físicas de la BD.
	- Metadatos del propio catálogo.
	
	No contiene información externa a la base de datos, como la definición de los datos, su uso o los programas que los manipulan.

**CATÁLOGO SQL ESTÁNDAR**
****
**Partes**:
- *Esquema*: un usuario puede tener varios esquemas y un esquema pertenece a un único usuario. Es una colección de elementos individuales como tablas, vistas, triggers...
- *Catálogo*: conjunto de esquemas con nombres únicos. Los elementos de la base de datos son accesibles mediante `catálogo.esquema.elemento`. Todo catálogo debe contener un esquema `information_schema` que contiene la información de todos los esquemas de dentro del catálogo.
- *Clúster*: colección de catálogos. Cada usuario tiene uno asociado. Para un usuario podría decirse que es su base de datos, puesto que es el máximo ámbito en el que se puede ejecutar una consulta SQL.
- *Entorno SQL*: contexto en que los datos pueden existir y donde se pueden realizar operaciones sobre ellos. Es una instancia del SGBD que se está ejecutando en una instalación concreta.

**Almacenamiento**: el SGBD debe almacenar los metadatos en un conjunto de tablas llamado `definition.schema` que no puede ser accedido por el SQL de los usuarios.

**Exposición**: los datos se exponen mediante una colección de vistas denominado `information.schema`.
- Contiene las vistas:
	- *SCHEMATA*: esquemas del usuario actual.
	- *TABLES*: tablas persistentes del usuario actual.
	- *COLUMNS*: columnas de las tablas del usuario actual.
	- *VIEWS*: vistas del usuario actual.
	- *DOMAINS*: dominios accesibles por el usuario actual.

**CASOS DE USO**
****
**ORACLE**
**Elementos de una instalación**:
- Una única base de datos (versión 11) o varias, el `Container Database` (versión 12). Podemos añadir `Pluggable Database` desde esta última versión.
- Usuarios propios.
- Un esquema por usuario con el mismo nombre.
- Objetos dentro de un esquema, como tablas, vistas, índices, etc. Se nombran como el nombre de usuario o esquema y el del objeto unidos por un punto. Por ejemplo, `scott.emp`. Para cada tipo de objeto existen vistas.
    - user_< objeto >: objetos creados por el usuario.
	- all_ < objeto >: objetos a los que el usuario tiene acceso. 
    - DBA_ < objeto >: todos los objetos. Solo pueden ser accedidas por los usuarios con rol DBA.
    - CDB_: todos los objetos de todas las PDB si se consulta desde el `Container CDB$ROOT`. Requiere también rol de DBA.
- < objeto > puede tomar diferentes valores:
	- objects
	- tables
	- views
	- indexes
	- constraints
	- synonyms

**POSTGRESQL**
- **Nomenclatura**

| **Estándar** | **PostgreSQL** |
| ------------ | -------------- |
| Esquema      | Schema         |
| Catálogo     | Database       |
| Cluster      | -              |
| Entorno SQL  | Cluster        |
- **Elementos de una instalación**:
	- Un usuario puede crear múltiples bases de datos y una base de datos es de un único usuario.
	- Un usuario puede acceder a bases de datos de otros usuarios, pero no existe como tal un clúster.
	- Dentro de una BD puede haber varios esquemas. Un usuario puede crear varios esquemas y un esquema es de un único usuario.
	- Cada base de datos contiene el `information_schema`.

- **Ejemplos**
```sql
-- Creación de esquemas
CREATE SCHEMA abd;
CREATE SCHEMA mai;
CREATE SCHEMA bda;

-- Definición de la tabla en el esquema 'abd'
CREATE TABLE abd.estudiante (
    dni VARCHAR(9),
    apellidos VARCHAR(80) NOT NULL,
    nombre VARCHAR(40) NOT NULL,
    login VARCHAR(50) NOT NULL,
    CONSTRAINT pk_estudiante PRIMARY KEY (dni),
    CONSTRAINT u_login_est UNIQUE (login)
);

-- Definición de la tabla en el esquema 'mai'
CREATE TABLE mai.estudiante (
    dni VARCHAR(9),
    apellidos VARCHAR(80) NOT NULL,
    nombre VARCHAR(40) NOT NULL,
    login VARCHAR(50) NOT NULL,
    CONSTRAINT pk_estudiante PRIMARY KEY (dni),
    CONSTRAINT u_login_est UNIQUE (login)
);

-- Definición de la tabla en el esquema 'bda'
CREATE TABLE bda.estudiante (
    dni VARCHAR(9),
    apellidos VARCHAR(80) NOT NULL,
    nombre VARCHAR(40) NOT NULL,
    login VARCHAR(50) NOT NULL,
    mencion VARCHAR(3) NOT NULL,
    CONSTRAINT pk_estudiante PRIMARY KEY (dni),
    CONSTRAINT u_login_est UNIQUE (login),
    CONSTRAINT ch_mencion CHECK (mencion IN ('ES', 'SI'))
);
```

```sql
select table_schema, table_name, table_type 
from information_schema.tables
where table_schema in ('abd','bda','mai');
```

| **table_schema** | **table_name** | **table_type** |
| ---------------- | -------------- | -------------- |
| abd              | estudiante     | BASE TABLE     |
| bda              | estudiante     | BASE TABLE     |
| mai              | estudiante     | BASE TABLE     |
Consulta propia de PostgreSQL utilizando `pg_catalog`:
```sql
select schemaname, tablename 
from pg_catalog.pg_tables
where schemaname in ('abd','bda','mai');
```

|**schemaname**|**tablename**|
|---|---|
|abd|estudiante|
|bda|estudiante|
|mai|estudiante|