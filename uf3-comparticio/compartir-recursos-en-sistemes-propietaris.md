# Compartir recursos en sistemes propietaris



### Compartir recursos en sistemes propietaris

### Els sistemes d'arxiu FAT i NTFS

El Windows  Server proporciona dos tipus de sistema de format i d’arxius FAT 32 i NTFS 5.0 i posteriors

El sistema d’arxius FAT va aparèixer al 1981 amb l’IBM PC-DOS de dues disqueteres sense disc dur. Entre les seves característiques \(FAT 16\) hi ha la forma d’apuntar-se en una taula d’assignacions el lloc d’inici d’un fitxer, la fragmentació al disc dels arxius i les limitacions que provoca els 16 bits.

El sistema NTFS va néixer amb l’NT al 1993 per resoldre les limitacions de la FAT, per incorporar més fiabilitat i oferir més facilitats de recuperació i d’accés als arxius.

Els dos sistemes poden conviure en un o en més discs durs. Es diu que NTFS és un sistema més segur i fiable per als arxius i directoris d’un disc dur.

Les últimes actualitzacions del Windows incorporen FAT 32, que permeten gestionar discs durs més grans de 2 Gb.

El Windows Server ofereix a partir de  la versió NTFS 5.0 que incorpora entre altres possibilitats la definició de **quota de disc** i **l’encriptació** d’arxius mitjançant clau pública/privada.

#### L’accés als directoris

L’accés i la seguretat dels directoris i arxius d’un equip amb Windows 2008 dins d’una xarxa depenen de quatre factors:

* Capacitats atorgades a grups, per exemple, la capacitat de permetre compartir un recurs o la de formatar un servidor.
* Drets que l’administrador pot estendre, per exemple, el dret d’iniciar una sessió local.
* Comparticions de directoris.
* Permisos concedits als comptes sobre directoris i arxius.

Els dos últims varien més freqüentment i repercuteixen bastant més que els altres dos sobre l’administració de la xarxa.

{% hint style="info" %}
#### Compartir una carpeta o directori

         ---&gt;**EQUIVAL A FER VISIBLE A  LA XARXA UNA CARPETA O DIRECTORI**
{% endhint %}

#### 

#### La compartició i la seguretat

**Compartició**

Un dels objectius d’una xarxa és compartir recursos i serveis, com les carpetes o directoris. Per poder veure o accedir a un subdirectori o a una unitat des de la xarxa, la primera acció és compartir. Les comparticions s’apliquen a les unitats o als directoris i és la primera acció per poder veure’ls i venen acompanyades de drets diferents: lectura, escriptura supressió...

Per defecte, i per raons de seguretat, un SO no comparteix una carpeta, és l’administrador qui decideix i permet accedir a un arxiu o carpeta a través de la xarxa.

En el Windows no es comparteixen els arxius, només les  carpetes.

               ///Esquema accés carpetes i permisos - TODO

**Seguretat a nivell de recurs o de compte**

La seguretat definida a nivell de recurs demana una contrasenya per accés o recurs, per exemple no cal contrasenya per accedir a un directori de només lectura però si per accedir a un de lectura i escriptura.

La seguretat definida a nivell d’usuari/ària permet l’accés a un recurs a partir d’una identificació inicial.

El Windows XP permet definir la seguretat a nivell de recurs o d’usuari/ària sempre que l’opció Permitir a los otros usuarios acceder a mis impresoras y a mis directorios compartidos estigui activa.

El Windows 200 en qualsevol moment un directori es pot compartir i la seguretat es defineix exclusivament el nivell d’usuari/ària.

#### Compartir sobre el Windows Server

**Compartició i restriccions**

**;-\) Primer compartir**

Es tracta de compartir un directori en un servidor Windows.

* Inicieu una sessió amb un compte d’administració i obriu l’Explorador.
* Creeu un subdirectori anomenat per exemple, Musica i seleccioneu-lo. Si ja existeix creeu-ne un altre i seleccioneu-lo.
* Amb el botó dret del ratolí accediu a Compartir. Observeu el quadre de diàleg.

![](https://blogs.msmvps.com/juansa/files/2011/09/compartir01.jpg)

Quadre de diàleg en compartir un subdirectori

* Activeu l'opció Compartir esta carpeta de Uso compartido avanzado

Al quadre de Recurso compartido es pot canviar el nom del recurs compartit. Aquest nom és el que es podrà veure a través de la xarxa.

* Observeu que el nom de la carpeta coincideix amb el que s’ofereix.

Si una carpeta s’ha de veure des de DOS el nom ha de seguir la norma 8 caràcters i tres d’extensió \(8.3\) , si no és així pot tenir fins a 255 caràcters.

* Observeu que es poden restringir el número de connexions simultànies.

També es pot afegir un comentari indicatiu del contingut de la carpeta.

En mode comanda

Per compartir, per exemple el directori C:\DATA, amb el nom DATAComp i incloure un comentari :

 **net share DATAComp=c:\Data /remark:"Del dept. com."**

**Permisos sobre una carpeta o directori**

Els permisos en compartir inicialment es redueixen a tres.

* Feu clic sobre el botó Permisos.

El quadre de diàleg mostra els permisos de compartició aplicats a la carpeta.

* Observeu que el permís per defecte per al grup Todos és Control total.
* Observeu els permisos per a una carpeta compartida.

Leer L'usuari/ària pot llegir els arxius, executar programes i recórrer les subcarpetes.

Cambiar L'usuari/ària disposa del dret de Leer i pot crear, modificar o suprimir carpetes i arxius.

Control total L'usuari/ària disposa del dret de Cambiar i pot modificar els permisos dels recursos compartits.

* Observeu les dues caselles Asignar i Denegar. Serveixen per assignar o rebutjar un permís a un usuari o grup.

;-\) A un usuari que forma part de diversos grups amb permisos diferents i contraris, s’aplica el menys restrictiu, excepte si ha estat denegat mitjançant la corresponent marca. Denegar predomina sobre Asignar.

Aquestes restriccions s’apliquen sobre comptes i decideixen qui pot accedir i de quina forma.

El permís per compartir un directori el poden atorgar els integrants del grup d’administradores i els d’operador de servidor en el Windows 2008 server.

Els accessos s’apliquen sobre tot el directori i serveix per a tot el domini. Per tant un compte que tingui el dret d’Iniciar sesió local concedit inicia una sessió local i pot accedir a tots els directoris FAT del servidor.

* Feu clic sobre el botó Agregar.
* Incorporeu un compte aperez i els grups als quals pertany.
* Sobre el directori doneu el permís de Cambiar a un grup i rebutgeu \(Denegar\) el de Control total a l’altre.
* Accediu amb el compte aperez i observeu les possibilitats d’accedir al directori.

A continuació eliminareu els permisos.

* Torneu a accedir com administrador.
* Seleccioneu el compte i els grups i feu clic sobre el botó Quitar

Una mateixa carpeta pot disposar de diversos noms i de diferents drets de compartició.

* Premeu el botó Nuevo recurso compartido.
* Observeu com podeu indicar els permisos desitjats relacionats amb la carpeta.

#### L’accés, la connexió i la desconnexió

**L’accés**

Una vegada compartida la carpeta. aquesta és accessible per la xarxa.

* Feu doble clic sobre la icona de xarxa i després sobre la icona Tota la xarxa.
* Seleccioneu el lloc on hi és el recurs compartit \(domini o grup de treball i nom de màquina\).
* Feu doble clic sobre la icona que representa a l'ordinador,

Ja es pot accedir als recursos compartits si es disposa dels permisos

També es pot accedir a través de la seva ruta UNC:

\\Servidor\RecursCompartit

**Connexió a un directori o unitat des d’una estació**

**Des del Windows 7-10**

Un directori d’un servidor pot aparèixer a l’Explorador d’una estació com si fos una unitat local. Això és útil quan s’utilitza des de la xarxa de forma continuada.

* Accediu a una estació amb Windows 7-10
* Seleccioneu la icona de xarxa i feu clic sobre el botó dret del ratolí.
* Activeu l’opció Assignar a unitat de xarxa.
* A la persiana d’unitat veieu si existeix alguna connexió.
* Indiqueu la unitat T, per exemple, i al camí Ruta d’accés indiqueu el nom del servidor seguit amb el nom de la compartició T \(abans heu compartit TREBALL amb el nom T\). Ha de ser una ruta UNC \\\servidor\compartido.

\\\Serv\T

* Activeu la casella de forma que es torni a connectar a l’inici de sessió.
* Comproveu que es veu com la unitat T

**Des del Windows Server**

Inicialment el procés és el mateix llevat de l’assistent que proporciona i les dues opcions:

a\) Es pot connectar al recurs amb els permisos de l'altra compte d'usuari/ària.

* Accediu a una carpeta compartida.
* Premeu sobre Nombre de usuario diferente i indiqueu en el quadre de diàleg un nom i contrasenya d’un compte diferent.

1. L'opció una carpeta Web o lloc FTP permet agregar als favorits de xarxa la connexió a un directori compartit a través d’un servidor web o d’ftp

**Descompartició i desconnexió**

**Deixar de compartir des del servidor**

Desfer la compartició d'una carpeta equival a suprimir la possibilitat d'accedir a ella a través de la xarxa juntament amb tots els permisos que l'afecten.

* Premeu el botó dret del ratolí sobre el nom de la carpeta compartida i seleccioneu l'opció Compartir.
* Seleccioneu el nom de la compartició que desitja suprimir.
* Premeu Quitar recurso compartido i feu clic sobre Acceptar.

Si la carpeta disposa d’un nom de compartició o es vol deixar de compartir:

* Premeu sobre No compartir esta carpeta.

**Desconnexió de l’estació**

Abans de continuar heu de deixar de compartir el directori i desconnectar la unitat.

* Accediu a l’Explorador del servidor, seleccioneu el subdirectori i amb el botó dret del ratolí accediu a l’opció Compartir. Desactiveu aquesta opció.
* Seleccioneu l’Entorn de xarxa de l’estació i amb el botó dret accediu a Desconnectar.

**Directoris amb compartició especial des d’una estació \(veure més endavant\)**

* Des de l’estació accediu com a administrador amb el compte super a Inicio\|Executar i executeu **\\Serv**, observeu com accediu al servidor.
* Observeu que un dels directoris al qual teniu accés és NETLOGON.

Si es pot visualitzar és que està compartit.

* De la mateixa manera accediu al recurs

\\\Serv\C$ i a \\\Serv\ADMIN$

#### Seguretat sobre NTFS

**Compartició i fitxers NTFS**

**Permisos de compartició**

Hi ha diferències entre les possibles restriccions que ofereix el Windows 2000 si és format FAT o NTFS.

Les diferències entre les possibles restriccions que ofereix si és format FAT o NTFS són sobre directoris i arxius. Sobre aquests inclou una segona capa d’opcions de seguretat i s’apliquen els accessos des de la xarxa i locals des del propi servidor

Com ha estat indicat les restriccions d’un directori o arxiu NTFS compartit \(FAT o NTFS\) són sobre la forma del permís de compartició. Els permisos de compartició que s’apliquen sobre comptes són:

Lectura Cambio Control total

**Permisos NTFS**

Les restriccions dels permisos de compartició s'apliquen als comptes que accedeixen a través de la xarxa informàtica. Per als comptes que tenen autorització d’inici local i també per als que ho fan a través de la xarxa existeixen altres possibilitats com són les restriccions aplicables als fitxers NTFS.

Aquest sistema d'arxius permet implementar els atributs de seguretat i d'auditoria en les carpetes i arxius. El sistema NTFS manté una llista actualitzada de control d’accés \(ACL\) per arxiu/carpeta amb el números d'usuari \(SID\) i els seus permisos.

Els permisos que ofereix el Windows 2008 per a directoris i arxius NTFS són combinació de les diferents formes d’accés a ells, és a dir, dels diferents atributs d’una carpeta o d’un arxiu.

**Quins són els permisos NTFS que es poden donar?**

**;-\) Permisos estàndards sobre carpetes**

La llista de permisos està ordenada de menys a més al contrari de com s’ofereixen al quadre de diàleg. És la següent:

* Escribir Permet crear arxius i carpetes i modificar els atributs \(lectura exclusiva, archivo oculto...\). Un compte amb aquest permís també necessita el de leer si vol accedir al directori.
* Leer Permet llegir el contingut de la carpeta i els seus arxius i els seus atributs.
* Listar el contenido de la carpeta: Com l’anterior més el dret de moure's per la carpeta.
* Lectura y ejecución Com els dos anteriors més el dret a moure's per les carpetes per accedir a altres arxius i carpetes i executar si són programes.
* Modificar Permet suprimir la carpeta i totes les accions acordades pel permís Lectura y ejecución.
* Control total Permet canviar els permisos i prendre possessió, i per tant sobre totes les accions permeses per els altres permisos.

Per assignar els permisos NTFS a una carpeta:

* Seleccioneu la carpeta TEMP i cliqueu a sobre amb el botó dret del ratolí.
* Premeu sobre Propiedades i sobre la fitxa Seguridad.
* Observeu la relació dels permisos.
* Cliqueu sobre el botó Agregar. Des d’aquí podeu donar un permís a un compte de grup o d’usuari/ària.

**;-\) Permisos estàndards sobre un arxiu**

Els permisos sobre un arxiu

* Escribir Permet sobre-escriure l'arxiu, canviar els atributs, veure els permisos concedits a arxiu i conèixer al propietari.
* Leer Permet llegir l'arxiu, visualitzar els seus atributs i els permisos associats i saber qui és el propietari.
* Lectura y ejecución Permet les mateixes accions que Leer més executar l’arxiu si és executable.
* Modificar Permet suprimir els arxius, a més de les accions dels anteriors.
* Control total Permet a més de Modificar, prendre possessió de l'arxiu i canviar els permisos.

Com es fa?

* Accediu a través de l’Explorador i seleccioneu un arxiu.
* Feu clic a sobre amb el botó dret del ratolí i premeu sobre Propiedades i sobre la fitxa Seguridad.
* Observeu la relació dels permisos.
* Feu clic sobre el botó Agregar. Des d’aquí podeu donar un permís a un compte de grup o d’usuari/ària.

**Permisos avançats**

Cada permís és una associació d'atributs NTFS que es poden aplicar i establir sobre una carpeta a un compte aïlladament quan els permisos estàndard no s'ajusten a la demanda. Cal vigilar per evitar incórrer en incoherències.

Per personalitzar els permisos heu de:

* Accediu a la carpeta i a les propietats del recurs al qual vol aplicar permisos NTFS.
* Seleccioneu la fitxa Seguridad, i afegiu un compte, \(el que es veurà afectat pel permís\).
* Seleccioneu el compte i feu clic sobre Avanzada.

En aquesta finestra apareix la llista dels atributs NTFS assignats a cada usuari.

* Premeu sobre Ver o modificar.
* Observeu els atributs.

Els dos atributs que falten es poden veure en baixar la persiana del quadre diàleg.

**Aplicacions dels permisos NTFS**

Un compte d’usuari/ària pot formar part de diversos grups. Aquests grups poden tenir permisos diferents. Els permisos són en realitat per a l'usuari una combinació de tots els permisos dels grups. Excepte si hi ha un denegat. Aquest preval sobre el demés.

* Seleccioneu un arxiu i doneu a tots els usuaris el permís de Control total.
* Seleccioneu el compte elalana i activeu la marca a Denegar sobre l’opció Control total.

Observeu com aquest usuari no pot accedir a l’arxiu.

**Qui pot donar permisos?**

Per assignar permisos NTFS a un arxiu o carpeta, cal ser el propietari o l'administrador, o disposar de permisos següents:

Control total, Cambiar permisos Apropiación

Aquest últim converteix a l'usuari en el propietari del document.

#### Personalització

**Herència i apropiació**

**Herència de permisos**

Una carpeta traspassa els seus permisos a les carpetes que conté.

* Creeu una carpeta dins del vostre directori personal i accediu a l’opció Propiedades i a Seguridad observeu els permisos.

El color gris de les caselles indica que els permisos procedeixen d’una carpeta pare.

* Desmarqueu la casella per suprimir l’herència

Un missatge indica les possibilitats

* La primera permetrà conservar l’herència, pero les caselles, marcades amb fons blanc, permetran modificar els permisos: Copiar.
* La segona suprimeix l’herència i conserva els permisos que s’han atribuït a l’arxiu o a la carpeta: Quitar.
* Suprimiu l’herència.
* Comproveu que els arxius i la carpeta han quedat modificats.
* Recupereu l’herència marcant de nou la casella.

**Apropiació d'un arxiu o una carpeta**

Cada arxiu o carpeta situat en un volum NTFS posseeix un propietari que pot modificar els permisos per llegir, escriure.... El propietari és, per defecte, qui crea l'esmentada carpeta o arxiu.

Un usuari pot apropiar-se d'un recurs si compta amb el permís especial Tomar posesión però no pot convertir a un altre usuari en propietari d'aquest.

* Inicieu una sessió amb un compte no d’administrador, creeu una carpeta i seleccioneu-la.
* Premeu amb el botó dret del ratolí, seleccioneu l’opció Propiedades i activeu la fitxa Seguridad
* Feu clic sobre el botó Avanzada i accediu a la fitxa Propietario.
* Observeu qui té dret de prendre possessió.
* Inicieu una nova sessió com administrador.
* Repetiu el procés anterior, seleccioneu el vostre compte i feu clic sobre el botó Aplicar per prendre possessió de la carpeta.

L’administrador pot prendre possessió, però no pot transferir aquesta propietat. Queda un senyal inesborrable que l’administrador ha estat allí.

**Un directori personal per a cada compte: U:\**

**Creació**

La creació d’una carpeta personal per a cada compte d'usuaris/àries i sent els únics amb accés permès a ell és una acció que facilitarà el treball en xarxa.

* Obriu una sessió com administrador i accediu a l'explorador del Windows.
* Si no existeix creeu un directori en una partició NTFS anomenat PERSONAL.
* Compartiu el directori amb el mateix nom.
* Accediu a la consola Usuarios y equipos del Active Directory i trieu un compte d'usuari.
* Feu clic amb el botó dret del ratolí i accediu a Propiedades. Obriu la fitxa del Perfil i a seleccioneu Conectar.
* Indiqueu i seleccioneu en el menú desplegable la lletra U i en la casella A: la ruta UNC. Utilitzeu la variable %username% per crear automàticament el directori i assignar el permís Control total únicament al compte.

\\\Serv\personal\ %username.

* Feu clic sobre el botó Acceptar per tancar les propietats del compte.

La variable %username% és particularment útil per al compte de prova al qual s'assignen propietats i s'utilitza per crear una còpia de tots els usuaris.

**Comprovació**

Després del lligam, la comprovació de la connexió al directori.

* Obriu una sessió amb un compte d'usuari que disposi d'un directori personal.
* Feu doble clic sobre la icona Mi PC i comproveu-ho.

La lletra d'unitat condueix directament al directori personal, no a l'arrel de la carpeta com en Windows NT4.

#### Complement sobre permisos

**Còpia i desplaçament d'arxius i carpetes**

Per copiar o desplaçar un arxiu o partició l'usuari/ària ha de disposar dels permisos adequats. Per desplaçar un arxiu l'usuari ha de disposar del dret Escritura en la carpeta de destí i del permís Modificar en la carpeta origen.

La taula següent mostra el resultat de moure o copiar un arxiu o una carpeta.

|  | Mateixa partició | Diferent partició |
| :--- | :--- | :--- |
| Desplaçar | Conserva els permisos | Permisos del destí |
| Copiar | Permisos del destí | Permisos del destí |

* Creeu dos directoris en dues particions NTFS diferents.
* Creeu un arxiu de text en cada una de les dues carpetes.
* Doneu permisos diferents al mateix usuari en cada carpeta.

Podeu comprovar la validesa de l’esquema anterior.

**Funcionament dels permisos**

* Només poden accedir-hi els comptes que tenen el dret per accedir-hi. Es pot accedir a un directori o arxiu només si el compte de l’usuari/ària té permís o si pertany a un grup amb permís.
* Per defecte, un directori compartit, els subdirectoris i els arxius inclosos tenen els mateixos drets i permisos.
* Es poden assignar a grups i a usuaris.
* Subdirectoris i arxius hereten els permisos del directori pare. Es poden modificar. No hereten les restriccions anomenades permisos de compartiment.
* Els permisos són acumulatius, i la negació d’un anul·la i preval sobre els altres.
* Un arxiu o directori és propietat del compte que el crea, per tant és qui pot donar permisos sobre ell, fins i tot la de permetre prendre possessió.
* El permís d’arxiu preval sobre el del directori.

