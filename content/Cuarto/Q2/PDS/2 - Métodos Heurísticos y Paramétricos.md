---
Name: 2 - Métodos Heurísticos y Paramétricos
tags:
  - teoría
asignatura: PDS
---
***[[Proyectos de Desarrollo Software]]***

**JUICIO DE EXPERTOS**
****
> Técnica de estimación software basada en el conocimiento previo e intuición de un grupo de personas con experiencia en la realización y estimación de proyectos.

Se suele utilizar cuando no podemos basarnos en datos empíricos para poder estimar. 
Para reducir riesgos y hacerla más precisa, se acostumbra a buscar la opinión de más de una persona para tomar la decisión de cómo estimar.

***MÉTODO DELPHI***
> Proceso sistemático e iterativo encaminado hacia la obtención de las opiniones y, si es posible, del consenso de un grupo de expertos.

Para poder llevar a cabo este proceso, existirá una persona que lo coordine y varios subcoordinadores.

1. *Características*
	- Mantener el anonimato de las personas participantes.
	- Hacer sesiones de feedback controladas.
	- Llegar a la solución por medio de estadísticas extraídas de quien participa.
	- Realizar varias iteraciones hasta llegar a un consenso entre las partes.
2. *Funcionamiento*
	1. Cada coordinador expone las especificaciones del proyecto de manera individualizada a los miembros del panel.
	2. El grupo de expertos responde a las preguntas planteadas.
	3. La coordinación evalúa las respuestas y decide si se ha llegado a un consenso.
	4. Durante el desarrollo del proceso, solo existen reuniones individuales, nunca entre personas del grupo.

***DELPHI DE BANDA ANCHA***
> Variante del método Delphi donde se realizan las estimaciones de modo individuales, pero se hace una reunión grupal antes de responder a las preguntas.

La ventaja que ofrece este método es el intercambio de opiniones y experiencias anteriores.
Esto puede dar lugar a evaluaciones más subjetivas, sobre todo por parte de inexpertos.

1. *Método*
	1. Una vez la coordinación expone el proyecto y entrega las preguntas, los participantes intercambian opiniones sobre la estimación del mismo.
	2. Posteriormente, responden el cuestionario de forma individualizada y una vez están resultados, se les entrega una estimación promedio del grupo para que hagan una nueva estimación.
	3. De nuevo, se reúnen los expertos compartiendo opiniones y se repite el proceso de encuesta hasta que converja la estimación.

**ESTIMACIÓN POR ANALOGÍA**
****
> Versión más formal que el juicio de expertos, donde se utilizan datos de proyectos anteriores para estimar el actual.

Esto sirve para poder afinar la estimación de aquellos puntos en común entre los proyectos anteriores y el actual.

1. *Ventajas*
	- [p] Existe un histórico de proyectos para afinar las estimaciones.
2. *Inconveniente*
	- [c] No es fácil discernir el grado de similitud entre proyectos anteriores y el actual.

**ESTIMACIÓN POR DESCOMPOSICIÓN**
****
> Consiste en dividir el proyecto en tareas pequeñas y estimar cada una de ellas para conseguir la estimación final.

Se suele acompañar de un diagrama de descomposición (WBS) que se crea en la fase de diseño.

1. *Ventajas*
	- [p] Permite entender desde un bajo nivel las tareas que se van a abordar.
2. *Inconveniente*
	- [c] La descomposición puede no hacerse de un modo adecuado, olvidándonos de requisitos y generando desvíos en la estimación.

**ESTIMACIÓN ALGORÍTMICA**
****
> Técnica de estimación software basada en fórmulas matemáticas y modelos predictivos que hacen uso de correlaciones entre elementos y datos históricos.

Antes de aplicarlas a un dominio específico, se extrapolan a otros proyectos ya realizados para comprobar su buen funcionamiento.

**MÉTODOS DE ESTIMACIÓN PARAMÉTRICOS**
****
> Estiman los principales parámetros de un proyecto tomando como referencia el tamaño del producto que se va a desarrollar.

Los más usados son:
- Métodos basados en cálculo de puntos de función (FPA, E&QFP, ...).
- SLIM (Software Lifecycle Management).
- COCOMO (Constructive Cost Model).
