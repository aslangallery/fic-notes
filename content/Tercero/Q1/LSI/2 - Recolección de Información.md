---
Name: 2 - Recolección de Información
tags:
  - teoría
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

**CONCEPTOS**
****
- *Hosts Discovery*: descubrir las máquinas que hay en una red. Fáciles de descubrir server web, server DNS, server mail, máquinas en DMZ. Uso de herramientas como Nmap.
	- **DMZ - Zona Desmilitarizada**: red perimetral que se encuentra dentro de la red interna de la organización. Se encuentran ubicados exclusivamente todos los recursos de la empresa que deben ser accesibles desde Internet, como el servidor web o el de correo. 
- *Port Scanning*: envío de paquetes a determinados puertos de una máquina para ver si están escuchando o no.
- *FingerPrinting*: saber qué está funcionando, identificar características específicas de un dispositivo o servidor remoto. Incluyen sistema operativo, configuración de red...
- *FootPrinting*: recolectar información de una organización. Pueden ser:
	- **Activo**: ingeniería social, obtener información confidencial mediante la manipulación de usuarios legítimos.
	- **Pasivo**: no interactúa directamente con el objetivo. Ej.: redes sociales, wikis, blogs, webs...
- *Google Hacking/Dorks*: BBDD con información semántica de Google. Expresiones de búsquedas que permiten sacar información interesante.

**FUENTES PÚBLICAS**
****
1. *Herramientas de recolección de información*
	- **OSINT (Open Source Intelligence)**: recopilación y análisis de información de fuentes abiertas en Internet.
2. *Gestión de rangos de red*
	- **NIC (Network Information Center)**: servicios de los países que gestionan y proporcionan información de dominios, servicios DNS...
	- **RIPE (Redes IP Europeas)**: gestionar direccionamiento IP.
	- **IP6.nl**: página para introducir un dominio IPv6 y devuelve el rango de dominios, IPv4s asociadas a dicha web...
3. *Host Discovery*
	Las IPs importantes tienen las primeras direcciones de la red, es preferible camuflarlas en medio de todas las IPs para que sea más difícil encontrarlas.
	- **Ping**: se puede filtrar, podría dar la máquina por muerta cuando está en funcionamiento.
	- **Traceroute**: marca todos los routers por los que va pasando el paquete hasta la máquina objetivo. Con la opción `-T` se usan paquetes SYN, lo que mostrará toda la ruta aunque se filtre el tráfico ICMP.
	- **Robtex**: recolección de información sobre máquinas.
	
	  ***DNS***
		**Dos tipos**:
		- *Primarios*: mantienes la información de la BBDD.
		- *Secundarios*: no se mantiene nada, se configura para que con determinada periodicidad se comuniquen con los primarios para hacer transferencias.

		**Tipos de registros**:
		- A (directa): nombre in IP `nino in A 193.144.43.128`
		- AAA: registro de dirección IPv6.
		- CNAME: registro de nombre canónico.
		- NS (Name Server): definir el servidor de nombres de un dominio.
		- MX (mail exchange): se indican los servidores de correo electrónico.
		- PTR (inversa): de IP a nombre.

		**Herramientas para trabajar con DNS** (no meter PTR en DNS):
		- `nslookup`: saber si el DNS está resolviendo correctamente.
		- `dig`: búsquedas en los registros DNS, a través de los nombres de servidores.
		- `dnsenum`: devuelve dirección de host, name servers y servers de correo. Intenta hacer una transferencia de zona.
		- `dnsmap`: registro DNS por fuerza bruta o diccionarios, siguiente opción si no acepta transferencia de zona ni registros PTR.
		- `dnsrecon`: usa registros PTR, aporta mucha información, el host podría no usar registros PTR.

	 ***Caché Snooping***
	   Investiga un servidor DNS para saber si determinadas páginas están en caché.
		- `dnsrecon -t snoop -n x.x.x.DNS -D fichero`
			Es no recursivo. Cuando hace peticiones, pone el flag "RD" a 0, solo resuelve si lo tiene en caché.
		- **Estimación de tiempos**
			La opción anterior puede no funcionar con algunos DNS. La solución a lo anterior es lanzar consultas contra el servidor y medir tiempos, las que tarden muy poco estarán cacheadas.

4. *Port Scanning*
	Testear de forma remota algunos puertos y determinar su estado. La herramienta que se utiliza es `nmap`.
	1. *Tipos*
		- **Activo**: se interactúa con un puerto para ver cómo reacciona.
		- **Pasivo**: observando la paquetería, en las cabeceras se ve el puerto de origen/destino.
	2. *Escaneo de puertos con nmap*
		- `-sS`: escaneo default (SYN scan o STEALTH). 
		- `-sA`: escaneo ACK para saber si en medio hay un firewall de control de estado. Si no hay uno, devuelve un RST (la mayor parte de las veces). Si existe un firewall, no responden porque droppean el escaneo.
		- `-sN, -sX, -sF`: 
			- `-sN` no da ningún flag
			- `-sX` fija FIN, PSH y URG
			- `-sF` fija el flag FIN
		- `IP`: host discovery y si está levantado hace port scanning.
		- `-sP`: host discovery manda ICMP, ping, SYN y ACK. Opción `-D` para hacer decoy scan y no ser descubierto.
		- `-PU 53,65`: para puertos UDP.
		- `-O`: fingerprinting de SO.
	3. *Estados de puertos*
		- **Open**: abierto, acepta conexiones TCP o paquetes UDP.
		- **Closed**: cerrado, no hay nada escuchando, pero el puerto está siendo atendido.
		- **Filtered**: impide que las pruebas alcancen el puerto.
		- **Unfiltered**: puerto accesible, pero no se sabe si está abierto o cerrado.
		- **Open|filtered**: no se puede saber si está abierto o filtrado.
		- **Closed|filtered**: no se puede saber si está cerrado o filtrado.

5. *Fingerprinting*
	1. *FingerPrinting de SO*
		Identifica el sistema operativo y determina qué está corriendo a ese nivel.
		`nmap -O`
	2. *Fingerprinting de puertos*
		Identificar el servicio que corre en una máquina y qué versión.
	3. *FingerPrinting de servicios*
		Identifica tipo y versión de un servicio que se ejecuta en un puerto determinado.
		`nmap -sV`
	4. *Técnicas de FingerPrinting*
		- **A nivel TCP-IP**: identificar SO, servicios o aplicaciones en una red mediante el análisis de las respuestas a solicitudes TCP/IP. 

6. *Búsqueda de Vulnerabilidades*
	1. Usando herramientas:
		- *Propósito general*:
			- **OpenVAS**: marco de software de varios servicios y herramientas que ofrecen escaneo y gestión de vulnerabilidades informáticas.
			- **Nessus**: programa de escaneo de vulnerabilidades en diversos sistemas operativos.
		- *Orientadas en aplicaciones web*
			- **NikTop**
			- **OWASP ZAP**
			- **W3af**

	2. De forma manual:
		Usando CVE, CCE, CVSS, NVD, etc.

7. *EXPLOITS*
	Aprovechar y atacar vulnerabilidades.
	1. *Manuales y Herramientas*
		- **Metasploit**: marco de pruebas de penetración de código abierto que proporciona información, analiza y ataca vulnerabilidades de seguridad.
		- **Cobalt Strike**: herramienta que permite a los equipos de seguridad emular la actividad de los ciber-delincuentes dentro de una red.
		- **Fat Rat**: desarrollar troyanos.
		- **SHODAN**: motor de búsqueda que permite al usuario encontrar iguales o diferentes tipos específicos de equipos conectados a Internet a través de una variedad de filtros.
		- **SEC**: report de ataques de ingeniería social.
		- **w3af**: a parte de analizar vulnerabilidades en aplicaciones web, permite explotar dichas vulnerabilidades.
	2. *Payloads*
		Porción de código que se entrega e intenta ejecutar en un sistema como parte de un ataque cibernético.
		Cuando definimos un payload se hace de la manera más discreta posible (ofuscación) para que sea difícil de detectar. 
		- **Single**: indicen sobre una vulnerabilidad para que se pueda tener acceso a la máquina.
		- **Stagers**: abre conexión entre la máquina y el atacante.
		- **Stages**: se cargan en el sistema después de que el stager haya establecido conexión. Ej.: Meterpreter (etapa que proporciona interfaz interactiva para el atacante).

		***Ofuscación***
		Modificar un binario poco a poco para que haga las mismas funcionalidades pero que no sea detectado por los antivirus.
		- **msfpayload**: permite generar ejecutables con un payload en su interior.
		- **msfencode**: le pasas distintos métodos de ofuscación para convertir a los binarios.
		- **msfvenom**: combinación de `msfpayload` y `msfencode`
	3. *WAF* (Web Application Firewall)
		Protege las aplicaciones web de ataques. Detiene y previene ataques como inyecciones SQL, XSS... 
		- **modsecurity**
		- **InfoGuard**
		- **CloudFare**

8. *Post Exploit*
	- **Escalado de privilegios**
		Explotar cierta vulnerabilidad proporciona ciertos privilegios en una máquina.
	- **Pivoting**
		Se utiliza una máquina intermedia para atacar en remoto a otras máquinas. 

**TCP HANDSHAKE**
****
Pasos entre dos dispositivos de red, cuando intentan establecer una conexión a través del protocolo TCP. Se divide en tres pasos principales:

1. *Solicitud de conexión (SYN)*
	El cliente envía el paquete TCP llamado SYN al servidor.
	El paquete contiene información sobre el puerto de origen, destino y el número de secuencia inicial, que se usa para mantener el seguimiento de los datos.
2. *Aceptación de conexión (SYN-ACK)*
	Si el servidor está disponible y dispuesto a aceptar la conexión, responderá con un paquete SYN-ACK. 
	El paquete contiene un número de secuencia generado por el servidor y confirma la solicitud de conexión del cliente.
3. *Confirmación de conexión (ACK)*
	El cliente responde al servidor con un paquete ACK, confirmando que ha recibido el SYN-ACK.
	A partir de este punto, la conexión se considera establecida y los datos pueden comenzar a transmitirse de forma segura.

**FIREWALL**
****
1. *Sin control de estado*
	Filtra conexiones pero no obliga a que las conexiones se hagan mediante handshake.
2. *Con control de estado*
	Comprueba que la conexión siga en handshake de TCP. En UDP es diferente y más complicado:
	- La primera vez que le llega la paquetería de una máquina, la registra y la deja pasar.
	- Si le responde la máquina, entonces le concederá el paso las próximas veces.
	- Si no hay respuesta por parte de la máquina, la próxima vez meterá un bloqueo.

Tipos de firewalls:
- *Router*: router que a mayores realiza listas de acceso.
- *NAT*: lo mismo que lo anterior, pero traduce la intranet a direccionamiento público.
- *Transparente*: no tiene IPs, trabaja a nivel de MAC. No se puede ver a nivel de red.

**IDLE SCAN/ZOMBIE SCAN**
****
Técnica de escaneo de puertos, sin que el atacante tenga una dirección IP directa y sin generar actividad aparente en la máquina objetivo. Se basa en la ing. inversa de paquetes y en el concepto de "zombies" (máquinas intermedias involuntarias) para ocultar la verdadera fuente del escaneo.

Funcionamiento básico:
1. *Atacante*: usa una máquina que controla como "zombie". La máquina envía paquetes de escaneo a la máquina objetivo.
2. *Máquina zombie*: se usa para enviar los paquetes. Actúa como intermediario.
3. *Máquina objetivo*: máquina que el atacante desea escanear sin ser detectado.

Técnica de fragmentación de paquetes IP y análisis de las respuestas de la máquina objetivo. El atacante envía paquetes de solicitud a la máquina "zombie" y esta los redirige a la máquina objetivo. Después la máquina "zombie" monitorea la respuesta del host objetivo y determina si un puerto está abierto o cerrado según la respuesta.

**OWASP**
****
Una de sus líneas de trabajo es el TOP 10. Cada 4 años publican eso y muestra el estado sobre las vulnerabilidades de las aplicaciones web.

1. *Broken Access Control*
	Problemas de gestión de usuarios.
	- **IDs inseguros**
	- **Path traversal**
	- **Problemas de permisos**
2. *Cryptographic Failures*
	Errores en la implementación a nivel criptográfico que comprometen la seguridad de un sistema.
3. *Injection*
	Mala validación de los datos, permitiendo el acceso a ficheros del sistema, datos de usuarios, accesos restringidos...
	- **XSS (Cross-Site Scripting)**
	- **SQL**
4. *Insecure Design*
	Problemas relacionados con patrones de diseño. Incluyen vulnerabilidades como XXE, permitiendo a los atacantes acceder a los datos a través de archivos XML. 
5. *Security Misconfiguration*
	Errores en configuración de seguridad que resultan en vulnerabilidades. 
6. *Vulnerable and Outdated Components*
	Riesgos relacionados a componentes desactualizados o vulnerables.
7. *Autentication*
	Problemas asociados a la autenticación.
8. *Software and Data Integrity Failure*
	Problemas en la integridad del software y los datos.
9. *Security login and monitorize*
	Problema de monitorización adecuada y registros que alerten sobre ataques o actividades sospechosas.
10. *Server Side Request Forgery*
	Permite al atacante realizar solicitudes a través de un servidor (pivoting).

- **ASVS**
	Guía de pasos para poder verificar el nivel de seguridad de un aplicativo web. 

- **ZAP**
	Software de detección de vulnerabilidades de aplicación web desarrollado dentro de OWASP. 

- **WAF**
	Firewall de aplicativo web. Ej.: modsecurity
	WAFW00F realiza fingerprinting a una URL para comprobar si hay un WAF (si no hay WAF, es fácil atacar).

**TÉCNICAS PARA EVITAR FINGERPRINTING**
****
Generaciones de Fingerprinting:
1. *Generacion ASCII*
	Mandando paquetes contra una máquina y mirando las cabeceras con las que me respondía para sacar información de estas.
2. *Generación TCP-IP*
	Coger distintas cabeceras con distintos datos y lanzar contra una máquina esas cabeceras con distintos parámetros. Después analizar las respuestas que da a esas cabeceras para sacar información de la máquina.
3. *Generación ICMP*
4. *Generación C7*

`/proc/syslnet`: se pueden ajustar variables o parámetros relacionados con el comportamiento de la máquina a nivel de red.

**IP TABLES MANGLE**
****
Tabla específica de iptables que se encarga de modificar paquetes, para ello existen las opciones:
- *TOS (Type Of Service)*
	Usado para definir el tipo de servicio de un paquete y se debe usar para definir cómo los paquetes deben ser enrutados.
- *TTL (Time To Live)*
	Cambia el campo de tiempo de vida de un paquete. Se puede usar para cuando no queremos ser descubiertos por ciertos proveedores de servicios de Internet.
- *MARK*
	Usado para marcar paquetes con valores específicos.

**MÉTODOS DE INFORMATION GATHERING**
****
1. *Spidering*
	Lo hacen los buscadores. Recorre el árbol completo de un `index.html` y lo descarga. Enlaza páginas, subpáginas... 
2. *Crawlering*
	Hace análisis semántico de todo el contenido. Lo hacen también los buscadores.
3. *Scrappering*
	Recolecta información de otros lados y la compara con lo que se ha recogido con el `crawlering`.
4. *Hardening*
	Endurece la seguridad configurando o securizando sistemas. Una herramienta básica es `lynis audit system` que hace una auditoría de seguridad de la máquina y proporciona consejos para mejorar la seguridad de Linux.
	- Considera instalar `libpam-tmpdir`, añade seguridad a los procesos de autenticación.
	- Considera instalar `apt-list bugs`, muestra las vulnerabilidades o bugs de un paquete.
	- Considera instalar `needrestart`, indica qué librerías necesitan un reinicio.
	- Considera instalar `debscan`, analiza vulnerabilidades.
	- Considera instalar `debsums`, gestiona la integridad de los paquetes. Mira los hash de los paquetes instalados y los compara con los paquetes originales.
	- Considera instalar `fail2ban`, protege las máquinas contra ataques de password guessing. 
	- `DEP` (Data Execution Prevention): protección contra los desbordamientos de pila y buffer overflow.
	- `/etc/security/limits.conf`: se pueden meter variables para limitar cosas.
		- `core`: limita el tamaño máximo de los ficheros `core`.
		- `memblock`: máximo tamaño de memoria que puede utilizar.
		- `maxloging`: número máximo de sesiones que puede abrir.
		- Hay más variables como para limitar procesos, RAM...
	- `chage`: hace que los passwords expiren cada x tiempo.
	- `/etc/shadow`: están los passwords hasheados.
	- Recomienda instalar librerías como `pam_cracklib`, `pwquality` y `pampasswdqc`. Sirven para especificar políticas sobre contraseñas (longitud, ciertos caracteres...).
	- Cambiar el `UMASK` de 022 a 027.
		- 022: permisos default a `rwxr_xr_x`
		- 027: permisos default a `rwxr_x__`
	- Bajar la variable `TMOUT` en `/etc/profile` o `$HOME/.profile` y en `/etc/bash.bashcr` o `$HOME/.bashcr`.
	- Recomienda poner `/home`, `/var` y `/tmp` en particiones separadas.
	- `LVM`: volúmenes lógicos virtuales.