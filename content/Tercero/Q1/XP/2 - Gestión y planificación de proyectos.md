---
Name: 2 - Gestión y planificación de proyectos 
tags:
  - teoría
asignatura: XP
---
***[[Gestión de Proyectos]]***

**OBJETIVOS**
****
1. Satisfacer necesidades de información de gestión.
2. Fomentar cultura de gestión que contribuya al aumento de la productividad.

**METODOLOGÍA**
****
1. *Definición de proyecto*
	Conjunto de:
	- Recursos humanos, materiales, financieros...
	- Organización en actividades.
	- Planificación.
	- Productos específicos.
	- Objetivos a alcanzar.
	- Entorno de riesgo.
	
	Características:
	- **Discreto**: inicio y/o fin definidos.
	- **Complejo**: tareas interrelacionadas.
	- **Único**: en relación al producto que se obtiene y al entorno.

2. *Modelo de ciclo de vida*
	- **Evaluación** y aprobación del proyecto.
	- **Planificación**: asignación de recursos.
	- **Ejecución**: planificación detallada, ejecución y seguimiento. En caso de que haya una desviación del 20% comparado con cómo debería ir el proyecto, se debería volver a planificar.
	- **Finalización**.
	- **Gestión**: riegos, calidad, cambios.

**CONCEPTOS DE PLANIFICACIÓN Y SEGUIMIENTO**
****
1. *Definición de tarea/actividad*
	- Unidad elemental de la planificación.
	- Identificada por duración y/o consumo de recursos.
	- Una vez comenzada se vuelve homogénea.
	- Produce un resultado.
2. *Hito*
	- Su duración y su esfuerzo son 0.
	- Indica un acontecimiento.
	- No consume recursos.
	- Describen puntos de control de seguimiento.
	- También se usan para subcontratas.
		- [c] esfuerzo
		- [p] tiempo
		- [p] coste
		- [p] calidad
3. *Recurso*
	Debe ser dado de alta si su uso va a ser compartido y origina conflictos de uso.
	Clases de recurso:
	1. **Humanos**
		Individuales o grupos de recursos homogéneos (deben tener igual nivel de conocimientos, nivel...).
	2. **Materiales**
	3. **Maquinaria**
		Problemática de aperos (solución en prácticas):
			Tractorista con apero (automatiza labor de cultivo)
			Cooperativa con 4 tractoristas/tractor.
			Solo hay 3 aperos
			Se quieren cultivar las 4 propiedades
			¿N.º recursos dados de alta? -> 4 de tipo trabajo/esfuerzo (humano)
			Apero -> 300% -> recursos de tipo esfuerzo
			¿N.º recursos por actividad (24h x h)?
				- Tractorista 100% + apero 100% -> 12 h x h -> el apero tiene esfuerzo pero el esfuerzo lo lleva el tractorista, el apero sólo está pero si no está, no se puede hacer la actividad.
		**Project no permite recursos tipo apero**

	Otra clasificación:
	-  **Consumibles**: no se reutilizan.
	-  **Recurrentes**: pueden ser reutilizados.
	
4. *Planificación y programación*
	1. **Planificación**: establecimiento de las actividades.
	2. **Programación**: planificación + asignación de recursos.

**ASPECTOS IMPORTANTES**
****
- *Calendario*: importante tener en cuenta los periodos y festivos.
- *Duración*: los proyectos se pueden medir en días, horas, semanas...
- *Pool de RRHH y datos asociados*: gestionar el departamento al que pertenece el personal, coste por hora, disponibilidad...
- *Elementos de coste adicionales*: coste de recursos consumibles, materiales...

**TÉCNICAS USUALES**
****
1. *Técnicas de representación*
	- **Diagrama de Gantt**
		Representación en escala temporal de actividades. Representación simplificada de red de precedencia (grafo dirigido acíclico). Las barras de las tareas indican la duración.
	- **Red de precedencia**
		Basados en grafos dirigidos acíclicos. Permite reflejar relaciones entre las actividades de un proyecto. La red que se obtiene permite identificar el camino crítico de la planificación.
		Existen dos posibles notaciones:
		1. *PDM*: Los nodos son las actividades y los vectores son las restricciones. Existen 4 tipos de dependencia (CC, CF, FC, FF). 
		2. *ADM*: Obliga a manejar hitos. Los vectores son las actividades y los nodos son las dependencias. Existen 3 tipos de precedencias (lineales, de convergencia y de divergencia).
	- **Histograma**
		Diagrama de barras que representa la distribución de datos cuantitativos de una variable. Muestra la asignación de recursos a lo largo del tiempo.
2. *Técnicas de estructuración*
	- **WBS (Work Breakdown Structure)**
		Estructura las tareas por tipos, niveles... atendiendo a diferentes niveles de detalle. Estimación por descomposición (se desglosa, estimando los nodos hoja, hasta llegar a una actividad elemental).
	- **OBS (Organisational Breakdown Structure)**
		Estructura por unidades organizativas que poseen responsabilidad sobre la realización del proyecto.
3. *Técnicas de programación*
	- **PERT (Program Evaluation and Review Technique)**
		Se orienta a eventos o sucesos (ADM) y permite considerar probabilidad.
		Pasos a seguir:
		1. Elaborar red de precendencia ADM.
		2. Cálculo de tiempos: pesimista (tiempo máximo), optimista (tiempo en el mejor de los casos) y más probable. Realizar media ponderada con la siguiente fórmula:
			$$(optimista + 4*masProbable + pesimista) / 6$$
		3. Cálculo de fechas early y late de cada actividad. Así se calcula la holgura.
		4. Determinación del camino crítico. Un hito y una actividad crítica tienen holgura 0.
		5. Definición de fechas más tempranas de inicio y fin.
	- **CPM (Critical Path Method)**
		Se orienta a actividades (PDM) y no permite considerar probabilidad. Permite calcular la lista de actividades con menos flexibilidad en su ejecución (camino crítico).  
		Pasos a seguir:
		1. Elaboración de red de precedencia PDM.
		2. Determinar restricciones lógicas entre las actividades.
		3. Asignar la duración de cada actividad en función del esfuerzo y de la asignación de recursos.
		4. Obtener el camino crítico.
		5. Afinar la planificación:
			- Romper el camino crítico para reducir la duración del proyecto.
			- Nivelación de los recursos.
		6. Establecer la línea base del proyecto.

**TIPOS DE RESTRICCIONES**
****
1. *CC (Comienzo a Comienzo)*
	Actividad B no empieza hasta que A no haya comenzado.
2. *CF (Comienzo a Fin)*
	B no puede acabar hasta que A no haya comenzado.
3. *FC (Comienzo a Fin)*
	B no puede comenzar hasta que A no haya terminado.
4. *FF (Fin a Fin)*
	B no puede terminar hasta que A no haya terminado.

- [n] No confundir línea base en gestión de proyectos con línea base en gestión de configuración de software.

**COMUNICACIÓN EFICAZ**
****
