# Gestió d'usuaris i grups locals

GNU/Linux és un sistema multiusuari, és a dir, està pensat perquè diversos usuaris treballin a la vegada, i cadascun disposa del seu propi espai personal d'emmagatzematge per desar els seus arxius.

Per accedir a un sistema, s'ha de tenir un compte d'usuari, és a dir, una parella login/password que l’administrador del sistema ha d’haver donat d’alta.Només l'usuari root, també anomenat superusuari, usuari su, que en Linux és l'administrador del sistema, pot crear, eliminar o administrar els comptes d'usuari.

## **Usuaris per defecte en Linux** 

Els usuaris més importants que inclou Linux per defecte són:

* **root**: administrador del sistema \(per defecte està deshabilitat\).
* **nobody**: usuari anònim o convidat.
* usuari principal: és el que es crea al instal·lar el sistema.
* Té permisos d'administrador \(pot executar la comanda sudo\).

Les dades bàsiques d'un usuari són:

* nom del compte o login.
* identificador \(és un número\).
* grup principal \(és obligatori que cada usuari tingui un grup principal\).
* grups secundaris.
* directori personal: carpeta on l'usuari té control total per crear arxius i carpetes.
* intèrpret de comandes que utilitzarà per defecte \(normalment /bin/bash\).

Els identificadors dels usuaris del sistema solen estar entre el 0 \(identificador de l'usuari root\) i el 999.Els identificadors d'usuaris creats posteriorment comencen a partir del 1000 \(aquest sol ser l'identificador de l'usuari principal\).

{% hint style="info" %}
**Es poden veure els usuaris del sistema amb la comanda `getent passwd.`**  
{% endhint %}

## **Grups d’usuaris locals en Linux** 

Els grups serveixen per facilitar la gestió de permisos dels usuaris.

Assignant permisos a un grup, s'assignen a tots els usuaris que hi pertanyen.

### Grups per defecte

Els grups més importants que inclou Linux per defecte són:

* **root**: grup al que pertany l'usuari root \(l'administrador del sistema\).
* **nogroup**: grup al que pertany l'usuari nobody \(usuari anònim o convidat\).
* **sudo**: grup dels usuaris que poden executar la comanda sudo.
  * L'usuari principal, el que es crea al instal·lar el sistema, pertany a aquest grup.

Cada grup té un identificador \(un número\).

Normalment, els identificadors dels grups del sistema estan entre el 0 \(aquest és l'identificador del grup root\) i el 999.

Els identificadors de grups creats posteriorment comencen a partir del 1000.  


{% hint style="info" %}
**Es poden veure els grups del sistema amb la comanda `getent group`.**  
{% endhint %}

## **Fitxers d’usuaris i grups** 

{% hint style="info" %}
**ATENCIÓ: els següents arxius no s'haurien d'editar mai manualment!**

**Per fer qualsevol modificació s'han d'utilitzar les comandes corresponents.**  
{% endhint %}

### **/etc/passwd**

Conté informació sobre els usuaris. Cada línia és un usuari i cada camp està separat per dos punts \(:\).

Només el root té permís d'escriptura. La resta d'usuaris i grups només el poden llegir.

`usuari:x:1000:2000:usuari local,,,:/home/usuari:/bin/bash`

* usuari és el nom del compte d'usuari \(login\).
* Una x en el segon camp indica que la contrasenya es troba en l'arxiu /etc/shadow.
* El tercer camp \(1000\) és l'identificador d'usuari.
* El quart camp \(2000\) és l'identificador del grup principal.
* Nom complet de l'usuari i altres informacions.
* /home/usuari és el directori personal de l'usuari.
* /bin/bash és l'intèrpret de comandes per defecte.



