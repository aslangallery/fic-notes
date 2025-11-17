---
Name: 3 - Todo es peligroso
tags:
  - teoría
asignatura: ER
---
***[[Ingeniería Requisitos]]***

**EL LENGUAJE NATURAL**
*****
El lenguaje natural es muy impreciso. Es muy difícil comunicarse de manera informal. Muchas veces esto da lugar a errores que cuestan tiempo y dinero.

La mayoría de requisitos y especificaciones se escriben en lenguaje natural, de manera informal. Las descripciones de los proyectos también.

Algunas especificaciones están escritas en una combinación de lenguaje natural y lenguaje formal. También existe una pequeña minoría escrita únicamente en lenguajes formales.

No es necesario evitar el lenguaje natural, pero sí es recomendable tener mucho cuidado con él. 

**CUANTIFICADORES UNIVERSALES**
****
Cuando asumimos un cuantificador universal como cierto, se suelen producir errores. Hay que buscar siempre excepciones. Los cuantificadores universales también aparecen en especificaciones formales y causan el mismo tipo de problemas.

Los cuantificadores universales son válidos cuando se utilizan para expresar condiciones que deben ser verdaderas para todos los elementos de un conjunto dado.

En la ingeniería de requisitos se usan dos modos gramaticales:
1. **Modo indicativo**: establece hechos.
2. **Modo optativo**: expresa deseos o intenciones.

Se pueden declarar con cada uno:
1. **Dominio**:
	Contexto en el que las funcionalidades de un sistema tienen un significado y un efecto. Es una entidad con significado propio, independientemente del sistema que se vaya a construir.

	El dominio describe el mundo real en modo indicativo. Establece los hechos que son ciertos independientemente del sistema a implementar.

2. **Requisitos**:
	Un requisito es una afirmación en modo optativo sobre el sistema que queremos construir para mejorar el mundo real. Describe lo que ofrece el sistema y cómo queda el mundo después de que el sistema esté funcionando.

Normalmente las afirmaciones universales en indicativo son peligrosas. Si se asumen como ciertas, se puede dar que el sistema no sea capaz de manejar todas las posibles entradas.
Cuando los clientes definen el dominio, tienden a utilizar cuantificadores universales y los ingenieros de requisitos las asumen como ciertas. Normalmente, en algún punto del ciclo de vida del proyecto se da con las excepciones y se producen fallos.

Para arreglar los posibles errores que puedan producir las afirmaciones en indicativo con cuantificadores universales, se debe preguntar al cliente para descubrir posibles excepciones o confirmar si un cuantificador universal es válido.

La clave en modo indicativo es evitar los cuantificadores universales. En cambio, en optativo puede haberlos, siempre y cuando cubramos todas las excepciones del dominio.

**AMBIGÜEDADES DEL LENGUAJE**
****
Un ejemplo es que el cliente diga que la fecha de nacimiento ha de ser válida. Podemos interpretar que el usuario tiene que ser mayor de edad, que la fecha tiene que ser anterior a hoy, que tenga un formato concreto...

Una especificación no es ambigua si para cada requisito tiene solo una interpretación.

- **Desambiguación subconsciente**
	Cuando una persona comunica un requisito, trata de transmitir la idea que tiene en la cabeza. El problema es que si no lo hace de la forma más clara posible, el receptor puede darle cualquier interpretación. Esto se llama desambiguación subconsciente.


