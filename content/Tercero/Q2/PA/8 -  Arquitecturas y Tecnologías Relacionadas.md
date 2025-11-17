---
Name: 8 - Arquitecturas y Tecnologías Relacionadas
tags:
  - teoría
asignatura: PA
---
***[[Programación Avanzada]]***

**BACKEND**
****
***ARQUITECTURAS BASADAS EN MICROSERVICIOS***
> Todo el backend corre dentro de un servicio.

En la mayoría de los casos, esto no representa un problema.
Sin embargo,
- Cuando el código del backend es demasiado grande, puede ser más complejo de gestionar, tanto en desarrollo como en despliegue.
- Cuando se desea escalar el backend, es necesario replicar el servicio múltiples veces.

1. *Ventajas*
	- [p] Desarrollo: cada microservicio puede ser desarrollado por un equipo diferente.
	- [p] Despliegue: cada microservicio puede desplegarse independientemente del resto.
	- [p] Escalabilidad: cada microservicio puede escalarse de forma independiente.
2. *Desventajas*
	- [c] Arquitectura mucho más compleja.
	- [c] Solo justificable para backends muy grandes.
3. *Despliegue*
	Cada microservicio está replicado en varias instancias y cada una corre dentro de un contenedor.
	Cuando un contenedor se ejecuta, el árbol de procesos que arranca se ejecuta dentro del SO en la que corre, y el árbol se ejecuta de forma aislada del resto de contenedores.
	Se suelen desplegar en plataformas cloud.

**FRONTEND**
****
Los SDK nativos permiten desarrollar aplicaciones nativas con las máximas capacidades posibles.
Requieren desarrollar una aplicación nativa distinta para cada SO que se desee soportar.

Alternativamente existen soluciones ***cross-platform*** que, a partir de una única distribución fuente, generan ejecutables para distintos SO.

Existen varios tipos de soluciones cross-platform:
- Las que compilan a código nativo.
- Las que usan tecnologías web
	- Las que permiten implementar aplicaciones híbridas.
	- React Native.