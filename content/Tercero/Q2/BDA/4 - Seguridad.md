---
Name: 4 - Seguridad
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**INTRODUCCIÓN A LA SEGURIDAD EN BBDD**
****
La seguridad es fundamental para cualquier organización. Se centra en garantizar:
- _Confidencialidad:_ proteger datos privados ante accesos no autorizados.
- *Disponibilidad:* dar acceso a todos los datos a los que haya permiso de acceso.
- _Integridad:_ evitar modificaciones no adecuadas y daños en la información.

La seguridad está vinculada a elementos muy diversos:
- Edificios
- Personas
- Dispositivos
- Sistemas operativos
- Comunicaciones
- Software
- Bases de datos

1. **Normas de seguridad**
	- Es obligatorio cumplir con la LOPD y con el RGPD. Esto incluye los derechos de disposición de la información, de confidencialidad, etc.
	- Cada organización dispone de distintos niveles de seguridad. Esto implica tener varios grupos de usuarios con ciertos permisos de acceso a los datos y una serie de acciones permitidas.
2. **ISO 27001**
	 La ISO 27001 es un estándar de seguridad de información que permite establecer los requisitos necesarios para crear, mantener y mejorar un Sistema de Gestión de Seguridad de la Información en una organización. Son un conjunto de buenas prácticas que permiten a las organizaciones gestionar los riesgos e incluso evitarlos.
	 
	 El análisis de riesgos se basa en:
	 - Inventario de activos
	 - Inventario de amenazas
	 - Definición de nivel de riesgo aceptable
	 
	 **Ciclo de Deming**
	 La ISO 27001 se basa en el ciclo de Deming o Plan-Do-Check-Act.

	```mermaid
	graph LR
	    A[Planificar] --> B[Hacer]
	    B --> C[Verificar]
	    C --> D[Actuar]
	    D --> A
	    style A fill:#B0C4DE,stroke:#333,stroke-width:2px;
	    style B fill:#98FB98,stroke:#333,stroke-width:2px;
	    style C fill:#FFD700,stroke:#333,stroke-width:2px;
	    style D fill:#FFB6C1,stroke:#333,stroke-width:2px;
	    %% Set node text color to black
	    %% A,B,C,D,E
	    classDef blackText fill:#FFFFFF, color:#000000;
	    class A,B,C,D,E blackText;
	```
	La metodología Plan-Do-Check-Act se basa en:
	- _Plan:_ establecer objetivos y procesos y hacer una planificación temporal.
	- _Do:_ implementar los objetivos y procesos.
	- _Check:_ verificar el éxito o fracaso de las medidas.
	- _Act:_ poner en marcha los cambios necesarios encontrados en la fase anterior.

3. **Seguridad en las bases de datos**
	 La seguridad se implementa en los gestores de bases de datos para proteger las BD de amenazas y ataques. Los ataques pueden producir:
	 - Pérdidas de confidencialidad.
	 - No disponibilidad.
	 - Pérdida de integridad.
	
	 Podemos implementar medidas para contrarrestar o eliminar riesgos:
	 - Control de acceso
		 - Obligatorio
		 - Discrecional
	- BD estadísticas
	- Cifrado de datos
	- Auditorías

**AUTENTICACIÓN Y AUTORIZACIÓN**
****
Los AuthID son identificadores utilizados para representar usuarios o roles que tienen ciertos niveles de autorización o permisos dentro del sistema de gestión de bases de datos. Pueden ser:
- Identificadores o nombres de usuario.
- Nombres de rol.
- `public` que hace referencia a todos los AuthID.

El identificador de usuario se utiliza para conectarse a la base de datos. Identifica a una persona o programa que accede a la base de datos. No hay un estándar para la creación de nombres de usuario.

```sql
-- En Oracle
create user "nombre" idenfitied by "contraseña";
create user usuario identified externally;
```

```sql
-- En Postgre
create user "nombre" with password 'contraseña';
```

1. **Autenticación**
	 La autenticación es el primer proceso que realiza el gestor para el control de acceso de los usuarios. Identifica a un usuario y comprueba su identidad. Consiste en:
	1. El usuario indica su AuthID.
	2. Se verifica que el usuario es quien dice ser, normalmente mediante una contraseña. Puede hacerlo el SGBD o puede hacerse de forma externa, por ejemplo el SO o un LDAP.
	3. Se pasa a la fase de autorización.

2. **Autorización**
	 La autorización es lo qué puede hacer un usuario en la base de datos. Aunque un usuario se autentique, no tiene por que poder siquiera conectarse a la base de datos.
	
	Existen dos tipos de control de acceso:
	- *Control de acceso obligatorio:* no está definido en el estándar.
	- *Control de acceso direccional:* está definido en el estándar.

3. **Control de acceso obligatorio**
	 Se basa en políticas del sistema, por lo que son modificables por el usuario. Se le asigna a cada objeto de la base de datos una clave de seguridad. A cada usuario se le asigna un nivel de autorización para cada clase de seguridad. Los usuarios pueden leer o escribir un objeto basándose en los niveles de autorización y clases de seguridad.
	 Se basa en garantizar que los datos confidenciales no pasen a un usuario sin el nivel de seguridad adecuado.
	 
	- **Modelo Bell-LaPadula**
		Se basa en definir clases de seguridad:
		- *TS*: top secret.
		- *S*: secret.
		- *C*: confidential.
		- *U*: unclassified.
		Asigna una clase de seguridad a los objetos y a los usuarios y define dos reglas:
		- *Simple*: un sujeto puede leer un objeto si la clase del sujeto es mayor o igual que la del objeto.
		- *Estrella*: un sujeto puede escribir un objeto si la clase del sujeto es menor o igual que la del objeto.

**CONTROL DE ACCESO DISCRECIONAL**
****
Está basado en la concesión y revocación de privilegios. Existen dos tipos de privilegios:
- **Privilegios de sistema o servidor**: especifican acciones que un usuario puede llevar a cabo, sin estar vinculadas con ningún objeto en concreto. No está definido en el SQL estándar, pero prácticamente todos los SGBD los incluyen. 
- **Privilegios a nivel de objeto**: están asociados a un objeto concreto y dependerán de su tipo. Están definidos en el SQL estándar, aunque los gestores utilizan habitualmente privilegios adicionales. El creador de un objeto tiene todos los privilegios sobre este y la potestad de pasar dichos privilegios a otros usuarios. Si un usuario pretende seleccionar datos de una tabla a la que no tiene acceso, el SGBD debería indicar que tal tabla no existe.

Para conceder privilegios sobre objetos de la BD se hace uso de la sentencia GRANT
```SQL
GRANT {<lista_privilegios> | ALL PRIVILEGES}
	ON <objeto>
	TO <lista_authids>
	[WITH GRANT OPTION]
```
- Un usuario sólo puede conceder un privilegio sobre el objeto si tiene ese privilegio y la potestad de pasarlo a otros.
- El creador de un objeto tiene todos los privilegios sobre él y la potestad de pasarlos.
- La sentencia `GRANT` permite conceder varios privilegios a la vez, pero sobre un único objeto.
- `ALL PRIVILEGES` es la lista de todos los privilegios aplicables al tipo de objeto.
- `<lista_authids>` es una lista de AuthIDs que pueden ser usuarios, roles o PUBLIC.
- Utilizando `WITH GRANT OPTION` se concede también la potestad de pasar ese privilegio a otros.
```SQL
GRANT SELECT, DELETE, INSERT ON tab1 TO usuario1;
GRANT UPDATE(sal) ON emp TO scott, role345 WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON EMP TO theboss;
GRANT SELECT ON emp TO PUBLIC; --todos los usuarios, incluso los creados en el futuro, podrán seleccionar de emp
```

Para revocar un privilegio se utiliza la sentencia REVOKE.
```sql
REVOKE [GRANT OPTION FOR] {<lista_privilegios> | ALL PRIVILEGES}
	ON <objeto>
	FROM <lista_authids>
	{CASCADE | RESTRICT}
```
- Un usuario solo puede revocar un privilegio que haya concedido con una sentencia GRANT previamente.
- Usando `GRANT OPTION FOR` solo se retira la potestad para pasar privilegios a otros, pero los privilegios se conservan.
- Si algún usuario de la `<lista_authids` concedió algún privilegio a otros:
	- `CASCADE` retira el privilegio en cascada a todos los AuthIds implicados.
	- `RESTRICT` produce un error y no revoca los privilegios.
```sql
REVOKE DELETE, INSERT ON tab1 FROM usuario1;
REVOKE UPDATE(sal) ON emp FROM scott CASCADE;
REVOKE SELECT ON emp FROM PUBLIC;
```

Al ejecutar una sentencia `GRANT` concediendo un privilegio a un AuthID, se establece una línea de concesión. Cuando se propaga un privilegio a terceros, se pueden crear cadenas de concesión complejas. 
```sql
(u1) grant select on t to u2, u3 with grant option; 
-- u1 da permisos a u2 y u3 con grant

(u2) grant select on t to u4;
(u3) grant select on t to u4;
-- tanto u2 como u3 dan el permiso a u3
-- cada sentencia grant da lugar a una línea de concesión

(u3) revoke select on t from u4;
-- u3 revoca su línea de concesión, pero u4 mantiene el privilegio
-- debido a la linea de concesión desde u2

(u2) revoke select on t from u4;
-- u2 revoca su línea de concesión, ahora u4 pierde el privilegio
```

```sql
-- los puntos 1-3 son iguales que los anteriores
(u1) revoke select on t from u3 cascade;
-- u1 revoca el privilegio a u3 con la opción en cascada
-- u3 pierde el privilegio y su línea de concesión y la de u4 desaparecen

(u1) revoke grant option for select on t from u cascade
-- u1 revoca la grant option a u2 en cascada
-- u2 mantiene el privilegio pero pierde la potestad de pasarlo
-- u4 pierde su línea de concesión y su privilegio
```

- **Roles**: un rol es un tipo especial de AuthID que se utiliza para facilitar la gestión de los privilegios. Sus características son:
	- No puede conectarse a la BD (no es un usuario).
	- Podemos concederle privilegios u otros roles.
	- Podemos asignar el rol a otros AuthIDs, con lo que pasan a tener los privilegios de rol concedido.
	```sql
	create role facturador;
	create role gestor;
	
	grant all privileges on factura to facturador;
	grant select on artigo to facturador;
	
	grant all privileges on artigo to gestor;
	grant facturador to gestor;
	
	grant facturador to facturador1, facturador2;
	grant gestor to gestor1;
	revoke facturador from facturador2;
	revoke refrences on factura from facturador;
	
	drop role gestor;
	```
	
	La concesión y revocación de roles utiliza también sentencias `GRANT` y `REVOKE`.
	```sql
	grant <rol> [, <rol>, ...]
	to <lista_authids> [with admin option]
	
	revoke [admin option for] <rol> [, <rol>, ...]
	from <lista_authids>
	```
	
	Para los roles, las sentencias `GRANT` y `REVOKE` no incluyen ninguna cláusula `ON <objeto>` y utilizan opcionalmente la cláusula `WITH ADMIN OPTION`.
	El funcionamiento de esta cláusula no es exactamente igual a `WITH GRANT OPTION`. En este caso, además del rol, se pasa la potestad de *administrar* ese rol:
	- Permite conceder privilegios u otros roles a ese rol.
	- Permite conceder el rol, o revocarlo, a cualquier lista de AuthIDs.

**TÉCNICAS ADICIONALES**
****
Además de los controladores de acceso, podemos utilizar otras técnicas que mejoren la seguridad en las bases de datos:
- **Vistas**
	A veces se quiere limitar el acceso a ciertas filas o columnas de una tabla. No siempre es posible dar los permisos sobre las tablas.
	```sql
	grant select(empno, ename, job, mgr, hiredate) on emp to usr;
	-- oracle no sorporta
	```
	Algunos gestores pueden para columnas pero no para filas.
	Para poder limitar con facilidad el acceso a filas o columnas, podemos retirar los permisos sobre la tabla. Crear una vista con las filas o columnas deseadas y dar permiso a la vista.
	```sql
	-- si usr tenía privilegios sobre emp;
	revoke all privileges on emp from usr;
	
	create view emp20 as 
	select empno, ename, job, mgr, hiredate, sal, comm, deptno
	from emp
	where deptno = 20
	with check option;
	grant all privileges on emp20 to usr; 
	```

- **Bases de datos estáticas**
	Son bases de datos que muestran solo datos obtenidos a partir de una serie de tuplas. Por debajo sí que contienen datos confidenciales, pero solo se publica la información calculada. Es una forma de anonimizar. 
	
	La desventaja es que a veces se puede llegar a los datos confidenciales a través de los calculados. Normalmente no se publican datos que no vengan de cierto número mínimo de tuplas.

- **Cifrado de información**
	Las comunicaciones con los SGBD deben estar cifradas, sobre todo si es una conexión a través de internet. Habitualmente se usan SSL/TLS y certificados. También se pueden cifrar los datos de la BD, ya que alguna información, como contraseñas, no se debe filtrar.
	S
	Se debe proteger la información ante accesos externos:
	- *Para contraseñas*: cifrar externamente la información y almacenar solo la clave cifrada. Se suele usar cifrado unidireccional, como HASH o MD5. Al hacer el `login` se filtra la clave introducida y se compara con la almacenada.
	- *Para información confidencial*: se suele utilizar cifrado bidireccional para recuperar la información cifrada.

- **Prevención de inyección SQL**
	Las inyecciones de SQL son problemas de seguridad que se suelen dar al controlar mal la entrada de los datos en programas que acceden a la base de datos.
	```python
	query = "select datanac from persoa where nome = '{}'"
	nompers = input('Introduce el nombre de la persona:')
	# introducimos Ada Lovelace
	
	# parece correcto:
	# query.format(nompers) => "select datanac from persoa 
							# where nome = 'Ada Lovelace'"
	cursor.execute(query.format(nompers)) # => OK
	
	# pero ¡CUIDADO!
	# introducimos nombre: ''; DROP TABLE persoa; --
	print(query.format(nompers)) # select datanac from persoa
								# where nome = ''; DROP TABLE persoa; --
	cursor.execute(query.format(nompers)) # => elimina la tabla persona
	
	# Una posible solución consiste en utilizar parámetros:
	query = "select datanac from persoa where nome = %(nompers)s"
	nompers = input('Introduce el nombre de la persona:')  
	# Introducimos nombre: '; DROP TABLE persoa; --
	
	# Buscaría una persona llamada: '; DROP TABLE persoa; --
	# No ocurre la inyección SQL
	cur.execute(query, {'nompers': nompers})
	```

- **Backups**
	Se suelen considerar elementos de seguridad. La pérdida de información suele darse por:
	- Borrados
	- Mal funcionamiento del software
	- Fallos del hardware
	Es importante tener un plan global de salvaguarda del sistema y realizar pruebas constantes para verificar su funcionamiento.
