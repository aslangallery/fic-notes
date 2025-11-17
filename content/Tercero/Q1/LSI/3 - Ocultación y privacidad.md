---
Name: 3 - Ocultación y privacidad
tags:
  - teoría
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

**OCULTACIÓN**
****
Ocultar información para proteger de amenazas y ataques.

**PROXYS**
****
Servidor intermediario entre usuario y destino. Un usuario realiza una conexión mediante un proxy y reenvía la solicitud al destino para después enviar la respuesta del servidor de vuelta al usuario.

- *Open proxys*
	Tipos:
	- **Proxy-web**
		Enrutamiento de solicitudes y respuestas HTTP entre cliente y servidor web.
	- **Proxy-socks**
		Protocolo de red que actúa como una puerta de enlace facilitando las conexiones a través de un servidor. Se configura en `/etc/socks.conf`

	Pueden agregar información a las cabeceras de las solicitudes y respuestas.
	- `REMOTE_ADDR`: x.x.x.x (IP de la máquina origen). Si yo utilizo proxy, al pasar por él se sustituye mi IP por la del proxy.
	- `X_FORWARDED_FOR`: lista de direcciones IP que indica las direcciones por las que pasó la solicitud a través de proxys.

	Niveles de anonimato:
	- **Transparentes**
		No ocultan dirección IP, pero mejoran rendimiento de la red mediante almacenamiento en caché.
	- **Ruidosos**
		Dan privacidad, pero incluyen información falsa en las cabeceras HTTP para confundir a los servidores y ocultar la identidad del usuario.
	- **Alta anonimicidad**
		Ofrecen el mayor grado de anonimato, ocultan completamente la dirección IP y no revela que el cliente está usando un proxy.

**HONEYPOTS**
****
Máquinas trampa, se configuran para atraer a atacantes y simular vulnerabilidades. Se hace con el fin de estudiar y recopilar información sobre sus métodos.

**BORRADO DE FICHEROS**
****
Herramientas que permiten borrar ficheros completamente de forma segura:
- `srm`
- `shred`
- `wipe`
- `sfill`: borra el espacio libre de los espacios de almacenamiento.
- `sswap`: borra las particiones de `swap`
- `smem`: borra la memoria RAM.

**FUNCIONES HASH**
****
1. *SALT*
	Valor aleatorio que se utiliza como parte del hashing, garantizando que aunque dos usuarios tengan la misma contraseña, sus valores de hash sean diferentes. 

2. *Password Guessing*
	Ataques contra servicios tipos login-password, probando passwords continuamente. Son mucho más lentos, por lo que se deben crear buenos diccionarios. Los captchas sirven para frenar este tipo de ataques. 
	Herramientas:
	- medusa
		```
		medusa -M ssh -q
		medusa -h 10.11.48.x -u lsi -P fichero.txt ssh -f
		```
	- th-hydra
	- ncrack

	Protección:
	- **IPtables**: reglas hash-limit (limitar cantidad de intentos por minuto).
	- **OSSEC (HIPS)**: sistema de detección de intrusiones a nivel de host. Monitoriza los logs del sistema, archivos de configuración y otras cosas del SO continuamente.
	- **fail2ban**: detecta ataques de password guessing.

**CONEXIONES**
****
*DNS leaks*
	Se filtra la información relacionada con las DNS de un usuario, aunque se use una VPN o proxy para proteger su privacidad.  

**REDES DE ANONIMATO**
****
Redes que permiten a los usuarios comunicarse de forma anónima. Usan cifrado por capas (onion routing). 
Ejemplos:
- freenet
- i2p
- ipfs
- lokinet
- tor
- zeronet: tipo P2P

*Red Tor*
Su objetivo es que la gente pueda usar Internet de forma anónima. Protege al usuario haciendo rebotar sus comunicaciones sobre una red distribuida de relays.
Se compone por:
- *Onion routers*: enrutan el tráfico y proporcionan anonimato ocultando la identidad y la ubicación del usuario. Los nodos ayudan a enrutar el tráfico a través de comunicación TLS. 
	Mínimo se seleccionan 3 nodos TOR y la única IP que se ve en destino es la del **exit node**. Cada nodo tiene una clave pública y privada. Un nodo sólo conoce al anterior y a su sucesor, NO es un cifrado extremo a extremo.
	Cada uno de los nodos descifra la información de un paquete con su clave privada y obtiene la información del siguiente nodo.
- *Onion proxys*: usuarios finales de la red Tor. Se eligen una serie de nodos al establecer la conexión. 

Tipos de webs:
- *Servicios ocultos*: dominio .onion de TOR, hacen referencia a servicios, servidores web, etc.
- *Deep web*: páginas web no indexadas. Páginas dinámicas de un servidor web, páginas que necesitan de autenticación.
- *Dark web*: conjunto de redes de anonimato.

**PORT NOCKING**
****
Consiste en tener un servicio tirado y configurar una secuencia de SYN y ACK en determinados puertos. Esto levanta el puerto al que queremos entrar y configurará, de ser necesario, el IPtables y los wrappers para que dejen pasar a esa máquina. Al terminar hay que cerrar la sesión.

*Configuración del fichero* `/etc/knockd.conf`
```sh
[openssh]
sequence = 7000, 7015, 9001
seq_time = 10
tcpflags = SYN
command = iptables -A INPUT -s %IP% -y ACCEPT sys start ssh

[closessh]
sequence = 6000, 6015, 9018
```

*Conectarse y cerrar la sesión*
```sh
knockd x.x.x.x 7000 7015 9001
ssh x.x.x.x
knockd x.x.x.x 6000, 6015, 9018
```

***PREGUNTA EXAMEN***
****
**¿Cómo saltarnos un firewall?** 
Queremos conectarnos a un servidor de juegos desde nuestra máquina de trabajo, pero el firewall filtra la conexión por el puerto 22 y el puerto 3128.
1. Cambiamos en nuestra máquina de casa que la conexión ssh se hace por el puerto 443 (https), porque el firewall no deja salir paquetería hacia el puerto 22 y así el firewall al pensar que es https no revisa el paquete porque va cifrado. Importante destacar que va cifrado por ser una conexión ssh.
2. Desde la máquina del trabajo nos conectamos por ssh a la máquina de cada, forwardeando nuestro puerto local 3128 de casa con el puerto 3128 de la máquina de juegos.
3. Ahora con la conexión abierta por ssh, desde la máquina del trabajo vemos en nuestro puerto 3128 el puerto del servidor de videojuegos.

**¿Cómo saltarnos un proxy?**
*Corkscrew* permite saltarse proxies HTTP. Tunelizador con SSH a través de un proxy. Ese proxy solo resuelve peticiones HTTP. Para que esto funcione, el servidor proxy tiene que tener el HTTP CONNECT METHOD, que forwardea conexiones TCP.


