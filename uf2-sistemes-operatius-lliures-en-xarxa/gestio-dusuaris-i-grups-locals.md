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



