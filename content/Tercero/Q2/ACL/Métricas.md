---
Name: Métricas
tags:
  - práctica
asignatura: ACL
---
***[[Aseguramiento de la Calidad]]***

1. **Defina, en una plantilla, una métrica que permita conocer el grado de cumplimiento actual de los costes definidos en el Plan de Proyecto** 

| 1                       | Cumplimiento de costes                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Objetivo*              | Determinar el grado de cumplimiento de los costes definidos en el Plan de Proyecto                                                                                                                                                                                                                                                                                         |
| *Fórmula de cálculo*    | CA/CP * 100                                                                                                                                                                                                                                                                                                                                                                |
| *Unidad de medida*      | %                                                                                                                                                                                                                                                                                                                                                                          |
| *Origen de los datos*   | Plan de Proyecto                                                                                                                                                                                                                                                                                                                                                           |
| *Datos de entrada*      | CA: Costes actuales               CP: Costes planificados                                                                                                                                                                                                                                                                                                                  |
| *Periodicidad*          | Seguimiento                                                                                                                                                                                                                                                                                                                                                                |
| *Criterios de análisis* | Un valor de crecimiento no proporcional al valor del porcentaje de completitud del proyecto indica desviaciones en el coste. Si se detecta un crecimiento muy rápido del porcentaje, se deben tomar acciones preventivas para reducir costes. Si se detecta que el porcentaje se acerca al 100% antes del final del proyecto, se deben tomar acciones correctivas. detecta |

2. **Defina, en una plantilla, una métrica que permita conocer el nivel de productividad de los programadores de un proyecto en comparación con otros proyectos de la empresa.** 

| 2                       | Nivel de productividad de programadores                                                                                                                                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Objetivo*              | Determinar el nivel de productividad de los programadores de un proyecto en comparación con otros proyectos de la empresa.                                                                                                                              |
| *Fórmula de cálculo*    | PFC/PFP * 100                                                                                                                                                                                                                                           |
| *Unidad de medida*      | %                                                                                                                                                                                                                                                       |
| *Origen de los datos*   | Plan de Proyecto                                                                                                                                                                                                                                        |
| *Datos de entrada*      | PFC: Puntos Función Completados        PFP: Puntos Función Planificados                                                                                                                                                                                 |
| *Periodicidad*          | Seguimiento                                                                                                                                                                                                                                             |
| *Criterios de análisis* | Si se detecta que un programador tiene un porcentaje alto de cumplimiento de hitos en un proyecto y bajo en el otro, se le asignarán más tareas del primero y menos del segundo. Además, se reasignarán las horas de otro programador de forma inversa. |

3. **Defina, en una plantilla, una métrica que permita conocer el grado de implementación de los casos de uso previstos para la iteración actual.** 

| 3                       | Grado de implementación de CU                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| *Objetivo*              | Determinar el grado de implementación de los casos de uso previstos para la iteración actual                             |
| *Fórmula de cálculo*    | CUI/CUP  * 100                                                                                                           |
| *Unidad de medida*      | %                                                                                                                        |
| *Origen de los datos*   | ERS                                                                                                                      |
| *Datos de entrada*      | CUI: Casos de Uso Implementados CUP: Casos de Uso Previstos                                                              |
| *Periodicidad*          | Seguimiento                                                                                                              |
| *Criterios de análisis* | Cuanto mayor sea el porcentaje, más cerca se estará de cumplir con totalidad con los requisitos funcionales del cliente. |

4. **Defina, en una plantilla, una métrica que permita conocer el tiempo medio invertido en las revisiones de progreso del proyecto.** 

| 4                       | Tiempo medio invertido en revisiones                                                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Objetivo*              | Determinar el tiempo medio invertido en revisiones de progreso del proyecto                                                                             |
| *Fórmula de cálculo*    | STR/NR                                                                                                                                                  |
| *Unidad de medida*      | Horas                                                                                                                                                   |
| *Origen de los datos*   | Revisiones                                                                                                                                              |
| *Datos de entrada*      | STR: Sumatorio Tiempo Revisiones NR: Número Revisiones                                                                                                  |
| *Periodicidad*          | Al cerrar el proyecto                                                                                                                                   |
| *Criterios de análisis* | Si alguna de las revisiones de un proyecto ha durado una cantidad de horas muy diferente a la media, se debe revisar cual fue el motivo y justificarlo. |

5. **Defina, en una plantilla, una métrica que permita determinar el grado de criticidad de un proyecto.** 

| 5                       | Grado de criticidad                                                                                                                                                |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| *Objetivo*              | Determinar el grado de criticidad de un proyecto                                                                                                                   |
| *Fórmula de cálculo*    | NTC                                                                                                                                                                |
| *Unidad de medida*      | Valor numérico                                                                                                                                                     |
| *Origen de los datos*   | Plan de Proyecto                                                                                                                                                   |
| *Datos de entrada*      | NTC: Número de Tareas Críticas                                                                                                                                     |
| *Periodicidad*          | Seguimiento                                                                                                                                                        |
| *Criterios de análisis* | El impacto de las consecuencias se mide del 1 al 5, siendo 1 un riesgo asumible y 5 un riesgo perjudicial. Cuanto mayor sea el número, más crítico es el proyecto. |

6. **Defina, en una plantilla, una métrica que permita detectar el grado de desviación en tareas críticas de un proyecto.** 

| 6                       | Grado de desviación                                                                                                                             |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| *Objetivo*              | Determinar el grado de desviación en tareas críticas de un proyecto.                                                                            |
| *Fórmula de cálculo*    | (TRTC - TPTC)/TPTC * 100                                                                                                                        |
| *Unidad de medida*      | %                                                                                                                                               |
| *Origen de los datos*   | Plan de Proyecto                                                                                                                                |
| *Datos de entrada*      | TRTC: Tiempo Real Tareas Críticas TPTC: Tiempo Planificado Tareas Críticas                                                                      |
| *Periodicidad*          | Seguimiento                                                                                                                                     |
| *Criterios de análisis* | Un porcentaje positivo indica desviaciones en las tareas críticas y un aumento de la duración del camino crítico y de la duración del proyecto. |

7. **Defina, en una plantilla, una métrica que permita conocer el grado de acoplamiento entre las clases de diseño en un proyecto.** 

| 7                       | Grado de acoplamiento entre clases                                                                         |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| *Objetivo*              | Determinar el grado de acoplamiento entre las clases de diseño en un proyecto.                             |
| *Fórmula de cálculo*    | NCERC/TC                                                                                                   |
| *Unidad de medida*      | Valor numérico                                                                                             |
| *Origen de los datos*   | Diagramas de diseño de bajo nivel                                                                          |
| *Datos de entrada*      | NCERC: Número Clases Externas Referenciadas por la Clase TC: Total Clases                                  |
| *Periodicidad*          | Fase de diseño de bajo nivel                                                                               |
| *Criterios de análisis* | Cuanto mayor sea el nivel, mayor será el acoplamiento de la clase con otras clases del sistema y viceversa |

8. **Defina, en una plantilla, una métrica que permita conocer el grado de compleción de los requisitos iniciales de un proyecto.**

| 8                       | Grado de compleción de requisitos iniciales                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| *Objetivo*              | Determinar el grado de compleción de los requisitos iniciales de un proyecto                                                   |
| *Fórmula de cálculo*    | RIC/TRI * 100                                                                                                                  |
| *Unidad de medida*      | %                                                                                                                              |
| *Origen de los datos*   | ERS                                                                                                                            |
| *Datos de entrada*      | RIC: Requisitos Iniciales Completados TRI: Total Requisitos Iniciales                                                          |
| *Periodicidad*          | Seguimiento                                                                                                                    |
| *Criterios de análisis* | Una vez se alcance el 100%, se habrá completado la totalidad de los requisitos sugeridos por el cliente en la primera reunión. |
