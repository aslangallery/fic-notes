---
Name: 3 - Estimación del Tamaño Funcional
tags:
  - teoría
asignatura: PDS
---
***[[Proyectos de Desarrollo Software]]***

**ESTIMACIÓN DEL TAMAÑO**
****
***ESTIMACIÓN POR LÍNEAS DE CÓDIGO***
> Técnica de estimación basada en la cuantificación del número de líneas de código fuente esperadas en el programa a desarrollar. 

 Existen dos formas de establecer la medición:
1. *LOC Físicas*: se incluyen el total de líneas de código y los comentarios, las líneas en blanco se incluyen si no superan el 25% del total.
2. *LOC Lógicas*: se cuenta el número de sentencias del código fuente.

***ESTIMACIÓN CON PUNTOS FUNCIÓN (FPA)***
> Mide el tamaño de un proyecto por medio de la cuantificación de las funcionalidades o requisitos especificados por parte de los clientes.

- *Características*:
	- Independencia de la tecnología de desarrollo.
	- Fácil de aplicar.
	- Basada en los requisitos del cliente.
	- Focalizado en las funcionalidades proporcionadas.

**MÉTODOS BASADOS EN PUNTOS DE FUNCIÓN**
****
El método más utilizado es el IFPUG. Para realizar estimaciones en etapas iniciales del proyecto, donde se tiene poca información, se pueden utilizar estas variantes:
- *FP Lite*: derivado del IFPUG, se simplifican y reducen las fases.
- *SFP (Simple Function Point)*: se simplifica el IFPUG FPA haciéndolo más directo.
- *UCP (Use Case Points)*: usa como entrada los casos de uso del proyecto.
- *E&QFP (Early & Quick FP)*: método ágil para estimar PF en etapas tempranas.

Estas variantes son más imprecisas que las anteriores, pero suficientes en las etapas iniciales, donde se carece de información.

***MÉTODO IFPUG***
> Asociación que tiene como objetivo la creación de manuales y la promoción de la estimación de software por medio del FPA.

1. *Tipos de estimaciones*
	- *Proyecto de desarollo*: funcionalidades requeridas por el cliente.
	- *Proyecto de mantenimiento*: modificaciones sobre un proyecto ya entregado.
	- *Aplicación*: funciones que existen implementadas en un producto.
2. *Cálculo de PF*
	1. **Identificación del alcance y límites de la aplicación**
		Identificar qué funcionalidades se medirán, qué interacciones existen con otros sistemas y qué datos forman parte del análisis.
		- *Transacciones*: entradas (EI), salidas (EO), consultas (EQ).
		- *Datos*: almacenados en aplicación (ILF), usados por la aplicación pero administrados y almacenados de modo externo (ELF).
		
		El alcance viene definido por el conjunto que se va a medir, el propósito del cálculo y aquellas funcionalidades que son relevantes para el PFC.
		
		Los límites definen lo que es externo a la aplicación, indican el borde entre el sw y el usuario.
	2. **Identificar los 5 elementos funcionales**
		Se clasifican en 2 grupos:
		- *Funciones de datos*: representa grupos de datos relacionados lógicamente e identificados por el usuario. Existen ILF y ELF.
		- *Funciones transaccionales*: representan funcionalidades que el sistema facilita al usuario para procesar los datos. Existen EI, EO y EQ.
		
		Se deben tener en cuenta las siguientes definiciones
		- *Proceso elemental*
			> Mínima unidad de actividad que es significativa para el cliente.
			> Ha de ser autosuficiente y dejar a la aplicación en estado consistente.
			
		- *Información de control*
			> Conformada por los datos que influyen en un proceso elemental, especificando qué, cuándo o cómo se procesan los datos.
			
		Para que la información se considere **ILF** se debe cumplir:
		1. La información que agrupa debe ser lógica e identificable desde el POV del usuario.
		2. La agrupación de datos es mantenida por un EP dentro de la aplicación.
		3. No se ha identificado como un ELF.

		Para que la información se considere **ELF** se debe cumplir:
		1. Lo mismo que el apartado 1 de ILF.
		2. La agrupación de datos es referenciada por la aplicación, pero externa a ella.
		3. Es considerada como un ILF en otra aplicación.

		La principal diferencia entre EI, EO y EQ es su cometido principal.

		| Función                               | EI                    | EO                    | EQ                    |
		| ------------------------------------- | --------------------- | --------------------- | --------------------- |
		| Alterar el comportamiento del sistema | **Función principal** | Función secundaria    | No permitido          |
		| Mantener un ILF o más                 | **Función principal** | Función secundaria    | No permitido          |
		| Mostrar información al usuario        | Función secundaria    | **Función principal** | **Función principal** |

		Los **EOs** y **EQs** comparten una serie de reglas comunes:
		1. Se envían datos fuera de los límites de la aplicación.
		2. Para el proceso identificado, igual que con EI pero con EO y EQ.
		![[Pasted image 20250527195001.png]]

		Para identificar funciones transaccionales:
		- Si datos se reciben desde fuera -> **EI**.
		- Si proceso mantiene un ILF -> **EI** o **EQ**.
		- Si proceso tiene algún cálculo o función derivada -> **EO** o **EI**.
		- Si proceso recupera datos de ILF/ELF -> **EQ**.
	3. **Evaluar la complejidad**
		La complejidad la medimos en base a:
		1. *Funciones de datos*
			> Se determina contando los DET (Data Element Type) y RET (Record Element Type).

			Los DET son campos únicos (no repetidos en la misma función) y entendibles por el usuario.
		
			Un RET está conformado por un subgrupo de elementos de un ILF/ELF y existen dos tipos de RET:
			- *Opcionales*: el usuario puede usar alguno de los subgrupos durante un EP que añade o crea una instancia de los datos.
			- *Obligatorios*: el usuario debe usar al menos uno.
		2. *Funciones transaccionales*
			> Se define contando DET y FTR (File Type Referenced).

			Un FTR se define como:
			- Un ILF leído o mantenido por una función transaccional.
			- Un ELF leído por una función transaccional.
	4. **Calcular PF sin ajustar (PFSA)**
		Para cada elemento, el valor resultante será $complejidad = cantidad×peso$
	5. **Evaluar Factores de Ajuste (GSCs)**
		> Se evalúan 14 características generales que puntúan la funcionalidad de la aplicación. Sirven para ponderar los PF y reflejar de un modo más técnico el PFC.

	6. **Calcular Factor de Ajuste (VAF)**
	7. **Calcular PF ajustados (PFA)**
		$PFA = PFSA × (0,65 + 0,01 × TFA)$

***MÉTODO FP LITE***
> Método de estimación por PF derivado del FPA de IFPUG donde se reducen los pasos para el cálculo de los FPA.

1. *Identificar alcance y límites*
2. *Identificar 5 elementos funcionales*
3. *Anotar cálculo de complejidad (media)*
4. *Calcular valor final de PF, con margen de error de ±20%*

***MÉTODO E&QFP***
> Permite calcular el tamaño funcional de un proyecto cuando la información disponible sobre los requisitos es limitada.

1. *Principales características*
	- Derivado del FPA de IFPUG.
	- Permite estimaciones en fases muy tempranas.
	- Usa niveles de granularidad para categorizar componentes.
	- Maneja estimaciones en rangos: mínimo, más probable, máximo.
	- Permite estimación multinivel (detallada + global).
2. *Pasos*
	1. **Establecer tipo de estimación PF**
	2. **Identificar límites y alcance**
	3. **Establecer nivel de detalle**
	4. **Identificar componentes, datos y transacciones E&QFP**
		- *Nivel 1*: igual a IFPUG.
		- *Nivel 2*: componentes no clasificados (UBFCs).
		- *Nivel 3*: grupos de componentes (TP, GP, GDG).
		- *Nivel 4*: macroprocesos (MP).
	5. **Definir valores de PF sin ajustar**
	6. **Establecer Factor de Ajuste (VAF)**
	7. **Ajustar valores de PF**

***MÉTODO SFP***
> Estándar del IFPUG que trata la estimación del tamaño funcional de una aplicación basándose en el FPA, pero reduciendo la identificación de los elementos funcionales y sus complejidades.

1. *Características*
	- Derivado del FPA del IFPUG y el E&QFP.
	- Usado en fases muy tempranas.
	- Adquirido por IFPUG en 2019.
	- Aplicación ágil y con lenguaje comprensible.
2. *Pasos*
	1. **Obtención de información**
	2. **Identificación de alcance y límites**
	3. **Identificación de BFCs**
		- *Identificación de Ficheros Lógicos*
		- *Identificación de Procesos Elementales*
	4. **Calcular tamaño funcional**
	5. **Documentar proceso**

***COMPARACIÓN GENERAL ENTRE MÉTODOS***

|Característica|LOC|IFPUG|FP Lite|SFP|E&QFP|
|---|---|---|---|---|---|
|Basado en requisitos|No|Sí|Sí (simplif.)|Sí (simplif.)|Sí (rango)|
|Tecnología independiente|No|Sí|Sí|Sí|Sí|
|Precisión|Baja|Alta|Media|Media-Baja|Media-Alta|
|Aplicable en fases tempranas|No|No|Sí|Sí|Sí|
|Nivel de detalle requerido|Alto|Alto|Medio|Bajo|Variable|

**MÉTODOS BASADOS EN PUNTOS DE CASO DE USO**
****
***MÉTODO DE PUNTOS DE CASO DE USO (UCP)***
> Mide el resultado en función de la cantidad y complejidad de sus casos de uso y actores, ajustada mediante factores externos.

1. *Características*
	- Estima el tamaño funcional del software a partir de:
		- Cantidad y complejidad de los casos de uso.
		- Tipo y cantidad de actores.
		- Factores técnicos y del entorno.
	- Relacionado con metodologías orientadas a objetos y ciclos de vida iterativo-incrementales.
	- Más directo que FPA para estimaciones tempranas.
	- No certificado ni estandarizado oficialmente.
	- Muy dependiente de la calidad del modelado de los casos de uso.
2. *Pasos*
	1. **Clasificación de actores según su complejidad (UAW)**
	2. **Clasificación de casos de uso según complejidad (UUCW)**
	3. **Cálculo de UCP sin Ajustar (UUCP)**
	4. **Determinación de Factores de Complejidad Técnica (TCF)**
	5. **Determinación de Factores de Entorno (EF)**
	6. **Cálculo de Casos de Uso Ajustados (AUCP)**

**MÉTODOS DE ESTIMACIÓN EN METODOLOGÍAS ÁGILES**
****
> Son diferentes a los de un proyecto tradicional. La principal premisa en la que se basa es en el conocimiento y la experiencia del equipo.

Hay 2 parámetros fundamentales:
- *Velocidad*
- *Puntos historia*

***HISTORIAS DE USUARIO***
> Descripción corta y esquemática que resume la necesidad concreta de un usuario.

Usada para el ERS, es una representación de un requisito escrito en 1 o 2 frases utilizando lenguaje común. 

***PUNTO HISTORIA***
> Unidad de medida relativa utilizada para estimar el esfuerzo necesario para completar una HU.

Se suele usar este término por varios motivos:
- Mayor consenso entre personas del equipo.
- Menor tiempo de estimación.
- Menor estrés en el equipo.

1. *Estimación*
	El número de puntos de historia de una tarea representa el esfuerzo ideal de todo el equipo. A la hora de estimar, se debe conocer el equipo que la desarrollará, ya que depende de quien trabaje en ella. 
	Antes de estimar hay que estar de acuerdo y definir qué significa que algo está terminado:
	- Tipos de pruebas necesarias y su grado de cobertura.
	- Nivel de calidad del producto software.
	- Grado de documentación requerido.
	- Entorno de la entrega.
2. *Cálculo de puntos de historia*
	Se asignan relativizando unas historias frente a otras. La idea fundamental es que el equipo ha estimado todas las historias a la vez. 
	Es mejor estimar usando rangos o escalas de posibles valores, fijando un mínimo y un tope de puntos historia.
	Se utiliza la escala de Fibonacci y no una secuencia lineal.
	Algunas tarjetas especiales que se usan a a hora de estimar son:
	- 0: historia hecha o que no llevará casi trabajo.
	- ?: una persona no sabe cuánto puede llevar la HU.
3. *Planning Poker*
	1. El propietario presenta las HU a estimar. Suele darse un tiempo máximo de discusión.
	2. Cada una de las personas toma un mazo de cartas que suelen estar numeradas con la secuencia para estimar y escoge la que represente su estimación.
	3. Se publican todas las estimaciones, mostrando a la vez la carta seleccionada.
	4. Si existe una gran dispersión entre las estimaciones, se vuelve a discutir la HU y se vuelve a realizar el proceso.
	5. Se llega a un consenso.

***¿CUÁNDO DEBE ESTIMAR UN EQUIPO?***
Pronto para cumplir con la priorización y predicción a largo plazo. Se hacen varias sesiones de Planning Poker:
- Una cuando el propietario del producto ya tenga definidas las HU en la pila.
- Otra se realiza una vez por sprint, puede ser en la reunión de preparación del siguiente sprint.

***VELOCIDAD***
> Mide la cantidad de trabajo completado en términos de puntos historia por sprint.

Es el elemento clave y más importante a la hora de estimar con puntos historia.
Se obtiene sumando los puntos de historia de todas las historias terminadas en un sprint.

1. *¿Quién decide que algo es valioso para el producto?*
	Propietario del producto.
2. *¿Qué elementos se incluyen en la velocidad?*
	Las historias de usuario completamente terminadas y que aportan valor al producto.

La velocidad mide el trabajo realizado en función de la estimación inicial. Si una HU llevara 20 horas y se termina en 40, la velocidad que aporta es 20 horas. 

***PUNTOS HISTORIA VS PUNTOS FUNCIÓN***
1. *Puntos historia*
	- Miden tamaño en función del esfuerzo.
	- Trabajan con requisitos poco documentados.
	- Se basan en la experiencia del propio equipo.
	- No están normalizados.
	- Son específicos de cada equipo.
2. *Puntos función*
	- Miden tamaño en función de la complejidad.
	- Trabajan con requisitos exhaustivos.
	- Se basan en fórmulas.
	- Están normalizados.
	- No pueden compararse entre equipos.