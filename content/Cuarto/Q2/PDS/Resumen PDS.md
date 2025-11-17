---
Name: Resumen PDS
tags:
  - teoría
asignatura: PDS
---
***[[Proyectos de Desarrollo Software]]***

# Guía de Estudio: Estimación y Gestión de Proyectos de Software

## I. Estimación del Tamaño Funcional de un Proyecto Software

### A. Métricas de Tamaño del Producto

- **Líneas de Código (LOC/SLOC):** Cuantificación del número de líneas de código fuente esperadas.
- **Puntos de Función (PF):** Una métrica independiente de la tecnología, basada en los requisitos del cliente y enfocada en las funcionalidades.

### B. Métodos Basados en Puntos de Función

1. **IFPUG (International Function Point Users Group):**

- **Objetivo:** Promover la estimación de software mediante FPA (Function Point Analysis), basado en el "Análisis de Puntos de Función" de Allan Albrecht.
- **Ventajas:** Mayor independencia de la tecnología, fácil aplicación, basado en requisitos del cliente, focalizado en funcionalidades.

1. **Cálculo (7 pasos):** 
	1. Identificación del alcance y límites de la aplicación.
	2. Identificación de los 5 elementos funcionales:
		- **Funciones de Datos**:
			- ***Ficheros Lógicos Internos (ILF):*** Datos mantenidos dentro de los límites de la aplicación.
			- ***Ficheros de Interfaz Externos (ELF):*** Datos referenciados, pero mantenidos por otra aplicación.
		- **Funciones Transaccionales:
			- ***Entradas Externas (EI):*** Procesos que procesan datos o controlan información que entra a la aplicación.
			- ***Salidas Externas (EO):*** Procesos que envían datos o información de control fuera de los límites de la aplicación, conteniendo al menos un cálculo o fórmula.
			- ***Consultas Externas (EQ):*** Procesos que retornan datos o información de control de un ILF o ELF, sin cálculos ni datos derivados, y sin alterar el sistema.

	3. Evaluación de la complejidad (Baja, Media, Alta) para funciones de datos (usando DETs y RETs) y funciones transaccionales (usando DETs y FTRs).
		- **DET (Data Element Type):** Un campo único y entendible por el usuario.
		- **RET (Record Element Type):** Un subgrupo de elementos de un ILF o ELF.
		- **FTR (File Type Referenced):** Archivos referenciados por funciones transaccionales.

	4. Cálculo de los PF Sin Ajustar (PFSA): Suma de los puntos de función de cada elemento funcional por su peso de complejidad.
	5. Evaluación de los 14 atributos de ajuste (GSCs - General System Characteristics), puntuados de 0 a 5.
	6. Cálculo del Factor de Ajuste (TFA): Suma de las puntuaciones de los 14 atributos de ajuste.
	7. Cálculo del Valor Final de los PF (PFA): PFA = PFSA x (0,65 + (0,01 x TFA))


2. **Métodos de Estimación Temprana:** Se usan en fases iniciales o para comparar alternativas.
	- ***FP LITE:*** Simplifica el IFPUG asumiendo complejidad media para todos los elementos, con un margen de error de ±20%.
	- ***E&QFP:*** Pasos similares al IFPUG, permitiendo definir valores de PF sin ajustar (mínimo, probable, máximo) y un Factor de Ajuste de Valores (VAF). Útil en proyectos incrementales, donde la primera iteración es el punto de partida y las siguientes son mejoras.

### C. Métodos Basados en Puntos de Caso de Uso (UCP)

- **Definición:** Mide el tamaño funcional en función de la cantidad y complejidad de casos de uso y actores, ajustado por factores externos.
- **Componentes clave:**
	- ***Actores:*** Personas o entidades externas que interactúan con el sistema.
	- ***Casos de Uso:*** Funcionalidades del sistema.

1. **Cálculo (Pasos):**
	1. Cálculo de los Puntos de Caso de Uso sin Ajustar (UUCP): UUCP = UAW + UUCW (Suma de pesos sin ajustar de actores y casos de uso).
	2. Determinación de los Factores Técnicos (TCF): 13 factores puntuados de 0 a 5.
	3. Determinación de los Factores de Entorno (EF): 8 factores puntuados de 0 a 5.
	4. Cálculo de los Casos de Uso Ajustados (AUCP): AUCP = UUCP x TCF x EF.

### D. Métodos de Estimación en Metodologías Ágiles

- **Puntos de Historia:** Medición relativa del esfuerzo de una historia de usuario. No hay una fórmula fija.
- **Escalas comunes:** Fibonacci Modificada (0, ½, 1, 2, 3, 5, 8...), Duplicación (1, 2, 4, 8...), T-Shirt Sizing (XS, S, M, L...).
- **Técnica principal:** Planning Poker.
- **Velocidad:** Elemento clave, mide la cantidad de trabajo (puntos de historia) completado por sprint. Solo historias completamente terminadas cuentan.

## II. Estimación del Tamaño No Funcional del Software - Método SNAP

### A. Requisitos Funcionales vs. No Funcionales

- **Requisitos Funcionales:** Definen _qué_ debe hacer el sistema (comportamiento y características).
- **Requisitos No Funcionales (NFR):** Definen _cómo_ se deben realizar las tareas (rendimiento, seguridad, usabilidad, aspectos técnicos).
- **SNAP (Software Non-Functional Assessment Process):** El único método reconocido para la estimación del tamaño no funcional, desarrollado por IFPUG en 2008 y reconocido como estándar ISO (32430:2025) desde febrero de 2025.
- **Relación FPA y SNAP:** Son dos componentes independientes del tamaño del software. Puntos de Función + SNAP ≠ Tamaño Total del Producto. Se pueden mantener los 14 VAFs del FPA, asegurando consistencia con SNAP.

### B. Objetivos de SNAP

- Medir el tamaño no funcional acordado con el cliente.
- Medir el desarrollo y mantenimiento basados en NFRs y tecnología.
- Mejorar la estimación de costes.
- Estimar proyectos puramente técnicos.
- Permitir comparativas consistentes entre proyectos.

### C. Conceptos Clave en SNAP

- **Datos de Código (Code Data):** Datos usados para cumplir NFRs sin alterar el significado de los datos de negocio (ej. sustitución, estáticos, valores válidos). Se cuentan como 1 FTR en SNAP, y su complejidad se mide por RETs.
- **Particiones:** Grupos de funciones de software dentro de los límites de una aplicación que comparten criterios y valores de evaluación no funcionales (mantenibilidad, portabilidad, etc.).

### D. Pasos para Aplicar SNAP (7 Pasos)

1. **Recabar la información disponible:** Documentación, reuniones con stakeholders.
2. **Determinar el objetivo del conteo, ámbito de aplicación, límites y particiones:** Consistente con FPA (desarrollo, mejora, aplicación).
3. **Identificación de los requisitos no funcionales (NFR):** Pueden ser explícitos o implícitos, y pueden contener aspectos funcionales y no funcionales (requiriendo división y acuerdo con el cliente).
	- **Asociar los NFRs a subcategorías e identificar las SCUs (SNAP Counting Units):** Unidad de medida SNAP, componente o actividad cuya complejidad y tamaño se evalúa.
	- **SNAP Point Size (SP):** Medida del tamaño no funcional para una subcategoría y una SCU. SP = Constante * Complejidad.
	- **Parámetros de la Complejidad:** Elementos para medir la complejidad de un SCU.
		- Existen 4 categorías y 14 subcategorías.
		- **Ejemplos de subcategorías y sus consideraciones:**
			- ***Validación de Datos de Entrada:*** Evalúa niveles de anidamiento en la validación (ej. if-elses, while).
			- ***Operaciones Lógicas y Matemáticas:*** Considera operaciones matemáticas complejas (algoritmos, regresión) y operaciones lógicas extensas (>=4 niveles de anidamiento o >=38 DETs).
			- ***Formateo de Datos:*** Trata requisitos de estructura, formato, o información administrativa de una transacción (ej. cifrado/descifrado). No se cuentan si son funcionales y ya medidos por FPA.
			- ***Movimientos Internos de Datos:*** Medida de transferencia de información _interna_ entre particiones. 1 SCU para transacciones síncronas, 2 SCUs para asíncronas.
			- ***Aportar Valor Añadido a los Usuarios por Cambios en la Configuración:*** Representa valor de negocio adicional por cambios en datos de referencia o código sin alterar la estructura o código del programa (ej. gestión de planes de suscripción o roles).
			- ***Múltiples Métodos de Entrada/Salida:*** Permisibilidad de la aplicación para recibir/enviar datos por múltiples métodos.
			- ***Múltiples Plataformas:*** Aplicación desarrollada para múltiples entornos hardware-software.
			- ***Tecnologías de Bases de Datos:*** Características y operaciones añadidas a la BBDD para cumplir NFRs (ej. añadir campos/tablas no funcionales, cambiar índices, capacidad, queries).
4. **Determinar el tamaño SNAP de cada sub-categoría.**
5. **Calcular el tamaño SNAP total.**
6. **Documentar el proceso.**

## III. Estimación de Esfuerzo, Duración y Coste

### A. ISBSG (International Software Benchmarking Standards Group)

- **Repositorio:** Contiene estimaciones de proyectos de software terminados y en desarrollo.
- **Calidad de los datos:** Proyectos con calificación (A, B, C o D).
- **Métodos de estimación:** Regresión/Algoritmos, Comparación, Analogía.
- **Factores no considerados:** Habilidades del equipo, innovación técnica, subcontratación, estabilidad de requisitos, niveles de calidad, presupuesto.
- **Estimación de Esfuerzo:** Tablas de calibrado basadas en alcance, plataforma y lenguaje.
- **Estimación de Duración:** DURACIÓN = C x TAMAÑO^E (donde Tamaño son los PFSA). Si no se ajusta a las categorías, DURACIÓN = 0.370 x ESFUERZO^0.328.
- **Ponderación por fases:** Planificación (9%), Especificación (11%), Diseño (15%), Construcción (43%), Pruebas (16%), Implantación (6%).

### B. COCOMO (COnstructive COst MOdel)

- **Modelo de estimación:** Basado en líneas de código (LOC) y factores de ajuste. Versiones COCOMO 81, COCOMO II. COCOMO III busca incluir puntos de función/SNAP.
- **Nivel de Prototipos (COCOMO II):** Utiliza Puntos Objeto (OP) para estimación temprana.
- **Cálculo de OP:** Basado en el número y complejidad de pantallas, informes, y módulos en lenguajes de 3ª generación.
- **Nivel de Diseño Inicial y Post-Arquitectura:Factores de Escala (Wd):** PREC (Precedencia), FLEX (Flexibilidad), RESL (Resolución de la arquitectura/riesgo), TEAM (Cohesión del equipo), PMAT (Madurez del proceso).
- **Multiplicadores de Esfuerzo (M):** 17 multiplicadores que expresan el impacto sobre el esfuerzo de desarrollo (ej. TOOL, SCED, SITE, USR1, USR2).

### C. SLIM (Software Lifecycle Management)

- **Modelo:** Observa que la distribución del personal se asemeja a una curva de Rayleigh.
- **Aplicabilidad:** No adecuado para proyectos pequeños (<5.000 SLOC, <1.5 personas/año, <6 meses).
- **Parámetro de Productividad (PI):** Escala de valores enteros de 1 a 36 asociados a tipos de aplicación y sus desviaciones estándar. La relación entre PI y PP es exponencial.
- **Parámetro de Incorporación de Personal (PIP):** Constante obtenida por calibración de proyectos anteriores.

## IV. Gestión de Riesgos

### A. Medición del Riesgo

- **Cualitativo:** Escalas subjetivas (poco, medio, alto); se usa cuando no hay suficiente información.
- **Cuantitativo:**
	- Riesgo = Probabilidad x Impacto
	- Riesgo = Probabilidad x (Degradación x Valor)

### B. Tratamiento de Riesgos

- **Evitar el riesgo:** Eliminar la causa del riesgo.
- **Mitigar el riesgo:** Reducir la probabilidad o el impacto (ej. medidas de seguridad, cifrado).
- **Transferir el riesgo:** Trasladar el riesgo a otra parte (ej. seguros, terceros).
- **Aceptar el riesgo:** Asumir el riesgo si el coste de tratamiento es mayor que el beneficio.

### C. Niveles de Riesgos

- **Riesgo inherente:** Riesgo antes de aplicar controles.
- **Riesgo actual:** Riesgo con los controles existentes.
- **Riesgo residual:** Riesgo que persiste después de haber aplicado las medidas de gestión.

### D. Ciclo de Gestión de Riesgos (ISO 27005)

1. **Establecimiento del contexto:** Definición y documentación para la gestión.
	- **Evaluación del riesgo:**
		- Identificación del riesgo
		- Estimación del riesgo
		- Evaluación del riesgo
2. **Tratamiento del riesgo.**
3. **Aceptación del riesgo.**
4. **Comunicación del riesgo.**
5. **Monitorización y revisión del riesgo.**

## V. Madurez del Software

### A. TRL (Technology Readiness Level)

- **Origen:** Desarrollado por la NASA en los años 70 para evaluar la madurez de componentes antes de misiones espaciales.
- **Propósito:** Medir empíricamente los requisitos mínimos de calidad o madurez de los componentes para reducir riesgos.
- **Niveles (TRL 1 a TRL 9):TRL 1:** Principios básicos observados y reportados.
- **TRL 9:** Sistema probado con éxito en entorno real.

## VI. Buenas Prácticas y Errores en la Estimación

### A. Buenas Prácticas

- **Aprender de errores pasados:** Utilizar modelos de referencia internos.
- **Comparar estimaciones:** Usar diferentes métodos o ajustes.
- **Estimar por rangos:** Optimista, Probable, Pesimista, para manejar la incertidumbre.

# Cuestionario de Preguntas Cortas

Responde a cada pregunta con 2-3 frases.

1. ¿Cuáles son las dos métricas más utilizadas para determinar el tamaño de un producto de software según los modelos paramétricos?
2. Explica la principal ventaja de la estimación por Puntos de Función (PF) en comparación con la estimación por Líneas de Código (LOC).
3. En el método IFPUG, ¿qué son los Ficheros Lógicos Internos (ILF) y los Ficheros de Interfaz Externos (ELF)?
4. ¿Cuál es la diferencia fundamental entre una Salida Externa (EO) y una Consulta Externa (EQ) en el contexto de las funciones transaccionales de IFPUG?
5. Menciona dos casos en los que se recomienda la aplicación de métodos de estimación temprana, como FP LITE o E&QFP.
6. ¿Qué son los Puntos de Historia en las metodologías ágiles y cómo se diferencian de los Puntos de Función en su cálculo?
7. Define qué es el método SNAP (Software Non-Functional Assessment Process) y cuál es su propósito principal en la estimación del software.
8. Según SNAP, ¿por qué el Tamaño Funcional y el Tamaño No Funcional no se suman para obtener el tamaño total del producto?
9. En el contexto de la gestión de riesgos, explica la diferencia entre riesgo inherente y riesgo residual.
10. ¿Cuál es el propósito de los Technology Readiness Levels (TRL) y quién los desarrolló inicialmente?

# Clave de Respuestas del Cuestionario

1. Las dos métricas más utilizadas son Líneas de Código (LOC o SLOC) y Puntos de Función. Ambas sirven como entrada para los modelos paramétricos que determinan el tamaño de una aplicación de software.
2. La principal ventaja de la estimación por Puntos de Función es su mayor independencia de la tecnología de desarrollo, ya que se basa en los requisitos funcionales del cliente en lugar de en la cantidad de código. Esto permite una estimación más centrada en la funcionalidad proporcionada.
3. Los Ficheros Lógicos Internos (ILF) son grupos de datos mantenidos dentro de los límites de la aplicación, mientras que los Ficheros de Interfaz Externos (ELF) son grupos de datos que son referenciados por la aplicación pero son mantenidos por otra aplicación. Ambos son componentes de las funciones de datos en IFPUG.
4. Una Salida Externa (EO) es un proceso que envía datos o información de control fuera de los límites de la aplicación y debe contener al menos un cálculo o fórmula matemática. En contraste, una Consulta Externa (EQ) retorna datos o información de control sin realizar cálculos ni crear datos derivados, y no altera el comportamiento del sistema.
5. Los métodos de estimación temprana son útiles para realizar estimaciones en fases iniciales del proyecto cuando se busca una estimación rápida sin entrar en muchos detalles técnicos. También son valiosos para comparar alternativas, permitiendo evaluar el impacto de diferentes enfoques de desarrollo o funcionalidades.
6. Los Puntos de Historia son una medida relativa del esfuerzo que requiere una historia de usuario en metodologías ágiles. A diferencia de los Puntos de Función, no existe una fórmula matemática fija para calcularlos; en su lugar, se asignan relativizando el esfuerzo de unas historias frente a otras, a menudo mediante técnicas como Planning Poker.
7. SNAP (Software Non-Functional Assessment Process) es el único método reconocido para la estimación del tamaño no funcional del software, desarrollado por el IFPUG. Su propósito principal es medir los requisitos no funcionales, como rendimiento, seguridad o usabilidad, proporcionando una visión más completa del tamaño del software junto con la estimación funcional.
8. Según el IFPUG y SNAP, el Tamaño Funcional y el Tamaño No Funcional se consideran dos componentes independientes del tamaño total del producto y no se suman directamente. Esto se debe a que representan dimensiones distintas del software: los requisitos funcionales cubiertos por FPA y los requisitos técnicos y de calidad cubiertos por SNAP.
9. El riesgo inherente es el nivel de riesgo antes de que se aplique cualquier tipo de control o medida de gestión. Por otro lado, el riesgo residual es el riesgo que permanece o persiste después de que se han implementado y aplicado todas las medidas de tratamiento de riesgos contempladas.
10. Los Technology Readiness Levels (TRL) fueron desarrollados por la NASA para valorar el grado de calidad o madurez de sus proyectos, especialmente los componentes de misiones espaciales. Su propósito es medir empíricamente los requisitos mínimos a cumplir para que los componentes sean aptos, ayudando a evitar el mayor número de riesgos posibles.

# Preguntas en Formato de Ensayo

1. Compara y contrasta los métodos de estimación IFPUG y Puntos de Caso de Uso (UCP), destacando sus similitudes y diferencias en la forma en que cuantifican el tamaño funcional de un software y en qué fases del proyecto podrían ser más adecuados.
2. Explica la importancia de la estimación del tamaño no funcional del software utilizando el método SNAP. Detalla cómo SNAP complementa la estimación funcional (FPA) y analiza los beneficios de tener en cuenta ambos tipos de requisitos para una estimación integral del proyecto.
3. Analiza el papel de la "velocidad" en las metodologías ágiles, específicamente en el contexto de los Puntos de Historia. Describe cómo se calcula la velocidad y por qué es un elemento tan crucial para la estimación y planificación de proyectos ágiles.
4. Describe los diferentes enfoques para medir el riesgo (cualitativo y cuantitativo) y explica las circunstancias en las que cada uno sería preferible. Además, detalla las cuatro estrategias principales para el tratamiento de riesgos, proporcionando un ejemplo para cada una.
5. Discute cómo los repositorios de estimación como ISBSG y modelos como COCOMO y SLIM contribuyen a una estimación más precisa del esfuerzo, duración y coste en proyectos de software. Identifica las limitaciones o factores no considerados por estos métodos que podrían influir en la precisión de la estimación.

# Glosario de Términos Clave

- **Actores:** En el método UCP, personas o entidades externas que interactúan con el sistema.
- **AUCP (Adjusted Use Case Point):** Puntos de Caso de Uso Ajustados; el valor final de la estimación UCP después de aplicar factores técnicos y de entorno.
- **COCOMO (COnstructive COst MOdel):** Modelo de estimación de esfuerzo basado en líneas de código y factores de ajuste.
- **Datos de Código (Code Data):** Categoría de datos en SNAP que se utilizan para cumplir requisitos no funcionales sin alterar el significado de los datos de negocio (ej., sustitución, estáticos, valores válidos).
- **DET (Data Element Type):** Tipo de Elemento de Datos; un campo único y entendible por el usuario, utilizado en la evaluación de complejidad de IFPUG y SNAP.
- **Duración (ISBSG):** Tiempo estimado para completar un proyecto, calculado con fórmulas basadas en el tamaño o esfuerzo del proyecto y factores de calibrado.
- **EI (Entrada Externa):** En IFPUG, un proceso que procesa datos o información de control que entra a la aplicación.
- **ELF (Fichero de Interfaz Externo):** En IFPUG, un fichero lógico que es referenciado por la aplicación pero es mantenido por otra aplicación.
- **EO (Salida Externa):** En IFPUG, un proceso que envía datos o información de control fuera de la aplicación y contiene al menos un cálculo o fórmula.
- **EQ (Consulta Externa):** En IFPUG, un proceso que retorna datos o información de control de un fichero lógico interno o externo, sin cálculos ni datos derivados.
- **FPA (Function Point Analysis):** Análisis de Puntos de Función; un método de estimación del tamaño funcional del software.
- **FP LITE:** Método de estimación temprana que simplifica el IFPUG asumiendo complejidad media y aplicando un margen de error fijo.
- **FTR (File Type Referenced):** Tipo de Fichero Referenciado; número de archivos referenciados por funciones transaccionales en IFPUG y SNAP.
- **IFPUG (International Function Point Users Group):** Organización que promueve el uso y desarrollo de los Puntos de Función.
- **ILF (Fichero Lógico Interno):** En IFPUG, un grupo de datos mantenidos dentro de los límites de la aplicación.
- **Impacto (Riesgo):** La consecuencia de que un riesgo se materialice, medido a través de la degradación y el valor del activo.
- **ISBSG (International Software Benchmarking Standards Group):** Repositorio internacional de datos de proyectos de software terminados y en desarrollo para estimación y benchmarking.
- **LOC (Lines of Code) / SLOC (Source Lines of Code):** Líneas de Código / Líneas de Código Fuente; una métrica de tamaño de software basada en el recuento de líneas de código.
- **Métricas:** Instrumentos de medición para determinar el tamaño de una aplicación de software.
- **NFR (Requisitos No Funcionales):** Definen _cómo_ se debe comportar un sistema (ej., rendimiento, seguridad, usabilidad).
- **Particiones (SNAP):** Grupos de funciones de software dentro de los límites de una aplicación que comparten criterios y valores de evaluación no funcionales similares.
- **PF (Puntos de Función):** Una métrica de tamaño de software independiente de la tecnología, basada en la funcionalidad percibida por el usuario.
- **PFA (Puntos de Función Ajustados):** El valor final de los Puntos de Función después de aplicar los 14 atributos de ajuste en IFPUG.
- **PFSA (Puntos de Función Sin Ajustar):** La suma de los puntos de función antes de aplicar los 14 atributos de ajuste en IFPUG.
- **PI (Productivity Index):** Índice de Productividad; una escala de valores en el modelo SLIM que representa la productividad de desarrollo.
- **Planning Poker:** Técnica utilizada en metodologías ágiles para asignar Puntos de Historia, donde los miembros del equipo estiman de forma relativa.
- **Puntos de Historia:** Medida relativa del esfuerzo de una historia de usuario en metodologías ágiles.
- **Puntos Objeto (OP):** Una métrica de tamaño utilizada en COCOMO II para estimaciones a nivel de prototipos, basada en pantallas, informes y módulos.
- **Riesgo Actual:** Nivel de riesgo con los controles existentes.
- **Riesgo Inherente:** Nivel de riesgo antes de que se aplique cualquier control.
- **Riesgo Residual:** Nivel de riesgo que persiste después de haber aplicado las medidas de tratamiento.
- **RET (Record Element Type):** Tipo de Elemento de Registro; un subgrupo de elementos de un ILF o ELF, utilizado en la evaluación de complejidad de IFPUG y SNAP.
- **SCU (SNAP Counting Unit):** Unidad de Conteo de SNAP; el componente o actividad cuya complejidad y tamaño se evalúa para los requisitos no funcionales.
- **SLIM (Software Lifecycle Management):** Modelo de estimación de proyectos que observa una distribución de personal similar a una curva de Rayleigh.
- **SNAP (Software Non-Functional Assessment Process):** Método reconocido por IFPUG para la estimación del tamaño no funcional del software.
- **SP (SNAP Point Size):** Medida del tamaño no funcional para una subcategoría y una SCU en el método SNAP.
- **TCF (Technical Complexity Factor):** Factores técnicos; se usan en el cálculo de UCP para ajustar el tamaño.
- **TFA (Total Factor de Ajuste):** Suma de las puntuaciones de los 14 atributos de ajuste en IFPUG.
- **TRL (Technology Readiness Level):** Nivel de Madurez Tecnológica; escala utilizada para valorar el grado de calidad o madurez de un proyecto o componente.
- **UCP (Use Case Point):** Puntos de Caso de Uso; una métrica de estimación del tamaño funcional basada en la cantidad y complejidad de casos de uso y actores.
- **UUCP (Unadjusted Use Case Point):** Puntos de Caso de Uso sin Ajustar; la suma de los pesos sin ajustar de los actores y casos de uso en el método UCP.
- **VAF (Value Adjustment Factor):** Factor de Ajuste de Valores; término utilizado en IFPUG y E&QFP para ponderar los puntos de función.
- **Velocidad (Agile):** Medida de la cantidad de trabajo (puntos de historia) completado por un equipo en un sprint.




1. A
2. B
3. C
4. B
5. B
6. B
7. B
8. C
9. B
10. B
11. C
12. B
13. B
14. C
15. B
16. B
17. C
18. C
19. C
20. C