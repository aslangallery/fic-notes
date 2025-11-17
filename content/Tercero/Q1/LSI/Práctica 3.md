---
Name: Práctica 3
tags:
  - práctica
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

*PROTOCOLOS SEGUROS Y AUDITORÍAS DE SEGURIDAD*

1. **Tomando como base de trabajo el SSH pruebe sus diversas utilidades**
	**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf4BSbWxedKOf4Mc5ZeR8qEuKTAO3hnW2HbQYH1MdEdbE7Iqg0Y5A2q6xc8obTU_B4MNuh8PAosQC7oKUmL81d5LFTgtcCgrsenK5j8fih7na9rDMscazqYgufFSEvu3YoQvSkjrMDPLut8Cq0_KbGqbxwK?key=pN5sXoQu3LIdO0u3q0tgHg)**
	
	`ssh -v lsi@10.11.49.50`
	Explicación paso por paso:
	1. Establece la conexión y se negocian los algoritmos que se van a usar.
	2. Cuando se decide el algoritmo, el cliente verifica que realmente es el servidor:
		- Si es la primera vez que se conecta, le enviará su clave pública y el cliente la guardará en `$HOME/.ssh/ssh_known_hosts`. Cada vez que el cliente se conecte al servidor, este le mandará un mensaje encriptado con la pública del servidor. Por lo tanto, como el servidor tiene la privada, será capaz de desencriptar el mensaje y demostrar que es él.
	3. Se produce un intercambio de claves para preparar la encriptación simétrica de la sesión.
	4. Los dos extremos producen un par de claves temporales y se intercambian las públicas, las cuales se utilizarán como claves para general la clave simétrica.
	5. El cliente debe iniciar sesión, para ello tenemos dos opciones (contraseña o clave). En nuestro caso será el método de clave.
	6. Para ello, el cliente le manda su clave pública al servidor y este la guardará en `$HOME/.ssh/authorized_keys`. Cada vez que el cliente quiera iniciar sesión, el servidor le mandará el mensaje encriptado con la pública del cliente. Si el cliente tiene la clave privada, será capaz de desencriptar el mensaje y demostrar que el cliente es de fiar.
	**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc-fH1g65XZZLBheLoNrf9_a4kimO0iNgWnnGzZI92aQxNW9SJ5DrpmU_grXBHDmBsMZ8uojOiL_0hjflMMaNf_ZEKNDGP-9YGPkDeXXIqTeNMYnuX7MJdQNndnTRFzsqpc3lJkrlqtVrlMP11enaTOsvz8?key=pN5sXoQu3LIdO0u3q0tgHg)**

	- **Abra un shell remoto sobre SSH y analice el proceso que se realiza. Configure su fichero ssh_known_hosts para dar soporte a la clave pública del servidor.**
		1. Hacemos loggin como root.
		2. Creamos el archivo `ssh_known_hosts`
			`nano /etc/ssh/ssh_known_hosts`
		3. Copiamos la clave pública de nuestro compañero en el archivo
			`ssh-keyscan 10.11.49.50 >> /etc/ssh/ssh_known_hosts`
		4. Si al conectarnos con `ssh lsi@10.11.49.50` no sale fingerprinting es que está bien.
	- **Haga una copia remota de un fichero utilizando un algoritmo de cifrado determinado. Analice el proceso que se realiza.**
		```shell
		scp -c chacha20-poly1305@openssh.com /home/lsi/algcif.txt lsi@10.11.49.50:/home/lsi
		```
	- **Configure su cliente y servidor para permitir conexiones basadas en un esquema de autenticación de usuario de clave pública.**
		*HACERLO COMO LSI*

		`ssh-keygen -t rsa`
		Nos pedirá frase de paso. La frase de paso da una huella digital, que se usa para, con un algoritmo simétrico, cifrar la `id_rsa`. La dejamos en blanco.

		**CLIENTE**
		1. Generar el par de claves público-privadas
			`ssh-keygen -t rsa`
		2. Enviar la clave pública al servidor (nuestro compañero)
			`scp id_rsa.pub lsi@10.11.49.50:./.ssh/id_rsa.pub`

		**SERVIDOR**
		1. `cd /home/lsi/.ssh`
		2. Copiar la clave en `authorized_keys`
			`cat id_rsa.pub >> authorized_keys`

		**COMPROBACIÓN**
		*DESDE LSI*
		Si al hacer `ssh lsi@10.11.49.50` deja entrar sin contraseña, está bien.

	- **Mediante túneles SSH securice algún servicio no seguro**
		**CLIENTE**
		`ssh -P -L PUERTO:10.11.49.50:80 lsi@10.11.49.50`
		Abre una conexión ssh y redirige localhost de mi puerto al 80 del compañero. Se crea un túnel ssh seguro.

		**COMPROBACIÓN**
		`curl localhost:PUERTO`

	- **“Exporte” un directorio y “móntelo” de forma remota sobre un túnel SSH**
		```sh
		sshfs lsi@10.11.49.50:/home/lsi/DIRECTORIOCOMPA /home/lsi/MIDIRECTORIO
		```

	- **PARA PLANTEAR DE FORMA TEÓRICA.: Securice su servidor considerando que únicamente dará servicio ssh para sesiones de usuario desde determinadas IPs.**
		Para permitir solo conexiones de determinadas IPs, en el fichero `/etc/ssh/sshd_config`, en la opción AllowUsers meteremos los usuarios e IPs de las conexiones que queramos permitir.

2. **Tomando como base de trabajo el servidor Apache2**
	- **Configure una Autoridad Certificadora en su equipo**
		1. `cd /usr/lib/ssl/misc`
		2. `./CA.pl -newca`
			Aquí se generan dos archivos en `demoCA`:
			- `private/cakey.pem` -> clave privada de la CA
			- `cacert.pem` -> certificado autofirmado de la CA, clave pública.
		3. Mandamos el certificado a nuestro compañero.
			`cp demoCA/cacert.pem /home/lsi/carlotaCert.crt`
			`scp /home/lsi/carlotaCert.crt lsi@10.11.49.50:/home/lsi`

	- **Cree su propio certificado para ser firmado por la Autoridad Certificadora. Bueno, y fírmelo.**
		Mi compañero genera una solicitud del certificado.
		1. `cd /usr/lib/ssl/misc`
		2. `./CA.pl -newreq-nodes`
			Se generan dos archivos:
			- `newreq.pem` -> certificado solicitado y clave pública
			- `newkey.pem` -> clave privada
		3. `scp /usr/lib/ssl/misc/newreq.pem lsi@10.11.49.52:/home/lsi`

		Ahora firmo yo la solicitud que me ha enviado.
		1. `mv newreq.pem /usr/lib/ssl/misc`
		2. `.CA.pl -sign` -> genera `newcert.pem` que es el certificado firmado
		3. `scp /usr/lib/ssl/misc/newcert.pem lsi@10.11.49.50:/home/lsi`

		*Archivos finales* 
		1. `newcert.pem` → certificado firmado
		2. `newkey.pem` → clave privada del servidor
		3. `newreq.pem` → contiene el certificado solicitado y la clave pública
		4. `cacert.pem` → clave pública de la CA
		5. `private/cakey.pem` → clave privada de la CA

	- **Configure su Apache para que únicamente proporcione acceso a un determinado directorio del árbol web bajo la condición del uso de SSL. Considere que si su la clave privada está cifrada en el proceso de arranque su máquina le solicitará la correspondiente frase de paso, pudiendo dejarla inalcanzable para su sesión ssh de trabajo**
		Mi compañero configura el apache.
		1. `a2enmod ssl`
		2. Mueve al directorio correspondiente los certificados.
			`cp newcert.pem /etc/ssl/certs/newcert.pem`
			`cp newkey.pem /etc/ssl/private/newkey.pem`
			`cp carlotaCert.crt /etc/ssl/certs/carlotaCert.crt`
		3. Editar el archivo de configuración de apache y poner los paths correctos a los certificados.
			```sh
			SSLEngine on
			SSLCertificateFile /etc/ssl/certs/newcert.pem
			SSLCertificateKeyFile /etc/ssl/private/newkey.pem
			SSLCACertificateFile /etc/ssl/certs/carlotaCert.crt
			```
		4. `a2ensite default-ssl`
		5. `systemctl restart apache2`

		**COMPROBACIÓN**
		1. Yo -> `lynx https://10.11.49.50`
		2. `openssl s_client -showcerts -connect 10.11.49.50:443`
			Ver el certificado con el cuño (firma).

	- **Tomando como base de trabajo el openVPN deberá configurar una VPN entre dos equipos virtuales del laboratorio que garanticen la confidencialidad entre sus comunicaciones**
		1. `apt install openvpn`
		2. `lsmod | grep tun`
			Si no muestra nada hay que hacer lo siguiente:
			`modprobe tun`
			`echo tun >> /etc/modules`
		3. `cd /etc/openvpn`
		4. `openvpn --genkey secret clave.key`
			Genera una clave simétrica que usaremos para cifrar y descifrar los datos.
		5. `nano tunel.conf`
			```sh
			local 10.11.49.52 (al reves en el compi)
			remote 10.11.49.50 (al reves en el compi)
			dev tun1
			port 5555
			comp-lzo
			user nobody
			cipher AES-256-CBC
			ping 15
			ifconfig 172.160.0.2 172.160.0.1 (al reves en el compi)
			secret /etc/openvpn/clave.key
			```
		6. `scp ./clave.key lsi@10.11.49.50:/home/lsi`
		7. `openvpn --config /etc/openvpn/tun1.conf`
			Poner ambos este comando para abrir la conexión al túnel.
		8. `reboot`
		9. `ifconfig -A` -> debe salir el túnel levantado.
		10. `ping -c 4 172.160.0.1`

6. **En este punto, cada máquina virtual será servidor y cliente de diversos servicios (NTP, syslog, ssh, web, etc.). Configure un “firewall stateful” de máquina adecuado a la situación actual de su máquina.**
	Un *firewall stateful* es un tipo de firewall que analiza y rastrea el estado de las conexiones de una red. A diferencia del stateless (evalúa cada paquete de manera individual), este mantiene un registro del estado de las conexiones activas, para tomar decisiones precisas sobre permitir o bloquear tráfico. Por ejemplo, en el caso de una respuesta a una solicitud HTTP, el firewall permitirá el tráfico de vuelta si reconoce que es parte de una conexión previamente permitida.

	El hecho de que sea stateful lo vemos en reglas establecidas usando un módulo conntrack, como estas: 
	```sh
	iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT 
	iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	```
	Estas reglas permiten el tráfico que ya pertenece a una conexión existente (ESTABLISHED) o relacionado con una conexión existente (RELATED).

	**Contenido del script**: 
	- IPv4 e IPv6 soportados. 
	- Usa conntrack para manejar conexiones establecidas y relacionadas.
	- Servicios configurados: 
		- Cliente NTP (yo) porque Carlos es cliente y cambian esas líneas para él.
		- Cliente y servidor SSH
		- Acceso a servidores DNS
		- Acceso a HTTP y HTTPS
		- Syslog remoto 
	- ICMP e ICMPv6 permitidos de forma específica (ping) 
	-  Reinicio programado a los 2 mins (evitar bloqueos) 
	- Limpieza adecuada de las reglas para IPv4 e IPv6 
	
	**Explicación**
	1. Limpieza incial: 
		- `iptables -F` e `ip6tables -F` eliminan las reglas ya existentes 
		- `iptables -X` e `ip6tables -X` eliminan las cadenas definidas por el usuario.
	2. Políticas por defecto: 
		- Todo el tráfico se bloquea por defecto (DROP) excepto el relacionado o el configurado explícitamente.
	3. Reglas por servicio: 
		- *SSH*: permite conexiones específicas por IPv4 e IPv6 desde y hacia Ips confiables. Si solo configuras las reglas para INPUT (tráfico entrante), el firewall permitirá que otro sistema inicie conexiones SSH hacia ti, pero tu sistema no podrá iniciar conexiones SSH hacia otro lugar. Las reglas de OUTPUT para SSH permiten que tu sistema establezca nuevas conexiones SSH hacia servidores remotos. 
		- *ICMP e ICMPv6*: Permite pings entre compañeros y redes relacionadas. 
		- *NTP*: Configura la máquina como cliente para IPv4 e IPv6. 
		- *DNS*: Permite el tráfico de salida hacia servidores DNS específicos. 
		- *HTTP/HTTPS*: Permite el tráfico web saliente. 
		- *Rsyslog*: Permite Rsyslog remoto con IPv4 e IPv6. 

	Una vez que tenemos el script, le damos permisos con `chmod +x firewall.sh` y lo ejecutamos con `./firewall.sh`. En otra terminal mientras vamos probando que podamos conectarnos con ping y ssh a las máquinas correspondientes y demás.

	```shell
		#!/bin/sh
	
	    /sbin/iptables -F
	
	    /sbin/iptables -X
	
	    /sbin/iptables -P INPUT DROP
	
	    /sbin/iptables -P OUTPUT DROP
	
	    /sbin/iptables -P FORWARD DROP
	
	    /sbin/ip6tables -F
	
	    /sbin/ip6tables -X
	
	    /sbin/ip6tables -P INPUT DROP
	
	    /sbin/ip6tables -P OUTPUT DROP
	
	    /sbin/ip6tables -P FORWARD DROP
	
	    #localhost
	
	    /sbin/iptables -A INPUT -i lo -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -o lo -j ACCEPT
	
	    /sbin/ip6tables -A INPUT -i lo -j ACCEPT
	
	    /sbin/ip6tables -A OUTPUT -o lo -j ACCEPT
	
	    #client NTP
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p UDP --dport 123 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p UDP --sport 123 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    #server rsyslog
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p TCP --sport 514 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p TCP --dport 514 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    #ssh con el comp
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p TCP --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p TCP --sport 22 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p TCP --sport 22 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p TCP --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    #eduroam
	
	    /sbin/iptables -A INPUT -s 10.20.32.0/21 -d 10.11.49.52 -p TCP --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.20.32.0/21 -p TCP --sport 22 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    #vpn
	
	    /sbin/iptables -A INPUT -s 10.30.8.0/21 -d 10.11.49.52 -p TCP --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.30.8.0/21 -p TCP --sport 22 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.30.8.0/21 -d 10.11.49.52 -p TCP --dport 3000 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.30.8.0/21 -p TCP --sport 3000 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.30.8.0/21 -d 10.11.49.52 -p TCP --dport 9090 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.30.8.0/21 -p TCP --sport 9090 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.30.8.0/21 -d 10.11.49.52 -p TCP --dport 9100 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.30.8.0/21 -p TCP --sport 9100 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.30.8.0/21 -d 10.11.49.52 -p TCP --dport 8000 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.30.8.0/21 -p TCP --sport 8000 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    #ping
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p ICMP -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p ICMP -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p ICMP -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p ICMP -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    #http
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52  -p TCP --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p TCP --sport 80 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p TCP --sport 80 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p TCP --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    #https
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p TCP --dport 443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p TCP --sport 443 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p TCP --sport 443 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p TCP --dport 443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    #openvpn
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p UDP --dport 5555 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p UDP --sport 5555 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.11.49.50 -d 10.11.49.52 -p UDP --sport 5555 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.11.49.50 -p UDP --dport 5555 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 172.160.0.1 -d 172.160.0.2 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 172.160.0.2 -d 172.160.0.1 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 172.160.0.1 -d 172.160.0.2 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 172.160.0.2 -d 172.160.0.1 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    #dns server
	
	    /sbin/iptables -A INPUT -s 10.8.12.49 -d 10.11.49.52 -p UDP --sport 53 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.8.12.49 -p UDP --dport 53 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.8.12.50 -d 10.11.49.52 -p UDP --sport 53 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.8.12.50 -p UDP --dport 53 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.8.12.47 -d 10.11.49.52 -p UDP --sport 53 -m conntrack --ctstate ESTABLISHED,RELATED  -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.8.12.47 -p UDP --dport 53 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.8.12.49 -d 10.11.49.52 -p TCP --sport 53 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.8.12.49 -p TCP --dport 53 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.8.12.50 -d 10.11.49.52 -p TCP --sport 53 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.8.12.50 -p TCP --dport 53 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 10.8.12.47 -d 10.11.49.52 -p TCP --sport 53 -m conntrack --ctstate ESTABLISHED,RELATED  -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 10.8.12.47 -p TCP --dport 53 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    #debian server
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d 151.101.0.0/16 -p TCP --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s 151.101.0.0/16 -d 10.11.49.52 -p TCP --sport 80 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/iptables -A OUTPUT -s 10.11.49.52 -d downloads.metasploit.com -p TCP --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/iptables -A INPUT -s downloads.metasploit.com -d 10.11.49.52 -p TCP --sport 80 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    #ipv6
	
	    /sbin/ip6tables -A INPUT -s 2002:a0b:3132::1 -d 2002:a0b:3134::1 -p TCP --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    /sbin/ip6tables -A OUTPUT -s 2002:a0b:3134::1 -d 2002:a0b:3132::1 -p TCP --sport 22 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/ip6tables -A INPUT -s 2002:a0b:3132::1 -d 2002:a0b:3134::1 -p TCP --sport 22 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
	
	    /sbin/ip6tables -A OUTPUT -s 2002:a0b:3134::1 -d 2002:a0b:3132::1 -p TCP --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
	
	    ####
	
	    sleep 60
	
	    echo "fin"
	
	    /sbin/iptables -F
	
	    /sbin/iptables -X
	
	    /sbin/iptables -P INPUT ACCEPT
	
	    /sbin/iptables -P FORWARD ACCEPT
	
	    /sbin/iptables -P OUTPUT ACCEPT
	```

7. **Ejecute la utilidad de auditoría de seguridad lynis en su sistema y trate de identificar las acciones de securización detectadas así como los consejos sobre las que se deberían contemplar.**
	### Resumen de Observaciones Clave y Recomendaciones

	1. ***Arranque y Servicios***:
	    - *Protección del cargador de arranque GRUB*:
	        - No se ha configurado una contraseña en GRUB. Esto es crítico porque sin esta protección, se puede acceder al sistema en modo de usuario único sin contraseña.  
	            **Acción recomendada**: Configurar una contraseña en GRUB (`BOOT-5122`).
	    - *Servicios inseguros*:
	        - Varias unidades de servicio (`systemctl`) se marcaron como **"INSEGURO"**.  
	            **Acción recomendada**: Revisar y endurecer los servicios con `systemd-analyze security`.
	2. ***Usuarios, Grupos y Contraseñas***:
	    - *Contraseñas*:
	        - Falta configurar la complejidad de contraseñas con herramientas como `pam_cracklib` o `pam_passwdqc`.  
	            **Acción recomendada**: Instalar un módulo PAM para mejorar la robustez de las contraseñas.
	        - Las políticas de expiración de contraseñas no están habilitadas.  
	            **Acción recomendada**: Configurar una expiración mínima y máxima en `/etc/login.defs` (`AUTH-9286`).
	    - *Cuentas bloqueadas*:
	        - Se detectaron cuentas bloqueadas.  
	            **Acción recomendada**: Revisar si estas cuentas aún son necesarias y eliminarlas si no lo son.
	3. ***Sistema de Archivos y Permisos***:
	    - *Particiones separadas*:
	        - No hay particiones separadas para `/home`, `/var` o `/tmp`.  
	            **Acción recomendada**: Crear particiones separadas para estos directorios para limitar el impacto de un llenado accidental del sistema (`FILE-6310`).
	    - *Permisos de directorios de inicio*:
	        - Se encontraron directorios de inicio con configuraciones inseguras.  
	            **Acción recomendada**: Revisar permisos y establecer configuraciones estrictas (`0700`).
	4. ***SSH***:
	    - La configuración de SSH necesita endurecimiento:
	        - Reducir los valores de `MaxAuthTries` (de 6 a 3), `ClientAliveCountMax` (de 3 a 2), y `MaxSessions` (de 10 a 2).
	        - Deshabilitar `AllowTcpForwarding` y `TCPKeepAlive` si no son necesarios.  
	            **Acción recomendada**: Ajustar la configuración en `/etc/ssh/sshd_config` (`SSH-7408`).
	5. ***Kernel y Red***:
	    - *Parámetros de sysctl*:
	        - Algunos parámetros críticos de seguridad como `net.ipv4.conf.all.accept_redirects` y `kernel.kptr_restrict` no están configurados según las mejores prácticas.  
	            **Acción recomendada**: Ajustar los valores en `/etc/sysctl.conf`.
	    - *Protocolos no utilizados*:
	        - Protocolos como `dccp`, `sctp`, `rds` y `tipc` están habilitados, pero pueden no ser necesarios.  
	            **Acción recomendada**: Deshabilitarlos en el kernel (`NETW-3200`).
	6. ***Software y Paquetes***:
	    - *Actualización de paquetes*:
	        - Se encontraron paquetes vulnerables.  
	            **Acción recomendada**: Actualizar el sistema con `apt-get upgrade` y considerar herramientas automáticas como `unattended-upgrades`.
	    - *Paquetes no purgados*:
	        - Hay configuraciones residuales de paquetes eliminados.  
	            **Acción recomendada**: Limpiar estas configuraciones con `dpkg --purge`.
	7. ***Apache y Web***:
	    - *Modificaciones en seguridad*:
	        - No se encuentra configurado `TraceEnable Off`.  
	            **Acción recomendada**: Editar `/etc/apache2/conf-enabled/security.conf` para deshabilitar `TraceEnable` (`HTTP-6660`).
	
	### Prioridad en la Acción
	1. Configurar la protección de GRUB y reforzar la configuración de SSH.
	2. Actualizar los paquetes y eliminar configuraciones residuales.
	3. Ajustar configuraciones del kernel y endurecer los servicios críticos.
	4. Implementar políticas de contraseñas robustas y revisar cuentas de usuario.

8. **EN LA PRÁCTICA 2 se obtuvo un perfil de los principales sistemas que conviven en su red, puertos accesibles, fingerprinting, paquetería de red, etc. Seleccione un subconjunto de máquinas del laboratorio de prácticas y la propia red. Elabore el correspondiente informe de análisis de vulnerabilidades. Puede utilizar como apoyo al análisis la herramienta Nessus Essentials (disponible para educación en https://www.tenable.com/tenable-for-education/nessus-essentials bajo registro para obtener un código de activación) para su instalación en la máquina debian de prácticas**
	Generar report de Nessus y ver las vulnerabilidades.
	En mi caso:
	1. ***SMB Signing No Requerido (Medium)***
	    - **Descripción**: Permite ataques de intermediarios al no requerir firma de mensajes SMB.
	    - **Solución**: Configura el sistema para que requiera firma de mensajes SMB:
	        - En Windows: habilita "Microsoft network server: Digitally sign communications (always)" en las políticas de seguridad local.
	        - En Samba: ajusta la configuración con `server signing`.
	2. ***Certificado SSL No Confiable (Medium)***
	    - **Descripción**: El certificado SSL del servidor no es confiable, lo que puede facilitar ataques de intermediarios.
	    - **Solución**: Compra o genera un certificado SSL válido y confiable.
	3.  ***Enumeración de Servicios DCE/RPC (Informativo)***
	    - **Descripción**: Se identificaron varios servicios DCE/RPC que podrían ser utilizados para ataques específicos.
	    - **Solución**: Limita el acceso a estos servicios restringiendo las configuraciones de red.
	4. ***Puertos Abiertos (Informativo)***
	    - **Descripción**: Existen múltiples puertos abiertos que podrían ser vulnerables (ej., 445, 3306, 8834).
	    - **Solución**: Revisa los servicios asociados a los puertos abiertos y desactiva aquellos no esenciales.
	5. ***Información del Sistema Expuesta***
	    - **Descripción**: Datos como nombre del host, versión del sistema operativo y dialectos SMB están accesibles.
	    - **Solución**: Asegúrate de que las políticas de red y seguridad limiten la exposición de esta información.

	