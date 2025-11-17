---
Name: 4 - Sistemas de producción
tags:
  - teoría
asignatura: SI
---
***[[Sistemas Inteligentes]]***

**INTRODUCCIÓN**
****
Existen dos paradigmas a la hora de realizar un programa:
- *Programas secuenciales*: dependen de los datos, condiciones iniciales, parámetros, respuestas introducidas por los usuarios, resultados de cálculos previos.. El flujo de control a seguir y el uso de los datos está programado implícitamente en el código. El **defecto** está en su secuencialidad: las bifurcaciones que se ejecutan al manipular la información solo se realizan en puntos concretos del código.
- *Programas basados en eventos*: se adecúan a las situaciones donde hay estímulos continuos que obligan a tomar decisiones sobre el flujo de ejecución. El programa debe responder a dichos estímulos tomado un camino u otro, siendo los datos inesperados (no como el secuencial, que se sabe de donde vienen). No se usan estructuras de control, pero los **sistemas de inferencia dirigidos por patrones (SIDP)** deben ser capaces de reconocer patrones en los datos para poder ejecutar determinados trozos del código y realizar las tareas correspondientes.

Un SIDP emplea una estructura modular, cada módulo se encarga de reconocer los patrones y crear la respuesta adecuada. Los módulos se dividen según su función en **antecedente** (lado izquierdo) y **consecuente** (lado derecho). El primero se encarga de acceder a los datos para verificarlos y compararlos con los patrones del módulo, mientras que el segundo se encarga de modificar y/o escribir los datos generados. Estos módulos se denominan "reglas", de modo que los SIPD compuestos por ellas se denominan **Sistemas de Producción** o Sistemas Basados en reglas (SBR).

**SISTEMAS DE PRODUCCIÓN**
****
Es un sistema inteligente basado en unas reglas que trabajan con una base de hechos y usan mecanismos de emparejamiento que forman parte explícita de su arquitectura.
- **Sistemas dirigidos por los datos (antecedentes)**: las inferencias se obtienen cuando los antecedentes de algunas de sus reglas de producción se emparejan con, al menos, una parte de los hechos que describen el estado actual. Cuando se produce el emparejamiento decimos que "se ha activado una regla" y que puede ser ejecutada. Según la estrategia de exploración que tenga asignada, dicha regla puede ejecutarse o no. Son sistemas poco específicos porque ejecutan todas las reglas disponibles en función de la información que reciban.
- **Sistemas dirigidos por los objetivos (consecuentes)**: tanto los antecedentes como los consecuentes de las reglas deben ser ciertos al compararlos con los datos. Si es así, las reglas son activadas regresivamente y el emparejamiento se realiza a través de las conclusiones de las reglas. Para alcanzar una meta, se sigue un proceso recursivo en donde los antecedentes de un módulo son los consecuentes de su módulo anterior. Son sistemas más específicos porque su ejecución conlleva un proceso de búsqueda.

**ARQUITECTURA**
****
- **Base de conocimientos**
	Describe el dominio para el que el sistema de producción debe plantear sus soluciones. Está formado por dos elementos que deben comunicarse continuamente entre ellos, por lo que la información que almacenan/generan debe ser fácilmente comprensible para ambos. Consta de:
	- *Base de hechos (BH)*: almacena y manipula todos los hechos importantes del dominio (todos los conocimientos que tiene), formando el esqueleto declarativo del sistema.
	- *Base de reglas (BR)*: permite construir circuitos de inferencia a través de los cuales es capaz de generar nuevas conclusiones válidas, formando el esqueleto procedimental del sistema.

- **Memoria activa**
	Estructura que almacena toda la información estática necesaria para resolver el problema. Contiene información como los datos iniciales del problema, los añadidos posteriormente, aquellos hechos generados como resultado de los procesos de inferencia y las hipótesis de trabajo que todavía no han sido resueltas. La memoria activa almacena todos los cambios de estado del sistema, de modo que su contenido siempre representará el estado actual del sistema. Se encarga principalmente de interactuar con el mundo exterior para recibir aquella información que el sistema no es capaz de inferir por sí mismo (como los datos que el usuario inserta).
	
	Cuando el proceso de inferencia se detiene, la memoria almacenará el estado final del problema en donde se incluirán los datos, hechos e hipótesis que ha manipulado e inferido. Todos los hechos y datos se corresponden con entidades dentro de la BH, pero con valores concretos asignados. Los datos existentes representan información que procede del mundo real y las hipótesis resueltas son los resultados, cuyos valores se han de investigar y verificar si son correctos en la realidad.

- **Motor de inferencias**
	Está separado físicamente de la sección que manipula el conocimiento y está formado por un Intérprete de Reglas (IR) y una Estrategia de Control (EC). El MI se trata de un programa secuencial cuyo objetivo es determinar el siguiente paso a ejecutar a la hora de inferir un resultado. Sus funciones principales:
	- Examinar la memoria activa y determinar que reglas deben ejecutarse, para lo cual emplea una estrategia de búsqueda y una estrategia de resolución de conflictos. Esto le permite encontrar conexiones entre los estados iniciales del problema y los estados meta.
	- Controlar y organizar la ejecución de las reglas seleccionadas en el paso anterior.
	- Actualizar la memoria activa cuando sea necesario con los datos que se generan en el proceso.
	- Asegurar que el sistema mantiene un estado de autoconocimiento: conocer qué reglas se han activado y cuales se han ejecutado, cuales fueron los últimos datos agregados a la memoria activa, gestionar las prioridades de las reglas que se ejecutan...
	La EC examina la memoria activa y elige qué regla ejecutar según los **ciclos básicos del sistema de producción** y atendiendo también a ciertos parámetros, como el criterio de activación de la regla, las estrategias de búsqueda o la dirección de avance por el espacio de estados.

**EJEMPLO DE CICLO DE UN SISTEMA DIRIGIDO POR DATOS**
****
Supongamos que nuestro motor de inferencia tiene las siguientes **restricciones**:
- Empleará un **encadenamiento progresivo**, sigue una inferencia dirigida por datos.
- Activará todas aquellas reglas que emparejen con la memoria activa.
- Realiza **búsqueda en profundidad** en el espacio de estados, ejecutando la primera regla de entre aquellas que hayan sido activadas más recientemente.
- No ejecutará dos veces la misma regla.
- La inferencia terminará cuando la hipótesis (H) sea un hecho demostrado y se haya agregado a la memoria activa.

Su **funcionamiento** es el siguiente:
1. El emparejador examina los antecedentes de las reglas.
2. Selecciona aquellas que se corresponden con los hechos y datos existentes en la memoria activa.
3. Ejecuta la primera regla de las reglas activadas y su resultado se agrega a la memoria activa.
4. Comprueba si en la memoria activa aparece (H) como un hecho demostrado.
5. En caso contrario, repite el ciclo hasta encontrar una solución o hasta que todas las reglas hayan sido ejecutadas y no se llegue a ninguna solución.
	![[Pasted image 20240506094720.png]]

- **Ciclo 1**:
	Disponemos de los datos A y B en memoria activa, por lo que se activa la regla R4. Añadimos R4 al conjunto de reglas ejecutadas y agregamos su resultado a la memoria activa. Como H aún no forma parte de la memoria activa, debemos seguir activando reglas en ciclos posteriores.
	![[Pasted image 20240506095025.png]]
- **Ciclo 2**:
	Disponemos de C y D en memoria, se activa la regla R2, pero también tenemos E y C, por lo que se activa la regla R7 también. Ejecutaremos las reglas de menor a mayor, por lo que primero va la regla R2, añadiendo G a la memoria activa. Continuamos activando reglas en ciclos posteriores.
	![[Pasted image 20240506095225.png]]
- **Ciclo 3**:
	Disponemos de G en memoria, se activa la regla R5. Añade a memoria activa el dato X y como la hipótesis no está en memoria activa, seguimos activando reglas.
	![[Pasted image 20240506095338.png]]
- **Ciclo 4**:
	Disponemos de X e Y en memoria activa, se activa R1 y obtenemos Z.
	![[Pasted image 20240506095416.png]]
- **Ciclo 5**:
	Disponemos de Z y B en memoria, se activa R6 y agregamos V a la memoria.
	![[Pasted image 20240506095454.png]]
- **Ciclo 6**:
	Disponemos de E y V en memoria, por lo que se activa la regla R3. Agrega H a la memoria de modo que ya tenemos la hipótesis como resultado.
	![[Pasted image 20240506095551.png]]

El motor de inferencias ha necesitado 6 ciclos. El **circuito inferencial** generado es el siguiente:
![[Pasted image 20240506095638.png]]

**EJEMPLO DE CICLO EN UN SISTEMA DIRIGIDO POR OBJETIVOS**
****
Supongamos que nuestro motor tiene las siguientes **restricciones**:
- Empleará **encadenamiento regresivo**, sigue una inferencia dirigida por datos.
- Activará todas aquellas reglas que emparejen con la memoria activa.
- Realiza **búsqueda en anchura** en el espacio de estados.
- No ejecutará dos veces la misma regla.
- La inferencia termina cuando (H) sea un hecho demostrado y se haya agregado a la memoria activa.

Su **funcionamiento** es el siguiente:
1. El emparejador examina las conclusiones de las reglas.
2. Selecciona aquellas que se corresponden con la hipótesis de la memoria activa.
3. Se irán generando sucesivas subhipótesis que irán agregándose a la memoria activa, lo que se conoce como **retropropagación**.
4. Las conclusiones se infieren hacia adelante, por lo que la hipótesis inicial puede ser verificada, lo cual ocurre cuando una regla es directamente ejecutable y pueda establecer una conclusión.

- **Ciclo 1**:
	Debemos buscar aquella regla cuya conclusión sea la hipótesis H, de modo que seleccionamos R3. Tiene como antecedentes E y V, pero como solo tenemos E en la memoria activa, necesitamos obtener otra regla que nos aporte a V como resultado (R6). Añadimos V a la columna de hipótesis porque la necesitaremos para resolver el problema planteado. Como H aún no está como un dato real en la memoria activa, seguiremos activando reglas. Aquí comienza el proceso "hacia atrás".
	![[Pasted image 20240506121608.png]]
- **Ciclo 2**:
	Para obtener V debe activarse R6, que tiene como antecedentes a Z y B, pero como en la memoria activa solo tenemos B, debemos activar aquella que nos aporte como resultado Z. Añadimos Z a la hipótesis y agregamos V como dato.
	![[Pasted image 20240506122050.png]]
- **Ciclo 3**:
	Para obtener Z necesitamos R1, con sus antecedentes X e Y, nos falta X y para obtenerla necesitamos R5. Añadiremos X a la hipótesis y agregamos Z como dato.
	![[Pasted image 20240506122233.png]]
- **Ciclo 4**:
	Para obtener X necesitamos R5, con sus antecedentes F o G, no tenemos ninguno en memoria, así que necesitaremos activar las reglas R2 y R7 para obtenerlas. Agregamos F y G a la hipótesis y agregamos X como dato.
	![[Pasted image 20240506122358.png]]
- **Ciclo 5**:
	Disponemos de Z y B en memoria, se activa R6 agregando V a la memoria. Para obtener G necesitamos C y D en memoria, pero solo tenemos D. Para obtener F necesitamos tener E y C, pero solo tenemos E. Necesitamos obtener C para ambos casos, para lo cual necesitamos R4.
	![[Pasted image 20240506122805.png]]
- **Ciclo 6**:
	Comienza el proceso "hacia adelante". La hipótesis actual que tenemos que demostrar es la última añadida a la columna Hipótesis, la cual nos dice que tenemos que obtener C, necesitamos R4, cuyos antecedentes son A y B que están en memoria activa.
	![[Pasted image 20240506122943.png]]
- **Ciclo 7**:
	Debemos obtener F o G, para lo cual podemos ejecutar R2 o R7. Para evitar que F o G permanezcan en la memoria activa como hipótesis, ejecutaremos R2 y R7 a la vez para obtener los dos resultados. 
	![[Pasted image 20240506123014.png]]
- **Ciclo 8**:
	Debemos obtener X, para lo que necesitamos R5.
	![[Pasted image 20240506123154.png]]
- **Ciclo 9**:
	Debemos obtener Z, para lo que necesitamos R1.
	![[Pasted image 20240506123244.png]]
- **Ciclo 10**:
	Debemos obtener V, para lo que necesitamos R6.
	![[Pasted image 20240506123311.png]]
- **Ciclo 11**:
	Debemos obtener H, la hipótesis del principio, para lo que necesitamos R3.
	![[Pasted image 20240506123400.png]]

El motor de inferencias ha necesitado 11 ciclos. El **circuito inferencial** es el siguiente:
![[Pasted image 20240506123443.png]]

**FASES DE DECISIÓN Y EJECUCIÓN**
****
Un ciclo está compuesto de una **fase de decisión**, en la que se selecciona una de las reglas activadas, y una **fase de ejecución**, en la que se ejecuta y obtiene los resultados de ejecutar dicha regla. A la hora de ejecutar la fase de decisión deben tenerse en cuenta 3 factores:
- **Restricción**: debe simplificar el proceso de comparación entre reglas. Han de eliminarse del motor de inferencias aquellas reglas que no tienen nada que ver con el estado actual (representado por los datos que almacena la memoria activa en cada ciclo).
- **Equiparación**: se aplica al emparejamiento y trata de identificar qué reglas son relevantes dentro del contexto actual para poder aplicarlas. El resultado se denomina **conjunto conflicto** e incluye todas las reglas útiles para resolver el problema planteado.
- **Resolución de conflictos**: la decisión de qué regla ejecutar está condicionada por la estrategia de búsqueda que se emplea para seleccionar las reglas que se activan.