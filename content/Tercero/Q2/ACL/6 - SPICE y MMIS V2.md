---
Name: 6 - SPICE y MMIS V2
tags:
  - teoría
asignatura: ACL
---
***[[Aseguramiento de la Calidad]]***

**SPICE Y MMIS V2**
****
***SPICE***
>Siglas de Software Process Improvement and Capability Determination. Modelo de calidad basado en la evolución de la capacidad de los procesos y el nivel de madurez de la organización.

Permite evaluar del 0 al 5 el nivel de madurez de los procesos de una organización. Sigue un marco genérico que permite a las organizaciones adaptarse a sus necesidades. No propone un modelo de procesos específico, solamente proporciona pautas para evaluar y mejorar los modelos de procesos que decida la organización.

Se evalúa por procesos como en CMMI. Se puede hacer siguiendo la ISO 15504. Al aplicarse a software, el modelo de procesos se suele basar en la ISO 12207, que es un estándar para cubrir todos los procesos del ciclo de vida del software.

![[Pasted image 20250503121115.png]]

*Esquema de procesos*
1. ***Esquema PathFinder***
	Modelo complejo.
2. ***Esquema AENOR***
	Propone 3 niveles de procesos:
	- *Nivel 1*: la organización tiene los procesos implementados de forma que solo se alcanzan de forma muy básica los resultados. Define 3 procesos:
		- *SUM*: suministro.
		- *MVC*: modelo de ciclo de vida.
		- *GCS*: gestión de la configuración software.
	- *Nivel 2*: la organización demuestra planificación, control y seguimiento de sus procesos y productos. Define 7 procesos:
		- *RQU*: definición de los requisitos de usuario.
		- *RQSIS*: análisis de los requisitos del sistema.
		- *PP*: planificación del proyecto.
		- *ECP*: evaluación y control del proyecto.
		- *GC*: gestión de la configuración.
		- *MED*: medición.
		- *ACS*: aseguramiento de la calidad software.
	- *Nivel 3*: los procesos están estandarizados en toda la organización. Define:
		- Análisis de los requisitos del software.
		- Diseño de la arquitectura del software.
		- Diseño de la arquitectura del sistema.
		- Gestión de infraestructuras.
		- Gestión de recursos humanos.
		- Gestión de riesgos.
		- Gestión de la decisión.
		- Integración de sistemas.
		- Verificación de sistemas.
		- Validación de sistemas.
		- Operación de sistemas.

***MMIS V2***
>Siglas de Modelo de Madurez de Ingeniería del Software. Sirve como marco para evaluar y mejorar la madurez de los procesos de forma similar a SPICE.

Sigue el modelo de procesos de ISO 12207:2017. Define 21 procesos.
Sigue el modelo de evaluación de ISO 33000. Evalúa según ISO 33002 y propone métricas según USO 33020.

Define 5 niveles de madurez y 5 niveles de capacidad por proceso:
- *Nivel 1 - Básico*: los procesos de nivel de madurez 1 alcanzan nivel de capacidad 1.
- *Nivel 2 - Gestionado*: los procesos de nivel de madurez 2 alcanzan nivel de capacidad 2.
- *Nivel 3 - Establecido*: los procesos de nivel de madurez 2 y 3 alcanzan nivel de capacidad 3.
- *Nivel 4 - Predecible*: los procesos de nivel de madurez 2, 3 y 4 alcanzan nivel de capacidad 3. Además los procesos que la organización considere que deben definir métricas deben alcanzar nivel de capacidad 4.
- *Nivel 5 - Innovado*: los procesos de nivel de madurez 2, 3 y 5 alcanzan nivel de capacidad 3. Además los procesos que la organización considere que deben definir métricas deben alcanzar nivel de capacidad 5.

**ALGUNOS MODELOS DE CALIDAD MÁS**
****
***ITMARK***
>Guía diseñada para PYMEs y empresas pequeñas de menos de 10 empleados que les ayuda a buscar la mejora continua de forma sostenible. Se puede usar también en grandes empresas.

Sirve como base para luego implementar CMMI.

1. *Campos que evalúa ITMark*
	- Procesos de gestión y desarrollo software mediante niveles 2/3 de CMMI.
	- Procesos de gestión de negocio mediante ISO 9000 y EFQM.
	- Procesos de gestión de seguridad de la información mediante ISO 27000.
2. *Niveles*
	- *ITMark*: acredita que la empresa es consciente de temas de gestión técnica, seguridad y negocio y ha comenzado a controlarlos.
	- *ITMark Premium*: acredita que la empresa tiene buen nivel de negocio, seguridad y desarrollo software.
	- *ITMark Elite*: acredita que la empresa tiene alto nivel de negocio, seguridad y desarrollo software y ofrece productos de buena calidad.	

	|  ITMark   | CMMI    |
	| --- | --- |
	|   ITMark  | Un poco más de nivel 1    |
	|   ITMark Premium | Casi nivel 2    |
	|   ITMark Elite | Casi nivel 3    |

***SwTQM***
> Modelo propuesto por el ESI y EFQM. Se basa en CMMI como guía para los procesos de desarrollo, adquisición y mantenimiento de productos y servicios software y en EFQM para la gestión de negocio.

