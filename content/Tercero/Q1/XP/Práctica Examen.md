---
Name: Práctica Examen
tags:
  - práctica
asignatura: XP
---
***[[Gestión de Proyectos]]***

1. *Indique cómo modelaría en MS-Project la utilización de un servicio de cómputo en la nube (MS Azure) en varias tareas, teniendo en cuenta que no hay limitación en el uso por varias tareas (servicio bajo demanda) y que tiene un coste de 0,25€/h.*
	Crearía un recurso de tipo costo y añadiría dicho recurso a las tareas correspondientes. Después de añadirlo, se le asignarían las horas que dependen de la duración de la tarea. Por último se establece el coste de 0,25€/h para el recurso y Project calculará el coste total en función del tiempo que se utilice el recurso.
2. *Indique cómo modelaría en MS-Project la supervisión de 4 horas sobre una determinada tarea todos los martes y jueves mientras dicha tarea tenga lugar.*
	Primero crearía las tareas periódicas marcando que su duración es de 4 horas cada martes y jueves. Además, tendría que asignar el recurso encargado de la supervisión y asociar dichas tareas a la tarea que se está supervisando.
3. *Supongamos que un recurso humano solo puede dedicar 20 horas a la semana a una tarea crítica, y que esta tarea depende de otra que se completará en 5 días. ¿Cómo se modelaría en MS-Project?*
	Se crearía la tarea crítica con la duración estimada y se asignaría el recurso con un límite de disponibilidad de 20 horas por semana en la página de configuración de recursos. Como depende de otra, habría que poner una relación FC (o la que correspondiese) entre las dos tareas.
4. *Modelar en MS-Project un proyecto que incluye el uso de software licenciado con un costo fijo mensual de 500€ y el trabajo de un equipo externo que cobra 50€/hora.*
	Se deben crear dos recursos:
	- Uno de tipo Costo para el software con un costo de 500€.
	- Otro de tipo Trabajo, con una tasa de 50€/h.
5. *¿Cómo se configuraría en MS-Project una reunión semanal de seguimiento del proyecto (1 hora de duración), que se realice cada miércoles, pero solo si hay tareas activas pendientes de la semana anterior?*
	Se crearía una tarea periódica para celebrar la reunión cada semana con una duración de 1 hora en total.
	Para controlar que se haga solo cuando haya tareas activas pendientes de la semana anterior, habría que hacerlo manualmente. Es decir, se activaría la reunión según fuese necesario.
6. *Para elaborar ERS se usa la herramienta Rational Requisite Pro, con un coste de 850€/mes*
	Habría que crear un recurso de tipo Material con una tasa de 850€ y asignarla a la tarea con unidades [1/mes].
7. *Durante la instalación, DS1 se ocupa de supervisar el trabajo del resto de desarrolladores durante 2h al día todos los días que dure el proceso de instalación*.
	Crear una hamaca que englobe la tarea de Instalación y asignar al recurso DS1 al 25%.
8. *El recurso DS1 no participa como desarrollador en las tareas de implementación de unidades, documentación de unidades y ejecución de pruebas de unidad, sino que realiza una supervisión de dicha tarea los viernes durante 2h*.
	Se crea una tarea periódica de duración 2h con patrón de repetición cada viernes y fechas de comienzo y fin las estimadas para las tareas a supervisar. Por último, se asigna al recurso DS1 al 100%.
9. *El recurso DS1 debe supervisar el proceso de instalación en el cliente, realizando 2 comprobaciones, cuando la instalación vaya por el 50% y cuando se haya completado*.
	Se crean dos tareas de supervisión a las que se les asigna DS1. Hay que poner una relación a cada una. La primera tendrá una relación CC+50% con la tarea a supervisar, mientras que la otra tendrá una relación FC con la tarea de instalación.
	
	También se podría dividir la tarea original en dos partes, crear dos tareas de supervisión y asignar a DS1 a dichas tareas. En cuanto a las relaciones, se establecerían relaciones FC entre cada bloque de la tarea y su supervisión, además de establecer una FC entre los dos bloques de la tarea de instalación.