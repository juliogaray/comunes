 ﻿![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)
# Instalación de un Servidor Debian

(v20260128)

![Imagen alegórica de un servidor Debian](https://raw.githubusercontent.com/juliogaray/comunes/main/imágenes/DebianServer_512.webp)

En este tutorial vamos a ver cómo instalar un servidor Debian en una máquina virtual ejecutándose sobre VirtualBox.
Vamos a instalar la distro Debian (una de las distribuciones más comunes de GNU/Linux, y una que han usado como base muchas otras, incluyendo Ubuntu y Linux Mint).
En este ejemplo vamos a usar la versión 12.7. Veamos cómo descargarla:
## 1 Descargar la ISO
| En casa | En clase |
| ------ | ------ |
| Acudimos a la página de descargas de Debian: https://www.debian.org/distrib/netinst, y nos descargamos la «imagen pequeña», versión [amd64](https://es.wikipedia.org/wiki/X86-64). En el momento de elaborar estos apuntes, era la versión 12.9.0 (archivo imagen debian-12.9.0-amd64-netinst.iso). | Abre tu navegador y abre la dirección siguiente: http://172.30.12.180/?dir=ISOs/Linux. <br/>Descarga a tu ordenador la última versión de Debian disponible, en formato DVD (en la última actualización de estos apuntes, [debian-12.7.0-amd64-DVD-1.iso](http://172.30.12.180/ISOs/Linux/debian-12.7.0-amd64-DVD-1.iso)). |

&nbsp;

&nbsp;

&nbsp;

## 2 Hardware virtual

### 2.1 Configuración de la red virtual
Vamos a configurar una red virtual dentro de nuestro VirtualBox para conectar a ella esta máquina y las que creemos en el futuro:

1. En VirtualBox, ve al menú 'Archivo'.
2. Selecciona 'Herramientas' y, a continuación, 'Administrador de red'.
3. En la parte derecha, selecciona la pestaña 'Redes NAT'. Por lo general no habrá ninguna red creada.
4. Pincha en el icono 'Crear'. Por defecto, aparecerá una red llamada ```NatNetwork``` con la dirección 10.0.2.0/24, y el servicio DHCP habilitado.
5. Haz doble clic en la red recién creada, y **cambia el nombre al código del módulo** ('SEGI' para *Seguridad Informática,* 'AWEB' para *Aplicaciones Web,* 'SEGA' para *Seguridad y Alta Disponibilidad,* 'SERR' para *Servicios de Red e Internet,* 'RAL' para *Redes de Área Local,* etcétera).
6. Pincha en el botón 'Aplicar'

![Esquema de red NAT en VirtualBox](https://raw.githubusercontent.com/juliogaray/comunes/main/imágenes/RedNAT.png)

**NOTA:** La puerta de enlace es 10.0.2.1, y VirtualBox ofrece en esa red un servidor DHCP en la dirección 10.0.2.3.

### 2.2 Definición de la máquina virtual

Crea una máquina nueva pinchando en el icono 'Nueva'.

Al crear la máquina, **marca la casilla «Omitir instalación desatendida»**, para evitar que VirtualBox haga toda la instalación marcando las opciones que pone por defecto. Tú quieres configurar tu máquina a tu gusto, ¿no?
Crearemos la máquina con las siguientes características:
| Característica | Valor |
| ------ | ------ |
| Imagen ISO | La ISO Debian que acabas de descargar |
| Tipo | Linux |
| Versión | Debian 64 bit |
| Memoria | la que necesitemos. Un servidor no suele tener grandes requerimientos, porque no vamos a instalar un entorno gráfico. Si tu profesor no te indica otra cosa, usa 2048 MB. |
| Disco duro | lo que necesites para la tarea que vayas a desarrollar. Si no se indica otra cosa, 12 GB son suficientes. Usa **tamaño fijo** (marca la casilla «reservar tamaño completo»). |

**IMPORTANTE:** vamos a conectar nuestra máquina virtual recién creada a la *Red NAT* que definimos en la sección anterior. Abre el **apartado de «Red»** de la configuración de la máquina, y modifica el adaptador de red de esta manera:

| Adaptador | Característica | Valor |
| ------ | ------ | ------ |
| Adaptador 1 | «Conectado a:» | **Red NAT** |

Asegúrate de seleccionar la Red NAT adecuada, con el nombre del módulo (SEGI, SERR, AWEB, etcétera).

![Esquema de red de la máquina virtual](https://raw.githubusercontent.com/juliogaray/comunes/main/imágenes/RedNATYVM.png)

&nbsp;

&nbsp;

&nbsp;

## 3 Instalación de Debian, distro de GNU/Linux
Veamos, paso a paso, el proceso de instalación de Debian. Hemos dividido dicho proceso en las siguientes secciones:
* Instalación básica
* Configuración de red
* Gestión de repositorios
* Actualización
* Opciones de configuración para mayor comodidad

### 3.1 Instalación básica
Iniciamos la máquina. Lo primero que nos aparece es el menú Grub:
1. Seleccionamos la opción «Install».
2. En la pantalla de lenguajes, seleccionamos «Spanish – Español» (o el idioma que prefieras).
3. En **Ubicación,** seleccionamos «España».
4. En **Teclado,** «Español».
5. En **Nombre de la máquina,** el nombre que prefieras (no es relevante).
6. En **Nombre de dominio,** puedes dejarlo vacío.
7. En **Clave de superusuario**, déjala en blanco (y repite cuando te la pida de nuevo). Al hacerlo así, se deshabilita la cuenta **root** y se usa la contraseña del usuario que creemos a continuación para tareas de administración.
8. En **Introducir nombre completo de usuario**, teclea tu nombre y apellido(s).
9. En **Nombre de usuario para la cuenta**, introduce el nombre de usuario que vas a usar para entrar en el sistema. Yo suelo usar mis iniciales («jgg»).
10. A continuación te pedirá dos veces la contraseña para el nuevo usuario.
11. En **Zona horaria,** seleccionamos «Península».
12. Para el **Particionado de discos**, elige la primera opción: «Guiado – utilizar todo el disco».
13. En **Elija el disco a particionar**, pulsamos <INTRO> (sólo hay una opción, y ya estará seleccionada por defecto).
14. En **Esquema de particionado** elegiremos la primera opción («Todos los ficheros en una partición»).
15. Por último, confirmaremos el particionado («Finalizar el particionado y escribir los cambios en el disco»).
16. En la pantalla de confirmación, seleccionamos «Sí» para confirmar la escritura de los cambios en el disco. Aquí se inicia un proceso de copia de archivos que puede tardar bastante en completarse.
17. En este punto, dependiendo de la versión de Debian, es posible que el sistema te pregunte si deseas escanear otros medios de instalación. Elige «No» y pulsa <INTRO>.
18. Dependiendo del medio de instalación que estés usando, el programa de instalación te puede preguntar si **deseas usar una réplica en red**. Si es así, contesta **«Sí»**.
    1. A continuación te preguntará el **País de la réplica.** Obviamente elegimos «España».
    2. Después te preguntará qué replica deseas usar. Puedes elegir la primera y continuar.
19. En **Información de proxy**...  
**MUY IMPORTANTE:**

    | En casa | En clase |
    |------|------|
    | Déjalo en blanco y continúa.| Teclea ```http://172.30.12.178:3142```|

20. Después de configurar apt, se nos pregunta si deseamos participar en la encuesta sobre uso de paquetes. Seleccionamos «No» y seguimos adelante.
21. **IMPORTANTE:** a continuación se nos ofrece la opción de seleccionar los paquetes que deseamos instalar.  
Usa **LA BARRA ESPACIADORA** para **desmarcar TODO excepto los dos últimos:**

![Selección de paquetes](https://raw.githubusercontent.com/juliogaray/comunes/main/imágenes/Seleccionar_paquetes.png)

22. **Pulsa \<INTRO\> para continuar.** A partir de aquí el programa de instalación instalará más de mil paquetes de software. Esto llevará un buen rato, dependiendo de la máquina y del estado de nuestra conexión a Internet (si es que se usa).
23. Cuando nos pregunte si deseamos instalar el cargador de arranque GRUB en el registro principal de arranque, seleccionaremos «Sí». Y a continuación, cuando nos solicite el dispositivo donde instalarlo, seleccionaremos la segunda opción: «/dev/sda».
24. Una vez completada la instalación, seleccionaremos «Continuar» para finalizar y reiniciar.  
Quizá necesitemos indicarle a VirtualBox que extraiga la imagen ISO de instalación de la unidad de disco virtual, aunque las últimas versiones lo hacen automáticamente.

### 3.2 Configuración de red
Reinicia el servidor y entra en él con tu nombre de usuario y contraseña.

>**AVISO:** si creaste una contraseña de root en el paso 7 del punto anterior (en lugar de dejarla vacía), es probable que el comando ```sudo``` no esté disponible (en la sección 6 encontrarás instrucciones para instalarlo). En este caso, usa el comando ```su -``` para convertirte temporalmente en superusuario (verás que el indicador del sistema de la línea de comandos acaba con “#” para indicar que estás trabajando como superusuario) y ejecuta los comandos privilegiados que necesites en este modo sin el prefijo «sudo».

Vamos a cambiar la configuración de red. En el instituto no nos interesa usar direcciones IP dinámicas (con DHCP), así que configuraremos el interfaz de red de manera estática.
Edita el archivo ```/etc/network/interfaces``` con tu editor preferido (p.ej. nano):

```bash
sudo nano /etc/network/interfaces
```
En este archivo encontrarás una sección para el interfaz *loopback* (```lo```), y otra para el principal interfaz
de red, el Ethernet, que probablemente se llame ```enp0s3```, aunque puede ser algún otro identificador).
El interfaz principal, si existe, estará configurado dinámicamente, con DHCP. Habrá unas líneas parecidas a estas (recuerda que podría ser «eth0» o algún otro identificador en lugar de «enp0s3»):
```bash
# The primary network interface
allow-hotplug enp0s3
iface enp0s3 inet dhcp
```
Vamos a cambiar esa línea, **sustituyendo** ```dhcp``` **con** ```static```, y debajo especificaremos los datos de configuración estática del interfaz. La sección tiene que acabar pareciéndose al siguiente ejemplo. **Ojo: USA EL NOMBRE DE INTERFAZ CORRECTO.** Si el profesor no te indica otra cosa, **USA LOS DATOS DEL EJEMPLO (teniendo en cuenta que 'ЖЖ' es tu número de PC en clase)**:
```bash
# The primary network interface
allow-hotplug enp0s3
iface enp0s3 inet static
    address 10.0.2.ЖЖ
    netmask 255.255.255.0
    gateway 10.0.2.1
    dns-nameservers 10.239.3.7 10.239.3.8
```
Escribe los cambios (si estás usando el editor nano, pulsa ```<Ctrl>-O```) y sal del editor (en nano, pulsando ```<Ctrl>-X```).
Tras configurar el interfaz principal de red, necesitarás activarlo con el siguiente comando (asegúrate una vez más de usar el nombre correcto del interfaz, tal como acabas de escribirlo en el archivo interfaces):

```bash
sudo ifup enp0s3
```

Comprueba si existe el archivo ```/etc/resolv.conf```. Este archivo contiene la información que identifica el servidor o servidores DNS que debe usar la máquina. Dependiendo de cómo se haya hecho la instalación, es posible que no exista (porque el paquete *resolvconf,* que toma la información del archivo ```/etc/network/interfaces``` para crear el mencionado archivo ```/etc/resolv.conf``` no esté instalado). También es posible que exista, pero con el contenido equivocado. Créalo con *nano* si no existe.  
Edítalo con el siguiente contenido (**usa 10.239.3.7 en el instituto**; si estás en tu casa, usa la dirección IP de tu router doméstico):
```bash
nameserver 10.239.3.7
```

Si has tenido que cambiar este archivo, revisa la sección de solución de errores, al final de este documento. para instalar el paquete *resolvconf.*

### 3.3 Proxy de paquetes en el IES Poeta Paco Mollá

Dentro del instituto usamos un servidor caché de paquetes basado en [Apt-Cacher-NG](https://es.linux-console.net/?p=17116). La función de este sistema es descargar todos los paquetes que se soliciten desde la red del centro a los repositorios de software de Debian (y otras [distros](https://es.wikipedia.org/wiki/Distribuci%C3%B3n_Linux)), de manera que estén localmente disponibles cuando otros equipos los soliciten. De esta manera se ahorra mucho ancho de banda y se gana mucha velocidad para la descarga de paquetes.

Para poder usar sus servicios, debemos configurarlo en el archivo ```/etc/apt/apt.conf.d/02proxy```. Edítalo (créalo si es necesario) para que contenga únicamente la siguiente línea:
```bash
Acquire::http::Proxy "http://172.30.12.178:3142";
```
**Sé muy cuidadoso con las mayúsculas, espacios y caracteres especiales; cualquier error hará que no funcione bien**). Guarda el archivo y sal del editor.

#### 3.3.1 Desactivar el proxy de paquetes

Puedes necesitar prescindir del proxy en algunos supuestos, como por ejemplo:

- Si estás trabajando en casa
- Si el proxy, por alguna razón, no funciona.

En tal caso debes eliminar (o dejar comentada) la información del proxy. Esto puede ser necesario en dos lugares:

1. Si configuraste un proxy durante la instalación del sistema, en el paso 19, Debian habrá creado un archivo con dicha información: ```/etc/apt/apt.conf```. Para prescindir del proxy, deberás editar este archivo y comentar la línea, poniendo una almohadilla (```#```) al inicio de la misma.
2. Si has creado manualmente el archivo ```/etc/apt/apt.conf.d/02proxy```, puedes hacer lo mismo: comenta la línea que contiene poniendo una almohadilla (```#```) al principio.


### 3.4 Gestión de repositorios
Vamos a comenzar editando el archivo de fuentes con el siguiente comando:
```bash
sudo nano /etc/apt/sources.list
```
En este archivo vamos a **eliminar la línea que identifica el disco de instalación**, para que no nos pida que lo insertemos cada vez que queramos instalar o actualizar algún paquete. Busca la línea que comienza por ```deb cdrom:``` y **coméntala** (escribe «#» al principio de la misma). Guarda el archivo y cierra el editor.
> **Si todo ha ido bien durante la primera parte de la instalación, puedes saltarte el resto de esta sección; ve directamente a la sección siguiente (Actualización).**
*Sólo en caso de que el archivo de fuentes de Debian no se haya podido configurar bien en el paso 18 de la instalación necesitarás seguir las instrucciones de esta sección.*

Configura las fuentes de Debian (los repositorios de Internet de donde se descargan los paquetes y actualizaciones de software). Esta información debe registrarse en el siguiente archivo:
```bash
/etc/apt/sources.list
```
* Puedes usar un generador *online* de *sources.list,* como https://debgen.simplylinux.ch/. Si se aplica la configuración correcta, generalmente mostrará el contenido correcto para el archivo *sources.list.*
* O puedes buscar en tu buscador favorito «Cómo configurar sources.list en Debian X”, donde X es la versión de tu Debian. No debería ser muy difícil corregir el contenido del archivo.

**Para ahorrar tiempo, aquí puedes ver los contenidos correctos para algunas versiones de Debian que solemos usar en el instituto:**

##### Contenido de /etc/apt/sources.list para **Debian 12** (nombre en clave *Bookworm)*
```bash
# Repositorios oficiales Bookworm
deb http://ftp.es.debian.org/debian/ bookworm main contrib non-free
# deb-src http://ftp.es.debian.org/debian/ bookworm main contrib non-free
# Actualizaciones de sistema para Bookworm
deb http://ftp.es.debian.org/debian/ bookworm-updates main contrib non-free
# deb-src http://ftp.es.debian.org/debian/ bookworm-updates main contrib non-free
# Actualizaciones de seguridad para Bookworm
deb http://security.debian.org/debian-security bookworm-security main contrib non-free
# deb-src http://security.debian.org/debian-security bookworm-security main contrib non-free
```

##### Contenido de /etc/apt/sources.list para **Debian 13** (nombre en clave *Trixie)*
```bash
# Repositorios oficiales Trixie
deb http://ftp.es.debian.org/debian/ trixie main non-free contrib non-free-firmware
# deb-src http://ftp.es.debian.org/debian/ trixie main non-free contrib non-free-firmware
# Actualizaciones de sistema para Trixie
deb http://ftp.es.debian.org/debian/ trixie-updates main non-free contrib non-free-firmware
# deb-src http://ftp.es.debian.org/debian/ trixie-updates main non-free contrib non-free-firmware
# Actualizaciones de seguridad para Trixie
deb http://security.debian.org/debian-security trixie-security main non-free contrib non-free-firmware
# deb-src http://security.debian.org/debian-security trixie-security main non-free contrib non-free-firmware
```

Tendrás que editar de nuevo el archivo de fuentes con el siguiente comando:
```bash
sudo nano /etc/apt/sources.list
```
A continuación, añade el contenido adecuado para tu versión de Debian en el archivo. Como siempre, guarda el archivo (```<Ctrl>-O```) y sal del editor (```<Ctrl>-X```).

>**CONSEJO:** Si quieres copiar/pegar contenido de la web en la línea de comandos, no podrás hacerlo desde la ventana de terminal de la máquina virtual. Pero si haces una conexión SSH a la máquina, entonces SÍ podrás pegar texto en ella. Revisa la sección de solución de errores, al final de este documento, para aprender cómo hacerlo.

### 3.5 Actualización

Actualiza tu máquina con los siguientes comandos:
```bash
sudo apt update
...
sudo apt upgrade
```
Además vamos a instalar las **utilidades gnupg**, que nos pueden hacer falta más adelante para instalar repositorios adicionales. Hazlo con el siguiente comando:
```bash
sudo apt install gnupg
```

Apaga el equipo virtual:
```bash
sudo shutdown now
```
**Y AHORA TOMA UNA INSTANTÁNEA DEL MISMO desde VirtualBox.**

&nbsp;

&nbsp;

&nbsp;

## 4 Otras configuraciones y aplicaciones

### 4.1 Útiles para la línea de comandos

Vamos a instalar algunas aplicaciones que nos pueden facilitar la vida en el uso de la línea de comandos:

- **unzip** (paquete para descomprimir [archivos zip](https://es.wikipedia.org/wiki/Formato_de_compresi%C3%B3n_ZIP))
- **net-tools** ([herramientas de red](http://www.escomposlinux.org/lfs-es/lfs-es-5.0/appendixa/net-tools.html), útiles para resolución de problemas de conectividad)
- **locate** ([herramienta de búsqueda](https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/) de archivos rápida y cómoda)
- **tldr** ("too long; didn't read": [herramienta de ayuda más sencilla](https://tldr.sh/) de usar que [man](https://www.man7.org/linux/man-pages/manpipx1/man.1.html))
- **pipx** (gestor de aplicaciones de [repositorios Python](https://docs.python.org/es/3/installing/index.html))
- **eg** (herramienta de ayuda de la línea de comandos basada en [ejemplos](https://github.com/srsudar/eg))

```bash
sudo apt-get install unzip net-tools locate pipx
pipx install eg tldr
pipx ensurepath
```

### 4.2 Herramientas avanzadas
> **Pregunta a tu profesor si debes instalar las aplicaciones de este punto.**

Instalar estas aplicaciones es optativo. Puedes hacerlo si te apetece, pero no es imprescindible si el profesor no te lo indica:

- **git** (herramienta para la gestión de repositorios [Git](https://es.wikipedia.org/wiki/Git))
- **ufw** (cortafuegos basado en [IPTables](https://es.wikipedia.org/wiki/Iptables))

```bash
sudo apt-get install git ufw
```
Ten en cuenta que ***ufw*** es un cortafuegos, y necesitarás configurarlo cada vez que desees permitir algún tipo de conexión externa a la máquina. Para empezar, lo configuramos para aceptar conexiones SSH:
```bash
sudo ufw allow ssh
```

### 4.4 Instalar tmux para usar «ventanas de texto»
> **Paso optativo. Hazlo si lo deseas**

***tmux*** es una especie de «gestor de ventanas en modo texto». Nos permite configurar un terminal añadiendo pestañas, dividiéndolo en paneles, etc.
Este es un ejemplo de lo que permite hacer:

![Captura de pantalla de tmux](https://raw.githubusercontent.com/juliogaray/comunes/main/imágenes/tmux.png)

Existe una aplicación bastante más avanzada que hace lo mismo, incluso con más y mejores opciones, llamada byobu; pero las últimas versiones no funcionan bien, así que de momento no es recomendable usarla.

Para instalar **tmux** y ejecutarlo:

```bash
sudo apt-get install tmux
tmux
```

&nbsp;

&nbsp;

&nbsp;

## 5 Apaga y toma una instantánea
Si has instalado alguna de estas aplicaciones adicionales, de nuevo ESTE ES UN BUEN MOMENTO PARA APAGAR LA MÁQUINA:
```bash
sudo shutdown now
```
**Y TOMAR OTRA INSTANTÁNEA DE LA MISMA desde VirtualBox.**

&nbsp;

&nbsp;

&nbsp;

## 6 *FAQ* y resolución de problemas

### 6.1 Mi ventana de línea de comandos es demasiado grande (o pequeña). ¿Cómo cambio su tamaño?

**Si el tamaño de la ventana de la línea de comandos es demasiado grande o pequeño**, puedes ajustar el tamaño siguiendo estos pasos:

1. Edita el archivo de configuración de GRUB:

```bash
sudo nano /etc/default/grub
```

2. En este archivo, usa la combinación ```<Ctrl>-W``` para buscar las letras «GFX». Cuando las encuentres, asegúrate de que la línea que las contiene está comentada (debe comenzar por el carácter ```#```); si no es así, pon tú el carácter mencionado al principio de la línea.

3. Añade debajo la siguiente línea:

```bash
GRUB_CMDLINE_LINUX="video=1024x768"
```
Guarda el archivo y sal del editor. Acabamos de cambiar la configuración de GRUB, de manera que añada a todas las entradas del menú de arranque el parámetro "video=1024x768", que serña tomado en cuenta por el kernel durante el arranque para establecer el tamaño de la pantalla. Puedes usar otros tamaños estándar de admitidos por la tarjeta gráfica, como ```640x480```, ```800x600``` o ```1280x720```, entre otros. Probablemente ```1024x768``` sea el más cómodo.

4. Actualiza la configuración efectiva de Grub:

```bash
sudo update-grub
```
Este comando crea la nueva configuración de Grub que será efectiva la próxima vez que inicies la máquina.

Reinicia la máquina y compruébalo.

### 6.2 Problemas de redes (p. ej. si no se pueden descargar actualizaciones)

Si tu máquina tiene problemas para actualizar paquetes, aquí tienes una guía para investigar el problema y resolverlo.

Estas son las cosas que tienes que comprobar:

### 6.2.1 ¿La configuración de red en Virtual Box es correcta?

En la sección 2 de estos apuntes se indica cómo debe estar configurada la red en Virtual Box. Asegúrate de que la configuración de tu Virtual Box es correcta. **Ahora.**

¿Es correcta? ¿Seguro? Míralo otra vez. ¿Todo bien? Vale.

### 6.2.2 ¿La configuración de red en Debian es correcta?

Este es un problema muy común: algún fallo en el contenido del archivo ```/etc/network/interfaces```.

Comprueba el archivo, siguiendo las instrucciones del punto 3.2. Es frecuente equivocarse en la dirección IP, o dejarse la palabra «dhcp» en lugar de poner «static», o comerse alguna letra.

Si hay algún error, corrígelo y, después de salir del editor, ejecuta ```sudo systemctl restart networking```

Si todo va bien, no debería dar ningún error.

### 6.2.3 resolvconf

Si obtienes algún error indicando problemas al **resolver** un dominio, seguramente tu configuración de DNS no es correcta. Comprueba el contenido del archivo ```/etc/resolv.conf```. Sigue para ello las instrucciones al final del punto 3.2.

Si tuviste que editar el archivo resolv.conf después de configurar la red, la razón es que el paquete ```resolvconf``` no estaba instalado. Este paquete es responsable de tomar la información sobre servidores DNS del archivo ```/etc/network/interfaces``` y configurarla en el archivo ```/etc/resolv.conf```
Instálalo con el siguiente comando:
```bash
sudo apt install resolvconf
```
Puede que necesites reiniciar tu máquina virtual tras instalar este paquete.

### 6.2.4 Problemas con los repositorios

Un problema **muy, muy frecuente** es que el archivo con la definición de los repositorios no está bien. Edita el archivo ```/etc/apt/sources.list``` y comprueba su contenido. En el punto 3.4 de estos apuntes tienes la información necesaria para configurar correctamente este archivo.

### 6.2.5 Problemas con la configuración del servidor caché de paquetes

Comprueba que la configuración está como se indica en el punto 3.3.

### 6.2.6 Llama a tu profesor

Si nada de lo que has mirado ha funcionado, llama a tu profesor(a). Pero tu profesor(a) va a seguir estos puntos para investigar el problema. **Si constata que no lo has corregido como es debido por despiste, desidia o vagancia, el castigo puede ser absolutamente terrorífico, y puede que tengas que arrepentirte el resto de tu vida. Que a lo mejor resulta mucho más corta de lo que creías.**

### 6.3 El comando *sudo* no está disponible
Si el comando *sudo* no está disponible (esto sucede normalmente si has activado la cuenta **root** en el paso 7 de la instalación básica), puedes instalarlo con el siguiente comando:
```bash
apt install sudo
```
Después de instalarlo, tendrás que darle permiso a tu cuenta para usarlo. Para hacerlo, añade tu cuenta al grupo *sudo:*
* En Debian 9 o versiones anteriores: ```sudo adduser tu_nombre_de_usuario sudo```
* En Debian 10 o versiones posteriores: ```sudo usermod -aG sudo tu_nombre_de_usuario```

Podrás usar «sudo» a partir de la próxima vez que hagas «login» en la máquina.

### 6.4 ¿Cómo puedo conectarme con SSH a mi máquina en *Red NAT?*

Acceder por SSH a tu máquina virtual en Red NAT no es tan sencillo, porque la máquina está en una red (virtual) diferente que sólo está conectada a tu máquina anfitriona por NAT. Para poder acceder a ella necesitamos usar [*redirección de puertos*](https://es.wikipedia.org/wiki/Redirecci%C3%B3n_de_puertos):

1. En VirtualBox, ve al menú «Archivo» &rarr; «Herramientas» &rarr; «Administrador de red».
2. Selecciona la red NAT que estás usando para esta máquina.
3. En la parte inferior del interfaz de VirtualBox, selecciona la pestaña «Reenvío de puertos».
4. En la sección «IPv4», añade una nueva regla con el icono '<span style="color:green;">**+**</span>'.
5. En la nueva regla, usa estos datos:
    - Nombre: «Acceso SSH»
    - Protocolo: ```TCP```.
    - IP anfitrión: es mejor dejarlo vacío (podrías poner la dirección IP de tu máquina anfitriona, o 127.0.0.1, pero eso te obligaría a usar esa misma dirección en el comando ```ssh``` que verás más abajo. Si lo dejas vacío, puedes usar cualquiera de las dos direcciones o el nombre 'localhost').
    - Puerto anfitrión: escribe la suma de 2000 + ЖЖ (recuerda, el último número de la dirección IP de tu máquina virtual).
    - IP invitado: la dirección IP de tu máquina virtual.
    - Puerto invitado: ```22```, el puerto estándar usado por el protocolo SSH.
6. Haz clic en el botón «Aplicar».

Ya puedes conectarte a tu máquina usando la redirección de puertos. Hazlo con el siguiente comando:

 ```ssh -p 20ЖЖ tu_cuenta_de_usuario@127.0.0.1```

Te preguntará si hay que confiar en esa dirección, (dile que sí tecleando «yes»), y la contraseña para el usuario elegido. Si todo va bien, estarás conectado a la máquina virtual a través de SSH, y podrás pegar texto en esta ventana sin problemas.

**NOTA**: ```20ЖЖ``` representa el número de puerto definido en el paso 5, más arriba (2000 + el último número de la dirección IP de la máquina virtual). Nos vamos a conectar a ese puerto de nuestra máquina anfitriona (127.0.0.1 es la IP localhost, como sabrás), y esa conexión será redirigida al puerto 22 de la máquina virtual. Podrías usar cualquier otro puerto de tu máquina que no esté en uso.

&nbsp;

&nbsp;

&nbsp;

## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2026 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)



