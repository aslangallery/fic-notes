---
Name: 5 - Modelos de razonamiento
tags:
  - teoría
asignatura: SI
---
***[[Sistemas Inteligentes]]***

**MODELOS Y DOMINIOS**
****
- **Modelos categóricos**: dominios de la naturaleza marcadamente simbólica, con soluciones que pueden establecerse con total seguridad.
- **Modelos probabilísticos**: dominios de naturaleza estadística, soluciones no obtenibles de forma unívoca.
- **Modelos de razonamiento bajo incertidumbre**: dominios con incertidumbre, inherente a datos o a los propios mecanismos inferenciales.
- **Modelos de razonamiento basado en conjuntos difusos**: dominios en los que los elementos diferenciales incluyen matices lingüísticos.

**MODELO CATEGÓRICO**
****
- *Formalmente*:
	- X = {manifestaciones} = {x1, x2, ..., xn}
	- Y = {interpretaciones} = {y1, y2, ..., ym}
- Las relaciones causales entre manifestaciones e interpretaciones se formalizan a través de la función de conocimiento E.
	- E = E (X, Y)
- *Problema lógico*:
	- Dadas unas manifestaciones caracterizadas por una función _f_, encontrar la función _g_ que satisface:
		- E: (f → g)
		- E: (¬g → ¬f)
		- E = E (x1, ..., xn, y1, ..., ym)
- _Procedimiento semántico para el modelo categórico_:
	1. Identificación de M.
	2. Identificación de I.
	3. Construcción de E.
	4. Construcción del conjunto completo de complejos de manifestaciones.
	5. Construcción del conjunto completo de complejos de interpretaciones.
	6. Construcción del conjunto completo de complejos de manifestación-interpretación.
	![[Pasted image 20240506202059.png]]

- _Construcción del conjunto completo de complejos de manifestación-interpretación_:
	- **Base lógica expandida**:
		- BLE = M x I = {m1i1, m1i2, m1i3, m1i4, 
						m2i1, m2i2, m2i3, m2i4, 
						m3i1, m3i2, m3i3, m3i4, 
						m4i1, m4i2, m4i3, m4i4}
		- La solución a cualquier problema está en BLE, pero hay muchas combinaciones absurdas.
		- El papel del conocimiento _E_ es eliminarlas y pasar a una base lógica reducida E: (BLE → BLR).
	![[Pasted image 20240506202444.png]]
