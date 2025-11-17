---
Name: 2 - Paralelismo
tags:
  - teoría
asignatura: CP
---
***[[Concurrencia y Paralelismo]]***


**INTRODUCCIÓN**
****
1. *Modelo secuencia de computador* (Arquitectura von Neumann)
	Idea de que los programas y los datos deben ser almacenados en la misma memoria. La unidad de procesamiento (CPU) debe poder acceder a ellos de manera secuencial. La arquitectura consta de cuatro componentes principales: CPU, memoria, E/S, dispositivos de almacenamiento externos.
	Las instrucciones se ejecutan en serie y eso limita mucho la potencia computacional. Sus limitaciones principales son:
	- **Ley de Moore**: el n.º de transistores crece con el tiempo de forma exponencial. Es necesario poder disipar el calor y reducir el tamaño de los componentes.
	- **3 muros**: n.º de chips, frecuencia y memoria.
	- **Tamaño de ciertos problemas**.
	- **Resolución de problemas a tiempo real**.
	- **Complejidad de ciertos problemas**.

2. *Computación paralela*
	Se basa en la ejecución simultánea de instrucciones. La **High-Performance Computing**se basa en la computación paralela, consistiendo en el uso de sistemas informáticos altamente especializados para resolver problemas computacionales complejos y exigentes que no pueden ser resueltos por computadoras convencionales en un tiempo razonable.

**NIVELES DE PARALELISMO**
****
1. **Nivel Hardware**
	A nivel de hardware, para paralelizar se replican los componentes, como los cores de CPU, buses, memoria... Además se usan aceleradores de hardware como GPUs.
	Existen dos modelos de computadores paralelos:
	- *Computadoras paralelas de memoria distribuida*: cada procesador tiene su propia memoria local y los procesadores se comunican a través de una red de interconexión. Cada procesador trabaja en su propia porción de los datos y los resultados se combinan al final para obtener el resultado final. Cada procesador tiene acceso solo a su propia memoria local y debe comunicarse con otros procesadores para acceder a la memoria de otro procesador.
	- *Computadoras paralelas de memoria compartida*: todos los procesadores comparten un espacio de memoria común. Cada procesador puede acceder a cualquier ubicación de la memoria compartida, lo que permite un acceso rápido y fácil a los datos. Los procesadores pueden trabajar en diferentes partes de los datos al mismo tiempo, pero deben coordinarse para evitar conflictos de acceso a la memoria.
2. **Nivel Software Básico**
	Está formado por el SO, los gestores de recursos y el middleware. Es la capa que permite comunicar al hardware paralelizado con el software.
3. **Nivel Software Intermedio**
	Formado por todas las herramientas que permiten el manejo del hardware paralelizado e implementar aplicaciones paralelas. Ejemplos de este software son las librerías, compiladores, debuggers, analizadores de rendimiento...
	Existen 2 principales enfoques:
	- *Compilador de lenguaje paralelo o de lenguaje secuencial con directivas paralelas*: se utiliza en computadoras de memoria compartida.
	- *Librerías de paso de mensajes*: se utilizan en computadoras de memoria distribuida.
4. **Nivel Software**
	Son los códigos desarrollados por los usuarios que son ejecutables en un computador paralelo.
5. **Nivel Aplicación**
	Es el conjunto de todos los niveles anteriores.

**DEPENDENCIAS DE DATOS**
****
Para que las tareas puedan ser ejecutadas en paralelo, no puede haber dependencias entre ellas. Las dependencias de datos son:
- **Dependencia de flujo (RAW o verdadera)**: cuando una instrucción depende del resultado de otra que todavía no ha terminado su ejecución. Por ejemplo: 
	```armasm
	ADD r1, r2, r3;
	SUB r4, r1, r5;
	```
- **Antidependencia (WAR)**: ocurre cuando una instrucción intenta escribir en una posición de memoria antes de que otra instrucción la lea. Por ejemplo:
	```armasm
	ADD r3, r1, r3;
	ST r2, (0x01)
	```
- **Dependencia de salida (WAW)**: cuando dos instrucciones escriben en la misma posición de memoria. Puede generar un resultado incorrecto, dependiendo de cual se acabe de ejecutar primero. Por ejemplo:
	```armasm
	ADD r1, r2, r3;
	ADD r1, r4, r5;
	```


**MODELO DE PASO DE MENSAJES**
****
Consiste en comunicar uno o más procesos mediante rutinas que permiten recibir y enviar mensajes entre procesos. Es el programador el que tiene que evitar las dependencias, interbloqueos y race conditions.

El modelo de ejecución de un programa de paso de mensajes es un programa paralelo compuesto por múltiples tareas que utilizan su propia memoria local. Normalmente la relación es de una tarea por cada elemento de procesado. Cada envío de mensaje necesita un receptor. Pueden ser:
- **MPMD** (Multiple Program Multiple Data): cada tarea realiza su propio programa.
- **SPMD** (Single Program Multiple Data): todas las tareas comparten el mismo programa. Se divide internamente el código.

La estructura básica de un programa MPI es:
```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char *argv[]){
	int numprocs, rank, namelen;
	char processor_name[MPI_MAX_PROCESSOR_NAME];

	MPI_Init(&argc, &argv);
	MPI_Comm_size(MPI_COMM_WORLD, &numprocs);
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);

	//código
	
	MPI_Finalize();
}
```

- **Tipos de operaciones**
	1. *Punto a punto*: van de un proceso origen a un proceso destino. Tienen argumentos *dest* o *source*.
	2. *Colectivas*: involucran a todos los procesos a la vez. Suelen tener un argumento *root*.
	3. *Bloqueantes*: los procesos esperan a que el mensaje se reciba.
	4. *No bloqueantes*: los procesos continúan independientemente del receptor.
- **Tag y Status**
	El tag es un número que sirve para identificar mensajes punto a punto. Si no necesitamos ninguna etiqueta podemos usar la constante `MPI_ANY_TAG`.

	Es status es una estructura que contiene la información de la operación. SI no necesitamos el estado, podemos usar la constante `MPI_STATUS_IGNORE`. Contiene el número de elementos enviados, el origen y el tag del envío.

**ANÁLISIS DE ALGORITMOS PARALELOS**
****
Los objetivos de la programación paralela son reducir el tiempo de ejecución y acceder a recursos computacionales no presentes en un único procesador. Para ello son necesarios los algoritmos paralelos.

En el rendimiento de un programa secuencial influyen varios factores:
1. El lenguaje de implementación.
2. El compilador.
3. Los tipos de datos utilizados.
4. El número de FLOPS.
5. La complejidad del algoritmo.
6. Los recursos de hardware.
7. El acceso compartido al hardware.

En el rendimiento de un programa paralelo influyen otros factores:
1. La compilación es más lenta.
2. Las librerías suelen estar menos optimizadas.
3. Influyen más elementos.
4. La gestión de procesos produce cierta sobrecarga.
5. Existen problemas no paralelizables.

- **Ley de Amdahl**
	El tiempo de ejecución paralela es el tiempo que transcurre desde que empieza la ejecución en el primer procesador hasta que termina en el último procesador.

	La idea de la programación paralela es que con *p* procesadores, el problema se resuelva *p* veces más rápido que de forma secuencial. Sin embargo, esto no se cumple totalmente debido a los problemas mencionados. Los problemas se dividen en dos fracciones, *Fs* (secuencial) y *Fp* (paralelizable).
	
	$$T_{secuencial} = F_s + F_p$$$$T_{paralelo}(p) = F_s + F_p/p$$
	$$A_{paralela}(p) = T_s/T_p$$
	$$A_{max} = (F_s + F_p)/F_s$$

**MEDICIONES DE PRESTACIONES**
****
Las medidas son necesarias para:
- **Eficacia**: reducción del tiempo de ejecución.
- **Eficiencia**: reducción del tiempo de ejecución utilizando los menores recursos posibles. Toma valores entre 0 y 1, aunque puede ser superior a 1 en el caso de aceleración superlineal.
	$$Eficiencia = A/p$$
- **Aceleración**: toma valores entre 0 y *p*, aunque puede ser mayor si el algoritmo paralelo reduce el número de fallos caché (aceleración superlineal).
- **Coste**: suele ser mayor que el tiempo secuencial.
	$$Coste = p *  T_{paralelo}$$
	$$Sobrecarga = Coste - T_{secuencial}$$
- **Escalabilidad**: capacidad de mantener la eficiencia en la resolución de un problema al disponer de mayores recursos.
	- *Fuerte*: capacidad de un sistema paralelo para manejar una carga de trabajo fija de manera más rápida a medida que se agregan más recursos de hardware, como procesadores o núcleos de procesador. En otras palabras, si se duplica el número de recursos de hardware en un sistema paralelo escalable fuertemente, el tiempo de ejecución debería reducirse a la mitad, manteniendo la misma carga de trabajo.
	- *Débil*: capacidad de un sistema paralelo para mantener un nivel constante de eficiencia a medida que se agregan más recursos de hardware y se aumenta la carga de trabajo. En otras palabras, si se duplica el número de recursos de hardware en un sistema escalable débilmente, la cantidad total de trabajo que se puede realizar en un período de tiempo determinado debería aumentar en un factor de dos.


**METODOLOGÍA DE PROGRAMACIÓN PARALELA**
****
Un algoritmo secuencial describe una secuencia de operaciones de cómputo. El diseño de algoritmos multiprocesador debe tener en cuenta:
- Concurrencia entre procesadores.
- Asignación de datos y código a cada procesador.
- Acceso simultáneo a datos compartidos.
- Escalabilidad.

Se hace en dos pasos:
1. **Descomposición**: se descomponen los cálculos en tareas de grano fino, se encuentran sus dependencias y se busca la parte paralelizable.
2. **Asignación**: establece qué tareas son llevadas a cabo por cada proceso (NO POR CADA PROCESADOR).

**DESCOMPOSICIÓN EN TAREAS**
****
Los algoritmos paralelos se basan en trocear un cálculo complejo en trozos de menor tamaño. Cada una de las unidades resultantes se denomina tarea. El objetivo es que sean independientes entre sí para poder llevarlas a cabo de forma concurrente.

Por ejemplo, tenemos funciones polinomiales que queremos evaluar sobre un valor, o sea, $x = valor$ y obtener el mínimo $y$ resultante. Podemos representar los coeficientes como una matriz.
![[Pasted image 20240521124733.png]]

Otro ejemplo es la suma de vectores. Podemos sumar cada conjunto de términos de forma independiente:
![[Pasted image 20240521124803.png]]

- **Granularidad**
	Las tareas resultantes de una descomposición pueden tener distintos costes computacionales. La granularidad puede ser:
	- *Fina*: muchas tareas pequeñas.
	- *Gruesa*: pocas tareas pero de gran tamaño.
	
	Lo fina que puede ser la granularidad tiene un límite. Por ejemplo, en la suma de vectores, una tarea por cada posición. O en la comparación del resultado de los polinomios, (número de polinomios - 1) comparaciones.

- **Grafo de dependencias**
	Existen tareas que dependen del resultado de otras. Por ello, ciertas tareas no pueden empezar hasta que otras terminen. Esto se puede representar con un grafo de dependencias.

	Es un grafo acíclico y dirigido. Cada nodo representa una tarea y cada arista una dependencia. Por ejemplo:
	![[Pasted image 20240521125121.png]]

- **Grado de concurrencia**
	Es la medida que indica cómo de paralelizable es un algoritmo en base a sus dependencias. El grado máximo de concurrencia indica el máximo número de tareas que pueden ser ejecutadas a la vez. Se suele deducir del grafo de dependencias. Por ejemplo en el anterior, el grado máximo es 4.

	El máximo grado de concurrencia es un indicador de lo paralelizable que es una determinada descomposición, pero sólo de forma puntual. El grado medio de concurrencia es mejor indicador global.

	Un **camino crítico** es el camino más largo del grafo que va desde un nodo de comienzo hasta uno de finalización. Su coste se obtiene mediante la suma del coste de cada nodo que lo compone. El grado medio de concurrencia es la suma de los costes entre el coste del camino crítico. Por ejemplo:
	![[Pasted image 20240521130148.png]]

	Normalmente, cuanto menos es la granularidad, mayor es el grado medio de concurrencia. Por ello se deben buscar descomposiciones del grano más fino posible.

	Se puede incluir el coste de las comunicaciones en las aristas. Además, dependiendo de la técnica de descomposición, el grafo de dependencias puede sufrir cambios.

- **Factores a considerar**
	1. La creación de tareas puede ser estática, es decir, todas las tareas se crean al inicio de la ejecución; o dinámica, las tareas se crean durante la ejecución del algoritmo.
	2. El número de tareas obtenido es aconsejable que sea mayor que el de procesadores y que escale con el tamaño del problema.
	3. Balanceo de carga del coste.

**TÉCNICAS DE DESCOMPOSICIÓN**
****
- **Descomposición de dominio**
	Se usa en estructuras de datos de gran tamaño. Se hace en dos fases:
	1. *Troceo de datos*: se divide la estructura de datos en subdominios más pequeños, centrándose en la estructura más grande o la más utilizada. De esta forma, se facilita el manejo de los datos y se reduce la complejidad del problema.
	2. *Asociación de la computación a cada trozo*.

	Por ejemplo, para multiplicar una matriz por un vector, cada una de las tareas calculará columnas/procesos filas de la matriz.

	La regla del propietario nos dice que la tarea a la que se le asigna un dato es responsable de realizar todos los cálculos asociados al mismo. No siempre es posible de aplicar en todas las situaciones. En algunos casos, puede haber datos que necesiten ser procesados por varias tareas, lo que puede complicar la asignación de responsabilidades y hacer que la aplicación de la regla del propietario sea más difícil. Puede ser:
	- *Centrada en la entrada*: todas las computaciones que usen los datos de entrada serán realizadas por la tarea a la que se le asigna la entrada. Esto significa que la tarea será responsable de realizar todas las operaciones necesarias para procesar los datos de entrada y producir los resultados necesarios.
	- *Centrada en la salida*: la salida es computada por el proceso al que se le asigna la salida. Esto significa que la tarea será responsable de realizar todas las operaciones necesarias para producir los resultados y entregarlos al proceso que solicita la salida.

- **Descomposición funcional**
	Consiste en descomponer el cálculo en tareas según las partes diferenciadas. Se hace en dos pasos también:
	1. *Identificación de las fases del cálculo*.
	2. *Asignación de tareas a cada fase*.

	El grafo resultante suele tener forma de pipeline, es decir, una serie de pasos o procesos interconectados que se llevan a cabo en secuencia para procesar datos de manera eficiente y automatizada.

	Por ejemplo, para calcular una matriz por un vector por el vector ordenado. Se multiplica el vector por la matriz y se crea el vector ordenado de forma paralela. Una vez listo, se hace el producto escalar. Cada una de las tareas se podría descomponer.

- **Descomposición recursiva**
	Se basa en hacer divide y vencerás.
	1. Dividir el problema original en subproblemas más pequeños, de manera que cada subproblema sea lo suficientemente simple como para poder resolverse fácilmente.
	2. Resolver recursivamente cada subproblema por separado, utilizando la misma técnica de descomposición.
	3. Combinar los resultados de cada subproblema para obtener la solución del problema original.

	Si un subproblema ha alcanzado un tamaño crítico, se puede obtener su solución parcial sin necesidad de descomponerlo aún más. Este enfoque se utiliza a menudo en problemas que tienen una estructura recursiva natural, como la ordenación de una lista de elementos.

	Por ejemplo, para el cálculo de áreas bajo la curva de una función. Calculándola de forma geométrica tendríamos cierto error. Podemos descomponer la integral en dos partes y calcularlo de forma geométrica para reducir el error. Hacemos esto de forma recursiva hasta que el error sea lo suficientemente pequeño y luego sumamos todo.

- **Descomposición especulativa**
	Estrategia utilizada en ciertos problemas en los que no es posible dividirlos en tareas independientes, pero sí se pueden dividir funcionalmente en fases que dependen condicionalmente de los resultados de fases anteriores.

	En esta estrategia se inicia la ejecución de tareas condicionales excluyentes, sin esperar a la finalización de las tareas de las que dependen para seleccionar una de ellas. Esto significa que se ejecutan diferentes tareas de forma especulativa, asumiendo que se cumplirá una determinada condición en función de los resultados parciales obtenidos.

	Una vez que la tarea de selección ha finalizado, se puede elegir el resultado correcto de entre los obtenidos por las tareas ejecutadas de forma especulativa. En otras palabras, se toma una decisión basada en la condición evaluada y se descartan los resultados incorrectos o no deseados generados por las tareas especulativas.

	Esta descomposición especulativa permite avanzar en la ejecución del problema, incluso cuando no se disponen de todos los resultados necesarios para tomar una decisión definitiva.

	Al ejecutar las tareas de forma especulativa, se pueden obtener resultados parciales que ayudan a guiar la selección final una vez se tenga la información completa.

	Por ejemplo, este problema se podría descomponer como:
	![[Pasted image 20240525121716.png]]
	
	$T_{sec} = T + T = 2T$

	El tipo de elemento tiene una tasa de acierto del 80%. Por tanto, si lo hacemos de forma especulativa:
	![[Pasted image 20240525121826.png]]

	$T_{esp} = 0.8T + 0.2(2T) = 1.2T$

**ASIGNACIÓN DE TAREAS**
****
Tras la descomposición, el siguiente paso es definir una correspondencia entre las tareas y los procesos que las van a ejecutar. Por ejemplo:
![[Pasted image 20240525122047.png]]
![[Pasted image 20240525122058.png]]

Se busca minimizar el tiempo de computación, comunicaciones e inactividad entre los procesos (desequilibrios de carga y dependencias).

- **Esquemas de asignación estática**
	Cuando se conoce el grafo de tareas antes de la ejecución, se pueden tomar antes las decisiones de asignación de tareas. Hay dos tipos:
	1. Descomposición de dominio → distribución por bloques.
	2. Descomposición funcional o recursiva → grafos de dependencias.

- **Distribución por bloques**
	Si tenemos P procesos, a cada proceso se le asignará un bloque de tamaño N/P redondeado hacia arriba. Por ejemplo, si tenemos 20 bloques y 3 procesos:
	1. P1 → 0-6
	2. P2 → 7 -13
	3. P3 → 14-19 (al último se le asignan los bloques restantes)

	Esto mismo se puede hacer por columnas o bloques de columnas y filas. Generalizando esto en k dimensiones, el tamaño de una sub-matriz en la dimensión d sería:
	![[Pasted image 20240525123300.png]]

- **Distribución cíclica por bloques**
	El objetivo de la distribución cíclica por bloques es balancear la carga de forma equitativa entre los procesos, independientemente de la regularidad de los cálculos sobre la matriz.
	![[Pasted image 20240525124021.png]]

- **Grafos de dependencias estáticos**
	Existen muchas situaciones donde el cálculo no se define como una matriz, sino como un grafo de dependencias. Por ejemplo, en el producto escalar se forma un árbol binario de orden k. Para aplicar una reducción a k datos necesitamos log2(k) pasos.
	![[Pasted image 20240525124252.png]]

- **Esquemas de asignación dinámica centralizados**
	En un esquema centralizado, todos los problemas o tareas se mantienen en una única ubicación centralizada. Un proceso maestro se encarga de administrar y distribuir la carga de trabajo a los procesos esclavos. El proceso maestro asigna trabajos repetidamente a cada uno de los procesos esclavos, que realizan el cómputo. A medida que los esclavos procesan los trabajos asignados, pueden devolver resultados y, en algunos casos, generar nuevos subproblemas. Estos subproblemas se envían de vuelta al proceso maestro para que los agregue a la colección centralizada de problemas.

	Este enfoque es efectivo cuando se trabaja con un número moderado de procesos esclavos y el coste de ejecutar subproblemas es alto en comparación con el coste de obtenerlos. En otras palabras, si la ejecución de cada subproblema lleva mucho tiempo y el tiempo dedicado a las comunicaciones con el proceso maestro es relativamente pequeño en comparación, entonces este esquema es beneficioso.

	Sin embargo, si el coste de ejecutar los subproblemas es bajo en comparación con el tiempo que lleva comunicarse con el proceso maestro, entonces las comunicaciones con el proceso maestro se vuelven un cuello de botella. Esto significa que la eficiencia general del sistema puede verse afectada debido a la sobrecarga de comunicación entre el proceso maestro y los esclavos.

	Algunos esquemas son:
	1. **Planificación por bloques**: estrategia de asignación de tareas en sistemas distribuidos donde los procesos esclavos capturan tareas en bloques de subproblemas. En lugar de recibir una tarea a la vez, los esclavos toman un bloque de subproblemas para procesar. Esto reduce la contención en las comunicaciones con el proceso maestro, ya que los esclavos no tienen que comunicarse constantemente para solicitar nuevas tareas. Sin embargo, es importante tener en cuenta el tamaño de los bloques de subproblemas. Si los bloques son demasiado grandes, puede haber desequilibrios de carga. Esto significa que algunos esclavos pueden terminar su bloque de subproblemas antes que otros, lo que puede generar una carga desigual hacia el final de la ejecución. Por lo tanto, es necesario encontrar un equilibro adecuado en el tamaño de los bloques para evitar desequilibrios significativos.
	2. **Colecciones locales de subproblemas**: cada esclavo almacena localmente algunos de los nuevos subproblemas generados para su resolución. Esto también reduce la contención en las comunicaciones con el proceso maestro, ya que los esclavos no tienen que enviar cada subproblema generado inmediatamente al maestro. Sin embargo, el almacenamiento local puede generar desequilibrios de carga si los esclavos almacenan demasiados subproblemas sin enviarlos al maestro. Si un esclavo acumula demasiados subproblemas sin compartirlos con el maestro, puede haber una carga desigual en la distribución de tareas y recursos.
	3. **Captación anticipada**: es una estrategia en la que los esclavos superponen la recepción de nuevos subproblemas con el cálculo de los subproblemas anteriores. En lugar de esperar a que un subproblema se resuelva por completo antes de recibir el siguiente, los esclavos pueden recibir continuamente nuevos subproblemas mientras trabajan en los anteriores. Esto mejora el grado medio de concurrencia, lo que significa que los esclavos pueden realizar cálculos en paralelo de manera más eficiente.

	El algoritmo finaliza cuando la colección de subproblemas está vacía o cuando todos los procesos esclavos han solicitado nuevos subproblemas.

- **Esquemas de asignación dinámica descentralizados**
	En un esquema completamente descentralizado no hay un maestro central que controle la asignación de subproblemas. En su lugar, los subproblemas se distribuyen entre los repositorios locales de cada proceso. Cada proceso es responsable de gestionar los subproblemas en su propio repositorio y contribuye a la solución general del problema.

	El equilibrio de la carga es una parte importante en los esquemas de asignación dinámica. Puede ser iniciado por el receptor o el emisor. 
	Cuando es iniciado por el receptor, es el proceso que necesita trabajos el que solicita subproblemas al resto de los procesos.
	Por otro lado, cuando es iniciado por el emisor, un proceso que tiene un gran número de subproblemas en su repositorio local decide distribuirlos entre otros procesos para equilibrar la carga de su trabajo.

	Además, existe la posibilidad de utilizar esquemas mixtos donde se combinan ambas estrategias dependiendo de la carga global del sistema. Si se puede estimar la carga global, se puede decidir cuándo aplicar el equilibrio de carga iniciado por el receptor o por el emisor.

	Los modos de seleccionar el proceso al que se le asigna el trabajo son:
	1. **Sondeo aleatorio**: el proceso que solicita trabajo elige al proceso receptor de forma aleatoria. Esto significa que no hay un patrón predefinido o criterio específico para seleccionar al proceso receptor. La elección aleatoria tiene la ventaja de equilibrar las solicitudes de trabajo recibidas por cada proceso, ya que no hay preferencia sistemática hacia un proceso en particular. Sin embargo, también puede haber cierta variabilidad en la carga de trabajo asignada a cada proceso debido a la aleatoriedad.
	2. **Sondeo cíclico**: cada proceso mantiene una variable, llamada "x", que se inicializa a un valor específico, por ejemplo i+1, donde "i" es el identificador del proceso. Esta variable indica el proceso al que el proceso actual debe solicitar trabajo. En cada petición de trabajo, la variable "x" se incrementa cíclicamente, lo que significa que se salta al siguiente proceso en secuencia. La ventaja del sondeo cíclico es que, en caso de no obtener trabajo de un determinado proceso, se solicita a otro proceso diferente. Esto ayuda a evitar la dependencia excesiva de un proceso en particular y distribuye las solicitudes de trabajo de manera más equitativa. Sin embargo, un inconveniente potencial es que si existe un alto acoplamiento entre los procesos, es posible que varios procesos realicen peticiones simultáneas al mismo proceso receptor, lo que puede generar cuellos de botella y afectar negativamente al rendimiento del sistema. Una posible solución para mitigar este inconveniente es centralizar la variable "x", lo que significa que un proceso centralizado controla y actualiza el valor de "x" para todos los procesos. Esto puede ayudar a evitar las peticiones simultáneas al mismo proceso receptor, ya que se garantiza que la asignación de trabajo sea controlada y equilibrada. Sin embargo, esta solución también incremente los costes de interacción, porque se requiere una mayor comunicación entre los procesos y el proceso centralizado encargado de gestionar la variable "x".

	En un sistema que trabaja en forma de anillo:
	1. Cada proceso puede estar en un estado inactivo, resolviendo problemas; o en un estado de espera, cuando se queda sin trabajo y ningún otro proceso puede cederle tareas.
	2. Cuando el proceso 0 pasa al estado de espera porque ha agotado su trabajo, envía un mensaje especial llamado "testigo de detección de fin" al proceso anterior en el anillo. Este mensaje indica que el proceso 0 ha finalizado su trabajo y está esperando la señal de finalización de los demás procesos.
	3. Cuando un proceso j (distinto de 0) recibe el testigo de detección de fin, se evalúa su estado actual. Si el proceso j está en estado de espera, retransmite el mensaje al proceso anterior en el anillo. Esto se hace para que el testigo de detección de fin continúe circulando por el anillo y se propague a los procesos que aún están activos.
	4. Si el proceso j está activo, retiene el testigo hasta que termine su trabajo y pase al estado de espera. En ese momento, el proceso j también retransmite el mensaje al proceso j-1 para que el testigo siga circulando.
	5. Este proceso de retransmisión continúa hasta que el testigo llega nuevamente al proceso 0. Cuando lo recibe de nuevo, sabe que todos los procesos han pasado al estado de espera, lo que indica que todos han finalizado su trabajo.

	En el algoritmo de terminación de Dijkstra:
	1. Todos los procesos y el testigo se establecen inicialmente en color blanco.
	2. Cuando un proceso "i" envía un mensaje de trabajo a otro proceso "j", donde j > i, el proceso i cambia su color a negro. Esto indica que el proceso "i" ha enviado trabajo y está en estado activo.
	3. Si un proceso en estado de espera recibe el testigo para reenviarlo y ese proceso tiene color negro, reenvía el testigo manteniendo el color negro. Después de reenviar el testigo, el proceso cambia su color a blanco. Esto asegura que el testigo mantenga el color negro cuando haya procesos en espera que hayan enviado trabajo recientemente.
	4. Si el proceso 0 recibe un testigo de color negro, cambia su color a blanco. Se hace para garantizar que el proceso 0 pueda recibir el testigo con color blanco solo cuando todos los procesos hayan terminado.
	5. El algoritmo termina cuando el proceso 0, estando en color blanco, recibe un testigo de color blanco. Esto indica que todos los procesos han finalizado y que se ha completado la ejecución del algoritmo.

	Al introducir los colores y las reglas de cambio de color, se evita el problema de que un proceso reactivado por otro proceso pueda interrumpir incorrectamente la terminación del algoritmo. Los colores negro y blanco se utilizan como marcas para indicar si un proceso ha enviado trabajo recientemente o no, y el testigo con su color correspondiente se propaga y se verifica adecuadamente durante el proceso de terminación.