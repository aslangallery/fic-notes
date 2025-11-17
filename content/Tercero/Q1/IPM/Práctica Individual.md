---
Name: Práctica Individual
tags:
  - practica
asignatura: IPM
---
***[[Interfaces Persona Máquina]]***

1. **Diseño estático**
	*Casos de uso*:
	- Buscar paciente.
	- Listar medicación del paciente.
	- Añadir medicación.
	- Editar medicación.
	- Eliminar medicación.
	- Añadir posología.
	- Editar posología.
	- Eliminar posología.
	![[Pasted image 20240920173338.jpg]]
	![[Pasted image 20240920181706.jpg]]
	![[Pasted image 20240920181710.jpg]]


2. **Diseño dinámico**
	![[Pasted image 20240921110454.png]]
	

3. **Información adicional**
	Como generador se ha utilizado "ChatGPT".
	- *Medida de la complejidad de la interfaz*
		>whats is the ciclomatic complexity of this diagram? 
		>flowchart TD 
		>A[Página Inicio] --> B[Buscar Paciente] 
		>B --> C[Mostrar Lista de Medicación de Paciente] 
		>
		>C --> D[Añadir Medicación] 
		>D --> E[Rellenar Campos] 
		>E --> F[Añadir Posología] 
		>F --> G[Cubrir Campos Posología] 
		>G --> H[Guardar Medicación] 
		>H --> |se añade la medicación| I[Listado de Medicación Actualizado] 
		>
		>C --> J[Editar Medicación] 
		>J --> |se elige la medicación a editar| K[Rellenar Campos a Editar] 
		>K --> L[Eliminar Posología] 
		>L --> O[Guardar Cambios] 
		>K --> M[Editar Posología] 
		>M --> N[Rellenar Campos a Editar] 
		>N --> O 
		>O --> P[Listado de Medicación Actualizado] 
		>
		>C --> Q[Eliminar Medicación] 
		>Q --> R[Seleccionar Medicación a Eliminar] 
		>R --> S[Guardar Cambios] 
		>S --> |se elimina la medicación| T[Listado de Medicación Actualizado] 
		>B --> U{¿Paciente encontrado?} 
		>U --> |sí| C 
		>U --> |no| W[Mostrar Mensaje Error] 
		>
		>A --> X{¿Conexión al servidor?} 
		>X --> |sí| B 
		>X --> |no| Y[Mostrar Mensaje Error Conexión]

$$
	\begin{gather}
	&\text{Complejidad Ciclomática} = E - N + 2P \\
	&\text{N (Número de Nodos)} = 24 \\
	&\text{E (Aristas)} = 26 \\
	&\text{P (Componentes Conectados)} = 1 \\
	&\text{Complejidad Ciclomática} = 26 - 24 + 2*1 \\
	&\text{Complejidad Ciclomática} = 4
	\end{gather}
$$
- *Casos de prueba*
	> Genera casos de prueba basados en el diagrama anterior, sin pasos de test.
	
	1. **Conexión exitosa al servidor** y carga de la página de búsqueda de pacientes.
	2. **Conexión fallida al servidor** y muestra del mensaje de error de conexión.
	3. **Búsqueda exitosa de un paciente** y visualización de la lista de medicación del paciente.
	4. **Búsqueda fallida de un paciente** y muestra del mensaje de error de "paciente no encontrado".
	5. **Visualización de la lista de medicación del paciente** tras una búsqueda exitosa.
	6. **Agregar una nueva medicación** a la lista de medicación del paciente.
	7. **Rellenar los campos de medicación** correctamente durante el proceso de añadir medicación.
	8. **Rellenar los campos de posología** correctamente durante la adición de medicación.
	9. **Guardar la nueva medicación añadida** y actualizar la lista de medicación.
	10. **Editar una medicación existente** de la lista de medicación del paciente.
	11. **Seleccionar una medicación para editar** y mostrar los campos de la medicación.
	12. **Editar los campos de posología** de una medicación existente.
	13. **Eliminar la información de posología** de una medicación existente.
	14. **Guardar los cambios en la medicación** después de editar o eliminar la posología.
	15. **Eliminar una medicación** de la lista de medicación del paciente.
	16. **Seleccionar una medicación para eliminar** y confirmar su eliminación.
	17. **Guardar los cambios después de eliminar una medicación** y actualizar la lista de medicación.
	18. **No encontrar un paciente** y mostrar un mensaje de error tras la búsqueda.
	19. **Intentar añadir una medicación con campos faltantes o inválidos** e impedir la presentación.
	20. **Intentar editar una medicación con detalles inválidos o incompletos** e impedir la presentación.
	21. **Intentar eliminar una medicación sin seleccionarla correctamente** e impedir la eliminación.
	22. **Mostrar la lista de medicación actualizada** después de cualquier cambio (adición, edición o eliminación).

	>Para los caminos que cubren más nodos, genera casos de prueba con paso

	**Camino 1: Añadir Nueva Medicación (Cubre Nodos A, B, C, D, E, F, G, H, I)**
	
	#### **Caso de Prueba 1: Añadir Nueva Medicación con Datos Válidos**
	
	**Descripción:** Verificar que se puede añadir una nueva medicación correctamente.
	
	**Pasos de Prueba:**
	1. Navegar a la página de inicio (**Nodo A**).
	2. Comprobar conexión con el servidor (**Nodo X**), si es exitosa, continuar.
	3. Buscar al paciente ingresando un nombre válido en la barra de búsqueda (**Nodo B**).
	4. Seleccionar al paciente encontrado y mostrar la lista de medicación (**Nodo C**).
	5. Hacer clic en el botón "Añadir Medicación" (**Nodo D**).
	6. Rellenar los campos de la nueva medicación (**Nodo E**).
	7. Hacer clic en "Añadir Posología" (**Nodo F**).
	8. Rellenar los campos de posología (**Nodo G**).
	9. Guardar la nueva medicación (**Nodo H**).

	**Resultados Esperados:**
	- La nueva medicación se guarda correctamente y se actualiza la lista de medicación del paciente (**Nodo I**).

	**Camino 2: Editar Medicación Existente (Cubre Nodos A, B, C, J, K, L, O, P)**

	#### **Caso de Prueba 2: Editar una Medicación Existente**
	
	**Descripción:** Verificar que se puede editar correctamente una medicación existente.
	
	**Pasos de Prueba:**
	
	1. Navegar a la página de inicio (**Nodo A**).
	2. Comprobar conexión con el servidor (**Nodo X**), si es exitosa, continuar.
	3. Buscar al paciente ingresando un nombre válido en la barra de búsqueda (**Nodo B**).
	4. Seleccionar al paciente encontrado y mostrar la lista de medicación (**Nodo C**).
	5. Hacer clic en "Editar Medicación" en una medicación existente (**Nodo J**).
	6. Seleccionar la medicación a editar y mostrar los campos de edición (**Nodo K**).
	7. Editar la información de la medicación o eliminar la posología si es necesario (**Nodo L**).
	8. Guardar los cambios (**Nodo O**).
	
	**Resultados Esperados:**
	
	- La medicación editada se guarda correctamente y se actualiza la lista de medicación del paciente (**Nodo P**).

	**Camino 3: Eliminar Medicación (Cubre Nodos A, B, C, Q, R, S, T)**

	#### **Caso de Prueba 3: Eliminar una Medicación Existente**
	
	**Descripción:** Verificar que se puede eliminar correctamente una medicación existente.
	
	**Pasos de Prueba:**
	
	1. Navegar a la página de inicio (**Nodo A**).
	2. Comprobar conexión con el servidor (**Nodo X**), si es exitosa, continuar.
	3. Buscar al paciente ingresando un nombre válido en la barra de búsqueda (**Nodo B**).
	4. Seleccionar al paciente encontrado y mostrar la lista de medicación (**Nodo C**).
	5. Hacer clic en "Eliminar Medicación" en una medicación existente (**Nodo Q**).
	6. Seleccionar la medicación a eliminar (**Nodo R**).
	7. Confirmar la eliminación y guardar los cambios (**Nodo S**).
	
	**Resultados Esperados:**
	
	- La medicación seleccionada se elimina correctamente y se actualiza la lista de medicación del paciente (**Nodo T**).

	**Camino 4: Búsqueda Fallida de Paciente (Cubre Nodos A, B, U, W)**

	#### **Caso de Prueba 4: Búsqueda Fallida de un Paciente**
	
	**Descripción:** Verificar que el sistema maneja correctamente una búsqueda fallida de un paciente.
	
	**Pasos de Prueba:**
	
	1. Navegar a la página de inicio (**Nodo A**).
	2. Comprobar conexión con el servidor (**Nodo X**), si es exitosa, continuar.
	3. Ingresar un nombre de paciente no válido en la barra de búsqueda (**Nodo B**).
	4. El sistema pregunta si el paciente fue encontrado (**Nodo U**).
	5. El sistema muestra un mensaje de error indicando que no se encontró el paciente (**Nodo W**).
	
	**Resultados Esperados:**
	
	- El sistema muestra correctamente un mensaje de error indicando que el paciente no fue encontrado.
