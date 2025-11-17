---
Name: 3 - Representación del conocimiento
tags:
  - teoría
asignatura: SI
---
***[[Sistemas Inteligentes]]***

****
La base del conocimiento de un agente es un conjunto de sentencias en un lenguaje formal. Para construir un agente basta con decirle lo que necesita saber. Con esta información se puede preguntar a sí mismo qué hacer para avanzar. 
Los agentes inteligentes se pueden analizar desde dos perspectivas:
- **Nivel de conocimiento**.
- **Implementación**.

Un agente basado en conocimiento (KB agent), recibe una percepción y devuelve una acción. El agente en cada llamada hace:
1. Se le dice lo que percibe.
2. Se pregunta qué acción debe realizar.
3. Ejecuta la acción.
Un ejemplo sería el juego del Wumpus. Es un problema:
- **No observable**: el agente solo percibe lo que tiene en sus casillas adyacentes.
- **Determinista**: las salidas están especificadas.
- **Secuencial**: las recompensas llegan tras ejecutar un determinado número de acciones.
- **Estático**: el Wumpus, las fosas, oro... no se mueven.
- **Discreto**: tiene un número finito de posibilidades.
- **De agente único**: el jugador.

**LÓGICA**
****
La lógica es el lenguaje formal que representa la información de tal forma que permita llegar a conclusiones. Tiene una sintaxis, que define las sentencias del lenguaje, y una semántica, que define su significado donde cada sentencia cierta es un modelo.

Se dice que un modelo satisface una sentencia `a` si esta hace que la sentencia sea cierta. Un modelo es un conjunto de elementos que hacen que `a` sea verdadera. `M(a)` es el conjunto de todos los modelos que satisfacen la sentencia. 
Una implicación lógica indica que una sentencia deriva lógicamente de otra. Se denota como `a |= b`

- **Inferencia lógica**:
	Se refiere al proceso mental de razonamiento que permite llegar a una conclusión lógica a partir de la información disponible. Es la habilidad de deducir o derivar conclusiones a partir de premisas o evidencias. Utiliza la implicación para derivar conclusiones. Por ejemplo:
	`KB |-i a` la sentencia `a` puede derivarse de KB mediante el proceso `i`.
	
	Un algoritmo inferencial es, por ejemplo, el **model checking**, que consiste en enumerar todos los modelos para comprobar si una sentencia es cierta en todos los modelos en los que KB es cierto. Un algoritmo inferencial que deriva solo sentencias implicadas es un sólido o preservador de verdad. Es completo si puede derivar cualquier sentencia implicada, es decir, si puede llegar a cualquier conclusión lógica que pueda ser derivada de las premisas o los hechos conocidos.
	
	Un proceso `i` es sólido si cuando `KB |-i a`, se cumple `KB |= a`.
	Es completo si cuando `KB |= a` se cumple `KB |-i a`.

**LÓGICA PROPOSICIONAL**
****
- **Conectivas lógicas**
	![[Pasted image 20240421104807.png]]

- **Equivalencias lógicas**
	­Dos sentencias α y β son lógicamente equivalentes si α ≡ β ­ sí y sólo sí α ≡ β y 
	β  ­≡ α .
	![[Pasted image 20240421105954.png]]

- **Validez**
	Una sentencia es válida si es cierta en todos los modelos. También se conoce como tautología. 
	El teorema de deducción dice que para dos sentencias a y b, a |= b si a => b es válido. Se comprueba mirando si a => b es cierto en todos los modelos.

- **Satisfacibilidad**
	Una sentencia es satisfacible si es cierta en algún modelo. La reducción al absurdo se basa en a |= b si a ⋀ ¬b es insatisfacible.

- **Métodos de prueba**
	Los métodos de prueba se dividen en:
	1. *Aplicación de reglas de inferencia*:
		- Generar nuevas sentencias en base a las antiguas.
		- La prueba es una secuencia de aplicación de reglas inferenciales para llegar a una conclusión.
		- Se suelen traducir las sentencias a una forma normal.
	2. *Comprobación de modelos*:
		- Verificar si un conjunto de proposiciones es verdadero en algún modelo o interpretación.
		- Existen diferencias técnicas. Una de las técnicas es la enumeración en una tabla de verdad, que puede ser exponencial en el número de proposiciones. Otras técnicas incluyen algoritmos de búsqueda heurística en el espacio de modelos, como los de mínimo conflicto o ascensión de colinas.

- **Monotonicidad**
	Se basa en que si KB |= a entonces KB ⋀ b |= a. Las sentencias que se infieren solo se incrementan a medida que se añade información a la base del conocimiento. No se pueden invalidad conclusiones ya inferidas, o sea que el cambio de opinión no sirve.

- **Factorización**
	La cláusula resultante de una regla de resolución debe contener solo una cláusula de cada literal, la eliminación de las copias se llama factorización. Por ejemplo:
	`a ⋁ b` junto a `a ⋁ ¬b` se reduce en `a`.

- **Forma normal conjuntiva**
	Toda sentencia de lógica proposicional equivale a una conjunción de cláusulas. Una sentencia expresada como conjunción de cláusulas está en FNC. El algoritmo para convertir una sentencia en una FNC es:
	1. Substituir a <=> b por a => b ⋀ b => a.
	2. Substituir a => b por ¬a ⋁ b.
	3. Usar De Morgan para las negaciones.
	4. Usar la propiedad distributiva.

- **Completitud**
	El cierre de resolución es un conjunto de cláusulas que se obtiene a partir de un conjunto inicial de cláusulas utilizando la regla de resolución de manera repetida. 
	
	La regla de resolución es una técnica de inferencia que se utiliza en lógica proposicional para derivar nuevas cláusulas a partir de cláusulas existentes. Dado un conjunto de cláusulas, se seleccionan dos cláusulas que contienen una variable proposicional complementaria y se realiza una operación de resolución para obtener una nueva cláusula, que contiene todas las variables que no se cancelan en las dos cláusulas originales. Este proceso se repite de manera recursiva hasta que no se pueden obtener nuevas cláusulas.
	
	El teorema fundamental de resolución dice que si se puede derivar la cláusula vacía a partir del conjunto inicial de cláusulas, entonces se concluye que el conjunto de cláusulas es insatisfacible. Si no se puede obtener la cláusula vacía a partir del conjunto inicial de cláusulas, entonces el conjunto de cláusulas es satisfacible.
	
	Se consideran dos formas restringidas de cláusulas:
	- *Cláusula definida*: es una disyunción de literales en la que exactamente un literal es positivo. Toda cláusula definida se puede describir como una implicación. Cualquier expresión lógica en forma de cláusula (disyunción de literales) puede ser reformada como una implicación, donde la premisa es una conjunción de literales positivos y la conclusión es un único literal positivo. 
	- *Cláusula de Horn*: es una disyunción de literales en la que a lo sumo un literal es positivo. Todas las cláusulas definidas son cláusulas de Horn, ya que son cláusulas con literales no positivos. Además, las cláusulas de Horn tienen la propiedad de que son cerradas bajo resolución, lo que significa que si se resuelven dos cláusulas de Horn, se obtiene una de Horn.

- **Encadenamiento progresivo**
	Sirve para determinar si un símbolo proposicional deriva de una base de conocimiento de cláusulas definidas. Se hace en tiempo lineal y sus pasos son:
	1. Seleccionar la meta.
	2. Buscar una regla que contenga la meta en su conclusión.
	3. Verificar si las condiciones de la regla están presentas en la base de hechos. SI no se cumplen, la regla no es aplicable y se busca otro.
	4. Si se cumplen todas, agregar la conclusión de la regla a la base de hechos.
	5. Verificar si se ha alcanzado la meta, sino volver al paso 2.
	6. Repetir los pasos 2 a 5 hasta que se alcance la meta o se agoten las reglas.

- **Encadenamiento regresivo**
	Funciona a la inversa que el encadenamiento progresivo. Es dirigido por las metas.

- **Comprobación de modelos proposicional efectiva**
	El problema SAT (Satisfacibilidad Booleana) se refiere a la pregunta de si existe al menos una asignación de valores de verdad a un conjunto de variables booleanas que satisfaga una expresión booleana dada. Muchos problemas de computación se pueden reducir a comprobar la satisfacibilidad de una sentencia proposicional.
	
	La escalabilidad SAT se puede corregir con:
	- *Análisis de componentes*: enfoque que implica dividir el problema en subproblemas más pequeños y manejables. Se busca identificar conjuntos de variables y cláusulas que no interactúan entre sí, lo que permite resolverlos de manera independiente.
	- *Ordenación de variables y valores*: implica elegir un orden en el que se asignan valores de verdad a las variables. Se pueden utilizar diferentes heurísticas para determinar el orden.
	- *Backtracking inteligente*: técnica utilizada en la mayoría de los algoritmos SAT para explorar diferentes asignaciones de valores de verdad. Utiliza información obtenida durante la ejecución del algoritmo para tomar decisiones más informadas sobre qué variables asignar y en qué orden. 
	- *Recomienzo aleatorio*: implica detener la ejecución de SAT después de un cierto número de intentos y comenzar de nuevo desde cero con una asignación aleatoria de valores de verdad. Esto puede evitar que el algoritmo quede atrapado en un subconjunto particular del espacio de soluciones que no tiene solución.
	- *Indexación adecuada*: implica organizar los datos de SAT de tal forma que la búsqueda y la recuperación de información sea lo más eficiente posible. EN particular, se pueden utilizar técnicas de indexación para buscar rápidamente cláusulas que contengan ciertas variables o valores de verdad.

- **Backtracking DPLL**
	```
	function DPLL_SATISFIABLE(s) returns true or false
		input:s //una sentencia en lógica propisicional.
		clauses //conjunto de cláusulas en la representación CNF de s
		Symbols //lista de símbolos proposición en s
		return DPLL(clauses, symbols, {})

	function DPLL(clauses, symbols, model) returns true or false
		if toda cláusula en clauses es cierta en model then return true
		if alguna cláusula en clauses es falsa en model then return false
		P, value <- FIN-PURE-SYMBOL(symbols, clauses, model)
		if P no-null then return DPLL(clauses,symbols-P,model U {P=value})
		P <- FIRST(symsbols); rest <- REST(symbols)
		return DPLL(clauses, rest, model U {P=true}) or
				DPLL(clauses, rest, model U {P=true}) 
	```

	Funciona de la siguiente manera:
	1. Se aplican reglas de simplificación para eliminar cláusulas y literales redundantes.
	2. Se elige una variable que aún no se le ha sido asignado un valor de verdad y se le asigna uno (true o false). Se actualiza la fórmula para reflejar esta asignación y se simplifica nuevamente. Si se encuentra una contradicción, se retrocede en el árbol de decisión y se intenta otra asignación. 
	3. Si no se encuentra una contradicción, se elige otra variable no asignada y se repite el proceso de asignación de valores de verdad. Esto se conoce como ramificación.
	4. Si se encuentra una contradicción en algún punto del proceso, se retrocede en el árbol de decisión y se hace una poda. Esto implica deshacer una o más asignaciones anteriores y elegir otro valor para la variable correspondiente.
	5. El árbol continúa ramificando y podando hasta que se encuentra una asignación de valores de verdad que satisface la fórmula o se determina que no existe tal asignación.

- **Algoritmos de búsqueda local**
	Hill-Climbing y Simulated-Annealing son algoritmos de búsqueda heurística que se pueden aplicar a problemas SAT para encontrar una solución satisfactoria. Ambos algoritmos parten de una solución inicial y realizan movimientos para tratar de mejorarla. 
	
	En Hill-Climbing se parte de una solución aleatoria y se generan soluciones vecinas realizando pequeños cambios en la asignación de valores de verdad. Luego se elige la solución vecina que tiene el valor de evaluación más alto y se repite el proceso hasta que se encuentra una solución o se llega a un máximo local. En este caso, la solución se considera la mejor considerada hasta el momento.
	
	En Simulated-Annealing se realiza un proceso similar, pero se permite la aceptación de soluciones peores con cierta probabilidad, lo que permite escapar de máximos locales. A medida que se realizan más iteraciones, la probabilidad de aceptar soluciones peores disminuye gradualmente, lo que permite que el algoritmo converja a una solución satisfactoria.
	
	El algoritmo WALKSAT es un ejemplo específico de Hill-Climbing. Se elige una cláusula al azar y se cambia el valor de verdad de un símbolo en la cláusula elegida. Luego se evalúa si la nueva solución es mejor que la anterior y se repite el proceso hasta que se encuentra una solución satisfactoria o se alcanza el número máximo de iteraciones permitidas. Si se alcanza y no se ha encontrado solución, se devuelve un fallo.

**REPRESENTACIÓN DEL CONOCIMIENTO**
****
La lógica proposicional permite el uso de procedimientos de resolución que facilitan el razonamiento con hechos. Sirve para crear estructures de representación que permitan agrupar propiedades y describir objetos complejos. 

Otra forma es con esquemas no formales de representación del conocimiento. Son capaces de representar objetos, categorías, eventos... y de manipular conocimiento para obtener nuevos conocimientos. Pueden ser:
- **Métodos declarativos**: el conocimiento se representa como una colección estática de hechos, para cuya manipulación se define un conjunto genérico y restringido de procedimientos. Sus ventajas son que las verdades del dominio se almacenan solo una vez y es fácil incrementar e incorporar nuevo conocimiento sin modificar el ya existente.
- **Métodos procedimentales**: la mayor parte del conocimiento se representa como procedimientos, lo cual le confiere al esquema de representación un carácter dinámico. Sus ventajas son que enfatizan más en las capacidades inferenciales del sistema, permiten explorar distintos modelos y técnicas, permiten trabajar con probabilidad e incorporan de forma natural la heurística.


- **Redes semánticas**
	Consisten en representar el conocimiento mediante conceptos y las reglas que existen entre ellos mediante un grafo y se emplean para hacer mapas conceptuales. Los nodos representan a un concepto y las aristas representan la relación entre ambos, las cuales son unidireccionales. Puede decirse que un enlace es una relación binaria entre nodos y por ello puede haber varias relaciones:
	- *Ocurrencia (pertenece)*: se relaciona un elemento con una categoría (grupo de elementos).
	- *Generalización (es un)*: relaciona una entidad concreta con otra más genérica.
	- *Agregación (es parte de)*: relaciona componentes de un objeto con el propio.
	- *Acción*: vincula una entidad con otra mediante una acción.
	- *Propiedades*: relaciona objetos con características de esos objetos.
	
	Una relación existente puede generar nuevo conocimiento haciendo uso de:
	- **Herencia de propiedades**: cualquier propiedad que sea cierta para una clase de elementos puede ser cierta para cualquier elemento de la clase que los contenga.
	- **Razonamiento**: se puede realizar de dos maneras:
		- *Por rastreo*: se siguen las relaciones entre dos objetos y se establece una nueva relación que los conecte, aunque puede suceder que la inferencia realizada no sea válida.
		- *Emparejamiento*: se crean nuevos fragmentos de la red semántica y se agregan a la red principal estableciendo nuevas relaciones. El problema es buscar qué relaciones son correctas al unir las redes.

- **Marcos (Frames)**
	Los frames intentan representar el problema mediante un razonamiento por semejanzas. Para ello describen clases de objetos que son representaciones estructuradas del conocimiento sobre una entidad. La ventaja de los frames es que permiten definir procedimientos para inferir rápidamente conocimiento a pesar de tener información incompleta o que no está representada explícitamente. Un frame consta de:
	- *Cabeza*: le da nombre al frame y representa la clase de objetos que se describe.
	- *Slot*: elementos que representan a un objeto o a una propiedad del frame. Pueden anidarse, de modo que a profundidad de un slot es el nivel de conocimiento que tiene y su contenido se especializa a medida que profundiza en los niveles (cada nuevo sangrado es un nuevo nivel).
	Para obtener nuevos conocimientos mediante semejanza se hace uso de la **herencia**.
	Un ejemplo sería la representación de un objeto *Pájaro* y cómo representar un *Gorrión* mediante la generalización de nuevo conocimiento:
	![[Pasted image 20240504195622.png]]
	
	- **Procedimientos (demons)**: 
		Existe un conjunto de procedimientos para generar nuevos conocimientos que están inactivos la mayoría del tiempo, pero que se activan cuando se cumplen determinadas condiciones para ejecutar acciones concretas. Por ejemplo:
		`IF_NEEDED, IF_ADDED, IF_REMOVED, IF_STORED, IF_RETRIEVED`.
		Cuando un *demon* se activa por un valor en una entrada del frame, se desencadenará una acción con el nombre de `D_"nombreAcción"` y luego el *demon* vuelve a ponerse inactivo.
		```
		Base_de_reglas
			IF_REMOVED
				D_reglas_de_eliminado
			REGLAS
				Regla_1
				Regla_2
				(...)
			PARAMETROS_DE_LOS_IF
				Parametro_1
					Regla_1
		```

- **Reglas de producción**
	Son esquemas empleados para representar el conocimiento procedimental que están basados en **premisas** (IF), que en caso de ser verdaderas ejecutan una **acción** o generan una **conclusión** (THEN), y que si son falsas ejecutan otra acción/conclusión (ELSE). La premisa debe estar construida con lógica formal y los operadores `AND, OR, NOT` y pueden anidarse varias premisas.
	
	La principal ventaja de las reglas de producción es que las condiciones y las acciones/conclusiones involucradas son explícitas y por lo tanto no sabemos cuáles podrían ser los posibles resultados en función del valor lógico que tomen las condiciones. Por lo general buscamos que las reglas de producción conformen una **unidad completa de razonamiento**, que una única regla englobe todas las acciones o conclusiones que estén relacionadas con las premisas que se evalúen en el IF.
	![[Pasted image 20240504200944.png]]