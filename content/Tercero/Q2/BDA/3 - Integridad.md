---
Name: 3 - Integridad
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**INTRODUCCIÓN A LA INTEGRIDAD**
****
La información de una base de datos debe ser:
- *Completa*: toda la información necesaria debe estar presente. Por ejemplo, no puede haber un empleado sin nombre.
- *Correcta*: el gestor debe evitar almacenar información errónea, por ejemplo, un sueldo negativo.

**Elementos que garantizan la integridad**:
- *SGBD transaccional*: considera que la base de datos está en un estado consistente antes de empezar una transacción y cuando termina esta.
- *Restricciones de integridad*: condiciones que se aplican sobre los datos.
- *BD activas*: `triggers` que se ejecutan en el servidor como respuesta a un evento en la BD.

**Fases de diseño**: las condiciones de integridad se aplican en todas las fases de diseño e implementación.
- _Modelo conceptual:_
    - _Diagrama ER:_ identificadores, cardinalidad, participación, etc.
    - _Documentación:_ condiciones que no se indican en el ER.
- _Modelo lógico:_ claves primarias, claves foráneas, etc.
- _Implementación física:_ restricciones de integridad y triggers.

**Condiciones de integridad en el modelo físico**
- _Valores requeridos:_ admisión o no de nulos.
- _Valores únicos:_ valores no duplicados.
- _Integridad de entidad y clave:_ clave primaria y candidatas sin nulos y duplicados.
- _Integridad referencial:_ claves foráneas.
- _Validez de datos:_ valores correctos en atributos, filas y dominios.

**RESTRICCIONES DE INTEGRIDAD EN SQL**
****
Pueden ser:
- **A nivel de columna**: en los atributos, por ejemplo, atributos que no permiten nulos o claves primarias de un solo atributo.

```sql
deptno numeric(2) constraint pk_dept primary key
```

- **A nivel de fila**: implican varios atributos.

```sql
constraint c_valid_date check (enddate>startdate)
```

Se pueden crear restricciones al crear la tabla con `create table` o mediante `alter table add`. Se puede eliminar una restricción con `alter table drop`.

Las restricciones tienen nombre. Si no se lo damos se lo pone el sistema por defecto. Es muy útil darles nombre.

**RESTRICCIÓN DE CLAVE PRIMARIA**
****
Las claves primarias no admiten nulos ni duplicados. Se especifican con la restricción `primary key`. Las tablas solo pueden tener una clave primaria.

```sql
create table dept(
    deptno numeric(2) primary key,
    dname ...
);

create table dept(
    deptno numeric(2),
    dname ...,
    primary key (deptno)
);

create table dept(
    deptno numeric(2) constraint pk_dept primary key,
    dname ...
);

create table dept(
    deptno numeric(2),
    dname ...,
    constraint pk_dept primary key (deptno)
);
```

Podemos eliminar o añadir restricciones con `alter table`:

```sql
alter table dept drop constraint pk_dept;
alter table leave add constraint pk_leave primary key(empno, startdate);
```

**RESTRICCIÓN DE VALORES REQUIRIDOS**
****
Esta restricción solo se aplica a nivel de atributo. Garantiza que no se puede insertar una fila con un nulo en ese atributo, que no se puede actualizar ese atributo a nulo y, si forma parte de una clave foránea, no tendría sentido especificar como acción referencial `set null`.

```sql
create table emp(
    ...
    ename varchar(50) not null,
    ...
);

create table emp(
    ...
    ename varchar(50) constraint nn_ename not null,
    ...
);
```

**RESTRICCIÓN DE UNIDAD**
****
Indica que los atributos o conjuntos de atributos no admiten valores duplicados en atributos no nulos. No implica que no admita nulos. Una clave candidata formada por un conjunto de columnas implica `not null` de cada una de sus columnas y `unique` del conjunto. Una restricción de clave primaria implica unicidad.

```sql
alter table emp
add email varchar(50)
constraint u_email unique;

create table dept(
    ...
    dname varchar(20) constraint nn_dname not null constraint u_dname unique,
    ...
);
```

**IMPLEMENTACIÓN EN SGBD REALES**
****
 -  **ORACLE**
	Oracle no implementa dominios. Implementa las restricciones:
	- Primary key
	-  Not null
	- Unique
	- Foreign key/ References
	- Check
	 Permite modo aplazado.
	 
 -  **Activación y comprobación**
	Las restricciones pueden estar en estado `enable` o `disable`. Por defecto comprueba las filas existentes, estado `validate`, pero puede indicarse que no `novalidate`.
	
	Esto se especifica al crear o modificar las tablas.
	
```sql
alter table <table> modify constraint <nome_restr> { enabled | disabled } {validate | novalidate};
	
alter table <table> { enable | disable } constraint <nome_restr>;
```
- **Claves foráneas**
	Con respecto a las claves foráneas Oracle:
	- No permite la cláusula `match`. Se comporta `match simple`.
	-  No permite
- **POSTGRESQL**
	PostgreSQL implementa dominios. Implementa las restricciones:
	- Primary key
	- Not null
	- Unique
	- Foreign key/ References
	- Check
	-  Exclude
	  Pueden consultarse las restricciones en el catálogo en `pg_catalog.pg_constraints` y en varias vistas de `information_schema`.