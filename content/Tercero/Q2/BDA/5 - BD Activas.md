---
Name: 5 - BD Activas
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**INTRODUCCIÓN A TRIGGERS**
****
Las BD pasivas son simples almacenes de datos. Hoy en día apenas existen.

Las BD activas realizan controles con restricciones y reaccionan ante eventos como cambios en los datos.
Las ventajas de esto son:
- *Control de restricciones*: validaciones de DNI, salario... o reglas como que un empleado no gane más que su jefe.
- *Actualización de datos*: generación de claves primarias, campos auto-actualizados...
- *Acciones externas a la base de datos*: alertas, como un stock mínimo de un producto.

Estas actividades se suelen implementar con triggers.

- **Restricciones**
	Las restricciones del estándar son:
	- *Not null*: no permite nulos.
	- *Unique y Primary key*: garantizan la unicidad. En caso de primary key, también evitan nulos.
	- *Check*: verifica la validez de los valores mediante una condición.
	- *Foreign key*: establece una relación entre dos tablas.
	```sql
	create table emp(
		empno numeric(3) not null,
		ename chat(20) not null,
		email char(25) not null constraint u_email unique,
		mgr numeric(3),
		deptno numeric(3),
		sal numeric(7,2),
		constraint pk_emp primary key(empno),
		contraint fk_emp foreign key(mgr)
		references emp(empno) on delete cascade on update cascade,
		constraint c_sal_pos check(sal>0) deferrable initially deferred);
	)
	```
	Debemos tener en cuenta que:
	1. La acción por defecto es abortar.
	2. Solo se puede hacer check a nivel de fila.
	3. No afectan a borrados de filas excepto `Foreign key`.
	4. Se pueden aplazar hasta el final de la transacción.

- **Afirmaciones**
	Las afirmaciones, o assertions, son restricciones adicionales que se pueden aplicar a una base de datos para garantizar la integridad de los datos. Funcionan como condiciones que deben cumplirse en todo momento, y si alguna de estas condiciones no se cumple, se produce una violación de la afirmación y la operación que desencadenó la afirmación puede ser rechazada.
	```sql
	create assertion <nome> check <condicion>;
	
	create assertion as_salarios_similares
	check (((select max(sal) from emp)-(select min(sal) from emp)) < 100);
	
	create assertion deptos_pequenos
	check (not exists (select deptno 
						from emp
						group by deptno
						having count(*) > 10));
	```
	Por defecto siempre abortan. Los SGBD no las implementan.

**TRIGGERS**
****
Los triggers son programación basada en eventos, también se llaman ECA (Evento- Condición-Acción).
Cuando se produce un evento, se dispara el trigger. Las condiciones son opcionales, por defecto se asume que son ciertas. Si la condición es cierta, se ejecuta la acción.

- **Control de salario inválido**
	insert; C: salario <= 0; A: no permitir la inserción.
	update; C: salario <= 0; A: no permitir la actualización.
- **Creación de clave subrogada (campo id)**
	insert; C: null; A: generar nuevo id.
- **Log de modificaciones en la tabla de empleados**
	insert; C: null; A: grabar entrada en el log.
	delete; C: null; A: grabar entrada en el log.
	update; C: null; A: grabar entrada en el log.

En el estándar, un trigger controla una única tabla o vista. Se consideran eventos insertar, actualizar y borrar. Cada trigger responde a un único evento y no valida los datos que ya existían. Las ventajas del uso de triggers son:
- Simplifica la codificación de aplicaciones que acceden a la BD.
- Dan mayor consistencia.

**TRIGGERS EN EL SQL ESTÁNDAR 2008**
****
Un trigger está formado por el nombre del trigger y el nombre de la tabla.

- **Eventos**
	Son las inserciones, actualizaciones y borrados. Se puede activar un trigger antes o después de un evento. En las vistas se puede utilizar `instead of`.
	La granularidad indica cuantas veces se lanza un trigger en un evento. Se indica con `for each {row | statement}`:
	- *Row*: una vez para cada fila afectada.
	- *Statement*: una vez para todo el evento.
- **Condiciones**
	Se indican con `when` y pueden omitirse.
- **Acciones**
	Deben ser atómicas. No se recomienda:
	1. DML de modificación de datos en triggers `before`.
	2. Sentencias de control de transacciones, conexión a BD o DDL.
- **Tablas de transición**
	Contienen los valores nuevos y antiguos de los triggers. También se conocen como nombres de correlación. Si se usan, se tienen que declarar en `referencing`.

**TRIGGERS EN ORACLE**
****
- **Crear o modificar un trigger**
	```sql
	create [or replace] trigger <nombre_trigger>
		{before|after|instead of}
			<evento> [or <evento>...] on <tabla>
		[referencing [new as tupla_nueva]
					[old as tupla_vella]]
		[for each row]
		[when (<condicion_sql_simple>)]
	{<bloque PL/SQL> | call <procedimiento(argumentos)>}
	```

- **Borrar trigger**
	```sql
	drop trigger <nombre_trigger>
	```

- **Activar o desactivar**
	1. Triggers individuales
		```sql
		alter trigger <nombre_trigger> {enable | disable}
		```
	2. Triggers asociados a una tabla
		```sql
		alter table <tabla> {enable | disable} all trigers
		```
		