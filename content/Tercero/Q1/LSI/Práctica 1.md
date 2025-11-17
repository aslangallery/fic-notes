---
Name: Práctica 1
tags:
  - práctica
asignatura: LSI
---
***[[Legislación y Seguridad Informática]]***

*CONFIGURACIÓN BÁSICA*

1. **Configure su máquina virtual de laboratorio con los datos proporcionados por el profesor. Analice los ficheros básicos de configuración (interfaces, hosts, resolv.conf, nsswitch.conf, sources.list, etc.)**
	La configuración de la máquina virtual depende de las IPs proporcionadas. La forma de configurar la máquina es editando el fichero `/etc/network/interfaces`:

	```
	auto lo ens33 ens34
	iface lo inet loopback

	iface ens33 inet static
		address 10.11.49.52
		netmask 255.255.254.0
		broadcast 10.11.49.255
		network 10.11.48.0
		gateway 10.11.48.1

	iface ens34 inet static
		address 10.11.51.52
		netmask 255.255.254.0
		broadcast 10.11.51.255
		network 10.11.50.0
	```

	El archivo de configuración `/etc/hosts` se utiliza para mapear nombres de host a direcciones IP antes de consultar servidores DNS. Resuelve nombres de host localmente en la máquina, sin hacer consultas a servidores DNS externos.
	```
	127.0.0.1 localhost
	10.11.49.52 debian

	# The following lines are desirable for IPv6 capable hosts
	::1 localhost ip6-localhost ip6-loopback
	ff02::1 ip6-allnodes
	ff02::2 ip6-allrouters
	```

	El archivo `/etc/resolv.conf` contiene información sobre la configuración de los servidores DNS que el sistema utilizará para resolver nombres de dominio en direcciones IP. Determina cómo se resuelven las consultas DNS en tu sistema. Además de nameserver, puede incluir otras directivas para configurar opciones adicionales, como búsqueda de dominio. dominio predeterminado...
	```
	domain udc.pri
	search udc.pri
	nameserver 10.8.12.49
	nameserver 10.8.12.50
	nameserver 10.8.12.47
	```

	El archivo `/etc/nsswitch.conf` define el orden y fuentes de búsqueda que se utilizan para resolver tipos de consultas de nombres en el sistema. NSS (Name Service Switch) determina cómo se busca y resuelve la info de nombres como usuarios, hosts, grupos...
	```
	/etc/nsswitch.conf
	Example configuration of GNU Name Service Switch functionality.
	If you have the glibc-doc-reference and info packages installed, try:
	info libc "Name Service Switch" for information about this file.

	passwd:         files systemd
	group:          files systemd
	shadow:         files
	gshadow:        files
	
	hosts:          files mdns4_minimal [NOTFOUND=return] dns myhostname
	networks:       files
	
	protocols:      db files
	services:       db files
	ethers:         db files
	rpc:            db files
	
	netgroup:       nis
	```

	El archivo `/etc/apt/sources.list` es el archivo de configuración clave para el sistema de administración de paquetes APT (Advanced Package Tool). Se especifican los repositorios desde los cuales el sistema puede obtener paquetes y actualizaciones de software. Cada línea corresponde a un repositorio y proporciona la ubicación de los archivos de índice de paquetes para los diferentes componentes del sistema.
	```
	deb http://deb.debian.org/debian/ buster main
	deb-src http://deb.debian.org/debian/ buster main
	
	deb https://deb.debian.org/debian-security buster-security main contrib
	deb-src https://deb.debian.org/debian-security buster-security main contrib
	
	deb http://security.debian.org/debian-security buster/updates main
	deb-src http://security.debian.org/debian-security buster/updates main
	```

2. **¿Qué distro y versión tiene la máquina inicialmente entregada?. Actualice su máquina a la última versión estable disponible.**
	Mostrar kernel de la máquina
	```
	hostnamectl
	uname -r
	```
	
	Paso 1: Verificar versión actual de Debian
	```
	lsb_release -a
	```
	
	Paso 2: Actualizar lista de paquetes.
	```
	sudo apt update
	```
	
	Paso 3: Actualización del sistema.
	```
	sudo apt upgrade
	```
	
	Paso 4: Actualizar Debian 10 a Debian 11.
	```
	nano /etc/apt/sources.list

	deb http://deb.debian.org/debian/ bullseye main
	deb-src http://deb.debian.org/debian/ bullseye main
	```
	
	Paso 5: Actualizar Debian 11 a Debian 12.
	```
	nano /etc/apt/sources.list

	deb http://deb.debian.org/debian/ bookworm main
	deb-src http://deb.debian.org/debian/ bookworm main
	```
	
	Paso 6: Actualización de paquetes y de la máquina y reiniciar para aplicar cambios.
	```
	sudo apt update
	sudo apt dist-upgrade

	sudo reboot
	```

3. **Identifique la secuencia completa de arranque de una máquina basada en la distribución de referencia (desde la pulsación del botón de arranque hasta la pantalla de login). ¿Qué target por defecto tiene su máquina?. ¿Cómo podría cambiar el target de arranque?. ¿Qué targets tiene su sistema y en qué estado se encuentran?. ¿Y los services?. Obtenga la relación de servicios de su sistema y su estado. ¿Qué otro tipo de unidades existen?.**

	Paso 1: Encendido de la máquina.
	- Presionar botón de encendido o secuencia correspondiente.

	Paso 2: BIOS/UEFI.
	- Máquina realiza verificación inicial del hardware y luego inicia el firmware BIOS o UEFI. 

	Paso 3: Carga del gestor de arranque.
	- La BIOS carga el gestor de arranque (GRUB), que muestra un menú de arranque donde se puede seleccionar el sistema operativo o la configuración deseada al iniciar.

	Paso 4: Inicio del Kerner de Linux.
	- El Kernel se carga en la memoria y comienza su inicialización. Es el núcleo del sistema operativo y se encarga de interactuar con el hardware.

	Paso 5: Proceso de inicio del sistema.
	- El Kernel inicia el proceso init o su sucesor systemd (sistema de inicio y administración de servicios que maneja el proceso de arranque).

	Paso 6: Target de inicio por defecto.
	- El target de inicio por defecto define qué servicios y recursos se deben iniciar. El target suele ser multi-user.target o graphical.target. Se puede verificar mediante el comando:
		```
		systemctl get-default
		```
		El target que venía por defecto era el graphical.target, que inicia el sistema con una interfaz gráfica (no nos hace falta), por lo que se cambia a multi-user.target

	Paso 7: Cambiar el target.
	```
	systemctl set-default multi-user.target
	```

	Paso 8: Listar targets de inicio y su estado.
	```
	systemctl list-units -t target
	```

	Paso 9: Listar todos los servicios.
	```
	systemctl list-units -t service
	```

	Además de targets y servicios, systemd también gestiona sockets, timers, dispositivos... Se pueden ver con el siguiente comando:
	```
	systemctl list-units -t help
	```

4. **Determine los tiempos aproximados de botado de su kernel y del userspace. Obtenga la relación de los tiempos de ejecución de los services de su sistema.**
	Tanto los tiempos aproximados de botado como la relación de los tiempos de ejecución de los services, se obtienen con la herramienta `systemd-analyze`. 

	Paso 1: Determinar el tiempo de arranque del kernel y userspace.
	```
	systemd-analyze
	```
	Para un desglose más detallado (árbol), se puede ejecutar:
	```
	systemd-analyze blame
	```

	Paso 2: Obtener relación de los tiempos de ejecución de los servicios.
	```
	systemd-analyze critical-chain
	```
	Lista de servicios críticos para el inicio y sus tiempos de inicio, da una idea de cuales son los que pueden estar causando retrasos en el inicio.
	Los tiempos de inicio pueden variar según la configuración y el hardware del sistema. 

5. **Investigue si alguno de los servicios del sistema falla. Pruebe algunas de las opciones del sistema de registro journald. Obtenga toda la información journald referente al proceso de botado de la máquina. ¿Qué hace el systemd-timesyncd?**
	Paso 1: Verificar el estado de los servicios.
	```
	systemctl --failed
	```
	Muestra una lista de los servicios que han fallado.

	Paso 2: Analizar el registro `journald`.
	- `systemd` utiliza el registro `journald` para almacenar registros del sistema. Se puede usar el comando `journalctl`. Para obtener información sobre el proceso de arranque de la máquina, se ejecuta el siguiente comando:
	```
	journalctl -b
	```
	Muestra los registros desde el último inicio del sistema.

	Paso 3: Investigar `systemd-timesyncd`
	- Servicio que se utiliza para sincronizar la hora del sistema con servidores de tiempo en Internet. Útil para mantener la hora del sistema precisa, lo que es importante para muchas aplicaciones y servicios que dependen de un reloj preciso. Obtener más información sobre el comando:
	```
	systemctl status systemd-timesyncd
	```
	Muestra información sobre el estado actual del servicio y si ha estado funcionando.

6. **Identifique y cambie los principales parámetros de su segundo interface de red (ens34). Configure un segundo interface lógico. Al terminar, déjelo como estaba.**
	Interfaz lógico:
	ej. ifconfig ens34:0 10.11.51.255 netmask 255.255.252.0
	(MUY IMPORTANTE LA MÁSCARA)

7. **¿Qué rutas (routing) están definidas en su sistema?. Incluya una nueva ruta estática a una determinada red.**
	Paso 1: Verificar rutas actuales.
	```
	ip route show
	o
	netstat -nr
	```

	Paso 2: Agregar nueva ruta estática.
	```
	ip route add 10.11.52.0/23 via 10.11.48.1
	```

	Paso 3: Eliminar ruta.
	```
	ip route del 10.11.52.0/23 via 10.11.48.1
	```

8. **En el apartado d) se ha familiarizado con los services que corren en su sistema. ¿Son necesarios todos ellos?. Si identifica servicios no necesarios, proceda adecuadamente. Una limpieza no le vendrá mal a su equipo, tanto desde el punto de vista de la seguridad, como del rendimiento.**
	```
	systemctl list-unit-files --type=service
	```

	Servicios ENABLED:
	1. `accounts-daemon.service`
		Gestiona cuentas de usuario y configuraciones de las cuentas.
	2. `anacron.service`
		Asegura que las tareas programadas se ejecuten de manera confiable, incluso en sistemas que no están siempre encendidos o tienen tiempos de inactividad.
	3. `apparmor.service`
		Limita los programas de acuerdo a un conjunto de reglas que especifican a qué archivos puede acceder un programa determinado.
	4. `avahi-daemon.service`
		En términos generales se ocupa de asignar automáticamente una dirección IP incluso sin presencia de un servidor DHCP, hacer la función de DNS y crear una lista de los servicios a fin de acceder a ellos fácilmente.
		```
		systemctl stop avahi-daemon.service
		systemctl disable avahi-daemon.service
		systemctl mask avahi-daemon.service
		```
	5. `bluetooth.service`
		Se utiliza para disponer de bluetooth en la máquina.
		```
		systemctl stop bluetooth.service
		systemctl disable bluetooth.service
		systemctl mask bluetooth.service
		```
	6. `console-setup.service`
		Configura la disposición del teclado y otras características relacionadas con la consola del sistema.
	7. `cron.service`
		Administra el cron, programa que permite la programación de tareas automatizadas en un sistema. Permite a los usuarios y admins programar la ejecución de comandos, scripts y programas en momentos específicos o intervalos regulares.
	8. `cups-browsed.service`
		Demonio que navega por las transmisiones Bonjour de las impresoras remotas compartidas por CUPs y hace que las impresoras estén disponibles localmente, reemplazando la navegación/emisión por CUPs que se dejó caer en CUPs 1.6.x
		```
		systemctl stop cups-browsed.service
		systemctl disable cups-browsed.service
		systemctl mask cups-browsed.service
		```
	9. `cups.service`
		Servicio para impresión.
		```
		systemctl stop cups.service
		systemctl disable cups.service
		systemctl mask cups.service
		```
	10. `e2scrub_reap.service`
		Realiza tareas de limpieza y mantenimiento en sistemas de archivos ext4, corrección y prevención de errores en el sistema de archivos.
	11. `getty.service`
		Servicio fundamental en sistemas Unix que se encarga de la gestión de terminales virtuales y conexiones seriales, permite a los usuarios iniciar sesión y trabajar en ellas.
	12. `keyboard-setup.service`
		Se asegura de que el teclado está configurado correctamente según la configuración regional y el idioma del sistema.
	13. `ModemManager.service`
		Demonio de gestión de módems de banda ancha móvil.
		```
		systemctl stop ModemManager.service
		systemctl disable ModemManager.service
		systemctl mask ModemManager.service
		```
	14. `NetworkManager.service`
		Hace que la configuración de la red sea lo más sencilla y automática posible. Si se usa DHCP, está destinado a reemplazar las rutas predeterminadas, obtener direcciones IP de un servidor DHCP y cambiar los servidores de nombres cuando lo considere oportuno. No es necesario porque tenemos la configuración en `/etc/network/interfaces`.
		```
		systemctl stop NetworkManager.service
		systemctl disable NetworkManager.service
		systemctl mask NetworkManager.service
		```
	15. `open-vm-tools.service`
		Parte de Open VMware Tools, suite de utilidades diseñada para mejorar la integración y el rendimiento de máquinas virtuales en entornos VMware.
		```
		systemctl stop open-vm-tools.service
		systemctl disable open-vm-tools.service
		systemctl mask open-vm-tools.service
		```
	16. `udisks2.service`
		Demonio responsable de administrar los dispositivos de almacenamiento extraíbles.
		```
		systemctl stop udisks2.service
		systemctl disable udisks2.service
		systemctl mask udisks2.service
		```
	17. `vgauth.service`
		Relacionado con VmWare.
		```
		systemctl stop vgauth.service
		systemctl disable vgauth.service
		systemctl mask vgauth.service
		```
	18. `wpa_supplicant.service`
		Se utiliza para administrar conexiones inalámbricas y autenticación en redes wifi.
		```
		systemctl stop wpa_supplicant.service
		systemctl disable wpa_supplicant.service
		systemctl mask wpa_supplicant.service
		```
	19. `plymouth.service`
		Gestiona la pantalla de arranque del sistema, mostrando gráficos y animaciones.
		```
		systemctl stop plymouth.service
		systemctl disable plymouth.service
		systemctl mask plymouth.service
		```

9. **Diseñe y configure un pequeño “script” y defina la correspondiente unidad de tipo service para que se ejecute en el proceso de botado de su máquina.**
	`nano /usr/local/bin/script.sh`
	```
	#!/bin/bash

	#Obtener el porcentaje de uso de la partición sda1

	USADO=$(df -h | grep sda1 | awk {'print $5'})
	USADO=${USADO/\%/}
	
	#Verificar si el espacio usado es mayor al 80%
	
	if [ $USADO -gt 80 ]; then
	
	   echo "$(date): WARNING! El disco se está llenando. El espacio utilizado = $USADO%" >> /usr/local/bin/disco.txt
		fi
	```

	`nano /etc/systemd/system/script.service`
	```
	[Unit]
	Description=Script que verifica que el tamaño del disco no pase del 80%
	
	[Service]
	ExecStart=/usr/local/bin/scripti.sh
	Type=oneshot
	RemainAfterExit=yes
	StandardOutput=journal
	
	[Install]
	WantedBy=multi-user.target
	```
	- `Type = oneshot`: el servicio se define como "oneshot", se ejecuta una vez y luego se considera terminado.
	- `RemainAfterExit=yes`: aunque el servicio se ejecute una vez, systemd lo considera activo incluso después de que termine la ejecución del script. Se usa para servicios que realizan una acción que tiene un efecto duradero.
	- `StandardOutput=journal`: redirige la salida estándar a `journalctl`

	`nano /etc/systemd/system/script.timer`
	```
	[Unit]
	Description=Ejecutar script.sh cada hora

	[Timer]
	OnCalendar=*-*-* *:00:00
	Persistent=true
	
	[Install]
	WantedBy=timers.target
	```

	```
	systemctl daemon-reload
	systemctl enable script.service
	systemctl enable script.timer
	systemctl start script.timer
	```

10. **Identifique las conexiones de red abiertas a y desde su equipo**
	`netstat -netua` muestra estadísticas y detalles relacionados con las conexiones de red, tablas de enrutamiento, interfaces de red...
	- `-n`: muestra direcciones IP y números de puerto en formato numérico, en lugar de resolverlos a nombres de hosts y servicios.
	- `-e`: incluye detalles como el número de paquetes enviados y recibidos, errores, colisiones...
	-  `-t`: muestra conexiones TCP.
	- `-u`: muestra conexiones UDP.
	- `-a`: muestra todas las conexiones, incluyendo las que están en escucha.
	- `-l`: muestra conexiones de red que están escuchando.
1. **Nuestro sistema es el encargado de gestionar la CPU, memoria, red, etc., como soporte a los datos y procesos. Monitorice en “tiempo real” la información relevante de los procesos del sistema y los recursos consumidos. Monitorice en “tiempo real” las conexiones de su sistema.**
	Mostrar procesos del sistema y los recursos consumidos
	```
	top
	```
	Muestra:
	- PID 
	- user del proceso
	- PR: prioridad del proceso, cuanto más bajo sea, mayor prioridad
	- NI: "nice", prioridad ajustada del proceso
	- VIRT: memoria virtual total usada
	- RES: memoria RAM usada
	- SHR: memoria compartida entre procesos
	- S: estado del proceso
	- %CPU: porcentaje CPU utilizado
	- &MEM: porcentaje memoria utilizada
	- TIME+: tiempo total de CPU consumido
	- COMMAND: nombre del proceso

	Mostrar conexiones del sistema:
	```
	netstat -netuac
	```

12. **Un primer nivel de filtrado de servicios los constituyen los tcp-wrappers. Configure el tcp-wrapper de su sistema (basado en los ficheros hosts.allow y hosts.deny) para permitir conexiones SSH a un determinado conjunto de IPs y denegar al resto. ¿Qué política general de filtrado ha aplicado?. ¿Es lo mismo el tcp-wrapper que un firewall?. Procure en este proceso no perder conectividad con su máquina. No se olvide que trabaja contra ella en remoto por ssh**
	Política aplicada:
	- Denegamos por defecto todas las conexiones en `hosts.deny`
	- Permitimos conexiones SSH solo a determinados rangos de IPs.

	Un TCP-wrapper no es lo mismo que un firewall:
	- **TCP-wrappers**
		Operan a nivel de aplicación, controlando acceso a servicios específicos.
		Filtran conexiones después de que el servicio ha recibido la solicitud de conexión.
	- **Firewall**
		Operan a nivel de red, inspeccionando tráfico a nivel de paquetes y controlando qué tráfico puede entrar o salir del sistema.
		Puede bloquear o permitir conexiones antes de que lleguen al servicio.

13. **Existen múltiples paquetes para la gestión de logs (syslog, syslog-ng, rsyslog). Utilizando el rsyslog pruebe su sistema de log local. Pruebe también el journald.**
	```
	logger "mensaje a enviar"
	tail /var/log/syslog //muestra las ultimas 10 lineas de syslog
	```

	Con `journalctl` se muestran los logs recientes.

14. **Configure IPv6 6to4 y pruebe ping6 y ssh sobre dicho protocolo. ¿Qué hace su tcp-wrapper en las conexiones ssh en IPv6? Modifique su tcp-wapper siguiendo el criterio del apartado h). ¿Necesita IPv6?. ¿Cómo se deshabilita IPv6 en su equipo?**
	`nano /etc/network/interfaces`

	```
	auto 6to4
	iface 6to4 inet6 v4tunnel
		address 2002:a0b:3134::
		netmask 16
		gateway ::10.11.48.1
		endpoint any
		local 10.11.49.52
	```

	Modificar `/etc/sysctl.conf` (0 -> enable, 1 -> disable)
	```
	net.ipv6.conf.all.disable_ipv6=0
	net.ipv6.conf.default.disable_ipv6=0
	net.ipv6.conf.lo.disable_ipv6=0
	```

	Modificar `/etc/hosts.allow`
	```
	#ipv6
	sshd: [::1], [2002:a0b:3134::] :spawn /bin/echo `date` %c connected via %d successfully >> /var/log/allow.log

	#rsyslog
	rsyslogd: 10.11.49.50 :spawn /bin/echo `date` %c connected via %d successfully >> /var/log/allow.log
	```

	Hacer `ping6` para verificar conexión
	```console
	root@debian:/home/lsi# ping6 -c 4 2002:a0b:3134::
	PING 2002:a0b:3134::(2002:a0b:3134::) 56 data bytes
	64 bytes from 2002:a0b:3134::: icmp_seq=1 ttl=64 time=0.093 ms
	64 bytes from 2002:a0b:3134::: icmp_seq=2 ttl=64 time=0.066 ms
	64 bytes from 2002:a0b:3134::: icmp_seq=3 ttl=64 time=0.064 ms
	64 bytes from 2002:a0b:3134::: icmp_seq=4 ttl=64 time=0.061 ms

	--- 2002:a0b:3134:: ping statistics ---
	4 packets transmitted, 4 received, 0% packet loss, time 3071ms
	rtt min/avg/max/mdev = 0.061/0.071/0.093/0.012 ms
	```

	Realizar conexión SSH y comprobar entrada en `/var/log/allow.log`
	```
	ssh lsi@2002:a0b:3134::
	```
	Conectarse normal a la máquina y luego comprobar en `var/log/allow.log` que se ha registrado correctamente la conexión SSH de la siguiente forma:
	```
	lun 30 sep 2024 13:32:08 CEST 2002:a0b:3134:: connected via sshd successfully
	```

	**DESHABILITAR IPV6**
	1. Se comentan las líneas relacionadas con la interfaz 6to4.
	2. Modificar las líneas del fichero `/etc/sysctl.conf` y cambiar los 0 por 1.
	3. Recargar la configuración de `sysctl`: `sysctl -p`.
	4. Verificar que IPv6 se ha deshabilitado: `cat /proc/sys/net/ipv6/conf/all/disable_ipv6`.


**PARTE 2**
****
**IMPORTANTE**
En `rsyslog.conf` mover `template` encima de los ficheros porque sino mezcla los logs como los logs del servidor.

3. **Haga todo tipo de propuestas sobre los siguientes aspectos: ¿Qué problemas de seguridad identifica en los dos apartados anteriores?¿Cómo podría solucionar los problemas identificados?**
	NTP usa UDP, lo que causa un problema por el gran número de vulnerabilidades. Algunos de ellos serían fallos de autenticación, ataques de DDoS, spoofing de IP...
	Rsyslog usa TCP. Tiene falta de control sobre los registros que se pueden enviar, saturando al servidor. Hace envío de contenido no deseado o introducción de registros falsos para ocultar actividades maliciosas.

	Se podrían solucionar los problemas cifrando el tráfico, poniendo parches, restricts (ignore en default), y levantando firewalls (filtra quién puede y quién no puede entrar).
4. **En la plataforma de virtualización corren, entre otros equipos, más de 200 máquinas virtuales para LSI. Como los recursos son limitados, y el disco duro también, identifique todas aquellas acciones que pueda hacer para reducir el espacio de disco ocupado.**
	```
	apt remove --purge 'gnome*'
	apt remove --purge plymouth
	apt remove --purge pulseaudio
	apt remove --purge pipewire
	apt remove --purge wireplumber
	apt remove --purge 'libreoffice*'
	apt remove --purge 'firefox*'
	apt remove --purge 'manpages*'
	apt remove --purge caribou
	apt remove --purge 'cups*'
	apt autoremove
	apt clean
	```
5. **Instale el SIEM splunk en su máquina. Sobre dicha plataforma haga los siguientes puntos**
	- **Genere una query que visualice los logs internos del splunk**
		`index="_internal" earliest=-24h`
	- **Cargue el fichero `/var/log/apache2/access.log` y el journald del sistema y visualícelos.**
		En `access.log` cambiamos en la máquina las IPs privadas por públicas de internet.
		Add Data -> Monitor -> Files & Directories -> Browse (access.log) -> Continously Monitor -> Poner "access combined" y darle a save -> Next -> Next -> Save
		Journal -> Add Data -> Monitor -> System journal -> Poner nombre -> Start searching -> `source="journal://aslan"` 
	- **Obtenga las IPs de los equipos que se han conectado a su servidor web (pruebe a generar algún tipo de gráfico de visualización), así como las IPs que se han conectado un determinado día de un determinado mes.**
		1. Cambiar IPs de access.log (obtenido con apache2 al entrar en 10.11.49.52:80)
		2. Hacer paso 1 del apartado b.
		3. Cambiamos filtro a "all time".
		4. Ponemos query en barra de búsqueda
		```
		source="/var/log/apache2/access.log" host="debian" date_month="october" date_mday="5"
		```
	- **Trate de obtener el país y región origen de las IPs que se han conectado a su servidor web y si posible sus coordenadas geográficas**
		```
		source="/var/log/apache2/access.log" ip="*" | iplocation ip | stats count by Country | geom geo_countries allFeatures=True featureIdField=Country
		```
	![[Pasted image 20241002182536.png]]
	- **Obtenga los hosts origen, sources y sourcestypes.**
		```
		source="/var/log/syslog"
		| stats count by host, source, sourcetype 
		```
	- **¿Cómo podría hacer que splunk haga de servidor de log de su cliente?**
		Ajustes -> Data Inputs -> Conexión TCP -> Propio servicio de logs
**shutdown -H**
