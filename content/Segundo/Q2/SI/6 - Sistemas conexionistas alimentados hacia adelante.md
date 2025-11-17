---
Name: 6 - Sistemas conexionistas alimentados hacia adelante
tags:
  - teoría
asignatura: SI
---
***[[Sistemas Inteligentes]]***

**NEURONAS ARTIFICIALES**
****
Una **neurona biológica** consta de varias dendritas por las cuales recibe impulsos eléctricos de otras neuronas (sinapsis). Con los valores que recibe por las entradas, los procesa en función de qué neurona haya enviado dicha señal y emite una respuesta a través de su axón hacia otras neuronas.

Una **neurona artificial i** tiene la misma estructura:
1. Conjunto de **n entradas xj** a través de las cuales recibe los datos.
2. **Bías b**, una entrada adicional con valor siempre 1 que indica la importancia de la neurona. Tiene su propio peso **θi**.
3. Cada entrada j de la neurona i tiene un **peso wij**, que permite modular la importancia de dicha entrada.
4. **Regla de propagación hi**, encargada de realizar la suma ponderada (peso de cada entrada multiplicado por su valor de entrada) de todas las entradas y sumar la bías multiplicada por su peso.
5. **Función de activación o transferencia yi**, que se le aplica al resultado de la regla de propagación para modificar el valor de la salida de la neurona *i* según nos interese.

La neurona artificial también se conoce como **Elemento Procesado (EP)**. Las entradas y salidas son números reales dentro de un intervalo que suele ser (0,1) o (-1,1).
Las unidades se conectan a través de conexiones, que codifican el conocimiento de la red con sus pesos asociados. Las conexiones pueden ser:
- *Excitatorias*: peso > 0.
- *Inhibitorias*: peso < 0.
- *Inexistentes*: peso = 0.
La bías de cada neurona indica cómo de dispuesta está la neurona a activarse.

**EJEMPLO PUERTAS LÓGICAS**
****
![[Pasted image 20240511083050.png]]

Para emitir la salida deseada ante unas determinadas entradas, se modifican los pesos y las bías. Este proceso se llama **entrenamiento o aprendizaje**. Para esto se necesita un conjunto de patrones que tienen una serie de entradas y una salida deseada. A partir de ellos es cuando se fijan los valores de pesos y bías. Se denominan **conjunto de entrenamientos**.

**ADALINE**
****
**ADAptative LINear Element** es el modelo más básico de RNA (Red Neuronal Artificial). Se trata de una única neurona que emplea una función de activación lineal. Para modificar los pesos de sus entradas, se emplea un algoritmo de **aprendizaje supervisado con corrección de error**.

- **Regla Delta**:
	1. *Aprendizaje*: fija los valores de los pesos de las conexiones y de las bías.
	2. *Supervisado*: necesita un supervisor que, para cada salida, diga qué error comete con respecto a la deseada.
	3. *Por corrección de error*: minimiza el error cuadrático medio (ECM) sobre todos los patrones de entrenamiento.
	![[Pasted image 20240511084527.png]]

	Para minimizar el error, se deriva con respecto a los pesos. Así, se minimiza la gradiente. La superficie del error no se conoce, pero se puede saber el mínimo con la gradiente.
	![[Pasted image 20240511090318.png]]
	![[Pasted image 20240511090343.png]]

	El proceso que sigue la Regla Delta es:
	1. Se inicializan los pesos de forma aleatoria.
	2. Se busca la gradiente descendente a través de la derivada del error.
	3. Se modifican los pesos para situarse en la zona con el mínimo error posible.
	
	La idea es realizar un cambio en los pesos proporcional a la derivada del error. La tasa del aprendizaje es la que controla la convergencia del entrenamiento:
	![[Pasted image 20240511090602.png]]

	El algoritmo de Adaline es:
	1. Se inicializan los pesos de forma aleatoria.
	2. Se aplica un patrón de entrada.
	3. Se calcula la salida.
	4. Se calcula el error.
	5. Se actualizan las conexiones.
	6. Se repiten los pasos del 2 al 5 para todos los patrones.
	7. Si el ECM es aceptable, se termina el algoritmo. Sino, se repite.

**PERCEPTRÓN**
****
Es un modelo anterior al Adaline, el primero de RNA. Se diferencia de Adaline en la función de transferencia y el algoritmo de entrenamiento.
Tiene una única neurona (EP) en la capa de salida. Sirve para resolver problemas linealmente separables. Por ejemplo, una puerta OR:
![[Pasted image 20240511092242.png]]

La regla más utilizada para la actualización de pesos es la Regla Delta:
𝑤𝑖(𝑡 + 1) = 𝑤𝑖(𝑡) − μ(𝑑(𝑡) − 𝑦(𝑡)) · 𝑥𝑖(𝑡)
Con esta regla de aprendizaje, se obtiene una convergencia finita si el conjunto de entrenamiento es linealmente separable.

- **Perceptrón multicapa**:
	Se conectan más neuronas a la salida de otras. Cada neurona recibe entradas y computa su salida mediante los pesos y la función de transferencia.
	El perceptrón se organiza en capas:
	- *Capa de entrada*: las neuronas de entrada no computan nada, solo almacenan las entradas para pasarlas a la siguiente capa. Una neurona por entrada.
	- *Capas ocultas*: están formadas por neuronas ocultas, no hay método para determinar cuántas hay en cada capa.
	- *Capa de salida*: emiten la salida de la RNA. Una neurona por cada salida deseada.
	- *Conocimiento de la red*: reside en los pesos de las conexiones y bias. El conocimiento no está centralizado, sino distribuido.

	Las funciones de transferencia se asume que son las mismas para todas las neuronas de la red, o al menos para las neuronas de la misma capa. Las más típicas son la función umbral, la lineal, la logarítmica sigmoidal o la tangente sigmoidal. Generalmente la función de transferencia en las capas internas no puede ser lineal.

	Un perceptrón puede aprender cualquier tipo de función o relación continua entre un grupo de variables de entrada y salida.

	Se aplica en ajuste de funciones y curvas, problemas de clasificación y problemas de regresión.

- **Algoritmo de backpropagation**:
	No se puede utilizar la Regla Delta porque no se conocen las salidas deseadas de las capas ocultas. Para ello se utiliza backpropagation.

	En la capa de salida puede haber más de una neurona, por tanto no basta con calcular un único error. Hay que minimizar el error de la suma de los cuadrados de los errores. Por tanto, podemos modificar los pesos en la capa de salida. Pero en las capas ocultas nos faltan como parámetros las salidas deseadas. Lo que se puede hacer es propagar el error capa a capa para atrás. Se repite el proceso hasta llegar a las entradas.

	El algoritmo es el siguiente:
	1. Se inicializan los pesos aleatoriamente.
	2. Se calculan los errores y se modifican los pesos.
	3. Se repite este proceso n ciclos (épocas).
	4. Si no se ha mejorado el error durante una serie de ciclos, se para el entrenamiento para evitar sobrentrenamiento.

- **Entrenamiento**
	No hace falta emplear todos los datos para entrenar una red, pero sí un subconjunto que cubra todo el espacio. Es importante que sean representativos. Permite generalización, es decir, incluir nuevos vectores de datos que no pertenezcan al conjunto de entrenamiento.
	El conjunto de datos debe ser:
	- **Significativo**: suficiente número de ejemplo.
	- **Representativo**: cubrir todas las regiones del espacio.

	Si un conjunto de aprendizaje contiene muchos más ejemplos de un tipo que del resto, la red se especializará demasiado en dicho subconjunto.
	Las entradas deben estar entre 0 y 1 o entre -1 y 1. Las salidas deben ser también normalizadas.

	En cuanto al control de convergencia, no se puede asegurar que a lo que se llega sea un mínimo global. Puede llegar a mínimos locales, no es determinista. Por lo tanto, si alcanzamos un mínimo local pero el error es satisfactorio, el entrenamiento es un éxito.

	Después del entrenamiento, se pasa otro conjunto de patrones: el conjunto de test. Son valores que no están presentes en el entrenamiento e indican si la red está bien entrenada o no.

	Cuando hay una mala generalización, es por culpa del sobrentrenamiento. Para evitarlo, podemos establecer que pare cuando lleve cierto número de ciclos sin mejorar la validación, o establecer un número de ciclos muy alto y memorizar la red con mejor resultado de validación.

	Hacen falta 3 conjuntos de patrones:
	- **Entrenamiento**: guía el proceso de entrenamiento.
	- **Validación**: supervisa el entrenamiento y lo para si es necesario.
	- **Test**: evalúa el resultado una vez terminado el entrenamiento. Es la única forma de saber si la red está bien entrenada.
