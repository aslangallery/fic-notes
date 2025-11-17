---
Name: 4 - CMMI
tags:
  - teoría
asignatura: ACL
---
***[[Aseguramiento de la Calidad]]***

**INTRODUCCIÓN A CMMI**
****
>El objetivo del CMMI es ayudar a las empresas a producir mejor software.

Para ello, hay que mejorar el tiempo y la forma de hacerlo, reducir el número de defectos e intentar reducir lo máximo posible el coste.

CMMI indica lo que hay que hacer, pero no cómo se hace.

***Calidad en el ciclo de vida***
La calidad debe cubrir todo el ciclo de vida del software, pero no es obligatorio hacerlo con un único modelo de calidad.

Podemos usar CMMI-ITIL para el manejo de servicios y CMMI-DEV para el desarrollo de software.

**CONSTELACIONES CMMI V1.3**
****
>CMMI son las siglas de Capability Maturity Model Integration.

Es una guía que sirve para mejorar los procesos asociados al desarrollo y mantenimiento del software. A diferencia de ISO, está solo enfocado al software y los resultados de las auditorías que realizan son públicos.

>Una constelación CMMI es un conjunto de prácticas y procesos que se agrupan para formar un modelo que se puede utilizar en un área específica de interés dentro de una organización.

***Constelaciones CMMI V1.3***
- *CMMI-DEV (development)*: guía para desarrollo de productos y servicios software.
- *CMMI-ACQ (acquisition)*: compra de productos y servicios.
- *CMMI-SVC (services)*: gestión de servicios a clientes.

***Niveles de madurez***
CMMI mide la calidad en niveles de madurez. En la ISO una empresa está certificada o no. En cambio, CMMI propone varios niveles donde en cada uno hay un conjunto de objetivos a alcanzar llamados Process Areas (PA). Una vez alcanzados todos los objetivos, se pasa de nivel.

No es posible saltarse niveles. Se puede solicitar directamente auditar para nivel 3, pero una vez esté en nivel 3, debe auditarse de nivel 4 para pasar al 5.

![[Pasted image 20250426112150.png]]

***Process Areas***
>Un PA es un conjunto de prácticas relacionadas que, al implementarlas correctamente, satisfacen un conjunto de metas que afectan directamente a la calidad de la zona en la que se enfocan.

1. ***Metas***
	- *Genéricas (GG)*: objetivos en múltiples PA.
	- *Específicas (SG)*: objetivos que solo aplican a una PA.
2. ***Prácticas***
	Son subdivisiones de las metas. No son obligatorias, se usan como ejemplo de posibles prácticas que se pueden hacer para satisfacer las metas. Si la organización es capaz de demostrar que su proposición es igual de válida que las prácticas que propone CMMI, es totalmente válido utilizarla.
	
	Se dividen en:
	- *Genéricas (GP)*: se aplican a múltiples PA para satisfacer las GG.
	- *Específicas (SP)*: se aplican a una PA para satisfacer una SG.
3. ***Esquema CMMI***
	![[Pasted image 20250426112656.png]]
4. ***Lista de PAs***
	1. **PAs base** (genéricas a todas las áreas de una organización)
		![[Pasted image 20250426113034.png]]
	2. **PA de CMMI-SVC**
		![[Pasted image 20250426113114.png]]
	3. **PA de CMMI-ACQ**
		![[Pasted image 20250426113136.png]]
	4. **PA de CMMI-DEV**
		![[Pasted image 20250426113200.png]]
	5. **Organización en niveles**
		![[Pasted image 20250426113400.png]]

**CMMI-DEV V1.3**
****
1. ***PAs y categorías***
	![[Pasted image 20250426113653.png]]
2. ***Nivel 2***
	1. *Gestión de Requisitos (REQM)*
		Gestionar los requisitos del producto y de los componentes del producto y asegurar la alineación de estos requisitos, los planes y los productos de trabajo del proyecto.
	2. *Planificación del Proyecto (PP)*
		Establecer y mantener planes que definan las actividades del proyecto.
	3. *Monitorización y Control del Proyecto (PMC)*
		Proporcionar una comprensión del progreso del proyecto para que se puedan tomar las acciones correctivas apropiadas cuando el rendimiento del proyecto se desvíe significativamente del plan.
	4. *Medición y Análisis (MA)*
		Desarrollar y mantener la capacidad de medición utilizada para dar soporte a las necesidades de información.
	5. *Aseguramiento de la Calidad del Proceso y del Producto (PPQA)*
		Proporcionar al personal y a la gerencia una visión objetiva de los procesos y los productos de trabajo asociados.
	6. *Gestión de la Configuración (CM)*
		Establecer y mantener la integridad de los productos de trabajo utilizando la identificación, el control, el informe del estado y las auditorías de la configuración.
	7. *Gestión de Acuerdos con Proveedores (SAM)*
		Gestionar la adquisición de productos y servicios de proveedores.
3. ***Nivel 3***
	1. *Desarrollo de Requisitos (RD)*
		Deducir, analizar y establecer los requisitos del cliente, producto y componentes del producto.
	2. *Solución Técnica (TS)*
		Seleccionar, diseñar e implementar soluciones para los requisitos.
	3. *Validación (VAL)*
		Demostrar que un producto o componente de producto cumple con su uso previsto cuando se ubica en el entorno previsto.
	4. *Verificación (VER)*
		Asegurar que los productos de trabajo cumplen con los requisitos especificados.
	5. *Integración del Producto (PI)*
		Ensamblar el producto a partir de sus componentes, asegurar que el producto, una vez integrado, se comporta correctamente y entregar el producto.
	6. *Gestión de Riesgos (RISKM)*
		Identificar problemas potenciales antes de que ocurran, para que las actividades de tratamiento de riesgos puedan planificarse y activarse para mitigar el impacto de los riesgos en los objetivos.
	7. *Gestión Integrada del Proyecto (IPM)*
		Cada proyecto ajusta los procesos estándar definidos a sus necesidades particulares.
	8. *Análisis y Resolución de Decisiones (DAR)*
		Definición de un proceso estructurado de toma de decisiones en el que las alternativas se comparan con criterios objetivos establecidos para así tomar la mejor decisión posible.
	9. *Definición de Procesos Organizacionales (OPD)*
		Establecer y mantener estándares en procesos organizacionales (procedimientos, guías, etc.).
	10. *Enfoque en Procesos (OPF)*
		Evidenciar el entendimiento de los procesos estándar por los miembros de la organización y la identificación de mejoras.
	11. *Formación (OT)*
		Definición de un plan para que los miembros de la organización puedan obtener las habilidades y conocimientos necesarios para su trabajo.
4. ***Nivel 4***
	- *Rendimiento de Procesos (OPP)*
		Mecanismos para derivar objetivos cuantitativos de calidad desde el conjunto de objetivos de negocio de la organización.
	- *Gestión Cuantitativa del Proyecto (QPM)*
		Manejo de métricas cuantitativas de los procesos para alcanzar los objetivos de calidad establecidos. Estos datos también tienen que ser analizados para identificar oportunidades de mejora de los procesos. Indicadores a nivel organizacional.
5. ***Nivel 5***
	- *Gestión del Rendimiento de la Organización (OPM)*
		Evidencias de que se identifican mejoras continuas para la organización.
	- *Análisis Causal y Resolución (CAR)*
		Evidencias de que se identifican las causas de los problemas y se toman acciones correctivas.
		

***Representación continua***
Alternativa a los niveles de maduración (staged), se trabaja por PAs. 
Cada PA tiene un Nivel de Capacidad (CL) del 1 al 5. Cada CL exige el cumplimiento de unas métricas a través de unas prácticas. 
Este enfoque es más flexible, ya que permite a la organización seleccionar las PA y los CL según sus objetivos. Existe un mecanismo de equivalencia entre continuos y staged.

![[Pasted image 20250426124214.png]]

**ASPECTOS IMPORTANTES Y DEFINICIONES DE CMMI**
****
Según su nivel de madurez, las organizaciones pueden ser:
- *Organización inmadura*: aquella que lleva a cabo sus proyectos sin una definición previa de los procesos a seguir. Apaga fuegos, impredecible.
- *Organización madura*: aquella que lleva a cabo sus proyectos de una manera estructurada. Procesos definidos y documentados. Responsabilidades definidas. Resultados predecibles. Eficiencia medible. Mejor calidad. Menor tiempo y coste.

Los distintos niveles implican:
1. *Nivel 1*: la organización es totalmente inmadura.
2. *Nivel 2*: se exige la planificación y control de proyectos y soporte básico.
3. *Nivel 3*: desarrollo del producto e institucionalización de los procesos. La documentación es clave.
	- Elegir ciclo de vida en PP se puede hacer ad-hoc en N2. Si se ha alcanzado N3, habrá un procedimiento estándar definido para seleccionar y documentar el CV.
	- Las descripciones de proceso las deben hacer quienes ejecutan dicho proceso, apoyados por calidad.
4. *Nivel 4*: se exige la medición cuantitativa de los proyectos y procesos a todos los niveles.
5. *Nivel 5*: se exige la mejora continua a través del análisis y comparación de mediciones a lo largo del tiempo. Reducción de costes al anticipar los problemas. Análisis estratégicos.

***CMMI aplicado a diferentes campos***
- *Gestión de proyectos*: PP y PMP en N2.
- *Gestión de riesgos*: ligera en PP y PMP en N2. Formal en RISKM en N3.
- *Gestión de la configuración*: CM en N2 para proyectos. En otros niveles se mejora con diferentes SP.
- *Ciclos de desarrollo*:
	- *Requisitos*: REQM en N2, RD en N3.
	- *Diseño e Implementación*: TS en N3.
	- *Integración de sistemas*: PI en N3.
	- *Pruebas*:
		- Unitarias: TS en N3.
		- Aceptación: VAL en N3.
		- Funcionales, sistema, integración: PI y VER en N3.

***Implementación de CMMI***
Se realiza un proyecto de mejora en la empresa. Hay que diagnosticar cómo se realizan los procesos actuales en la empresa. Además hay que tener disponible la documentación, herramientas, etc.
Si se detecta que no se cumple alguna PA, se realizan las mejoras necesarias. 

Se suelen designar una serie de perfiles:
1. *Sponsor*: lidera la acción, presupuesta y supervisa. Directivo.
2. *Champion*: relaciones públicas. Es el que tiene la idea y la vende a dirección. Suele ser el responsable de calidad.
3. *EPG (Engineering Process Group) Lead*: jefe del proyecto de mejora. Planifica el trabajo, asigna tareas y realiza seguimiento. Responsable de calidad.
4. *EPG Members*: jefes de los equipos PAT.
5. *PAT (Process Action Teams)*: profesionales experimentados que ejecutan los procesos. 
6. *Transition Partner*: 1 o 2 consultores externos que ayudan con su experiencia en el proyecto de mejora.

**CMMI V2.0**
****
>Actualización del modelo que se lleva a cabo tras la compra de CMMI por ISACA.

CMMI pasa a ser un modelo único, desaparecen las constelaciones, pero se pasa a arquitectura continua para que las empresas elijan las PAs que prefieran. De cara a la evaluación hay distintas vistas que agrupan las PAs que se van a evaluar.

El modelo se divide en 25 PAs, 18 de las cuales son comunes. Cada PA se divide en Practice Groups (PG) que pueden ir del 1 al 5. A su vez, cada PG se divide en Practices.

![[Pasted image 20250426124148.png]]

**EVALUACIÓN CMMI 2.0**
****
El proceso de evaluación se hace a través de distintas auditorías. Los auditores pueden emitir un diploma que señala la madurez de la empresa.

Existen 3 tipos distintos de auditorías:
1. *Benchmark appraisal*
	- El más riguroso.
	- Permite obtener el diploma acreditativo.
	- Válido 3 años.
	- Muestreo aleatorio de evidencias mediante métodos matemáticos para evitar sesgo en los datos.
2. *Sustainment appraisal*
	- Asegurar que la madurez se mantiene con el tiempo.
	- Puede extender el resultado de un Benchmark Appraisal 2 años más.
	- Se pueden hacer 3 antes de someterse a un nuevo Benchmark Appraisal.
	- Auditoría más reducida que la anterior.
3. *Evaluation appraisal*
	- Foto rápida del estado de los procesos para iniciar un programa de mejora.
	- Evaluación del progreso en la implantación de CMMI.

***¿Quién evalúa?***
El SEI acredita a los auditores o Lead Appraisers, encargados de emitir los certificados. Tras el benchmark appraisal, el LA envía cierta documentación al SEI para que este asegure la calidad de la auditoría. Si todo va bien, los resultados se aceptan y publican en el PARS. 

En los benchmark appraisal participan:
- *Sponsor*: encargado de liderar el proyecto de mejora.
- *Appraisal Team Leader*: responsable de realizar la evaliación (LA).
- *Appraisal Team Members (ATM)*: personal interno y/o externo con el suficiente conocimiento y experiencia.
- *Organizational Unit Coordinator (OUC)*: encargado de actuar de interfaz entre el equipo de evaluación y la empresa. Es quien proporciona principalmente la información necesaria al evaluador. 
- *Participantes seleccionados*: para proporcionar información (jefes de proyecto o desarrolladores de proyectos evaluados) durante la evaluación.

***¿Qué se evalúa?***
1. Empresa selecciona las PAs y PGs a evaluar.
2. Empresa selecciona qué unidad organizacional se va a evaluar.
3. Empresa selecciona una muestra de proyectos a evaluar.
	- *Proyectos objetivo*: proporciona evidencia para todas las PA a evaluar. No importa si está acabado o no. Debemos seleccionar por lo menos uno de este tipo.
	- *Proyecto no objetivo/Función apoyo*: aporta evidencia extra de alguna PA.
4. En la auditoría se comprueba que las PAs y PGs se han alcanzado comprobando los practices usando las evidencias objetivas (registros) que se dejan de las mismas. Hay 2 tipos de evidencia objetiva:
	- *Artefacto*: salida directa o indirecta de la implementación de una práctica (plan de proyecto, documento de estimación, etc.). Pre On-Site.
	- *Afirmación*: confirmación en palabras o escritas que corroboran la implementación de una práctica (entrevistas, cuestionarios, etc.). On-site, también pueden ser negativas.
	Las distintas evidencias se guardan en la **Base de Datos de Evidencias Objetivas**.
5. **Pre On-Site**
	- *Presentar*
		1. Obtener el compromiso de la gerencia y la organización.
		2. Al menos 6 meses antes de las actividades on-site.
		3. Sponsor + LA.
	- *Planificar*
		1. ATMs, formación, unidad organizacional, muestra de proyectos, calendario, riesgos, restricciones, logística.
		2. 4 meses antes de las actividades on-site.
		3. Sponsor + LA + OUC.
	- *Preparar*
		1. Ejecutar la formación, completar la BDEO, al menos una Readiness Review, convocar participantes seleccionados, completar el Plan de Evaluación en el SAS.
		2. Entre 4 meses y 2 semanas antes de las actividades on-site.
		3. LA + OUC + ATMs.
6. **On-Site** (Ejecución del plan de evaluación)
	- La BDEO y resto de información del sistema de calidad se copia a un repositorio. Sólo se añade a éste la información solicitada en la evaluación (que no puede ser de nueva creación). El incumplimiento puede causar fin de la evaluación.
	- Salas y equipamiento adecuado. Deben estar "aislados".
	- Revisión documental. Entrevistas y cuestionarios. Reuniones de presentación (inicial, resultados preliminares, resultados finales).
	- Para cada practice, se espera un artefacto directo, apoyado por uno indirecto y/o una afirmación.
	- LA + OUC + ATMs + representantes seleccionados.
	- Lo que sucede en la evaluación, se queda en la evaluación.
	- Se determina cómo se ha implementado:
		- *Fully Implemented (FI)*: artefactos directos presentes y adecuados. Apoyados por artefactos indirectos y/o afirmaciones. No se detectan debilidades significativas.
		- *Largely Implemented (LI)*: artefactos directos presentes y adecuados. Apoyados por artefactos indirectos y/o afirmaciones. Se han notado una o más debilidades. 
		- *Partially Implemented (PI)*: artefactos directos inadecuados o inexistentes. Artefactos indirectos y/o afirmaciones indican que parte de la práctica ha sido implementada.
		- *Not Implemented (NI)*: artefactos directos inexistentes o inadecuados. No se encuentra otra evidencia que soporte la práctica.
	- Si todas se evalúan como FI o LI, se considera que el PG está satisfecho. Aunque, si la mayoría de las practices tienen debilidades, podría considerarse que el PG no ha sido satisfecho.
7. Publicación y anuncio en el SAS.


