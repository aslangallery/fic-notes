---
Name: 2 - La máquina y el mundo
tags:
  - teoría
asignatura: ER
---
***[[Ingeniería Requisitos]]***

**EFECTOS EN EL ENTORNO**
****
- Interés en desarrollar software -> **máquina**.
- La máquina cambia el mundo, tiene un efecto -> **entorno**.
- Lo que tiene que hacer la máquina -> **requisitos**.

**MODELO WRSPM**
****
Modelo para aplicar métodos formales a la ingeniería de requisitos, relación entre entorno y sistema a través de **eventos compartidos**. La especificación de requisitos se hace a partir de estos eventos compartido.

Las siglas vienen de:
- **W**orld **R**equirements (**entorno**)
- **S**pecification (**interfaz**)
- **P**rogram **M**achine (**sistema**)

La especificación de requisitos es el puente que une el entorno con el sistema. Los requisitos son descritos en función del entorno. Buscamos que la especificación en función del conocimiento del dominio satisfaga los requisitos a la hora de implementar el sistema.

**EVENTOS COMPARTIDOS**
****
- Eventos del entorno: $e_h$
- Eventos del sistema: $s_h$
- Eventos compartidos: $e_v, s_v$
La especificación de requisitos solo debe recoger los eventos que comparten el sistema y el entorno.

**¿QUÉ ES EL ENTORNO?**
****
El entorno es la parte del mundo real, es importante conocer las restricciones y limitaciones que forman el entorno y los factores externos.

El diagrama de contexto es la herramienta que permite mostrar visualmente las entidades del entorno y del sistema y las conexiones entre ellas. Suelen indicarse en las relaciones cómo están relacionados.

**CONOCIMIENTO DEL DOMINIO**
****
$$S, E\rightarrow R$$

- **S**: especificación de requisitos
- **E**: dominio
- **R**: requisitos

Si conocemos el dominio es más fácil comunicarnos con el cliente y comprender sus necesidades. También ayuda a validar lo que pide el cliente y facilita el mantenimiento del sistema. Cuanto más sepamos sobre el dominio, mejor podemos prevenir errores o cambios futuros.

**REQUISITOS**
****
Son indicaciones de los cambios que queremos que haga el sistema en el entorno. Se expresan en subjuntivo.
- **Ejemplo**: el sistema debe avisar a una enfermera si ocurre un cambio en el ritmo cardíaco del paciente.
El entorno se expresa en indicativo, los requisitos son parte del entorno.

En la interfaz existen eventos del entorno que el sistema sí conoce. El sistema influye solo en esos eventos, pero de forma que afecte al resto del entorno.

Los requisitos tienen dos partes:
- **R**: parte optativa, expresa lo que el sistema debería cambiar cuando esté funcionando.
- **E**: parte indicativa, es lo que ya existe, una asunción del entorno.

**ESPECIFICACIÓN**
****
Para demostrar que los requisitos son satisfacibles por un software, hacemos una especificación de este.
$$P, M \rightarrow S$$
Esta relación indica que todos los artefactos relacionados con nuestro sistema tienen una función en él. Esto no es infalible, por ejemplo en un sistema de gestión de tráfico:
- **E**: los coches paran cuando el semáforo está en rojo.
No es verdad porque algunos conductores se saltan los semáforos.