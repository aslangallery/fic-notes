---
Name: Boletín SQL
tags:
  - práctica
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**AMPLIACIÓN SQL**
****
```sql
-- 1. Consultar la estructura de una tabla, por ejemplo emp.
desc emp;

-- 2. Seleccionar los datos de la tabla emp.
select * from emp.

-- 3. Obtener los datos de los empleados que tienen el menor salario de cada departamento.
	-- SUBCONSULTA DE FILA (operaciones simultáneas de comparación sobre      las columnas resultado de la subconsulta)
	select * from emp
	where (sal, deptno) in (select min(sal), deptno
							 from emp
							 group by deptno);
	-- SUBCONSULTA SINCRONIZADA (se ejecuta para cada fila de la tabla        que recorre la consulta de nivel superior)
	select * from emp e
	where sal in (select min(sal)
					from emp
					where deptno = e.deptno
					group by deptno);

-- 4. Obtén la salida siguiente:
	--  Codigo Nombre y empleo
		-------------------------------
	--   7369 SMITH trabaja de CLERK
	--   7499 ALLEN trabaja de SALESMAN
	--   7521 WARD trabaja de SALESMAN
	--   7566 JONES trabaja de MANAGER
select empno as "Código", ename || 'trabaja de' ||, job "Nombre y empleo"
from emp;

-- 5. Repetid el ejercicio anterior ordenando por el valor del c´odigo de los empleados de forma descendente.
select empno as "Código", ename || 'trabaja de' ||, job "Nombre y empleo"
from emp
order by empno desc;

-- 6. Repetid el ejercicio anterior ordenando ahora por el codigo del departamento, pero sin que dicho codigo aparezca en la tabla resultado.
select empno as "Código", ename || 'trabaja de' ||, job "Nombre y empleo"
from emp
order by deptno; -- se puede hacer ORDER BY sobre una columna que no esté                     en la claúsula SELECT. 

-- 6. Repetid el ejercicio anterior pero haciendo que se ordene por el codigo del departamento de manera ascendente y despues por el codigo del empleado de manera descendente, y que la salida sea:
	-- CODIGO Nombre y empleo Depart.
	  ----------------------------------
	-- 7934 MILLER trabaja de CLERK 10
	-- 7839 KING trabaja de PRESIDENT 10

select empno as "Código", ename || 'trabaja de' ||, job "Nombre y empleo"
from emp
order by deptno, empno desc;

--13. Obten el codigo, nombre, salario, comision, y la suma del sueldo y de la comision de los empleados, para los que tienen un salario mayor que 1000 y ordenados por la comision.
select empno, ename, sal, comm, sum(sal, comm) suma
from emp
where sal > 1000
order by comm;

--14. Repite el ejercicio anterior calculando la suma del salario y de la comision, aunque esta ultima sea nula. Ordena por esa suma.
select empno, ename, sal, comm, sum(sal, coalesce(comm, 0)) suma
from emp
where sal > 1000
order by suma

-- 16. Obten los codigos de los departamentos que aparecen en la tabla emp. Obtenlos de nuevo pero sin que se repitan sus valores.
select distinct deptno -- distinct no repite valores
from emp;

-- 17. Obten ahora los codigos de los departamentos y los empleos, primero todos los existentes, aunque se repitan filas, y despues sin repetir filas.
select distinct deptno, job
from emp;

-- 18. Realice un producto cartesiano de las tablas emp y dept.
cross join emp
cross join dept

-- 19. Realiza un equijoin de las tablas emp y dept.
select * 
from emp e join dept d
on e.deptno = d.deptno;

-- 20. Obten informacion en la que se reflejen, en cada fila, los nombres, empleos y salarios, tanto de los que superan el salario de Allen, como los del propio
select e.ename, e.job, e.sal, a.ename, a.job, a.sal
from emp e join emp a
on e.sal > a.sal
and a.ename = 'ALLEN'; --se podría poner where, pero es más correcto and

-- 21. Obt´en el nombre de cada empleado y el de su supervisor:
select e.ename, s.ename
from emp e left join emp s -- con left forzamos a que aparezca el                                       empleado sin supervisor
on e.mgr = s.empno;

-- 22. Obtener las referencias de los empleados con los datos de sus departamentos, para todos los empleados y todos los departamentos.
select empno, ename, d.deptno, dname
from emp e full join dept d
on e.deptno = d.deptno;

-- 23. Obten los salarios mınimos y el codigo de cada departamento.
select min(sal), deptno
from emp
group by deptno;

-- 24. Obten el salario mas alto de entre los salarios mınimos de cada departamento.
select max(sal)
from emp
where sal in (select min(sal)
			 from emp
			 group by deptno);

```

**CONTROL TRANSACCIONAL**
****
```sql
-- 1. Crea una tabla XEMP con la misma estructura que EMP, pero solo con
-- los datos de los empleados SMITH y KING.
create table XEMP as
	select * 
	from emp
	where ename in ('SMITH', 'KING');

-- 3. Sesión 1 actualice el salario de SMITH a 1000.
update XEMP
set sal = 1000
where ename = 'SMITH';

-- 4. Sesión 2 actualice comisión de SMITH a 100.
update XEMP
set comm = 100
where ename = 'SMITH';

-- 7. Sesión 1 baja el salario de KING a 400.
update XEMP
set sal = 4000
where ename = 'KING';

-- 8. Sesión 2 sube salario de SMITH a 1500.
update XEMP
set sal = 1500
where ename = 'SMITH';

-- 9. Sesión 1 establece salario de SMITH a 1350.
update XEMP
set sal = 1350
where ename = 'SMITH';

-- 10. Sesión 2 cambia salario de KING a 4700.
update XEMP
set sal = 4700
where ename = 'KING';

-- 13. Sesión 1 borra contenido de XEMP e inserta datos de KING.
delete from XEMP;

insert into XEMP
	select * from emp
	where ename = ' KING';

-- 14. Sesión 1 duplica el salario de KING y añade nuevo empleado con código 1234, nombre NOBODY y salario 1000.
update XEMP
set sal = sal * 2;
where ename = 'KING';

insert into XEMP (empno, ename, sal)
values (1234, 'NOBODY', 1000);

-- 16. Sesión 1 actualiza el salario de NOBODY a 2000.
update XEMP
set sal = 2000
where ename = 'NOBODY';

-- 19. Ambas transacciones en modo serializable.
set transaction isolationlevel serializable;
```