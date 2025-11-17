---
Name: 5 - Medición de la calidad
tags:
  - teoría
asignatura: ACL
---
***[[Aseguramiento de la Calidad]]***

**INTRODUCCIÓN A LAS MÉTRICAS**
****
Las métricas permiten saber a qué nos comprometemos, cuál es la probabilidad de conseguirlo y ver si estamos siguiendo el camino correcto.
Permite saber si los clientes están satisfechos con los productos, identificar tendencias y anticiparnos a los problemas.

La medición busca:
1. *Analizar*: entender qué pasa durante el desarrollo y mantenimiento.
2. *Controlar*: controlar qué ocurre en los proyectos.
3. *Predecir*: realizar estimaciones de tiempo, coste, etc.
4. *Mejorar*: buscar mejorías en procesos y productos.

***Conceptos básicos***
1. *Medida*
	Resultado de cuantificar una magnitud o un atributo de un producto o proceso.
2. *Medición*
	Conjunto de operaciones necesarias para obtener una medida.
3. *Métrica*
	Fórmula o algoritmo que utiliza una medición.

***Escalas de medición***
1. *Escala nominal*
	Agrupar elementos en categorías en función de un determinado atributo. Las categorías deben cubrir todo el conjunto de posibilidades y los atributos deben ser exclusivos de una categoría.
2. *Escala ordinal*
	Escala nominal ordenada. No ofrece información de la diferencia entre niveles.
3. *Razón o ratio*
	Escala de intervalos donde el 0 significa ausencia de valor. Un intervalo es una diferencia entre dos puntos donde toda diferencia entre todos los puntos adyacentes es idéntica.

**TIPOS Y EJEMPLOS DE MÉTRICAS**
****
1. ***Directas***
	Aquellas que no dependen de ningún otro atributo más que el que estamos midiendo. Permite realizar mediciones sin depender de ninguna otra métrica. Por ejemplo, el número de líneas de código.
2. ***Indirectas***
	Aquellas que combinan varias métricas. Por ejemplo, la satisfacción del cliente combina el número de quejas, el tiempo en que se tarda en proporcionar un servicio, etc.
3. ***Estáticas***
	Visión del fenómeno en un momento concreto.
	- *Ratio*: resultado de dividir una cantidad por otra, siendo ambas excluyentes. 
		$$\dfrac{\text{num testers}}{\text{num desarrolladores}}$$
	- *Proporción*: es una fracción del total.
		$$\dfrac{\text{num testers}}{\text{num empleados}}$$
	- *Porcentaje*: es la expresión en % de una proporción. No es recomendable a menos que la muestra sea muy grande.
4. ***Dinámicas***
	Visión del fenómeno durante un tiempo.
	- *Índice*: medida cuantitativa utilizada para evaluar y cuantificar algún aspecto específico del comportamiento, rendimiento o calidad de un sistema de software mientras está en funcionamiento. Por ejemplo, el índice de disponibilidad del sistema.

**Ejemplos de métricas**
1. ***Métricas de producto***
	- *Complejidad*: de diseño, ciclomática, cantidad de métodos por clase, etc.
	- *Mantenibilidad*: densidad de comentarios en código, índice de madurez del software, etc.
	- *Calidad*: cantidad de defectos, cantidad de problemas reportados, etc.
	- *Confiabilidad*: tiempo entre fallos, tiempo de recuperación, etc.
	- *Tamaño*: LOC, puntos función, etc.
	- *Usabilidad*: facilidad de aprendizaje, errores cometidos por los usuarios, etc.
2. ***Métricas de proyecto***
	- *Coste*: coste del desarrollo, coste mensual salarial, etc.
	- *Esfuerzo*: cantidad de horas trabajadas, distribución del esfuerzo por fase, etc.
	- *Productividad*: puntos función liberados por semana, etc.
	- *Estabilidad*: peticiones de cambio aceptadas en desarrollo, impacto del cambio, etc.
	- *Seguimiento*: desviación en coste, tiempo, etc.
3. ***Métricas de proceso***
	- *Pruebas*: cobertura de las pruebas, etc.
	- *Mantenimiento*: % de correcciones atrasadas, etc.

**MODELADO DE MÉTRICAS**
****
![[Pasted image 20250501200038.png]]

***Modelo ficha-indicador***
>Permite adaptar a cada organización las plantillas necesarias para sus métricas

Este modelo propone:

| Campos                              |
| ----------------------------------- |
| Nombre                              |
| Identificador/código                |
| Categoría                           |
| Objetivo                            |
| Descripción                         |
| Ámbito de uso                       |
| Fuente de toma de datos             |
| Entradas                            |
| Definiciones y abreviaturas         |
| Fórmula                             |
| Escala o unidad de medida           |
| Criterios de análisis               |
| Datos históricos                    |
| Metas                               |
| Frecuencia de reporte               |
| Información adicional               |
| Versión, fecha, responsables, firma |

***Cuadro de mandos***
>Herramienta de gestión que proporciona una visión rápida y clara de indicadores clave de rendimiento (KPIs) y de las métricas relevantes para una organización, departamento o proyecto.

Los KPIs son medidas utilizadas para evaluar el rendimiento (satisfacción del cliente, rotación del inventario, etc.).

**GQM Y PSM**
****
***Goal Question Metrics***
>Modelo basado en definir una meta, realizar preguntas sobre esta y evaluar su cumplimiento.

Un ejemplo puede ser que los clientes reclamen por retrasos en la resolución de incidencias de mantenimiento.
![[Pasted image 20250501200814.png]]

***Practical Software and Systems Measurement***
>Conjunto de prácticas para medir y evaluar el rendimiento y la calidad. Proporciona un marco para medir, evaluar y gestionar los aspectos clave del desarrollo de software.

1. *Componentes*
	- *Modelo de proceso y medición*
		Se describen las fases y tareas de forma iterativa.
	- *Modelo de información*
		Se definen las relaciones entre la terminología común y los conceptos de medición.
2. *Esquema*
	![[Pasted image 20250501201118.png]]
	![[Pasted image 20250501201719.png]]

**MÉTRICAS HABITUALES**
****
***MÉTRICAS DEL PROCESO DE PRUEBAS***
Podemos medir varios campos en el proceso de pruebas.
1. *Densidad de defectos durante las pruebas de sistema*
	Cuando tenemos muchos errores en las pruebas de sistema, puede significar que hubo muchos errores durante el desarrollo. Se debe hacer más esfuerzo en las pruebas y una detección de fallos más efectiva.
2. *Patrón de llegada de defectos durante las pruebas de sistema o tiempo transcurrido entre defectos*
	Mide la estabilidad o el tiempo transcurrido entre fallos. Se suele medir en semanas o meses.
3. *Métricas de control y seguimiento del plan de pruebas*
	- Porcentaje de trabajo realizado para preparar los casos de prueba.
	- Porcentaje de trabajo realizado en la preparación del entorno de pruebas.
	- Número de casos de prueba ejecutados.
	- Número de casos de prueba fallidos.
	- Índice de fallos.
	- Cobertura de pruebas.
	- Cumplimiento de fechas de hitos en las pruebas.
	- Coste de las pruebas.
4. *Curva S del progreso de pruebas*
	![[Pasted image 20250501202238.png]]
	Consiste en seguir el progreso de las pruebas y compararlo con el plan para poder tomar acciones. Permite evitar que se prescinda de las pruebas cuando hay retrasos.

***MÉTRICAS DEL PROCESO DE DETECCIÓN DE DEFECTOS***
Podemos medir ciertos campos en el proceso de detección de defectos.
1. *Patrón de eliminación de defectos en cada fase*
	Es necesario registrar los defectos en todas las fases del ciclo de desarrollo. En las fases de diseño y codificación también se usan métricas para la cobertura de la inspección, es esfuerzo, etc. 

2. *Efectividad en la eliminación de defectos (DRE)*
	Se mide como $\dfrac{\text{defectos eliminados fase desarrollo}}{\text{defectos producto}}$. 
	El denominador es una aproximación de los posibles defectos. Suele aproximarse como la suma de los defectos eliminados en desarrollo y los encontrados después.
	
	El DRE se puede calcular para todo el proceso de desarrollo o en cada fase. Cuando más alto mejor, ya que indica que se transmitirán menos defectos a las siguientes fases. 

***MÉTRICAS DEL PROCESO DE DETECCIÓN DE DEFECTOS EN EL CLIENTE***
Es más costoso conseguir un nuevo cliente que mantener uno ya existente. Por eso se deben mantener satisfechos y para ello hay que medir la calidad y satisfacción. 
1. *Problemas del cliente (PUM)*
	Se miden los problemas encontrados por un cliente en una unidad de tiempo. 
	Se mide como $\dfrac{\text{total de problemas reportados por cliente}}{\text{numero de licencias}×{\text{numero de meses}}}$ 

2. *Satisfacción del cliente*
	Se mide con encuestas o entrevistas. Se suele usar una escala con categorías de satisfacción, como muy satisfecho, satisfecho, insatisfecho, etc.
	Se estima el nivel de satisfacción global a partir de una muestra de clientes. 

***MÉTRICAS DEL PROCESO DE DETECCIÓN DE MANTENIMIENTO***
El proceso de mantenimiento busca corregir los defectos lo antes posible y hacerlo con una buena calidad para mejorar la satisfacción del cliente. 
1. *Trabajo acumulado en las correcciones*
	Es la cantidad de problemas reportados al final de cada unidad de tiempo. 
2. *Índice de gestión de trabajo acumulado (BMI)*
	Se mide como el porcentaje de $\frac{\text{numero de problemas cerrados en una unidad de tiempo}}{\text{numero de problemas registrados}}$. 
	También se puede calcular el número de problemas sin solucionar. 
3. *Tiempo de respuesta de las correcciones*
	Según la gravedad de los problemas, tiempo que tarda en solucionarlos. 
4. *Porcentaje de correcciones atrasadas*
	Se mide como el porcentaje de $\dfrac{\text{numero de correcciones atrasadas}}{\text{numero de correcciones solucionadas a tiempo}}$
5. *Calidad de las correcciones*
	Es el número de correcciones defectuosas. Se puede ver el porcentaje de correcciones que son defectuosas en base al total en una unidad de tiempo. 

***MÉTRICA DEL TAMAÑO DEL PRODUCTO***
Se hace con puntos función. Antes se hacía midiendo las LOC, pero ya no es aceptable, ya que varía mucho dependiendo de cosas como el lenguaje de programación o la habilidad de cada programador.

Una mejor medida es calcular el esfuerzo necesario para entregar una cantidad de puntos función. Es independiente de la tecnología y se puede aplicar en todos los ciclos de vida y todas las fases. 

Las etapas de este cálculo son:
1. Se identifican las funciones disponibles para el usuario y se organizan en componentes.
2. Se clasifican y se ponderan según la complejidad.
3. Se ajusta el total de acuerdo a las características del entorno.