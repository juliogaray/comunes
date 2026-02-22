![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)
# Instalación de un Servidor Debian
En este tutorial vamos a ver cómo instalar un servidor Debian en una máquina virtual ejecutándose sobre VirtualBox.
Vamos a instalar la distro Debian (una de las distribuciones más comunes de GNU/Linux, y una que han usado como base muchas otras, incluyendo Ubuntu y Linux Mint).
En este ejemplo vamos a usar la versión 12.2. Veamos cómo descargarla:
## Descargar la ISO
| En casa | En clase |
| ------ | ------ |
| Acudimos a la página de descargas de Debian: https://www.debian.org/distrib/netinst, y nos descargamos la «imagen pequeña», versión amd64. En el momento de elaborar estos apuntes, era la versión 12.2.0 (archivo imagen debian-12.2.0-amd64-netinst.iso). | Abre tu navegador y abre la dirección siguiente: http://172.20.20.5/?dir=ISOs/Linux. <br/>Descarga a tu ordenador la última versión de Debian disponible, en formato DVD (en el momento de crear estos apuntes, [debian-12.2.0-amd64-DVD-1.iso](http://172.20.20.5/ISOs/Linux/debian-12.2.0-amd64-DVD-1.iso)). |

## Hardware virtual
Al crear la máquina, **marca la casilla «Omitir instalación desatendida»**, para evitar que VirtualBox haga toda la instalación marcando las opciones que pone por defecto. Tú quieres configurar tu máquina a tu gusto, ¿no?
Crearemos la máquina con las siguientes características:
| Característica | Valor |
| ------ | ------ |
| Imagen ISO | La ISO Debian que acabas de descargar |
| Tipo | Linux |
| Versión | Debian 64 bit |
| Memoria | la que necesitemos. Un servidor no suele tener grandes requerimientos, porque no vamos a instalar un entorno gráfico. Si tu profesor no te indica otra cosa, usa 2048 MB. |
| Disco duro | lo que necesites para la tarea que vayas a desarrollar. Si no se indica otra cosa, 12 GB son suficientes. Usa **tamaño fijo** (marca la casilla «reservar tamaño completo»). |

**IMPORTANTE:** una vez creada la máquina, modificaremos el último valor importante antes de empezar **en la sección de «Red»** de su configuración:

| Adaptador | Característica | Valor |
| ------ | ------ | ------ |
| Adaptador 1 | «Conectado a:» | **Adaptador puente** |

La conexión a «Adaptador puente» hace que tu máquina virtual esté conectada a Internet al mismo nivel que la máquina anfitriona, como si compartiesen un conmutador de red.
Puede que para ciertas prácticas necesites más adaptadores de red, conectados de otras formas. Si es así, el profesor te lo indicará.

## Instalación de Debian, distro de GNU/Linux
Veamos, paso a paso, el proceso de instalación de Debian. Hemos dividido dicho proceso en las siguientes secciones:
* Instalación básica
* Configuración de red
* Gestión de repositorios
* Actualización
* Opciones de configuración para mayor comodidad

### Instalación básica
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
    | Déjalo en blanco y continúa.| Introduce la siguiente información: ```http://172.20.100.121:3142```|

20. Después de configurar apt, se nos pregunta si deseamos participar en la encuesta sobre uso de paquetes. Seleccionamos «No» y seguimos adelante.
21. **IMPORTANTE:** a continuación se nos ofrece la opción de seleccionar los paquetes que deseamos instalar.  
Usa **LA BARRA ESPACIADORA** para **desmarcar TODO excepto los dos últimos:**

![Selección de paquetes](https://raw.githubusercontent.com/juliogaray/comunes/main/img/Seleccionar_paquetes.png)

22. **Pulsa \<INTRO\> para continuar.** A partir de aquí el programa de instalación instalará más de mil paquetes de software. Esto llevará un buen rato, dependiendo de la máquina y del estado de nuestra conexión a Internet (si es que se usa).
23. Cuando nos pregunte si deseamos instalar el cargador de arranque GRUB en el registro principal de arranque, seleccionaremos «Sí». Y a continuación, cuando nos solicite el dispositivo donde instalarlo, seleccionaremos la segunda opción: «/dev/sda».
24. Una vez completada la instalación, seleccionaremos «Continuar» para finalizar y reiniciar.  
Quizá necesitemos indicarle a VirtualBox que extraiga la imagen ISO de instalación de la unidad de disco virtual, aunque las últimas versiones lo hacen automáticamente.

### Configuración de red
Reinicia el servidor y entra en él con tu nombre de usuario y contraseña.

>**AVISO:** si creaste una contraseña de root en el paso 7 del punto anterior (en lugar de dejarla vacía), es probable que el comando ```sudo``` no esté disponible. En este caso, usa el comando ```su -``` para convertirte temporalmente en superusuario (verás que el indicador del sistema de la línea de comandos acaba con “#” para indicar que estás trabajando como superusuario) y ejecuta los comandos privilegiados que necesites en este modo sin el prefijo «sudo».

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
Vamos a cambiar esa línea, sustituyendo ```dhcp``` con ```static```, y debajo especificaremos los datos de configuración estática del interfaz. La sección tiene que acabar pareciéndose al siguiente ejemplo. **ASEGÚRATE DE USAR EL NOMBRE DE INTERFAZ CORRECTO Y LA DIRECCIÓN IP Y OTROS PARÁMETROS PROPORCIONADOS POR TU PROFESOR**. En este ejemplo usaremos la **dirección IP ficticia** 192.168.2.XXX:
```bash
# The primary network interface
allow-hotplug enp0s3
iface enp0s3 inet static
    address 192.168.2.XXX
    netmask 255.255.255.0
    gateway 192.168.2.1
    dns-nameservers 192.168.2.1 8.8.8.8
```
Escribe los cambios (si estás usando el editor nano, pulsa ```<Ctrl>-O```) y sal del editor (en nano, pulsando ```<Ctrl>-X```).
Tras configurar el interfaz principal de red, necesitarás activarlo con el siguiente comando (asegúrate una vez más de usar el nombre correcto del interfaz, tal como acabas de escribirlo en el archivo interfaces):

```bash
sudo ifup enp0s3
```

Comprueba si existe el archivo ```/etc/resolv.conf```. Este archivo contiene la información que identifica el servidor o servidores DNS que debe usar la máquina. Dependiendo de cómo se haya hecho la instalación, es posible que no exista (porque el paquete *resolvconf,* que toma la información del archivo ```/etc/network/interfaces``` para crear el mencionado archivo ```/etc/resolv.conf``` no esté instalado). También es posible que exista, pero con el contenido equivocado. Créalo con nano si no existe.  
Edítalo con el siguiente contenido (**usa 192.168.2.1 en el instituto**; si estás en tu casa, usa la dirección IP de tu router doméstico):
```bash
nameserver 192.168.2.1
```
### Gestión de repositorios
Vamos a comenzar editando el archivo de fuentes con el siguiente comando:
```bash
sudo nano /etc/apt/sources.list
```
En este archivo vamos a **eliminar la línea que identifica el disco de instalación**, para que no nos pida que lo insertemos cada vez que queramos instalar o actualizar algún paquete. Busca la línea que comienza por ```deb cdrom:``` y **coméntala** (escribe «#» al principio de la misma). Guarda el archivo y cierra el editor.
> **Si todo ha ido bien durante la primera parte de la instalación, puedes saltarte el resto de esta sección; ve directamente a la sección siguiente (Actualización).**
*Sólo en caso de que el archivo de fuentes de Debian no se haya podido configurar bien en el paso 18 de la instalación necesitarás seguir las instrucciones de esta sección.*

En primer lugar, revisa la configuración del servidor caché de paquetes en tu máquina virtual. Edita el archivo ```/etc/apt/apt.conf.d/02proxy```
Debe contener únicamente la siguiente línea:
```bash
Acquire::http::Proxy "http://172.20.100.121:3142";
```
(Si necesitas corregirlo, **sé muy cuidadoso con las mayúsculas, espacios y caracteres especiales; cualquier error hará que no funcione bien**). Guarda el archivo y sal del editor.
Ahora necesitaremos configurar las fuentes de Debian (los repositorios de Internet de donde se descargan los paquetes y actualizaciones de software). Esta información debe registrarse en el siguiente archivo:
```bash
/etc/apt/sources.list
```
* Puedes usar un generador *online* de *sources.list,* como https://debgen.simplylinux.ch/. Si se aplica la configuración correcta, generalmente mostrará el contenido correcto para el archivo *sources.list.*
* O puedes buscar en tu buscador favorito «Cómo configurar sources.list en Debian X”, donde X es la versión de tu Debian. No debería ser muy difícil corregir el contenido del archivo.

**Para ahorrar tiempo, aquí puedes ver los contenidos correctos para algunas versiones de Debian que solemos usar en el instituto:**

##### Contenido de /etc/apt/sources.list para **Debian 11** (nombre en clave *Bullseye)*
```sourceslist
# Repositorios oficiales Bullseye
deb http://ftp.es.debian.org/debian/ bullseye main contrib non-free
# deb-src http://ftp.es.debian.org/debian/ bullseye main contrib non-free
# Actualizaciones de sistema para Bullseye
deb http://ftp.es.debian.org/debian/ bullseye-updates main contrib non-free
# deb-src http://ftp.es.debian.org/debian/ bullseye-updates main contrib non-free
# Actualizaciones de seguridad para Bullseye
deb http://security.debian.org/debian-security bullseye-security main contrib non-free
# deb-src http://security.debian.org/debian-security bullseye-security main contrib non-free
```
##### Contenido de /etc/apt/sources.list para **Debian 12** (nombre en clave *Bookworm)*
```sourceslist
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

Tendrás que editar de nuevo el archivo de fuentes con el siguiente comando:
```bash
sudo nano /etc/apt/sources.list
```
A continuación, añade el contenido adecuado para tu versión de Debian en el archivo. Como siempre, guarda el archivo (```<Ctrl>-O```) y sal del editor (```<Ctrl>-X```).

>**CONSEJO:** Si quieres copiar/pegar contenido de la web en la línea de comandos, no podrás hacerlo desde la ventana de terminal de la máquina virtual. Pero si haces una conexión SSH a la máquina, entonces SÍ podrás pegar texto en ella:
>1. Abre un terminal en tu ordenador anfitrión.
>2. Conéctate a la máquina virtual con el siguiente comando:
> ```ssh tu_cuenta_de_usuario@Dirección_IP_de_la_máquina_virtual```
>3. Te preguntará si hay que confiar en esa dirección, (dile que sí tecleando «yes»), y la contraseña para el usuario elegido. Si todo va bien, estarás conectado a la máquina virtual a través de SSH, y podrás pegar texto en esta ventana sin problemas (edita el archivo /etc/apt/sources.list como se ha indicado y pega el texto en él).

### Actualización
Antes de actualizar la máquina, nos aseguraremos de tener configurado el servidor caché de paquetes que tenemos en el centro (obviamente, sólo si estamos haciendo esta práctica en el centro).
Edita el archivo ```/etc/apt/apt.conf.d/02proxy``` y asegúrate de que contenga únicamente la siguiente línea:
```bash
Acquire::http::Proxy "http://172.20.100.121:3142";
```
**Si estás trabajando en casa, y esta línea existe en ese archivo** (porque hayas creado la máquina en el instituto), **coméntala poniendo una almohadilla (```#```) al principio de la línea.**

Ya puedes guardar y cerrar el archivo.

#### Proceso de actualización
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
#### resolvconf
Ahora puedes instalar el paquete ```resolvconf``` (este paquete es responsable de tomar la información sobre servidores DNS del archivo ```/etc/network/interfaces``` y configurarla en el archivo ```/etc/resolv.conf``` (si tuviste que editar el archivo resolv.conf después de configurar la red, la razón es que este paquete no estaba instalado):
```bash
sudo apt install resolvconf
```
Puede que necesites reiniciar tu máquina virtual tras instalar este paquete.

#### sudo
Si el comando «sudo» no está disponible (esto sucede normalmente si has activado la cuenta **root** en el paso 7 de la instalación básica), puedes instalarlo con el siguiente comando:
```bash
apt install sudo
```
Después de instalarlo, tendrás que darle permiso a tu cuenta para usarlo. Para hacerlo, añade tu cuenta al grupo *sudo:*
* En Debian 9 o versiones anteriores: ```sudo adduser tu_nombre_de_usuario sudo```
* En Debian 10 o versiones posteriores: ```sudo usermod -aG sudo tu_nombre_de_usuario```

Podrás usar «sudo» a partir de la próxima vez que hagas «login» en la máquina. Apaga el equipo virtual:
```bash
sudo shutdown now
```
**Y AHORA TOMA UNA INSTANTÁNEA DEL MISMO desde VirtualBox.**

## Otras configuraciones y aplicaciones
> **Pregunta a tu profesor si debes instalar las aplicaciones de este punto.**

Ahora instalaremos algunas aplicaciones más que nos harán falta o nos podrán servir más adelante:
```bash
sudo apt-get install unzip git ufw net-tools locate
```
Y, para evitar problemas tontos, configuramos el cortafuegos ufw para aceptar conexiones SSH:
```bash
sudo ufw allow ssh
```

### Cambiar el tamaño de la ventana de línea de comandos

**Si el tamaño de la ventana de la línea de comandos es demasiado grande o pequeño**, puedes ajustar el tamaño siguiendo estos pasos:

1. Edita el archivo de configuración de GRUB:

```bash
sudo nano /etc/default/grub
```

2. En este archivo, usa la combinación ```<Ctrl>-W``` para buscar las letras «GFX». Cuando las encuentres, asegúrate de que la línea que las contiene está comentada (debe comenzar por el carácter ```#```); si no es así, pon tú el carácter mencionado al principio de la línea.

3. Añade debajo las dos líneas siguientes:

```bash
GRUB_GFXMODE=1024x768x32
GRUB_GFXPAYLOAD_LINUX=keep
```

La primera línea le indica al sistema que el menú de arranque debe configurarse con un tamaño de 1024x768 píxeles (y 32 bits de color). La segunda línea le indica que mantenga ese tamaño después de haber iniciado el sistema.

4. Actualiza la configuración efectiva de Grub:

```bash
sudo update-grub
```
Este comando crea la nueva configuración de Grub que será efectiva la próxima vez que inicies la máquina.

Reinicia la máquina y compruébalo.


### Instalar tmux para usar «ventanas de texto»
***tmux*** es una especie de «gestor de ventanas en modo texto». Nos permite configurar un terminal añadiendo pestañas, dividiéndolo en paneles, etc.
Este es un ejemplo de lo que permite hacer:

![Captura de pantalla de tmux](https://raw.githubusercontent.com/juliogaray/comunes/main/img/tmux.png)

Existe una aplicación bastante más avanzada que hace lo mismo, incluso con más y mejores opciones, llamada byobu; pero las últimas versiones no funcionan bien, así que de momento no es recomendable usarla.

Para instalar **tmux** y ejecutarlo:

```bash
sudo apt-get install tmux
tmux
```

## Apaga y toma instantánea
Si has instalado alguna de estas aplicaciones adicionales, de nuevo ESTE ES UN BUEN MOMENTO PARA APAGAR LA MÁQUINA:
```bash
sudo shutdown now
```
**Y TOMAR OTRA INSTANTÁNEA DE LA MISMA desde VirtualBox.**
