---
Name: 1 - Introducción
tags:
  - teoría
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

**CONCEPTOS PRINCIPALES**
****
1. *Seguridad de la información*
	Medidas de prevención y protección de la CIA (confidencialidad, integridad y disponibilidad) de la información.
2. *Seguridad informática*
	Se encuentra dentro de la seguridad de la información. Conjunto de prácticas para proteger sistemas y datos.
3. *Vulnerabilidad*
	Debilidad de un activo que puede ser explotada por una amenaza. Posibilidad de que una amenaza se materialice sobre un activo y sea una vía de ataque potencial. Ej.: defectos en el hardware.
4. *CVE (Common Vulnerabilities and Exposures)*
	Lista de vulnerabilidades conocidas. Se utilizan en NVD (National Vulnerability Database).
5. *NVD (National Vulnerability Database)*
	Repositorio del gobierno de EEUU de datos de gestión de vulnerabilidades. Se basan en estándares representados mediante el Protocolo de automatización de contenido de seguridad. Aporta mucha más información que el CVE. Gestionada por el NIST (National Institute of Standards and Technology).
6. *CVSS (Common Vulnerability Score System)*
	Sistema de puntuación de vulnerabilidades, indicando la gravedad de la vulnerabilidad.
7. *CWE (Common Weakness Enumeration)*
	Lista de tipos de vulnerabilidades.
8. *CPE (Common Platform Enumeration)*
	Lista donde se registran distintos tipos de plataformas (sistemas, software, paquetes...).
9. *OVAL (Open Vulnereability and Assessment Language)*
	Lenguaje que se utiliza para intercambio de información de este tipo de vulnerabilidades y listas.
10. *EXPLOIT*
	Herramienta que utiliza un atacante para aprovecharse de una vulnerabilidad.
11. *VULNERABILIDADES ZERO-DAY*
	Vulnerabilidades que no tienen parches que las solucionen, es decir, que se acaban de descubrir y no están registradas.

**CATEGORÍAS DE ATAQUES**
****
1. *Ataques de Interrupción* (Denial Of Service - DoS)
	**Ataque contra la disponibilidad** y con detección inmediata.

	**DoS - Denial of Service**
	Saturación de un servicio o máquina a través de múltiples flujos de información.
	- *Lógicos*: ataques a puntos concretos vulnerables. Solución: **parchear**.
	- *Inundaciones*: no determinado por vulnerabilidades. Apertura de muchas conexiones para detener o ralentizar servicios. Algunas opciones para solucionarlo:
		- *Traffic shaping*: controla el ancho de banda del tráfico de red. Evitan la sobrecarga de las redes mal dimensionadas. Consiste en disminuir el ancho de banda de aquellas conexiones que no sabemos si son malas al 100%.
		- *Limitar n.º de conexiones por IP*: no sirve, autodenegación de servicio.
		- *Ideal*: identificar distintas conexiones legales de las conexiones de ataque.

	**DDoS - Distributed Denial of Service**
	Igual que un DoS pero desde varios puntos de conexión hacia un mismo punto de destino. Lo más común es usar BotNets.

2. *Ataques de Intercepción*
	**Ataque contra la confidencialidad**. Atacantes interceptan y escuchan comunicación entre las dos partes para robar información. Ej.: sniffing, uso de keyjacks o mousejacks, pirateo de software NO CRACKEADO, keyloggers.

3. *Ataques de modificación*
	**Ataque contra la integridad**. Alteran los datos transmitidos entre dos puntos sin ser detectados.  Se trata de un ataque contra la integridad (funciones hash). Ej.: modificación de datos (sql inject), keyloggers, modificación de mensajes, buffer-overflow, spoofing (suplanta cosas, IPs, MAC, DNS...)

4. *Ataques de Generación*
	**Ataque contra la autenticidad**. Atacantes crean datos falsos y los envían a una red. Creación de identidades falsas, ataques contra la autenticidad. Ejemplos: scapy, hping3, packit.


**SISTEMAS DE DETECCIÓN DE ATAQUES**
****
- *IDS - Intrusion Detection System*
	Programa usado para detectar accesos no autorizados a un ordenador o a una red. Monitorizan el tráfico entrante y lo cotejan con una BBDD actualizada de firmas de ataques conocidas. Ante cualquier actividad sospechosa, emiten una alerta a los sys-admin.
- *IPS - Intrusion Prevention System*
	Software de protección de sistemas contra ataques. Realizan un análisis en tiempo real de conexiones y protocolos, identificando incidentes mediante patrones, anomalías o comportamientos sospechosos. Permite el control de acceso y la implementación de políticas que pueden descartar paquetes y desconectar conexiones, actuando de manera preventiva.
-  *Sensores* 
	Software en muchos puntos de la red para prevenir/detectar intrusiones red.