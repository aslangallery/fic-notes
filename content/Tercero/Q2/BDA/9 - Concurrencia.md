---
Name: 9 - Concurrencia
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**INTRODUCCIÓN**
****
Los SGBD implementan el **control de concurrencia** para garantizar resultados correctos. Están basados en el uso de **transacciones**.

**PROBLEMAS DE CONCURRENCIA**
****
Existen 4 problemas asociados a la concurrencia:
- **Actualización perdida**.
	![[Pasted image 20240516113643.png]]

- **Lectura sucia**.
	![[Pasted image 20240516113748.png]]
	![[Pasted image 20240516113824.png]]

- **Análisis incoherente**.
	![[Pasted image 20240516113921.png]]
	![[Pasted image 20240516114020.png]]

- **Filas fantasma**.
	![[Pasted image 20240516114158.png]]


**SERIABILIDAD**
****
Dado un conjunto de transacciones, una **planificación** es una ordenación de operaciones (R/W) de esas transacciones, de forma que se mantenga el orden dentro de cada transacción.

Un **plan de ejecución en serie** es aquel en el que, para toda transacción Ti, las operaciones de Ti se ejecutan de forma consecutiva.
En caso contrario, se tratará de un **plan no en serie**. Al intercalar operaciones, permiten aumentar la concurrencia.

Los **planes de ejecución en serie** producen resultados correctos. Interesa determinar qué planes no serie producen resultados correctos.
![[Pasted image 20240516114809.png]]

Se dice que dos acciones entran en conflicto si cumplen que:
1. Son de transacciones diferentes.
2. Operan sobre el mismo dato.
3. Por lo menos una de ellas es una operación de escritura.

Dos planes son **equivalentes por conflicto** si, para cada par de operaciones en conflicto, el orden de operaciones es la misma en ambos planes.
Un plan es **serializable por conflictos** si es equivalente por conflictos a algún plan en serie.

![[Pasted image 20240516115104.png]]
A vista de estos planes, podemos deducir que:
1. P2, P3 y P4 son equivalentes por conflictos.
2. P4 es un plan en serie.
3. Por lo tanto, P2 y P3 son serializables por conflictos

- **Grafo de precedencia (grafo de serialización)**
	Sea un grafo G = (N, A):
	1. Se crea un nodo por cada transacción: N = T1, T2, ... , Tn.
	2. Aristas: A = a1, a2, ..., an.
		Se crea una arista Ti → Tk si existe una operación en Ti que aparece antes que una operación en Tk con la que entra en conflicto.
	
	Un plan de ejecución es serializable por conflictos sólo si el grafo de precedencia no tiene ciclos.
	![[Pasted image 20240516120054.png]]

- **Seriabilidad por vistas**
	Dos planes P1 y P2 son equivalentes si, para toda transacción Ti, Tk y todo dato A, se da lo siguiente:
	1. Si Ti lee el valor inicial de A en P1, también lo debe leer en P2.
	2. Si Ti es la transacción que escribe el último valor de A en P1, también debe hacerlo en P2.
	3. Si Ti lee un valor de A escrito por Tk en P1, también debe hacerlo en P2.
		- [k] Serializable por conflictos ⇒ Serializable por vistas

	Si hacemos la suposición de **escritura restringida** (toda escritura va precedida por una lectura del mismo dato), entonces la seriabilidad por conflictos y por vistas son idénticas.

	Si hay **escrituras a ciegas** (escritura de un dato sin hacer lectura previa), entonces puede haber planes serializables por vistas que no lo son por conflictos.
	![[Pasted image 20240516120854.png]]


**BLOQUEOS**
****
El principio básico del funcionamiento de una transacción debe conseguir un bloqueo sobre un ítem de datos antes de poder acceder a él.

*Operaciones básicas*:
1. **S(A)**: solicita bloqueo compartido sobre dato A.
2. **X(A)**: solicita bloqueo exclusivo sobre dato A.
3. **U(A)**: libera bloqueos sobre A o desbloquea A.

*Matriz de compatibilidad*:
![[Pasted image 20240516121708.png]]

*Reglas de funcionamiento*:
1. Antes de leer un dato hay que obtener un bloqueo compartido sobre él.
2. Antes de escribir un dato hay que obtener un bloqueo exclusivo sobre él.
3. Si una transacción solicita un bloqueo y no puede conseguirlo por ser incompatible con algún bloqueo ya concedido, quedará en espera hasta que los bloqueos incompatibles sean liberados.

- **Conversión de bloqueos**
	Dentro de una misma transacción y sobre el mismo dato, un bloqueo puede sufrir cambios:
	1. Promocionar un bloqueo: pasa de modo compartido a exclusivo.
	2. Degradar un bloqueo: pasa de modo exclusivo a compartido.

Una transacción que sigue el **Protocolo de Bloqueo en 2 Fases (2PL)** pasa por dos fases:
1. Fase de adquisición o crecimiento: la transacción puede adquirir nuevos bloqueos o promocionar los ya conseguidos.
2. Fase de liberación o decrecimiento: se inicia cuando la transición libera un bloqueo. A partir de ese momento, no puede adquirir nuevos bloqueos, solo liberar o degradar los existentes.
![[Pasted image 20240516122558.png]]
- **Variantes de 2PL**:
	1. *2PL rigoroso*: las transacciones no liberan los bloqueos exclusivos hasta el momento de confirmarse o anularse.
	2. *2PL estricto*: las transacciones no liberan ningún bloqueo, ni compartido ni exclusivo, hasta el momento de confirmarse o anularse.

El **interbloqueo/deadlock** es una situación en la que dos o más transacciones están en espera, cada una de ellas esperando a que la otra libere algún bloqueo para poder continuar. 
![[Pasted image 20240516122850.png]]
Existen dos formas de tratar los interbloqueos:
- **Técnicas de prevención**:
	Evitan la posibilidad de que se produzca un interbloqueo. Son técnicas pesimistas, porque es posible que se anule una transacción cuando no iba a ocurrir el interbloqueo.
	Se asocia a cada transacción una **marca de tiempo** siguiendo el orden de inicio.
	
	*Técnicas*:
	1. **Esperar-Morir**
		Si M(Ti) < M(Tk), Ti espera.
		SI M(Ti) > M(Tk), Ti muere y se reinicia posteriormente con la misma marca.
	2. **Herir-Esperar**
		Si M(Ti) < M(Tk), Ti ataca a Tk, con lo que Tk aborta y se reinicia posteriormente
		Si M(Ti) > M(Tk), Ti espera.

- **Técnicas de detección**:
	Se utiliza un grafo de espera G = (N, A).
	- N (nodos): cada nodo es una transacción en ejecución.
	- A (aristas): representan las dependencias.
		Se crea una arista si Tk está esperando a que Ti libere un bloqueo por el que está esperando.
	![[Pasted image 20240516124113.png]]

	*Resolución del interbloqueo detectado*:
	- Debe anularse una transacción (rollback).
	- Puede ser un rollback completo o un retroceso parcial.

Tipos de marcas de tiempo:
- *M(Ti)* (Marca de tiempo de la transacción): similar al usado en las técnicas de prevención del interbloqueo. 
- Para cada ítem de datos hay dos marcas de tiempo:
	- *ML(A)* (Marca de Lectura): marca de tiempo de la transacción más reciente que haya leído A.
	- *ME(A)* (Marca de Escritura): marca de tiempo de la transacción más reciente que haya escrito A.

	- [i] La transacción **más reciente** es aquella que tenga la **M(T) más alta**.
	![[Pasted image 20240518105737.png]]

- **Ordenamiento básico por marcas de tiempo**
	Este algoritmo especifica las reglas para leer y escribir un dato dependiendo de las marcas. Detecta acciones en conflicto y, si es necesario, aborta la transacción más reciente. Esta debe reiniciarse con una marca de tiempo nueva.
	![[Pasted image 20240518110140.png]]

- **Ordenamiento estricto por marcas de tiempo**:
	1. Si Ti solicita una operación (lectura/escritura) sobre A, teniendo M(Ti) > ME(A), siendo ME(A) = M(Tk), entonces Ti espera a que Tk finalice antes de realizar la operación.
	2. Es similar a un bloqueo, pero no habrá interbloqueos, ya que la espera solo se realiza si M(Ti) > M(Tk) (si fuese menor, Tk abortaría).

	- **Regla de escritura Thomas**:
		Es una variante del ordenamiento básico. No implementa la seriabilidad por conflictos, pero sí por vistas, y solo tiene aplicación cuando hay escrituras a ciegas. 
		El algoritmo para la lectura no cambia, pero sí el de la escritura:
		![[Pasted image 20240518110705.png]]

- **Multiversión**
	Mejora la concurrencia de los sistemas basados en 2PL o marcas de tiempo, manteniendo varias versiones del mismo dato.
	Cuando se solicita una lectura, esta siempre se podrá hacer y sin esperas.

	Cuando una transacción escribe un dato por primera vez, se creará una nueva versión de dicho dato.
	El ordenamiento multiversión por marcas de tiempo utiliza marcas similares a lo visto anteriormente.
	1. *M(T)*: marca de transacción T, vinculada a su inicio.
	2. Cada dato A tiene asociadas varias versiones A1, A2, ..., An. Cada versión contiene:
		- El valor del dato para la versión Ai.
		- *ML(Ai)*: marca de tiempo de la transacción más reciente que haya leído la versión Ai.
		- *ME(Ai)*: marca de tiempo de la **única** transacción que haya escrito el valor de la versión Ai.
	![[Pasted image 20240518111456.png]]
	![[Pasted image 20240518111921.png]]

**CONCURRENCIA EN SQL**
****
En SQL se puede establecer un **nivel de aislamiento**:
```sql
set transaction isolation level <nivel>
```

Los distintos niveles son:
1. `READ UNCOMMITTED`: permite lecturas no confirmadas.
2. `READ COMMITTED`: no permite lecturas sucias, pero no se garantiza lectura repetible.
3. `REPEATABLE READ`: garantiza lecturas repetibles.
4. `SERIALIZABLE`: la ejecución de las transacciones es equivalente a si se ejecutasen en serie.
	![[Pasted image 20240518112518.png]]
