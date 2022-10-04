# Gestió de dominis

## Domini

El **domini** és un component fonamental de l'estructura lògica de l'**Active Directory**.&#x20;

Per definició, es tracta d'un **conjunt d'objectes de tipus equip, usuari i altres classes d'objectes que comparteixen una base de dades de directori comú**. Aquests objectes interactuen amb el domini en funció dels seus respectius rols com ara, per exemple, els controladors de domini o simplement els equips membres de l'esmentat domini.

### Estructura organitzativa

Depenent de l'organització empresarial caldria especificar el tipus de rol que tindrà el servidor en el moment de promoure'l.

**Cas 1: Domini en un nou bosc**

Actúa com un únic controlador de domini, principal

**Cas 2: Controlador addicional en domini ja existent**

Actúa com a còpia del controlador principal

**Cas 3: Nou domini dins d'un domini existent.**

{% hint style="warning" %}
IMPRESCINDIBLE

Connectivitat entre els equips.

El servei DNS ha de tenir zona inversa i ser capaç de resoldre consultes inverses cap al servidor que actua com a PDC

En la configuració DNS de la IPv4 afegim com a servidor principal la IP del servidor PDC.


{% endhint %}

#### Les unitats organitzatives (UO)

Les unitats organitzatives (UO - Organizational Unit ) són els objectes contenidors més comunment utilitzats dins d'un domini **Active Directory**.  Encara que l'estructura de dominis i boscos (DIT - Directory Information Tree ) és rígida i complexa, les UO són fàcils de crear, editar, moure i esborrar.

Aquest contenidor pot incloure molts tipus d'objectes tals com a usuaris, contactes, equips, impressores, carpetes compartides i per descomptat també altres unitats organitzatives.

