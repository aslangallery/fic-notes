---
Name: 7 - Optimización
tags:
  - teoría
asignatura: BDA
---
***[[Bases de Datos Avanzadas]]***

**TRANSFORMACIÓN A ÁLGEBRA RELACIONAL**
****
![[Pasted image 20240514092107.png]]

- *Agrupación*:
	![[Pasted image 20240514092504.png]]
	donde:
	- Los Gi son columnas de agrupación (similares al group by de SQL). Pueden omitirse, y en ese caso se hace un solo grupo con toda la relación.
	- Los fi son funciones colectivas o de agregación, como COUNT, SUM...
		![[Pasted image 20240514092636.png]]

- *Ordenación*:
	- Ordena R de acuerdo con la lista de atributos C.
	- No pertenece a la álgebra relacional estándar ya que esta usa semántica de conjuntos (no hay orden).
		![[Pasted image 20240514092746.png]]


Es habitual representar el álgebra relacional de una consulta como un árbol.
1. Nodos internos = operaciones.
2. Nodos terminales = datos (relaciones).
![[Pasted image 20240514093559.png]]

**OPTIMIZACIÓN ALGEBRAICA**
****
1. *Cascada de selecciones equivale a conjunción*
	σθ1∧θ2 (R) = σθ1 (σθ2 (R))

2. *Conmutatividad de la selección*
	σθ1 (σθ2 (R)) = σθ2 (σθ1 (R))
	![[Pasted image 20240514094909.png]]

3. *Cascada de proyecciones equivale a la última proyección (Ci: conjunto de atributos)*
	ΠC1 (ΠC2 (...(ΠCn (R))...)) = ΠC1 (R)

4. *Conmutatividad de selecciones y proyecciones*
	σθ(ΠC(R)) = ΠC(σθ(R))

5. *Producto cartesiano más selección equivalente a θ-join*
	σθ(R × S) = R onθ S

6. *Conmutatividad del join y del producto cartesiano*
	R onθ S = S onθ R R × S = S × R

7. *Asociatividad del producto cortesiano y join natural. También para el θ-join*
	(R × S) × T = R × (S × T) 
	(R on S) on T = R on (S on T)
	(R onθ1 S) onθ2∧θ3 T = R onθ1∧θ3 (S onθ2 T)

8. *Distribución de la selección sobre el producto cartesiano, join natural o θ-join*
	σθ1∧θ2 (R × S) = (σθ1 (R)) × (σθ2 (S)) 
	σθ1∧θ2 (R onθ0 S) = (σθ1 (R)) onθ0 (σθ2 (S))

9. *Distribución de la proyección sobre el producto cartesiano, join natural o θ-join*
	ΠC1∪C2 (R × S) = (ΠC1 (R)) × (ΠC2 (S))
	ΠC1∪C2 (R onθ S) = (ΠC1 (R)) onθ (ΠC2 (S))

10. *Conmutatividad de la unión e intersección (no de la diferencia)*
	R ∪ S = S ∪ R 
	R ∩ S = S ∩ R

11. *Asociatividad de la unión e intersección*
	(R ∪ S) ∪ T = R ∪ (S ∪ T) 
	(R ∩ S) ∩ T = R ∩ (S ∩ T)

12. *Distribución de la selección por la unión, intersección y diferencia*
	σθ(R ∪ S) = (σθ(R)) ∪ (σθ(S)) 
	σθ(R ∩ S) = (σθ(R)) ∩ (σθ(S)) 
	σθ(R − S) = (σθ(R)) − (σθ(S)); incluso σθ(R − S) = (σθ(R)) − S

- **Reglas heurísticas**
	1. Realizar las selecciones lo antes posible.
	2. Descartar lo antes posible los atributos no necesarios mediante proyecciones.
	3. Sustituir la combinación de producto cartesianos más selección por un join, si es posible.
	4. Ordenar los joins, haciendo antes los más restrictivos.

	![[Pasted image 20240514100045.png]]
	![[Pasted image 20240514100252.png]]
	![[Pasted image 20240514100458.png]]
	