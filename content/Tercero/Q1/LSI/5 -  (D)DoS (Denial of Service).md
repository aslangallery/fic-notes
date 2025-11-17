---
Name: 5 - (D)DoS (Denial of Service)
tags:
  - teoría
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

Ataque de interrupción que busca que un servicio deje de funcionar o que su rendimiento sea lo más bajo posible.

**TIPOS DE DoS**
****
1. *Nivel Lógico*
	Producido por una vulnerabilidad que ha sido explotada. Nos podemos proteger con el parche correspondiente de la vulnerabilidad.
2. *Inundación*
	Ataques con muchas conexiones y alta paquetería que tiran abajo un servicio.
	**Factor de amplificación (FA)**: n.º de máquinas que se usan para la inundación.

	Tipos:
	- **Directo**
		IP origen → IP destino
		Se inyectan paquetes para inundar una IP de destino.
		```sh
		packit -b 0 -c 0 -sR -d 10.11.48.100 -F S -S 1000 -D 80 → SYN FLOOD
		```
	- **Reflectivo**
		Se inyectan paquetes con IP origen la máquina que se quiere inundar e IP destino otras máquinas. No son paquetes directos a la máquina que se quiere inundar, se envían paquetes a otras máquinas para que, cuando respondan, se inunde la máquina que se quería inundar.
		```sh
		packit -c 0 -b 0 -s 10.10.102.100 -d R -F S -S 1000 -D 80
		```
		Nos podemos proteger con un firewall con control de estado.

Herramientas de inyección de paquetes:
- **hping3**
- **scapy**
- **packit**

**SYN-FLOOD**
****
Nuestras máquinas tienen un TCB (Transmission Control Block), pila que almacena la información de las conexiones abiertas de nuestra máquina. Si se llega a llenar, no podría aceptar nuevas conexiones, quedaría muerta.

SYN-FLOOD llena las entradas antes de que salten los timeouts (SYN - SYN/ACK y empieza el timeout, si se acaba antes del ACK se cierra la conexión).

***Configuración para defendernos***
- `/proc/sys/net/ipv4/tcp_max_syn_blocklog` → Tamaño de la TCB (128 por defecto). 
- `/proc/sys/net/ipv4/tcp_synack_retries` → Timeout (5s por defecto). Si se pone un timeout muy pequeño, nos podemos hacer un DoS a nosotros mismos.
- **SYN COOKIES** → `/proc/sys/net/ipv4/tcp_syn_cookies` → 1 para activarla. 
	Cuando se llena la TCB, se genera un número de secuencia que codifica o hashea la dirección IP de origen, destino y el puerto. Se utiliza para realizar un handshake y, al recibir ACK, reconstruir la información anterior sin depender de la TCB.

**SYN PROXIES**
****
Todas las conexiones las intercepta el proxy, captando los SYNs, realizando el handshake y luego pasándole la conexión a la IP real. Con esto se evitan los ataques SYN FLOOD.

**SYN CACHE**
****
Usan una estructura de datos independiente a la TCB, con un tamaño limitado y en la que se guarda solo un subconjunto de datos. Si se completa el handshake y se recibe el ACK, los datos se copian a la TCB. 

**UDP-FLOOD**
****
Envío masivo de paquetes a puertos UDP aleatorios. 
- *UDP-flood*: herramienta simple que puede crear y enviar paquetes UDP.
- *Defensa*: filtrado de paquetes.

**RELACIONADOS CON QoS**
****
1. *Traffic shaping*
	Gestión del ancho de banda y la forma en la que se envían los datos en una red. El objetivo es suavizar el flujo de tráfico y evitar picos que puedan afectar negativamente al rendimiento.
2. *Packet shaping*
	Priorización y manipulación de paquetes individuales en función de ciertos criterios. Puede incluir la clasificación y asignación de prioridades a paquetes específicos según sus características.

**DDoS (Distributed Denial of Service)**
****
1. *SMTP (puerto 25)*
	- **Mailbox** → cada usuario tiene todo su correo electrónico en un fichero.
	- **Maildir** → cada mensaje de correo es un fichero.
2. *Reverse Proxy*
	Ayuda frente a ataques DDos. Es como un proxy, pero al revés. 
	Se trata de un servidor intermedio que gestiona el tráfico entre clientes y servidores de origen. A diferencia de un proxy convencional, este recibe solicitudes de clientes y las redirige a servidores específicos.
3. *Stress Tools*
	Herramientas para pruebas de carga.
	- **Apache bench** → hace un DoS y genera un informe.
	- **jMeter**
4. *Módulos de apache para defendernos*
	- **ModSecurity** → WAF.
	- **ModEvasive** → controla el n.º de peticiones por IP.
	- **ModReqTimeout** → establecer tiempos de espera y velocidades de datos mínimas para recibir requests.
	- **ModAntiLoris** → específico contra `slowloris` evitando demasiadas conexiones desde una dirección IP.

**PORT ISOLATION**
****
Aislamiento de puerto en DMZ. Hace que la DMZ no tenga conectividad con otras máquinas, es decir el conmutador no permite que dos de sus puertos se conecten. Así, si nos atacan la DMZ y se hacen con su puerto, seguiremos protegidos ya que no van a poder pivotear a otras máquinas.

**LAND ATTACK**
****
Intenta saturar un sistema al enviar paquetes falsificados con la misma dirección IP de origen y puerto que el destino, llevando al sistema a responder en un bucle infinito y causando una caída del rendimiento.

```sh
hping3 -s 10.11.48.100 -d 10.11.48.100
```

**PROTECCIÓN CONTRA DDoS**
****
1. *White Lists* → prioriza tráfico legal.
2. *Uso de TAPs* → procesado de tráfico.
3. *RTBL (Real Time Blacklist)* → BBDD de reputación de IPs.
4. *RBLmon* → comprueba qué máquinas tienen baja reputación en base a su IP.
5. *SBL (SPAMHAUS Blacklist)* → lista negra de correo electrónico.
6. *XBL (Exploit Blacklist)* → lista de IPs comprometidas por exploit o hijacking.

**SLAAC (Stateless Address Autoconfiguration)**
****
Protocolo en IPv6 para asignar direcciones IP a los dispositivos de manera automática y sin la necesidad de un servidor DHCP.

***FakeRouter6***
Ataque. Una máquina pone SLAAC y manda seguido paquetes router advertisement, enviando información fraudulenta a las máquinas y haciendo que se cambien seguido de link local.

**SLOWLORIS (slow headers)**
****
Abre un montón de conexiones con un servidor web y le pasa lentamente los campos de la cabecera, sin llegar a terminar el envío. Las cabeceras se finalizan con dos líneas en blanco. Con esto, el servidor se viene abajo.

Otros tipos:
- *slow http post* → abre muchas conexiones y envía cabeceras, luego envía los datos muy despacio, haciendo que no salten los timeouts para producir el máximo daño.
- *slow http read* → peticiones legítimas pero se ralentiza el proceso de lectura de las respuestas.

**ATAQUES A REDES WIFI**
****
```sh
airmon-ng start mon0

airdump-ng mon0

airplay-ng -0 0 -a xx:xx:xx:xx → deautenticar a todos de la WIFI en puntos de acceso con esa MAC
```

El protocolo 802.11W protege contra el último de los ataques. Cifra y autentica tramas, evitando ciertos tipos de DoS en WIFI.

**WIPS**
****
- *Cisco Adaptive Wireless IPS* → ofrecida por Cisco
- *OpenWIPS-NG* → de código abierto
