---
Name: 2 - Vistas
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**INTRODUCCIÓN**
****
Permiten definición de diferentes esquemas externos, ofreciendo a cada usuario/rol la visión adecuada ayudando a conseguir la independencia lógica.
Una vista es una tabla virtual que expone los datos que son resultados de una consulta sobre otras tablas/vistas.
- **Tabla**: almacena definición y datos en el espacio de datos.
	- **Permanente**: mantienen la estructura y los datos.
	- **Temporal**: mantienen la estructura pero los datos se eliminan.
- **Vista**: almacena definición en el catálogo, no almacena datos.
	- **Componentes**: nombre, lista de atributos/columnas, definición.
	- **Materializada**: almacenan los datos en el espacio de datos.

**VISTAS EN SQL ESTÁNDAR**
****
- **Definición**
```sql
	CREATE VIEW nombre_vista [(<lista_atributos>)]
		AS <sentencia_select>
		[<check_option>]
```

- **Eliminación**
```sql
	DROP VIEW nombre_vista [RESTRICT | CASCADE]
```
	1. restrict (predeterminado): si hay vistas independientes, no se borra.
	2. cascade: borra las vistas dependientes y después la actual.

- **Actualización**
	Para actualizar una vista, el SXBD debe poder llegar de una fila de vista a una única fila de la tabla base.
	- **Normas**:
		1. No hay eliminación de duplicados ni agrupamiento.
		2. No hay más de una tabla en el *from*.
		3. La consulta no es resultado de operaciones algebraicas.
		4. Si hay cláusula *where*, esta no puede contraer una subconsulta que use la misma tabla que usa una cláusula *from*.
		5. Se deben satisfacer las restricciones de la tabla base.
		6. Si la vista utiliza expresiones, la vista no permitirá actualizar la expresión, ni la inserción de filas nuevas.
	- **Tuplas migratorias**: aquellas que desaparecen de la vista.
	- **Check option**: se incluye en la sentencia de creación de la vista, permite evitar la aparición de tuplas migratorias, solo se admite en vistas actualizables.
		`WITH [CASCADED | LOCAL | CHECK OPTION]`
		1. *cascaded* (predeterminado): comprueba la condición de la propia vista y la de todas aquellas sobre las que está definida.
		2. *local*: comprueba la condición de la propia vista. 

**VISTAS EN ORACLE**
****
- **Creación de vistas**
```sql
	CREATE [OR REPLACE] VIEW nombre_vista [(<lista_atributos>)]
	AS <sentencia_select> [WITH CHECK OPTION]
```
	1. Permite order by en la sentencia select.
	2. with check option no tiene especificación de alcance, siempre en cascada.
	3. Permite especificar restricciones limitadas para las vistas.
	4. Usa CREATE OR REPLACE VIEW... para modificar la definición de una lista.

- **Borrado**
	`DROP VIEW nombre_vista;`
	1. Borra la vista y marca como inválidas las vistas definidas sobre ellas.
	2. Puede incluir *cascade constrains* si a vista tuviese restricciones.

- **Alter**
	`ALTER VIEW nombre_vista COMPILE;`
	1. Utilizado cuando la vista dejó de ser válida, comprueba dependencias y puede marcarla como válida.
	2. No sirve para modificar la consulta asociada a la vista.

- **Actualizaciones**
	- **Variantes del estándar:**
		- Permite insertar filas cuando la vista tiene expresiones.
		- Es actualizable aunque tenga un `where` con una subconsulta a la misma tabla del `from`.
		- Permite actualizar vistas definidas sobre `join` en la tabla preservada por clave.
- **Ejemplos**
```sql
insert into emp10_sal values(1234, 'pepe', null);
-- Incorrecto: Intentando insertar en una columna virtual 
```

```sql
insert into emp10_sal(empno, ename) values(1234, 'pepe');
```

```sql
create view empcaro as
select * 
from emp
where sal > (select avg(sal) from emp);

delete from empcaro;
```

```sql
create view empdep as
select empno, ename, sal, e.deptno, dname
from emp e 
join dept d on e.deptno = d.deptno;

insert into empdep(empno, ename, sal, deptno) values (1234, 'pepe', 1000, 10);
```

**VISTAS MATERIALIZADAS**
****
También se llaman `snapshots`. Almacenan el resultado de la consulta en disco. Las consultas son más rápidas, aunque ocupan espacio y hay que actualizar el contenido.
Oracle permite especificar cuando materializar la vista y cómo y cuándo refrescar esos datos para mantenerlos sincronizados con las tablas base.
- **MATERIALIZACIÓN**
	- Tipos:
		- _Inmediata_: cuando se define.
		- _Aplazada_: en el primer refresco.
		- _Prebuilt table_: utilizando una tabla preexistente.
	- Modos de refresh:
		- *Complete*: recrea la vista al completo. Puede hacerse en cualquier momento, pero es muy costoso.
		- *Fast*: default, hace incremental y, si no puede, completo.
	- Modos de actualización:
		- *On commit*: durante el commit de cada transacción de las tablas base.
		- *On demand*: de forma manual.
		- *Periódicamente*: se indica una periodicidad.
		- *Never refresh*: nunca.
- **Ejemplos**
```sql
create materialized view mv_empdep
build immediate -- Predeterminado
refresh force -- Predeterminado
on demand -- Predeterminado
enable query rewrite
as
select *
from emp natural join dept;

create materialized view mv_emphours
build deferred
refresh complete
start with sysdate next sysdate + 1
as
select e.empno, ename, p.prono, pname, hours
from emp e
join emppro ep on e.empno=ep.empno
join pro p on ep.prono=p.prono;

select mview_name, build_mode build, refresh_method metodo_ref,
refresh_mode modo_ref, fast_refreshable, rewrite_enabled rewrite
from user_mviews;
```
- **Consultas**
	Se pueden hacer consultas sobre vistas materializadas como si fueran tablas. Incluso se pueden añadir índices.
```sql
create index i_mv_empdep_sal
on mv_empdep(sal);

select *
from mv_empdep
where sal > 3000;
```
- **Reescritura de consultas**
	El `query rewrite` es una característica en Oracle que permite al optimizador de consultas utilizar vistas materializadas de manera inteligente, incluso si esas vistas no están directamente referenciadas en la consulta original. Esto se puede activar o desactivar.
```sql
alter session set query_rewrite_enabled={true | false}
```
