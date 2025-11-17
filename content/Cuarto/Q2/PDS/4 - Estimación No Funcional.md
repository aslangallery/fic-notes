---
Name: 4 - Estimación No Funcional
tags:
  - teoría
asignatura: PDS
---
***[[Proyectos de Desarrollo Software]]***

**Puntos Clave:**

1. **Requisitos Funcionales vs. No Funcionales:**

- Los **requisitos funcionales** definen qué debe hacer el sistema (comportamiento y características del software).
- Los **requisitos no funcionales** se centran en cómo se deben realizar estas tareas (parte técnica: rendimiento, seguridad, usabilidad, etc.).
- Otros métodos como FPA o UCP los tienen en cuenta, pero sin un análisis detallado.

1. **Introducción al Método SNAP:**

- SNAP es el **único método reconocido para la estimación del tamaño no funcional** del software.
- Fue desarrollado en **2008 por el IFPUG**.
- La versión más reciente del manual (v2.4, 2017) está disponible gratuitamente.
- Desde **febrero de 2025, SNAP es un estándar ISO (32430:2025)**.
- Existen dos niveles de certificación: Certified SNAP Practitioner (CSP) y Certified SNAP Specialist (CSS).
- Utilizado junto con FPA, SNAP proporciona una **visión completa del software**, enfocándose en rendimiento, seguridad y usabilidad, independientemente de la tecnología o lenguaje de programación.

1. **Objetivos de SNAP:**

- Medir el **tamaño no funcional del software** acordado con el cliente.
- Medir el desarrollo y mantenimiento basados en **requisitos no funcionales y tecnología**.
- Mejorar la **estimación de costes** al añadir una dimensión de valor no funcional.
- Realizar estimaciones en **proyectos técnicos** donde FPA no es aplicable.
- Permitir **comparativas consistentes** entre diferentes proyectos.

1. **Relación entre Tamaño Funcional (FPA) y SNAP:**

- SNAP y FPA **no son aditivos** para obtener el tamaño total del producto. El tamaño del software tiene dos componentes independientes: funcional y no funcional.
- La combinación de FPA y SNAP puede visualizarse como **tres dimensiones interrelacionadas**: Requisitos Funcionales (cubiertos por FPA), Requisitos Técnicos y de Calidad (cubiertos por SNAP).
- Es posible mantener los 14 VAFs (Valor de Ajuste Funcional) de FPA, asegurando que sean consistentes con los aspectos solapados con SNAP.

1. **Cuándo Realizar la Evaluación No Funcional (SNAP):**

- La evaluación no funcional puede realizarse en **cualquier momento del ciclo de vida del proyecto**:
- Estimación del proyecto.
- Monitorización de cambios de alcance.
- Evaluación de requisitos no funcionales entregados.
- La evaluación puede enfocarse de dos maneras:
- **Aproximar:** Realizar suposiciones sobre categorías o complejidades no funcionales.
- **Medir:** Identificar todas las categorías aplicables y calcular su complejidad detalladamente.
- No se usan puntos SNAP para el mantenimiento (corrección de problemas).

1. **Datos de Código (Code Data):**

- Una categoría de datos reconocida en SNAP.
- Datos usados para cumplir requisitos no funcionales **sin alterar el significado de los datos de negocio**. A menudo identificados por el equipo de desarrollo.
- Clasificados como: Sustitución (código + descripción), Estáticos o constantes (valores por defecto), Valores válidos (valores concretos o rangos).
- **Diferencia con Datos de Negocio:** Los datos de negocio son la información crítica que gestiona el sistema (cliente, producto). Los datos de código son valores abreviados para eficiencia en programación, que se traducen para el usuario.
- **Diferencia con Datos de Referencia:** Los datos de referencia son información complementaria estática para dar contexto (categorías de productos). La principal diferencia es que los datos de código son intercambiables (código vs nombre), mientras que los datos de referencia no tienen sustitutos.
- **Características Lógicas:** Esenciales para el funcionamiento, no necesariamente persistentes, a menudo identificados en diseño para NFRs, pueden ser mantenidos por usuarios/administradores, almacenan información para estandarizar transacciones, suelen ser estáticos.
- **Características Físicas:** Suelen tener una clave y uno o dos atributos, número estable de entradas, implementados de diversas maneras (aplicación separada, diccionario, hard-codeado).
- **Medición en SNAP:** Independientemente del número de tablas físicas, los datos de código se cuentan como **1 FTR en SNAP**. La complejidad de este FTR se analiza por el número de **RETs**, dado por el tipo de ocurrencias de datos de código (Sustitución, Estáticos, Valores Válidos). Si los tres tipos están presentes, se cuentan 3 RETs.

1. **Los 7 Pasos para Aplicar SNAP:**
2. **Recabar la información disponible:** Obtener la máxima información posible sobre aspectos no funcionales (documentos de requisitos, diagramas E-R, arquitectura, estándares, etc.). Asegurar la consistencia si los NFRs son implícitos.
3. **Determinar el objetivo del conteo, ámbito de aplicación, límites y particiones:** Analizar el tipo de evaluación (desarrollo, mejora, aplicación). Identificar el ámbito y lo que está fuera de los límites. Definir "particiones": grupos de funciones de software dentro de los límites que comparten criterios y valores de evaluación no funcionales (visión desde el usuario).
4. **Identificación de los requisitos no funcionales (NFR):** Identificar NFRs explícitos o implícitos. Asegurar la consistencia con el entorno empresarial o legislación. Separar requisitos mixtos (funcionales y no funcionales) en FURs y NFRs con acuerdo del cliente y desarrolladores.

- **Asociar los NFRs a subcategorías e identificar las unidades de conteo de SNAP (SCUs):**SNAP define **4 categorías y 14 subcategorías** genéricas que cubren requisitos de calidad y técnicos.
- Las categorías son grupos de componentes/procesos para cumplir un NFR. Las subcategorías son componentes/procesos/actividades ejecutadas para cumplir un NFR.
- Un NFR puede asociarse a más de una subcategoría.
- La unidad de medida para el tamaño y complejidad de un NFR en cada subcategoría es la **Unidad de Conteo de SNAP (SCU)**.
- El proceso para determinar los puntos SNAP es:

1. Identificar categorías y subcategorías asociadas a cada requisito.
2. Identificar los SCU para cada subcategoría.
3. Calcular el tamaño no funcional (Puntos SNAP - SP) para cada SCU en la subcategoría (SP = Constante * Complejidad).
4. Calcular el tamaño final no funcional.
5. **Descripción de las Categorías y Subcategorías SNAP (Pasos 4, 5):**

- La complejidad en cada subcategoría se mide de manera diferente, considerando los factores principales que afectan la complejidad y la dificultad de implementación.
- Los **Parámetros de Complejidad** son elementos a examinar para medir la complejidad de un SCU.
- **Operaciones con Datos (Categoría 1):** Trata cómo se procesan los datos para cumplir NFRs.
- **1.1 Validación de la Entrada de Datos:** Operaciones para permitir datos predefinidos o prevenir inválidos. SCU: Proceso elemental. Parámetros: Nivel de anidamiento (Baja <=2, Media 3-5, Alta >=6), Número único de DETs. Cálculo SP: 2-4 * #DETs (dependiendo del nivel de anidamiento). Considera manejo de errores/excepciones y uso de datos de código.
- **1.2 Operaciones Lógicas y Matemáticas:** Operaciones lógicas y matemáticas complejas (algoritmos, modelos, cálculos de rutas). SCU: Proceso Elemental. Parámetros: Número de FTRs accedidos (Bajo 0-3, Medio 4-9, Alto >=10), Lógica de procesamiento (Lógica/Matemática), Número de DETs. Cálculo SP: 4-10 * #DETs (Lógica) o 3-7 * #DETs (Matemática), dependiendo del nivel de FTRs. Considera operaciones lógicas complejas con 4+ niveles de anidamiento y/o 38+ DETs.
- **1.3 Formateo de Datos:** Requisitos sobre estructura, formato o información administrativa no directamente funcional. Incluye cifrado/descifrado. SCU: Proceso Elemental. Parámetros: Complejidad de la transformación (Baja: <=2 operadores, Media: cifrado/descifrado con librería, Alta: cifrado/descifrado local), Número de DETs involucrados. Cálculo SP: 2-5 * #DETs, dependiendo de la complejidad de la transformación. No se cuentan si ya están en FPA.
- **1.4 Movimientos internos de datos:** Mover o trasladar datos entre particiones dentro de los límites de la aplicación con gestión específica. SCU: Parte del proceso elemental que cruza particiones. Parámetros: Número único de FTRs leídos o actualizados (Bajo 0-3, Medio 4-9, Alto >=10), Número de DETs enviados entre particiones. Cálculo SP: 4-10 * #DETs, dependiendo del nivel de FTRs. Si es bidireccional: 1 SCU síncrono, 2 SCUs asíncronos. Si cruza múltiples particiones, se repite el cálculo.
- **1.5 Aportar valor añadido a los usuarios por medio de configuración de datos:** Valor de negocio adicional por añadir, modificar o eliminar datos de referencia o código sin cambiar estructura de datos o código de programa (ej: añadir planes de suscripción, gestionar roles de usuario vía configuración). SCU: Proceso elemental por fichero lógico. Parámetros: Número de registros/entradas configuradas (Bajo 1-10, Medio 11-29, Alto +30), Número de atributos únicos involucrados. Cálculo SP: 6-12 * #Atributos, dependiendo del número de registros.
- **Diseño de interfaz (Categoría 2):** Requisitos relacionados con la interacción del usuario con el software.
- **2.1 Interfaces de usuario:** Elementos gráficos únicos identificables por el usuario que permiten interacciones sin cambiar requisitos funcionales (ventanas, menús, iconos, controles, etiquetas). SCU: Grupo de pantallas en un proceso elemental. Parámetros: Suma del número de propiedades únicas configuradas (Baja <10, Media 10-15, Alta >15), Número de elementos UI únicos afectados. Cálculo SP: 2-4 * #Elementos UI únicos, dependiendo de la complejidad del tipo UI. Considera elementos de configuración estética y pantallas de administración no contadas en FPA.
- **2.2 Métodos de Ayuda:** Información de apoyo o detalles sobre partes del software (guías, pop-ups). SCU: El objeto de ayuda. Parámetros: Número de elementos de ayuda, Realización de capturas de pantalla. Cálculo SP: (#Elementos de ayuda / 16) + (#Elementos de ayuda / 16) * 2 (si hay capturas). No se cuentan múltiples instancias del mismo contenido ni si ya están en FPA. Si implica implementaciones en UI y el objetivo principal es la documentación, se cuenta aquí.
- **2.3 Múltiples métodos de entrada:** Permisibilidad de la aplicación para recibir más de un método de entrada para un SCU (PDFs, códigos de barras, formularios). SCU: Proceso elemental. Parámetros: Número de DETs en el SCU (Baja 1-4, Media 5-15, Alta +16), Número de métodos de entrada adicionales. Cálculo SP: 3-6 * #Métodos Adicionales, dependiendo del número de DETs. Solo se cuentan los métodos adicionales en una aplicación de desarrollo.
- **2.4 Múltiples métodos de salida:** Permisibilidad de la aplicación para entregar información a través de más de un método de salida para un SCU. SCU: Proceso elemental. Parámetros: Número de DETs en el SCU (Baja 1-4, Media 5-15, Alta +16), Número de métodos de salida adicionales. Cálculo SP: 3-6 * #Métodos Adicionales, dependiendo del número de DETs. Solo se cuentan los métodos adicionales en una aplicación de desarrollo.
- **Entorno Técnico (Categoría 3):** Aspectos sobre el entorno de ejecución de la aplicación.
- **3.1 Múltiples plataformas:** Permitir que la aplicación funcione en múltiples plataformas (arquitecturas, sistemas operativos, lenguajes de programación, hardware). SCU: El proceso elemental. Parámetros: Tipo de plataformas (cómputo, SW, HW), Número de plataformas (solo si hay 2+). Cálculo SP: Tabla con valores fijos (20-80) dependiendo de la categoría y número de plataformas.
- **3.2 Tecnologías de Bases de Datos:** Características y operaciones añadidas a la BBDD o sus campos para NFRs sin afectar funcionalidad (cambios en tablas de negocio/referencia/código para NFRs, añadir/eliminar/cambiar índices, vistas, particiones, capacidad, queries/inserts sin modificar funcionalidades). SCU: Proceso elemental. Parámetros: Complejidad de los FTRs (determinada por DETs y RETs del FTR), Número de cambios relacionados con la BBDD. Cálculo SP: 6-12 * #Cambios, dependiendo de la complejidad del FTR. Los cambios se consideran a nivel de características.
- **3.3 Procesos Batch:** Procesos por lotes no considerados en FPA o que se ejecutan dentro de los límites de la aplicación pero no los cruzan. SCU: El proceso por lotes identificado por el usuario. Parámetros: Número de DETs procesados, Número de FTRs leídos o actualizados (Baja 1-3, Media 4-9, Alta >10). Cálculo SP: 4-10 * #DETs, dependiendo del nivel de FTRs. La automatización de varios procesos por lotes que se ejecutan juntos se considera un solo SCU.
- **Arquitectura (Categoría 4):** Técnicas de diseño y codificación usadas.
- **4.1 Software Basado en Componentes:** Grupos de software dentro de los límites de la aplicación que se integran con software existente o para crear componentes. SCU: El proceso Elemental. Parámetros: Componente de terceros o reutilización "in-house", Número de componentes únicos involucrados. Cálculo SP: 3 * #componentes únicos (in-house) o 4 * #componentes únicos (terceros). No mide la funcionalidad del componente.
- **4.2 Múltiples interfaces de Entrada/Salida:** Aplicaciones que necesitan múltiples interfaces para procesar información, que no suponen un cambio de funcionalidad y no se miden en FPA. Se diferencia de 2.3/2.4 en que se replica la misma interfaz con la misma tecnología. SCU: El proceso elemental. (Nota: No se proporciona la tabla de cálculo SP para esta subcategoría en los extractos).

1. **Cálculo del Tamaño SNAP (Paso 6):**

- Similar a FPA.
- **Desarrollo (DSP):** DSP = ADD (NFRs añadidos).
- **Mantenimiento (ESP):** ESP = ADD + CHG (NFRs modificados) + DEL (NFRs eliminados).
- **Aplicación (ASPA):** ASPA = ASPB (Antes del cambio) + (ADD + CHGA) - (CHGB + DEL).

1. **Documentar el proceso (Paso 7):** (Aunque no se detalla en los extractos, se menciona como el último paso).

**Conclusión:**

El método SNAP es una herramienta crucial y estandarizada (ISO 32430:2025 desde 2025) para medir el tamaño no funcional del software. Complementa a los métodos de estimación funcional como FPA, permitiendo una comprensión más completa del alcance del proyecto, especialmente en aspectos técnicos y de calidad como rendimiento, seguridad y usabilidad. Su aplicación sigue un proceso estructurado de 7 pasos que van desde la recopilación de información hasta el cálculo del tamaño SNAP, identificando y cuantificando requisitos no funcionales a través de categorías, subcategorías y unidades de conteo específicas (SCUs). Los "Datos de Código" son un tipo particular de datos reconocido y medido dentro de SNAP. La distinción entre requisitos funcionales y no funcionales es fundamental para la correcta aplicación del método.

convert_to_textConvertir en fuente

NotebookLM puede ofrecer respuestas inexactas. Compruébalas.