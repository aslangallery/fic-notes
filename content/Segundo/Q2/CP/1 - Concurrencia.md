---
Name: 1 - Concurrencia
tags:
  - teoría
asignatura: CP
---
***[[Concurrencia y Paralelismo]]***

- **Programa concurrente**: diseñado para ejecutar varias tareas de forma simultánea.

**ARQUITECTURA HW**
****
- **Sistemas multiprocesador**: más de un procesador en el mismo sistema.
- **Sistemas multinúcleo**: dentro del mismo circuito integrado, más de un núcleo/procesador.
Los sistemas multiprocesador modernos son también multinúcleo normalmente.
Cada núcleo puede ejecutar más de una tarea. Los tipos son:
- **Temporal**: aumenta el rendimiento del procesador porque elimina las burbujas.
- **Simultánea**: duplicación dentro del pipeline de algunas partes del núcleo.

**ARQUITECTURA SW**
****
Un proceso es una instancia de la ejecución de un programa. Puede haber más de un proceso ejecutándose a la vez para el mismo programa. El kernel copia la TP del padre.

**THREADS**
****
Un thread es un hilo de ejecución de un proceso. Comparten memoria, fd, sockets y señales.
- **Creación**
```C
#include <pthread.h>
int pthread_create( 
	pthread_t *thread, // Identificador opaco
	const pthread_attr_t *attr, // Atributos del thread 
	void *(*start_fun)(void *), // Función inicio del thread 
	void *arg); // Parámetro de start_fun
```
- **Espera**
```C
int pthread_join( 
	pthread_t thread, // Thread por el que esperar 
	void **retval); // puntero para valor de retorno
```
- **Creación de threads**
```C
#include <pthread.h>
struct f_args{
	int reg;
}

void *thread_function(void *ptr){
	struct f_args *args = ptr;
	int *reg = malloc(sizeof(struct args));

	*reg = ...;
	return reg;
}

int main(){
	thrd_t t;
	struct args *args = malloc(sizeof(struct args));
	args->i = ...; args->c = ...;
	
	pthread_create(&t, NULL, thread_function, args);
	pthread_join(thr, &r);
	free(args);
}
```

- **Memoria compartida con procesos usando mmap**
	```C
	#include <sys/mman.h>
	
	int main(){
		char *shared;
		int size = 100;
		if((shared = mmap(NULL, size, PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANONYMOUS, 0, 0)) == NULL){ // (1)
			exit(0;)
		}
	
		if(fork() == 0){
			//Hijo, shared está compartido por ambos procesos
		}else //padre.
	}
	```
	(1) Significado parámetros
	- NULL -> dirección del mapa
	- size -> tamaño
	- PROT_READ | PROT_WRITE -> PROT_EXEC, PROT_NONE
	- MAP_SHARED -> no marcar como copy on write
	- MAP_ANONYMOUS -> sin fichero
	- 0 -> offset dentro del fichero por si no se quiere mapear el fichero entero
	- 0 -> descriptor del fichero, lo que devuelve open cuando se abre el fichero

**SECCIÓN CRÍTICA**
****

- **Sección crítica**: parte de código que accede a un recurso compartido con otro proceso o thread. 
```C
int i=0; // Variable global, compartida entre threads 
void *incrementar(void *arg) { 
	int v; // Variable local, una por thread 
	v=i; //Sección crítica
	v++; //Sección crítica
	i=v; //Sección crítica
} 

void lanzar_thread() { 
	pthread_t thr; 
	pthread_create(&thr, NULL, incrementar, NULL); 
}
```
 - **Mutex**: consiste en asegurar que dos procesos o threads no ejecutan su sección crítica al mismo tiempo. Para ello debemos gestionar cuándo se producen los accesos a las secciones críticas del programa. La manera más simple de hacerlo es permitir que se ejecute un thread concreto y dejar al resto de threads esperando para poder ejecutarse.
	```C
	void* incrementar(void *arg){
		int v;
		lock; //thread obtiene el control
		v = i;
		v++;
		i = v; //SECCIÓN CRÍTICA (modifica valor de i)
		unlock; //thread libera el control
	}
	```
	Con este mecanismo, el primer thread que llegue a ejecutar la instrucción LOCK pasa a bloquear el resto de hilos que traten de acceder a la sección crítica, haciéndolos esperar a que dicho thread desbloquee la sección crítica llamando a UNLOCK.

- **Instrucciones atómicas**: instrucciones que se ejecutan completamente y sin interrupciones. Se emplean para implementar bloqueos dentro del código, lo cual resulta útil a la hora de controlar las secciones críticas. Un ejemplo es la instrucción `test_and_set(void* )`, la cual permite cambiar el valor de una posición de memoria y devolver su valor antiguo en un único paso. Es importante que el argumento que reciba sea una variable compartida (como un puntero), sino se estaría realizando un bloqueo entre threads (solo estaría bloqueando al propio thread que llama a la función). Por lo general, esta función devuelve el valor 1 cuando se ejecuta. Podemos implementar un mutex usando esta función:
	```C
	void lock(int* mutex){
		while(test_and_set(mutex) == 1); //bucle infinito
	}
	
	void unlock(int* mutex){
		*v = 0;
	}
	```

- **Semáforos**: otro mecanismo de control de acceso a la sección crítica. Tiene un valor numérico que se le asigna al crearlo (bloquear -> decrementa valor; liberar -> incrementa valor; valor 0 -> proceso bloquea). P(bloquear) y V(liberar) son atómicas.
	```C
	int V(semaphore S) { 
		S=S+1; 
	} 
	
	int P(semaphore S) { 
		while(1) { 
			if(S > 0) { 
				S=S-1; 
				break; 
			} 
		} 
	}
	```
La estructura *sem* tiene que estar en memoria compartida entre los procesos.

Ejemplo de cómo podríamos ejecutar la función incrementar del ejemplo de _mutex_, pero usando semáforos para realizar bloqueos creando NUM_PROC procesos:
```C
void incrementar(sem_t* semaforo, int* shared_i){
	int v;
	sem_wait(semaforo); //disminuye el valor del semáforo
	v = *shared_i;
	v++;
	*shared_i = v;
	sem_post(semaforo); //aumenta el valor del semáforo
}

int main(){
	sem_t* semaforo;
	int* shared_i;
		
	shared_i = mmap(NULL, sizeof(int), PROT_READ | PROT_WRITE, MAP_SHARED |                      MAP_ANONYMOUS, 0, 0);
	semaforo = mmap(NULL, sizeof(semaforo), PROT_READ | PROT_WRITE,                              MAP_SHARED | MAP_ANONYMOUS, 0, 0);
	sem_init(semaforo, 1, NUM_PROCS - 1); //compartido entre procesos
		
	for(int i = 0; i < NUM_PROCS; i++){
		if(fork() == 0){
			incrementar(semaforo, shared_i);
			exit(0);
		}
	}
		
	sem_destroy(semaforo);
	munmap(semaforo); //borra el semáforo de la memoria virtual
		
	return 0;
}
```

**INTERBLOQUEO** 
****
Situación en que dos o más procesos están esperando por recursos que tiene ocupado el otro.
- **Condiciones para que se produzca el interbloqueo**
	- Exclusión mutua: existen recursos no compatibles.
	- Hold and wait: se permite que un proceso tenga recursos reservados mientras espera para reservar otros.
	- No apropiación: el SO no puede liberar los recursos reservados.
	- Espera circular: procesos esperan por recursos reservados por procesos que esperan por los primeros.
- **Tratamiento**
	Es tarea del programador de espacio de usuario evitar que se produzca el interbloqueo.
- **Detección**
	Se permite que ocurran interbloqueos y se corrigen después. Como el SO conoce los recursos reservados de un proceso y los recursos por los que espera, puede saber cuando se produce un interbloqueo. Cuando se detecta hay dos opciones:
	- Matar a los procesos involucrados.
	- Apropiar recursos reservados y dárselos a otro proceso para resolver la situación. Esta solución no funciona.
- **Prevención**
	Evitar una de las 4 condiciones necesarias se produzca.
	- Exclusión mutua: impidiendo que los procesos tengan acceso exclusivo a los recursos.
	- Hold and wait: 
		- Haciendo que los procesos reserven todos los recursos necesarios de una vez en vez de mantener reservados unos mientras esperan.
		- Haciendo que los procesos liberen los recursos reservados si no pueden reservar más.
	- No apropiación: no tiene solución.
	- Espera circular: evitar con reserva ordenada de recursos.
- **Evitación**
	Intenta prevenir que el sistema entre en una situación de interbloqueo conociendo el estado del sistema y los recursos que los procesos pueden reservar en el futuro. Requiere conocer a priori el uso de recursos que va a hacer un proceso, lo que limita mucho su uso.

**INANICIÓN**
****
Un proceso puede estar esperando acceso a un recurso compartido sin conseguirlo. Esta situación se llama **inanición**. Si requerimos la reserva de todos los recursos simultáneamente, los procesos que requieran una gran cantidad de recursos pueden quedar en este estado si hay muchos procesos que requieren pocos compitiendo por ellos.

**PRODUCTORES/CONSUMIDORES**
****
Disponemos de un **buffer compartido** entre los procesos, en donde los **productores** son aquellos procesos que generan productos y los insertan en dicho buffer, y los **consumidores** son aquellos procesos que consumen los productos y los extraen del buffer. El problema que se plantea es controlar el acceso al buffer compartido para que los datos se mantengan consistentes. Cuando el buffer esté **lleno**, los productores deben esperar a que los consumidores extraigan elementos de él para que dejen hueco y puedan insertar nuevos productos. Así mismo, cuando esté el **buffer vacío**, los consumidores deben esperar a que los productores inserten nuevos elementos.

- **Implementación mediante threads**:
	Asumiremos que el buffer tiene capacidad infinita y que no se vacía nunca.
	El productor debe encargarse de crear el elemento, bloquear el buffer para poder insertar el elemento dentro y luego desbloquear el buffer para que otros productores puedan insertar o que un consumidor pueda extraer elementos. Por otro lado, el consumidor debe encargarse de bloquear el buffer antes de poder manipularlo, luego extraer el elemento y desbloquear el buffer. Finalmente, realizará lo que considere conveniente con el elemento. Para simplificar, todos los elementos con los que se trabaja son del mismo tipo (elemento).
	```C
	void insertar(elemento e); //insertar un elemento
	elemento extraer();       //extraer (y eliminar) un elemento
	int elementos();         //n.º de elementos insertados
	int tam_buffer();       //capacidad máxima
	
	pthread_mutex_t mutex_buffer;
	
	//sección crítica del productor
	while(1){
		elemento e = crearElemento();
		pthread_mutex_lock(mutex_buffer);
		insertar(e);
		pthread_mutex_unlock(mutex_buffer);
	}
	
	//sección crítica del consumidor
	while(1){
		pthread_mutex_lock(mutex_buffer);
		elemento e = extraer();
		pthread_mutex_unlock(mutex_buffer);
	}
	... //tareas del consumidor con el elemento
	```

- **Tamaño del buffer limitado**: 
	El buffer tiene tamaño finito y puede estar vacío. El **consumidor** tiene que comprobar que haya elementos en el buffer antes de extraer y el **productor** tiene que comprobar que el buffer no esté completo antes de insertar.
	```C
	//sección crítica del productor
	int insertado;
	while(1){
		elemento e = crearElemento();
		insertado = 0;
		do{
			pthread_mutex_lock(mutex_buffer);
			if(elementos() < tam_buffer()){
				insertar(e);
				insertado = 1;
			}
			pthread_mutex_unlock(mutex_buffer);
		}while (!insertado);
	}
	```
Esta implementación es más sencilla, pero el problema es que tanto productores como consumidores deben esperar a que puedan realizar su acción, es decir, están comprobando constantemente el valor de la variable `insertado` hasta que tenga un valor concreto. Este tipo de espera activa **conlleva un alto consumo de CPU**, por lo que solo resultan factibles si sabemos que la espera es relativamente corta. Para solucionarlo, se añadirá un nuevo mecanismo de sincronización por condiciones.

- **Sincronización por condiciones**:
	Una **condición** permite a los procesos suspender su ejecución hasta que sean despertados. En ocasiones es necesario interrumpir la ejecución de un proceso durante su sección crítica hasta que el estado del recurso que se comparte cambie a causa de otro proceso. Este último thread, el que cambia el estado del recurso, es el encargado de despertar al resto de procesos que están durmiendo.
	```C
	#include <pthread.h>
	pthread_cond_t condicion; // Variable con la condición
	
	// Inicializa una condición con unos atributos dados
	int pthread_cond_init(pthread_cond_t*, pthread_condattr_t*);
	
	// Destruye el contenido de la condición
	int pthread_cond_destroy(pthread_cond_t*);
	
	// Envía una señal a UN ÚNICO HILO esperando por una condición
	int pthread_cond_signal(pthread_cond_t*);
	
	// Envía una señal a TODOS LOS HILOS esperando por una condición
	int pthread_cond_broadcast(pthread_cond_t*);
	
	// Duerme al thread que la llama, saliendo de la sección crítica
	int pthread_cond_wait(pthread_cond_t*, pthread_mutex_t*);
	
	// Mismo que wait(). Devuelve error si no se despierta en un tiempo dado
	int pthread_cond_timedwait(pthread_cond_t*, pthread_mutex_t*, const                                  struct timespec* );
	
	pthread_mutex_t mutex_buffer;
	pthread_cond_t buffer_lleno, buffer_vacio;
	// Sección crítica del productor
	while(1) {
		elemento e = crearElemento();
		pthread_mutex_lock(mutex_buffer);
		while ( elementos() == tam_buffer() ) { // Espera a que haya sitio
			pthread_cond_wait(buffer_lleno, mutex_buffer);
		}
		insertar(e);
		if ( elementos() == 1 ) { // Alerta de que buffer ya no está vacío
			pthread_cond_broadcast(buffer_vacio);
		}
		pthread_mutex_unlock(mutex_buffer);
	}
	
	// Sección crítica del consumidor
	
	while(1) {
		pthread_mutex_lock(mutex_buffer);
		while ( elementos() == 0 ) {
			pthread_cond_wait(buffer_vacio, mutex_buffer);
		}
		elemento e = extraer();
		if ( elementos() == tam_buffer() - 1 ) { // Detecta un hueco libre
			pthread_cond_broadcast(buffer_lleno);
		}
		pthread_mutex_unlock(mutex_buffer);
	}
	... // Tareas a realizar con el elemento extraído
	```
 
**FILÓSOFOS CENANDO**
****
**1ª aproximación: un mutex por tenedor**
```C
#define N
#define lfork(i) (((i) + 1) % N)
#define rfork(i) (i)
pthread_mutex_t fork[N];

void phil(int i){
	while(1){
		pick_up(i);
		eat();
		put_down(i);
		think();
	}
}

void pick_up(int i){
	int left = lfork(i)
	int right = rfork(i);
	while(1){
		pthread_mutex_lock(fork[left]);
		if(pthread_mutex_trylock(fork[left]) == 0){
			break;
		}else{
			pthread_mutex_lock(fork[right]);
			usleep(rand() % 10);
		}
	}
}

void put_down(int i){
	int left = lfork(i);
	int right = rfork(i);
	pthread_mutex_unlock(fork[right]);
	pthread_mutex_unlock(fork[left]);
}
```

**2ª aproximación: un mutex para toda la mesa**
```C
#define N
#define lphil(i) (((i) + 1) % N)
#define rphil(i) ((i) - 1) % N)
#define EATING 0
#define THINKING 1
#define HUNGRY
pthread_mutex_t mutex;
pthread_cond_t no_forks[N];

void pick_up(int i){
	int left = lphil(i)
	int right = rphil(i);
	pthread_mutex_lock(&mutex);
	phil[i] = HUNGRY;
	
	while(phil[left] == EATING || phil[right] == EATING){
		(1) pthread_cond_wait(&no_forks, mutex);
		(2) pthread_cond_wait(no_forks[i], mutex); //mejor opción
	}
	phil[i] = EATING;
	pthread_mutex_unlock(&mutex);
}

void put_down(int i){
	int left = lphil(i);
	int right = rphil(i);
	
	phil[i] = THINKING;
	
	(1) broadcast(no_forks);
	(2) //mejor opción
	if(phil[left] == HUNGRY){
		pthread_cond_signal(&no_forks[left]);
	}
	if(phil[right] == HUNGRY){
		pthread_cond_signal(&no_forks[right]);
	}
		
	pthread_mutex_unlock(&mutex);
}
```

**Prevención inanición**
```C
#define N
#define MAX_WAIT x
#define lphil(i) (((i) + 1) % N)
#define rphil(i) ((i) - 1) % N)
#define EATING 0
#define THINKING 1
#define HUNGRY
pthread_mutex_t mutex;
pthread_cond_t no_forks[N];
time_t wait_start[N];

void pick_up(int i){
	int left = lphil(i)
	int right = rphil(i);
	pthread_mutex_lock(&mutex);
	phil[i] = HUNGRY;
	wait_start[i] = time(NULL); //medir el tiempo desde que empezamos la espera
	
	while(can_i_eat(i)){
		(1) pthread_cond_wait(&no_forks, mutex);
		(2) pthread_cond_wait(no_forks[i], mutex); //mejor opción
	}
	phil[i] = EATING;
	pthread_mutex_unlock(&mutex);
}

int can_i_eat(int i){
	if(phil[left] == EATING || phil[right] == EATING){
		return 0;
	}
	
	time_t now = time(NULL);
	if(now - wait_start[i] > MAX_WAIT){ //nosotros llevamos mucho tiempo sin comer
		return 1;
	}
	
	//mirar si los vecinos llevan mucho tiempo sin comer
	if(now - wait_start[left] > MAX_WAIT || now - wait_start[right] > MAX_WAIT){
		return 0;
	}
	
	return 1;
}
```


**INTRODUCCIÓN A ERLANG**
****
**Características**:
- Funcional
- Concurrente
- Tolerante a fallos
- Distribuido
- Soporte de tiempo real
- Cambio de código en caliente

**Hola mundo en fichero hola_mundo.erl**
```Erlang
-module(hola_mundo). % Nombre de modulo
-export([hola/0]). % Funciones que exporta el módulo

hola() ->
	io:format("Hola Mundo!~n"). % Llamada a función format de io
```

**Compilación y ejecución**
```Erlang
$ erlc hola_mundo.erl
$ erl

Erlang/OTP 17 [erts-6.2] [source] [64-bit] [smp:4:4] [async-threads:10] 
[kernel-poll:false] 

Eshell V6.2 (abort with ^G) 
1> hola_mundo:hola(). 
Hola Mundo! 
ok 
2>
```

**Factorial**
```Erlang
-module(factorial).
-export([fact/1]). % exporta la función fact con 1 argumento

fact(N) ->
	if N == 0 -> 1;
		true -> N * fact(N - 1)
	end.
```
Las variables empiezan por mayúscula y no se puede reutilizar su nombre.

