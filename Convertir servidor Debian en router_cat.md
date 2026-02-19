![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)
# Convertir un servidor Debian en un *router*
En aquest tutorial veurem com configurar un servidor Debian amb dues interfícies de xarxa perquè funcione com a *router.*

El servidor efectuarà [NAT](https://ca.wikipedia.org/wiki/Network_Address_Translation) entre la interfície interna i l'externa, per a permetre l'accés a Internet de les màquines connectades a la xarxa interna, que no disposaran d'una altra eixida d'aquesta xarxa.

## Hardware virtual
### El servidor
El nostre servidor comptarà amb dues interfícies. En aquest tutorial usarem les dues primeres interfícies que es configuren per defecte en una màquina virtual, però l'exemple es pot traslladar fàcilment a qualsevol altra situació pràctica. Aquestes seran les connexions de xarxa del servidor:

| Interfície | Direcció IP | Descripció |
| ------ | ------ | ------ |
| enp0s3 | 192.168.2.202/24 | Interfície externa, amb eixida a Internet (en VirtualBox, «adaptador puente»). L'adreça IP donada com a exemple correspondria a una màquina virtual instal·lada en l'ordinador del professor. |
| enp0s8 | 10.0.0.1/24 | Interfície de la xarxa que es va a enrutar (en VirtualBox, «xarxa interna»)|

### El client
Usarem una màquina client per a comprovar la configuració. Ens servirà qualsevol sistema operatiu, preferentment amb un entorn gràfic. En aquest exemple usarem Debian, amb l'entorn gràfic que preferisques.

| Interfície | Direcció IP | Descripció |
| ------ | ------ | ------ |
| enp0s3 | 10.0.0.100/24 | Interfície única, connectada a la xarxa interna (en VirtualBox, «xarxa interna»). |

- La direcció de la porta d'enllaç és 10.0.0.1, òbviament.
- El servidor DNS pot ser el servidor que usem com a encaminador, si té actiu el servei DNS; o qualsevol altre servidor DNS que puguem utilitzar. A l'aula, per exemple, podem usar 192.168.2.1.

## Activació de la reexpedició IP *(IP forwarding)*
El primer pas és configurar el servidor perquè realitze aquesta funció, que consisteix en el fet que reenvie paquets IP d'una xarxa a una altra.

En el servidor, edita l'arxiu ```/etc/sysctl.conf```. Assegura't de configurar les següents línies amb el valor '1', per a activar la reexpedició:

```bash
...
## Configuració de reexpedició IPv4
net.ipv4.ip_forward = 1

## Configuració de reexpedició IPv6 (optativa; a nosaltres no ens fa falta)
net.ipv6.conf.all.forwarding = 1
...
```
Guarda l'arxiu i executa el següent comando per a activar els canvis:

```bash
sudo sysctl -p
```

## Configuració d'*iptables*
***iptables*** és el servei de filtrat de paquets de GNU/Linux. Tots els tallafocs disponibles per a aquest sistema es basen en alguna mena de gestió de *iptables.* Ací ho usarem per a proporcionar el servei NAT, de manera que els sistemes connectats a la xarxa interna tinguen accés pràctic a l'externa i a Internet.

1. Comencem instal·lant *iptables* (de fet el paquet *iptables-persistent,* que fa que les regles guardades en ```/etc/iptables/rules.v4``` —i un altre arxiu equivalent per a IPv6— es carreguen en cada arrancada de la màquina):

    ```bash
    apt install iptables-persistent
    ```
    Durant la instal·lació se'ns farà un parell de preguntes sobre si conservar les taules existents. Respondrem que «Sí» a ambdues.

2. El primer pas de configuració de *iptables* consisteix a permetre la reexpedició de paquets IP (que havíem activat a nivell del *kernel):*

    ```bash
    iptables -A FORWARD -j ACCEPT
    ```

3. Ara configurarem *iptables* perquè realitze la funció NAT per a la xarxa interna:

    ```bash
    iptables -t nat -s 10.0.0.0/24 -A POSTROUTING -j MASQUERADE
    ```

4. Com que les regles que canviem en *iptables* són, per defecte, efímeres, hem de guardar-les. El següent comando mostra les regles actives i les copia a ```/etc/iptables/rules.v4```:

    ```bash
    iptables-save | sudo tee /etc/iptables/rules.v4
    ```

Si tot ha anat bé, l'encaminament de la xarxa interna hauria d'estar ja actiu. Pots comprovar-ho des de la màquina client, que hauria de tindre connectivitat a la xarxa externa i a Internet.

## Netejar la configuració de *iptables*

**Si alguna cosa va malament, i necessites reinicialitzar la configuració de *iptables,*** pots fer-ho amb aquests comandos:

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
A continuació pots refer la configuració com necessites.

&nbsp;

&nbsp;

&nbsp;

## Referències
[Linode](https://www.linode.com/docs/authors/linode/) (2023) [Configure Linux as a Router (IP Forwarding)](https://www.linode.com/docs/guides/linux-router-and-ip-forwarding/). Akamai.



## Colofó
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2024 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollà](https://iespacomolla.es/), Alacant (Espanya)

