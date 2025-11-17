---
Name: 4 - Tácticas de rendimiento
tags:
  - teoría
asignatura: AS
---
***[[Arquitectura Software]]***

- [b] Las tácticas son estrategias complementarias independientemente de cuál sea la arquitectura elegida en el sistema. Se usan cuando queremos potenciar una determinada característica **no funcional**.

El **rendimiento** es un requisito no funcional que va a determinar un **rango** temporal aceptable para las respuestas a eventos que un sistema va a tratar. 
Este intervalo tiene que ser **cuantificable**, tenemos que ser capaces de realizar una prueba para dicho rendimiento.

El rendimiento se mide en:
1. *Latencia*: tiempo en procesar un evento.
2. *Deadline*: no tardar más de d.
3. *Throughput*: eventos procesados en t.
4. *Jitter*: variabilidad del throughput.
5. *Tasa de fallos*.

Los datos pueden llegar en diferentes distribuciones:
1. *Periódicos*.
2. *Estocásticos*: hay un rango.
3. *Esporádicos*: no hay ninguna distribución.

Queremos controlar la **latencia** de los eventos, pudiendo reaccionar ante una evidencia de un evento o ante una secuencia de ellos.

Aunque estamos hablando solo de peticiones, hay sistemas que tienen que actuar cuando pasa cierto tiempo, cuando llega un mensaje o cuando hay un cambio en el estado.

Hay que tener en cuenta si el sistema está cumpliendo con el rendimiento estando **activo** o **inactivo**, pudiendo aplicar en cada caso unas tácticas u otras.

**TÁCTICAS DE DEMANDA DE RECURSOS**
****
- **Incremento de la eficiencia computacional**: optimizar la eficiencia de algoritmos para reducir el consumo de recursos.
- **Reducir la sobrecarga computacional**: eliminar intermediarios. Dependiendo de la arquitectura puede traer consecuencias → en la arquitectura por capas, saltarnos capas.
- **Gestión de ratio de eventos**: atender menos eventos, destino los recursos que tengo a resolver menos eventos, para que al menos estos estén en el rango aceptable.
- **Control de la frecuencia de muestreo**: si estoy haciendo cosas y todas me llevan mucho tiempo, voy a hacer menos.
- **Limitar el tiempo de ejecución**: si me lleva más de cierto tiempo responder una petición, la interrumpo. De esta manera no retrasamos en responder otras.
- **Limitar el tamaño de los buffers**: poner un límite a las colas de entrada y salida. Hasta que no se procesen, no se reciben más, evitando que las propias colas se conviertan en una amenaza al rendimiento.

**TÁCTICAS DE GESTIÓN DE RECURSOS**
****
- **Introducir concurrencia**: intentar siempre eliminar secuencialidad, usando threads, llamadas asíncronas...
- **Replicación de datos o de procesos**: muchas veces la concurrencia ya implica introducir procesos. También con datos, si vamos a tener que acceder a la misma información varias veces, aunque esto suponga un coste.
- **Incremento de los recursos disponibles**: más capacidad de proceso, más almacenamiento.

**TÁCTICAS DE ARBITRAJE DE RECURSOS**
****
Estableceremos cómo será el uso de los recursos.

- **FIFO**: atiendo a las peticiones en orden de llegada.
- **Prioridad fija**: asigno estáticamente una prioridad a un grupo de usuarios y atiendo según dicha prioridad.
	- [i] Tiene un peligro, inanición de las prioridades más bajas.
- **Prioridad dinámica**: la prioridad a lo largo del tiempo se va reasignando para evitar esa inanición y garantizar que se van a atender todas → *Round Robin*.
- **Arbitraje estático**: conseguimos que cualquier petición, independiente de la prioridad, tenga ciertos momentos en la que la podamos parar y reemplazar por otra. Teniendo la posibilidad de que la primera puede seguir más adelante.

**RESUMEN**
****
![[Pasted image 20240524103817.png]]
