---
Name: 3 - Arquitecturas distribuidas
tags:
  - teoría
asignatura: AS
---
***[[Arquitectura Software]]***

**ARQUITECTURA CLIENTE-SERVIDOR DISTRIBUIDO**
****
Igual que la no distribuida, busca que las funcionalidades se desplieguen en **servicios independientes**.

Lo que se debe tener en cuenta para la distribuida es que cada componente, los clientes y el directorio están desplegados en **máquinas físicas diferentes**. Esto va a dotar a esta arquitectura de un extra de **disponibilidad** ya que, aunque una máquina deje de funcionar, el resto de servicios del sistema seguirán activos.

En la distribuida, al estar desplegados todos en la misma máquina, si esta se saturaba todo dejaba de funcionar.
Aunque esto no es aplicable al **intermediario**, que sigue siendo el punto único de fallo.

El cliente puede ser:
- *Cliente ligero*: solo sabe dónde está el intermediario, se comunicará con él y recibirá sus respuestas. Solo mostrará esta información al usuario, no tiene ninguna forma de completar esta información.
	- *Ventaja*: más fácil de mantener.
	- *Inconveniente*: la información que se muestra ya debe venir preparada.
- *Cliente pesado*: además de hacer las peticiones, cuando le llega la información devuelta hace algún procesado adicional.
	- *Ventaja*: aprovecha la capacidad de proceso de los clientes. Se reduce la cantidad de información que va por la red, hasta el cliente podría seguir funcionando por un momento aunque la comunicación con el servidor se viese interrumpida.
	- *Inconveniente*: hay que mantener al cliente.

- **Ventajas**
	1. Robustez.
	2. Escalabilidad.
	3. Mantenibilidad.
	4. Rendimiento.

- **Inconvenientes**
	1. El directorio sigue siendo punto único de fallo.

- **Representación C4**
	En el contenedor, la *boundary* limita el contenido lógico del sistema y las *boundarys* más pequeñas muestran los espacios físicos.
	![[Pasted image 20240523175701.png]]
	![[Pasted image 20240523175715.png]]

- **Aplicable en**
	Cuando se quiere aumentar la robustez, escalabilidad o rendimiento del cliente/servidor básico.

**ARQUITECTURA LÍDER-TRABAJADOR**
****
El líder es el punto de entrada al sistema y que no realiza el trabajo, sino que lo distribuye. Además de conocer dónde está cada uno de los servicios, también va a poder organizarlos (**GESTIÓN Y COORDINACIÓN DE LOS TRABAJADORES**).

Es decir, no sólo va a poder recibir peticiones del cliente y asignarlas al servicio, sino que también va a poder decidir si necesita más trabajadores porque los otros están ocupados. Por ello es el único punto de fallo.

Los trabajadores lo más normal es que sean iguales, aunque también puede haber grupos de trabajadores, en cuyo caso suele haber sublíderes por grupos.

- **Ventajas**
	1. El líder no trabaja, solo organiza el sistema, por lo que tenemos gran **rendimiento** y **escalabilidad**.

- **Inconvenientes**
	1. Puede ser que arranquemos un líder con demasiados trabajadores para la carga de trabajo o al revés, prevenir un mal reparto del trabajo.

- **Representación C4**
	La *boundary* externa sigue siendo una banda lógica y la más pequeña, las separaciones físicas. Es común hablar de Nodo en esta arquitectura. Cada nodo no tiene que tener el mismo número de trabajadores.
	![[Pasted image 20240523181039.png]]
	![[Pasted image 20240523181101.png]]

- **Aplicable**
	1. Cuando queremos tener tiempos de respuesta muy eficientes y posibilidad de reacción elástica a cambios de carga/demanda del sistema.

**ARQUITECTURA PEER-TO-PEER (P2P)**
****
Total ausencia de control de lo que pasa entre unos componentes y otros.
P2P dice que no importa cómo tengas distribuidos los servicios, siempre van a existir fallos que no podemos prevenir.

No se puede saber qué servicios estarán mejor comunicados, cómo nos podemos enfrentar a estos sin que sea parte de la codificación y sin comprobar qué latencia tienen para saber a dónde mandar la petición.

La arquitectura P2P propone que las funcionalidades del sistema estén todas en todos los componentes.
No existe un directorio. El cliente debe conocer dónde hay una copia del sistema, pero el elemento que conoce no tiene por qué ser el que conteste.

1. Si al que se le manda la petición está disponible, este contesta, pero sino manda la petición a otro de los componentes (el más disponible). El peer es capaz de decidir si atenderla o delegarla.
2. Si todos están muy ocupados, la petición da vueltas y nunca nadie la responde. Es fácil prevenir esa situación anexando un número de saltos máximos de la petición antes de devolver un error.
3. La petición va a poder ser atendida por cualquier copia (peer) del sistema.

- **Ventajas**
	1. Es prácticamente imposible que no consigamos que el sistema nos atienda. Si todos los nodos están saturados, se pueden añadir más -> **escalabilidad** y **disponibilidad**.
	2. El reparto de la carga no es centralizado, sino que cada peer tiene lógica de gestión -> **reparto uniforme**.

- **Inconvenientes**
	1. El reparto uniforme puede ser también un inconveniente.
		Si yo necesito implementar algún elemento de seguridad o control sobre qué componentes hacen qué cosas, no es posible. Cualquier peer puede arrancar una copia nueva y detectar peers maliciosos requiere lógica adicional que puede no ser parte del sistema.
	2. Ausencia de control o sincronización entre los componentes.
	3. Posibles riesgos de **seguridad**.

- **Representación C4**
	En el contenedor tenemos el *boundary* lógico y cada nodo/máquina tiene un peer desplegado. No es habitual que haya más de un peer desplegado en un nodo, suele ser relación 1 a 1.
	![[Pasted image 20240523183722.png]]

- **Aplicable en**
	1. Sistemas en los que los pares intercambian información local.
	2. Sistemas en los que el servidor, de ser preciso, solo sirve para que los pares conozcan a sus vecinos.