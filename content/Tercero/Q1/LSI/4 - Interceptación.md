---
Name: 4 - Interceptación
tags:
  - teoría
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

**SNIFFING**
****
De:
- *Tráfico Seguro*: información cifrada -> HTTPS
- *Tráfico Inseguro*: información no cifrada -> HTTP, FTP

Se puede usar para robo de información, auditorías de seguridad de red, análisis forenses post-mortem, etc. Se hace sobre ficheros `.pcap` (wireshark).

*Port mirroring*
Sirve para poder capturar tráfico que entra y sale de una interfaz sobre un switch. La captura se realiza conectando un sniffer en una interfaz X llamada "mirror".

**VLANS**
****
Máquinas que no tienen que compartir el ancho de banda. Es básicamente una extensión. Podemos tener un conmutador de red (switch) donde cada puerto se puede asignar a una VLAN. Son redes lógicas independientes en una misma red física. 

Permite tener máquinas pertenecientes a una misma subred en ubicaciones diferentes. Antes una misma subred tenía que compartir medio físico, pero ahora se puede tener una máquina aquí y otra en Madrid y que ambas pertenezcan a la misma subred.

**MAN IN THE MIDLE (MiTM)**
****
Ataque en el que un tercero intercepta y altera la comunicación entre dos partes sin su conocimiento. El atacante actúa como un intermediario no autorizado para obtener acceso a información confidencial.

**YERSINIA**
****
Herramienta de ataque en capa 2.

Ataca principalmente a estos protocolos:
1. ***STP (Spanning Tree Protocol)***
	Organiza los backbones de la red para evitar bucles en los paquetes. Habla por tramas **BPDU**. Convierte el grafo del backbone en un árbol para evitar bucles.
	- **Ataque en STP**: hablar STP y convertirte en nodo raíz, con lo cual te llegará todo el tráfico, si lo dropeas estarías haciendo un DoS.
	- **Protección**
		- *Root bridge guard*: donde pueden y donde no pueden ser root bridge.
		- *BPDU Guard*: filtrar puertos por donde no puede haber tramas BPDU.
2. ***DTP (Dynamic Trunking Protocol)***
	Negociación automática de enlaces de troncal entre switches.
3. ***802.1.Q***
	Protocolo de etiquetado que permite la identificación y el manejo de tráfico de VLANs.

VLAN hopping:
- *Switch Spoofing → ataque*
	Es un ataque con YERSINIA. Nos hacemos pasar por un switch, enviando tramas etiquetadas con distintas VLANs. Nos saltamos los firewalls (de capa 3) al pasar el paquete etiquetado y capturamos el tráfico de otras VLANs.

	La protección consistiría en hacer que solo los puertos que nosotros queremos puedan taggear tramas. Lo hacemos desde el conmutador.
- *Double Tagging → ataque*
	Emitir tramas desde una máquina con **doble etiquetado de VLAN**. Se añade un primer etiquetado correspondiente a la VLAN del emisor y se agrega un segundo etiquetado correspondiente a la otra VLAN. El objetivo es eludir la protección contra spoofing en el switch. Cuando la trama llega al switch, la interpreta como una **comunicación legítima** dentro de la VLAN del emisor, la propaga a otros conmutadores y retira la primera etiqueta. Al llegar a otros, es probable que interpreten la segunda y redirijan la trama a otra VLAN.

**ARP SPOOFING**
****
Consiste en enviar mensajes ARP falsos a Ethernet. La finalidad es asociar la dirección MAC del atacante con la dirección IP de otro nodo, como el default gateway. Si no funciona, se puede usar ICMP redirect o MAC flooding.

1. *ICMP Redirect*
	ICMP se usa para informar al host de una red que debe actualizar su tabla de enrutamiento.
	Si las máquinas aceptan los paquetes, consigues que las máquinas te configuren como **default router**. Así, el tráfico que va hacia fuera pasa por ti. Es un **HALF MITM**.

	**Atacar**
	```sh
	ettercap -M icmp mac_router/ip_router// /ip_maq_atacar//
	```
	**Defendernos**
	```sh
	cd /proc/sys/net/ipv4/conf/all/
	.../accept_redirect → aceptar o no redirects
	.../secure_redirects → solo para aceptar de máquians confiables
	.../sed_redirects → dejar o no a mi máquina mandar redirects

	cd /etc/sysctl.conf → 0 (deshabilitar), 1 (habilitar)
	net.ipv4.all.accept_redirect = 
	net.ipv4.all.serve_redirects = 
	net.ipv4.all.send_redirects = 
	net.ipv4.all.accept_source_routing =
	```

2. *MAC Flooding*
	Si a algunos conmutadores se les llena la memoria (inundando vía MACs falsas, llenando sus tablas CAM), pasan a funcionar en modo HUB (medio compartido, todos los paquetes a todos los puertos, interfaz de red en modo promiscuo), tenemos todo el tráfico.

**DHCP SPOOFING**
****
**Ataque con ettercap**
```sh
ettercap -Tq -M dhcp: 10.11.48.10-25,31/255.255.255.254.0// DNS ///
```
Interceptamos la conexión de un cliente DHCP y como servidor nos ponemos como su default router. Se le pasa un rango de IPs, que será el rango que máquinas que interceptemos.
Es un **HALF MITM** porque la máquina nos manda el tráfico, pero a la vuelta el router se lo envía directamente a la máquina afectada.

**Defensa**
Configurar un conmutador como **DHCP Snooping True** para bloquear los paquetes DHCP en los puertos que no sean realmente DHCP.

**PORT STEALING**
****
Los conmutadores tienen de forma dinámica asignada una MAC a cada puerto. El robo de puerto consiste en hacer un flood al puerto destino para que el conmutador asigne nuestra MAC como destino. 

```sh
ettercap -M port:[remote| tree] /x.x.x.x// [/x.x.x.x//]
```

**NDP (Neighbour Discovery Protocol)**
****
Protocolo parejo a ARP para IPv6. Hace uso de paquetes ICMP y manda paquetes a una dirección multicast.

```sh
ettercap -M ndp:[remote, oneway] //fe80:…/ …
```

**DNS SPOOFING**
****
Se falsean las resoluciones DNS. 
En `/etc/sttercap/etter.dns` se pueden añadir cosas como `www.google.es A 10.11.48.25` 
Luego se hace:  
```
ettercap -Tq -i ens33 -P dns_spoof -f repoison_arp -P sslstricp -M /10.11.48.x// /// 
``` 
Para securizar el DNS se hace con DNSSec. Es un protocolo seguro que garantiza  
autenticación, integridad de datos mediante registros dns key... Pero no cifra, nos  
pueden interceptar las peticiones dns. Eso sí, evita el DNS Spoofing. Para cifrarlo se  
puede hacer con DOT (DNS over TLS).  

*Evilgrade*
Herramienta que está pensada para, una vez hecho un ataque de envenenamiento  
DNS, suplantar a los servidores que son comprobados por las víctimas o clientes para buscar actualizaciones y troyanizar la máquina de la víctima mediante una actualización maliciosa.

**COSAS DE ARP**
****
- `arp -a` → tabla arp 
- `arp -d ip` → borra entrada de la tabla 
- `arp -s ip mac` → fija esa ip a esa mac en la tabla 
- `ARPTABLES` → Reglas y filtros a nivel de capa 2 
	`arptables -A input --sourcemac -j drop` 
- `ip link set dev ens33 arp off` → tira arp a nivel de kernel en mi maquina 
- `IPS SNORT` → puede vigilar el tema de ARP Spoofing 
- `nast -c` → barrido red ip-mac si cambia la mac nos avisa. 
- Con ettercap plugin `-P run_flood`: Floodeador de paquetes en la red con ettercap. Le mete una carga bestial al conmutador de red. 
- Protección: `UNICAST FLOODING PROTECTION` -> vigila la tasa de floodeo en los puertos para que no lo puteen, si alguno lo esta floodeando le baja el ritmo.

**PORT SPAN/PORT MIRRORING**
****
Clonar el tráfico de algunos puertos a un puerto a mayores. Puede dar problemas de cuello de botella.
Para evitarlo, existe el *trunk de puertos* (distintos a los de 802.1Q de VLANs). En vez de coger un puerto, coge por ejemplo 2 y los configura como un solo interfaz de red con el doble de ancho de banda. 

*Tipos de bounding/trunk*
- BOND 0 → **ROUND ROBIN** → se reparte paquetería entre las dos.
- BOND 1 → **ACTIVE-BACKUP** → funciona 1 hasta que muere y pasa a funcionar el 2.
- BOND 2 → **BALANCE XOR** → (MAC origen XOR MAC destino) mod n.º interfaces.
- BOND 3 → **BROADCAST** → todos los paquetes salen por todos.
- BOND 4 → **802.3AD** → estándar, proporciona más velocidad y disponibilidad.
- BOND 5 → **balance-tlb** → manda paquetes según la carga de los interfaces.
- BOND 6 → **balance-alb** → igual que arriba pero en recepción combina MAC destino

**PORT SECURITY**
****
- Mecanismo de seguridad a nivel de conmutador.
- El conmutador solo deja registrar una MAC por puerto.
- Los puertos los tendrá en modo *shutdown*. Si detecta una MAC que no es la que tiene registrada, hará un `disable` del puerto y notificará.
- Podemos seleccionar un puerto con Port Security, otro sin él y podemos tener distintas configuraciones en distintos puertos.

**TAP (Test Access Ports)**
****
- Dispositivos hardware de red.
- Se suelen colocar entre el border router y el FW.
- Esnifa el tráfico que entra y sale.
- Cajas negras pasivas. Mirrorean el tráfico a sistemas de análisis de paquetes.

**SORBS (Spam and Open Relay Blocking System)**
****
Servicio de BBDD para bloquear servidores de correo electrónico que pueden usarse como spam.

**SOC (Security Operations Center)**
****
Parte fundamental de la infraestructura principal de seguridad de una organización. Sus funciones principales son:
- Monitoreo continuo.
- Detección de amenazas.
- Respuesta a incidentes.
- Análisis forense.
- Actualizaciones en parches.
- Gestión de amenazas.

Se basa en estos pilares:
- *EDR (Endpoint Detection and Response)*
	Seguridad de los dispositivos finales, máquinas, servidores, etc.
- *SIEM (Sistemas de gestión de eventos)*
	Permiten integrar, procesar y analizar logs.
	Algunos ejemplos son: **SPLUNK**, prelude, CESGA, Logrhytm, etc.

**PLUGINS CON ETTERCAP**
****
- *ettercap*
	Herramienta usada en auditorías de seguridad. Con ella se pueden hacer:
	1. MITM.
	2. Fingerprinting positivo.
	3. Recolección de contraseñas.
	4. Terminar conexiones.
	5. Inyección de caracteres en una conexión.
	6. Filtrado/borrado de paquetes.
- *sslstrip*
	Ciberataque que se intenta hacer con los datos de un usuario cuando accede a una dirección web protegida mediante SSL/TLS.
- *sslstrip2*
	Versión mejorada, más eficiente y efectiva en ataques MITM en HTTPS. Implementa técnicas adicionales para evitar que las víctimas se den cuenta de que sus conexiones seguras se han vuelto inseguras.
- *HSTS (HTTP Strict Transport Security)*
	Protocolo que obliga a que todas las comunicaciones funcionen sobre HTTPS.

**SIDEJACKING**
****
Secuestro de sesión o secuestro de cookies. Uso de credenciales de identificación no autorizadas para secuestrar una sesión web válida de forma remota.

**COOKIES**
****
Strings que almacenan los sitios web en los dispositivos, contienen información sobre las interacciones en el sitio (preferencias, datos de inicio de sesión, información de seguimiento...).

Hay varios tipos:
1. *Supercookie*: difícil de eliminar, puede rastrear la actividad del usuario en línea de manera persistente.
2. *Cookie persistente*: permanece en el dispositivo durante un período más largo, almacenando información a largo plazo.
3. *Cookie zombie*: se regenera automáticamente incluso después de ser eliminada, utilizando otras técnicas para persistir en el dispositivo del usuario.
4. *Uso de HSTS como cookies*: se usan flags de la especificación HSTS para guardar un identificador que le permita al sitio reconocer al usuario desde cualquier web.