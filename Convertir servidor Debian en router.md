![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](http://julio.iespacomolla.es/Recursos-Comunes/Cabecera_CEICE_IESPPM_Transparente.svg)
# Convertir un servidor Debian en un *router*
En este tutorial vamos a ver cómo configurar un servidor Debian con dos interfaces de red para que funcione como encaminador.

El servidor efectuará [NAT](https://es.wikipedia.org/wiki/Traducci%C3%B3n_de_direcciones_de_red) entre el interfaz interno y el externo, para permitir el acceso a Internet de las máquinas conectadas a la red interna, que no dispondrán de otra salida de dicha red.

## Hardware virtual
### El servidor
Nuestro servidor contará con dos interfaces. En este tutorial usaremos los dos primeros interfaces que se configuran por defecto en una máquina virtual, pero el ejemplo se puede trasladar fácilmente a cualquier otra situación práctica. Estas serán las conexiones de red del servidor:

| Interfaz | Dirección IP | Descripción |
| ------ | ------ | ------ |
| enp0s3 | 192.168.2.202/24 | Interfaz externo, con salida a Internet (en VirtualBox, «adaptador puente»). La dirección IP dada como ejemplo correspondería a una máquina virtual instalada en el ordenador del profesor. |
| enp0s8 | 10.0.0.1/24 | Interfaz de la red que se va a enrutar (en VirtualBox, «red interna») |

### El cliente
Usaremos una máquina cliente para comprobar la configuración. Nos servirá cualquier sistema operativo, preferentemente con un entorno gráfico. En este ejemplo usaremos Debian, con el entorno gráfico que prefieras.

| Interfaz | Dirección IP | Descripción |
| ------ | ------ | ------ |
| enp0s3 | 10.0.0.100/24 | Interfaz único, conectado a la red interna (en VirtualBox, «red interna»). |

- La dirección de la puerta de enlace es 10.0.0.1, obviamente.
- El servidor DNS puede ser el servidor que usamos como router, si tiene activo el servicio DNS; o cualquier otro servidor DNS que podamos utilizar. En el aula, por ejemplo, podemos usar 192.168.2.1.

## Activación del reenvío IP *(IP forwarding)*
El primer paso que tenemos que dar es configurar el servidor para que realice esta función, que consiste en que reenvíe paquetes IP de una red a otra.

En el servidor, edita el archivo ```/etc/sysctl.conf```. Asegúrate de configurar las siguientes líneas con el valor '1', para activar el reenvío:

```bash
...
## Configuración de reenvío IPv4
net.ipv4.ip_forward = 1

## Configuración de reenvío IPv6 (optativa; a nosotros no nos hace falta)
net.ipv6.conf.all.forwarding = 1
...
```
Guarda el archivo y ejecuta el siguiente comando para activar los cambios:

```bash
sudo sysctl -p
```

## Configuración de *iptables*
***iptables*** es el servicio de filtrado de paquetes de GNU/Linux. Todos los cortafuegos disponibles para este sistema se basan en algún tipo de gestión de *iptables.* Aquí vamos a usarlo para proporcionar el servicio NAT, de manera que los sistemas conectados a la red interna tengan acceso práctico a la externa y a Internet.

1. Comencemos instalando *iptables* (de hecho el paquete *iptables-persistent,* que hace que las reglas guardadas en ```/etc/iptables/rules.v4``` —y otro archivo equivalente para IPv6— se carguen en cada arranque de la máquina):

    ```bash
    apt install iptables-persistent
    ```
    Durante la instalación se nos hará un par de preguntas sobre si conservar las tablas existentes. Responderemos que «Sí» a ambas.

2. El primer paso de configuración de *iptables* consiste en permitir el reenvío de paquetes IP (que habíamos activado a nivel del *kernel):*

    ```bash
    iptables -A FORWARD -j ACCEPT
    ```

3. Ahora configuraremos *iptables* para que realice la función NAT para la red interna:

    ```bash
    iptables -t nat -s 10.0.0.0/24 -A POSTROUTING -j MASQUERADE
    ```

4. Puesto que las reglas que cambiemos en *iptables* son, por defecto, efímeras, debemos guardarlas. El siguiente comando muestra las reglas activas y las copia a ```/etc/iptables/rules.v4```:

    ```bash
    iptables-save | sudo tee /etc/iptables/rules.v4
    ```

Si todo ha ido bien, el enrutamiento de la red interna debería estar ya activo. Puedes comprobarlo desde la máquina cliente, que debería tener conectividad a la red externa y a Internet.

## Limpiar la configuración de *iptables*

**Si algo va mal, y necesitas «resetear» la configuración de *iptables,*** puedes hacerlo con estos comandos:

```bash
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
iptables -t mangle -F
iptables -t mangle -X
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
```
A continuación puedes rehacer la configuración como necesites.

&nbsp;

&nbsp;

&nbsp;

## Referencias
[Linode](https://www.linode.com/docs/authors/linode/) (2023) [Configure Linux as a Router (IP Forwarding)](https://www.linode.com/docs/guides/linux-router-and-ip-forwarding/). Akamai.



## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2024 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollà](https://iespacomolla.es/), Alacant (Espanya)

