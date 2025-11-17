---
Name: 8 - Recuperación
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**INTRODUCCIÓN**
****
La recuperación es el proceso que permite devolver una base de datos a un estado consciente después de que ocurriese un fallo que la dejo inconsciente.
Está basada en la **redundancia**, que permite copiar o reconstruir los datos perdidos a partir de otros almacenados previamente.

**ALMACENAMIENTO Y FALLOS**
****
- *Almacenamiento volátil*
	Su contenido se pierde ante una caída del sistema (reinicio por ejemplo).

- *Almacenamiento no volátil*
	Su contenido no se pierde salvo que se produzca un fallo físico del dispositivo.
	Se mantiene ante una caída del sistema.

- *Almacenamiento estable*
	Garante la conservación de la información, nunca se pierde. 

Los datos se guardan en disco (no volátil) y su unidad de transferencia es el *bloque*. Los bloques se transfieren a un espacio en RAM (volátil): el buffer caché. Los programas acceden a los datos individuales utilizando su propia memoria (almacenamiento volátil).![[Pasted image 20240515181241.png]]

Desde el punto de vista de la recuperación, se pueden clasificar los fallos en:
- **Sin pérdida de almacenamiento**: no se pierden datos, pero la BD puede no ser consciente.
- **Con pérdida de almacenamiento volátil**: existen los ficheros de la BD, pero puede que no sean consistentes. Después de recuperar el sistema, debe recuperarse la BD a un estado completo, lo que normalmente implica una utilización de ficheros de log.
- **Con pérdida de almacenamiento no volátil**: los ficheros de BD desaparecieron o son incorrectos. Requiere la utilización de un backup para restaurar los datos y un proceso posterior, usando ficheros de log.

**TRANSACCIONES**
****
Una **transacción** es una unidad lógica de ejecución, formada por una secuencia de una o más operaciones de lectura y/o escritura sobre la base de datos.

Puede acabar de dos formas:
1. Confirmando los datos (`COMMIT`).
2. Anulando los cambios (`ROLLBACK`).

Propiedades **ACID** de una transacción:
- **Atomicity**: se ejecutan todas las operaciones o ninguna.
- **Consistency**: la transacción lleva la BD de un estado consistente a otro consistente.
- **Isolation**: las transacciones se ejecutan de forma independiente unas de otras. Los cambios parciales de una transacción no deberían ser visibles desde otras.
- **Durability**: los cambios realizados por una transacción confirmada quedarán grabadas en la BD y no se perderán incluso en presencia de fallos.

Una transacción está activa durante la ejecución de las operaciones.

Si desde este estado se solicita un `COMMIT`, la transacción pasa a estar **parcialmente confirmada**.
Si todo tiene éxito tras una serie de comprobaciones, pasa a estar **confirmada**. 

Estando activa, si se solicita un `ROLLBACK`, pasa al estado de **fracasada**. Tras las acciones necesarias, pasa a **abortada**.
![[Pasted image 20240515183313.png]]

**RECUPERACIÓN BASADA EN LOGS**
****
Los **ficheros de log** son ficheros de pequeño tamaño que almacenan los cambios realizados en la BD.

Los ficheros de log contienen **registros de transacción** que almacenan:
- Id de la transacción.
- Tipo de registro:
	1. Inicio de transacción: `START`.
	2. Finalización de transacción: `COMMIT`/`ROLLBACK`.
	3. Modificación de datos (DML): `INSERT`, `DELETE`, `UPDATE`.
- Solo para registros de modificación:
	- Id del dato.
	- Imagen anterior: valor del dato antes del cambio (solo para `DELETE` y `UPDATE`)
	- Imagen posterior: valor del dato después del cambio (solo para `INSERT` y `UPDATE`).

Los ficheros de log se utilizan con distintas técnicas de actualización:
- **Actualizaciones inmediatas**
	Cuando se solicita la modificación de un dato:
	1. Se escribe el registro correspondiente en el log.
	2. Se modifica el dato en los buffers de datos de la BD.
	
- **Actualizaciones diferidas**
	Cuando se solita la modificación de un dato:
	1. Se escribe el registro del log, pero **no** se modifican los datos.
	2. La modificación de los datos se hace en el momento del `COMMIT` de la transacción. 

En caso de error, se usan dos operaciones:
1. `redo(Ti)`: rehace los cambios de la transacción Ti, asignando a cada dato modificado la imagen almacenada en el log. También se conoce como *roll forward*.
2. `undo(Ti)`: deshace los cambios de la transacción Ti, en orden inverso al realizado en la transacción. También se conoce como *recuperación hacia atrás*.

Las operaciones *undo* se deben hacer antes que las *redo*.

- ***Actualizaciones inmediatas*** 
	Formato para logs de este ejemplo: <Ti, Id, Imagen anterior, Imagen posterior>
	![[Pasted image 20240515184553.png]]

- ***Actualizaciones diferidas***
	Formato para logs de este ejemplo: <Ti, Id, Imagen posterior>
	![[Pasted image 20240515184741.png]]


Un **checkpoint** fuerza a guardar los cambios de los buffers de la BD en disco.
Lleva a cabo las siguientes acciones:
1. Suspender temporalmente la ejecución de las transacciones en curso.
2. Escribir en los ficheros de log los buffers de log modificados.
3. Escribir en la BD en disco los bloques de datos modificados.
4. Escribir en el log un registro `CHECKPOINT` que incluya la lista de transacciones activas
5. Restaurar la ejecución de las transacciones.

El protocolo **WAL** (Write-Ahead Logging) especifica que siempre se debe registrar el cambio en los logs antes que en la BD física. Esto incluye:
1. Modificar el buffer de log antes que el buffer de datos.
2. Cuando se vuelca un buffer de datos a disco, antes deben volcarse los registros del buffer de log relacionados al fichero de log.

Su objetivo es asegurarse de guardar la información de cómo rehacer/deshacer las transacciones antes de hacer cambios. Si se volcasen a disco los buffers de datos antes y hubiese un fallo antes de volcar a disco el log, no seríamos capaces de rehacer/deshacer las transacciones.

- **Estrategias de volcado de buffers de datos**
	1. *Robar*: una página en el buffer de datos puede volcarse a disco antes de confirmar la transacción.
	2. *No robar*: no puede volcarse antes de confirmar.
	3. *Forzar*: todas las páginas modificadas en el buffer de datos deben volcarse a disco cuando se confirma la transacción.
	4. *No forzar*: no se obliga a volcar a disco al confirmar.

**COPIAS DE SEGURIDAD**
****
Una **copia de seguridad** o **backup** es una copia de ciertos datos que se almacena normalmente en un soporte distinto, que puede ser utilizada para restaurar los datos originales después de su pérdida.

Se debería hacer backup de:
1. Software.
2. Ficheros de configuración/auxiliares.
3. Ficheros de datos de la BD.
4. Logs offline.

Los fallos con pérdida de almacenamiento no volátil requieren uso de backups.
Los pasos a llevar a cabo serán:
1. Si el fallo fue catastrófico, puede perderse el software. Debe restaurarse o reinstalarse el SO y el SGBD y restaurar la configuración y ficheros aux del SGBD.
2. Restaurar los ficheros de datos a partir del último backup.
3. Utilizando los offline logs, devolver la BD al último estado consistente.
4. Si no se perdieron los online logs, aplicarlos para obtener el último estado consistente.

Si el SGBD lo permite, podemos planificar 2 tipos de backups periódicamente:
1. **Backup completo**: hace una copia completa de los ficheros de datos.
2. **Backup incremental**: hace una copia de los bloques de datos que cambiaron desde el último backup.