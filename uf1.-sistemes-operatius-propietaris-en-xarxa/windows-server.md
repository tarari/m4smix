# Windows Server

El sistema Windows Server, és un sistema operatiu dissenyat exclussivament per a servidors. La base de funcionament d'aquests equips és el concepte de domini.

Un **domini** és un conjunt d’equips \(clients i sevidors\), usuaris i grups d'usuaris que comparteixen una política de seguretat i una base de dades comuna que anomenem directori.

Cada domini té un nom únic.

El **controlador de domini** és una part fonamental dels sistemes operatius  servidors de Microsoft. S’encarrega  d’emmagatzemar les parelles usuari-paraula de pas dels comptes d’usuari que tenen accés al domini de xarxa, gestionant al mateix temps les autoritzacions i directrius que acompanyen l'inici de sessió.

Aquest **controlador** centralitza la funció d’autenticar i autoritzar l’accés al domini.

## Instal·lació Windows Server 

### Planificació de la instal·lació

**Comprovar els requeriments de maquinari**

Cal assegurar-nos que el nostre maquinari compleix els requisits mínims tan del sistema operatiu com de les aplicacions que s'han d'instal·lar.

**Preparar el programari a instal·lar i les dades a configurar**

Cal tenir preparat tot el programari i les dades necessàries: Sistema Operatiu, controladors, claus d'activació, etc.



{% hint style="success" %}
> **Nota:** Si no disposes d'una **llicència** de Windows Server, pots obtenir, de forma gratuita, un versió d'evaluació durant un període de 180 dies a la següent direcció: [https://www.microsoft.com/es-es/evalcenter/evaluate-windows-server-2019](https://www.microsoft.com/es-es/evalcenter/evaluate-windows-server-2019)
{% endhint %}

### **Crear la màquina virtual**

Cal crear una màquina virtual amb les característiques recomanades per instal·lar _**Windows Server 2019 64 bits Standard Edition**_.

* El nom de la màquina pot ser _**M4Win**_
* Es recomana que l'espai del disc es reservi de forma dinàmica.
* Es recomana posar 2 CPU com a mínim.

### **Planificar el disc**

Cal planificar la utilització del disc, decidir les particions necessàries i el sistema de fitxers a utilitzar.

#### Instal·lar el sistema operatiu <a id="instal&#xB7;lar-el-sistema-operatiu"></a>

Primer de tot, en la configuració d'emmagatzematge de la MV → unitat òptica \(CD/DVD\), posar la ISO \(CD d'instal·lació\) de Windows Server 2019, engegar la MV i seguir els passos següents:

**Triar les opcions relacionades amb el païs** Triar l'idioma, el format de la data, hora i moneda, i la disposició de les tecles en el teclat.

**Triar si es vol instal·lar o reparar el sistema** Evidentment, s'ha de triar Instalar.

**Introduir la clau d'activació** , si és avaluació, tindrem 180 dies

**Triar l'opció d'instal·lació**

Windows Server permet instal·lar-se en dues versions diferents:

* **Opció** _**Windows Server 2019 Standard \(Desktop Experience\)**_
  * Instal·lació del Servidor amb **GUI** _**\[Grafic User Interface\]**_
  * S’instal·la la interfície gràfica d'usuari amb totes les eines d'administració del servidor.
* **Opció** _**Windows Server 2019 Standard \(Core\).**_ _\*\*_
  * No s’instal·la la interfície gràfica d’usuari estàndard i el servidor s’administra amb la línia de comandes \(Windows PowerShell\) o bé remotament.
  * És l’opció predeterminada i recomanada per Microsoft.
  * **Avantatges**: requereix menys espai de disc \(uns 4GB menys\), és més eficient i augmenta la seguretat.

**Triar el tipus d’instal·lació**

Una vegada escollit l’opció d’instal·lació, cal que decidiu si fareu una instal·lació des de zero o bé optareu per actualitzar el vostre sistema.

* **Instal·lació des de zero \(personalitzada\):** El sistema operatiu que hi hagi a l’equip se substituirà completament, de manera que les configuracions i les aplicacions es perdran.
* **Actualització:** el sistema operatiu s’instal·la i es fa una migració de les configuracions, els documents i les aplicacions.

> Si durant el procés d’instal·lació en falta alguna informació que necessitem per continuar. En el moment que es demana On es vol instal·lar Windows? es pot accedir a la línia de comandes prement Majúscules i F10.

**Particionar el disc**

> Com a norma general i per millorar la gestió, és preferible crear  una **partició pel sistema operatiu** i una altra per les **dades**.

D'aquesta forma els possibles errors del sistemes operatiu no afectaran tant directament a les dades.

* Amb el botó _**Nuevo**_, crear una partició d'uns 35 GB. 
* Utilitzar l'espai restant per crear una altra partició.
* Seleccionar la partició de 15 GB per instal·lar el sistema i continuar endavant.

**Assignar contrasenya de l'usuari Administrador**

> S'ha de posar una contrasenya que compleixi els requisits de complexitat de Windows \(mínim 8 caràcters, minúscules, majúscules, números i símbols\).

És recomanable que trieu bé aquesta contrasenya i la poseu a tot arreu, tant per Windows com per Linux \(evidentment, en un cas real s'han de posar contrasenyes diferents\).

{% hint style="info" %}
**Recomanació**: Apunteu la contrasenya d'administrador en algun lloc, també a la Descripció de la màquina a VirtualBox
{% endhint %}

#### Post-instal·lació <a id="post-instal&#xB7;laci&#xF3;"></a>

Una vegada s’ha instal·lat el sistema operatiu caldrà proporcionar la informació següent a l’equip:

**Establir zona horària**

Per canviar l'hora del sistema es pot fer fent clic amb el botó secundari sobre la bara de tasques on hi ha la data i hora. I després seleccionar l'opció _**Ajustar data i hora**_.

**Canviar el nom de l’equip**

Durant la instal·lació es genera de forma automàtica un nom pel servidor poc descriptiu i difícil de recordar com _WIN-FGFKQDSH_. És recomenable canviar-lo i posar a la màquina un nom que sigui fàcil de recordar.

Fer clic amb el botó secundari del ratolí sobre la icona d'inici de Windows i seleccionar l'opció **Sistema**. Després anar a _**Cambiar configuración &gt; Cambiar... &gt; Nombre de equipo**_ o bé _**Settings &gt; System &gt; About &gt; Rename PC**_.

**Configurar la xarxa.**

Un servidor ha de tenir una **adreça IP estàtica** ja que els clients l'han de conèixer per poder accedir-hi i utilitzar els seus serveis.

Podeu crear una xarxa NAT, per tal de treballar i simular una xarxa de l'escola. Per exemple una xarxa local 10.0.2.0/24. Seguiu les instruccions del [video](https://youtu.be/mq1k_xZs7es)

L'adreça ha de pertànyer a la xarxa on està connectada la màquina:

* **Adreça IP - assignada per l'administrador de xarxa**
* **Màscara**:   **assignada per l'administrador de xarxa**
* **Porta d'enllaç \(GW\)**:  \(l'adreça del router virtual de la xarxa NAT\)
* **Servidors DNS**:  \(la mateixa porta d'enllaç de VirtualBox pot fer de servidor DNS o bé utilitzeu els de Google, 8.8.8.8 i 8.8.4.4\).

**Instal·lar Guest Additions o VMTools**

En el cas de **màquines virtuals**, pot ser molt útil instal·lar les eines addicionals del gestor de màquines virtuals \(en el cas de VirtualBox, les _**Guest Additions**_ i en VMWare les _**VMTools**_\).

Aquestes eines permeten disposar de més opcions per:

* Configurar la resolució de pantalla per tal que s'adapti a la mida de la finestra.
* realitzar accions de _**copiar i enganxar**_ text, gràfics i arxius entre la màquina real i la virtual.
* Accedir des de la màquina virtual \(guest\) a una carpeta de la màquina real \(host\) per poder passar arxius fàcilment.

**Instal·lació en Virtual Box**

Per instal·lar les _**Guest Additions**_ cal tenir la màquina virtual engegada i, en el menú de la mateixa finestra de la màquina virtual, seleccionar l'opció _**Dispositivos → Insertar imagen del CD de las Guest Additions**_. Això és equivalent a posar el CD d'instal·lació en la màquina virtual.

En la majoria de sistemes amb entorn gràfic, s'obrirà automàticament una finestra per instal·lar el contingut. Si no, cal obrir el CD i executar el programa _**VBoxWindowsAdditions**_.

**Instal·lació en VMWare**

Per instal·lar les _**VMTools**_ cal tenir la màquina virtual engegada i, en el menú de la mateixa finestra de la màquina virtual, seleccionar l'opció _**VM → Install VMWare Tools**_.

En la majoria de sistemes amb entorn gràfic, s'obrirà automàticament una finestra per instal·lar. Si no s'ha iniciat automàticament, cal anar al CD i executar el programa **setup.exe** que hi ha dins la carpeta setup.

**Fer una còpia de seguretat**

En una màquina real, només faltaria realitzar una còpia de seguretat completa i configurar les copies de seguretat periòdiques.

En el cas de les màquines virtuals, es pot fer un **snapshot** \(vigileu les preferències del Hypervisor on es desen les imatges\) o copiar el disc virtual.

Els snapshots es fan i es poden recuperar més ràpidament, però poden fer què la màquina funcioni una mica més lenta, sobre tot si el disc no és SSD. També fan què el disc virtual ocupi més espai.

> **ATENCIÓ**: un cop feta la instal·lació, configuració i comprovacions, feu una còpia del disc virtual de la màquina i guardeu-la bé. Us pot estalviar molta feina si en algun moment se us fa malbé la màquina.

**Instal·lació noves funcionalitats i característiques**

Instal·lar noves funcionalitats o noves característiques \(rols i característiques\):

* **Funcions de servidor \(rols\):** Conjunt de programes que fan una funció específica per diferents usuaris o altres equips d'una xarxa. Un servidor pot realitzar més d'una funció o rol.
  * Servidor de fitxers.
  * Servidor d'aplicacions.
  * Servidor de correu.
  * Terminal server: permet l’administració remota del servidor des d’un altre equip de la xarxa.
  * Control de domini.
  * Servidor DNS: realitza la resolució de noms del domini.
  * Servidor DHCP: realitza l’assignació direccions IP automàtiques.
  * etc.
* **Serveis de funcions \(serveis de rol\)** : afegeixen més funcionalitat a un servidor.
  * Alguns rols, com per exemple el de servidor de DNS, només tenen una funcionalitat, i per tant no tenen serveis de rol disponibles.
  * Altres, com per exemple el de servidor d'escriptori remot, tenen diferents serveis de rol que es poden afegir en funció de les necessitats de l'empresa.
* **Característiques:** són programes per complementar o augmentar la funcionalitat del servidor però no tenen cap relació amb els rols que desenvolupa.
  * Client TFTP
  * Client Telnet

> Un servidor es pot especialitzar en una única funció o en diverses.

#### Altres informacions <a id="altres-informacions"></a>

A la finestra d'inici ens avisa dels dies què queden. Un cop acabat aquest període, es pot allargar 180 dies més obrint un terminal i posant la següent comanda:

`slmgr.vbs /rearm`

Després cal reiniciar la màquina.

Per **Introduir o canviar la clau d'activació** cal anar a _**Sistema**_ i a la part inferior hi ha un enllaç amb l'opció _**Introducir o cambiar la clave de producto**_.

**Iniciar sessió**

Demana que es premin les tecles _**Ctrl+Alt+Supr**_, però en VirtualBox s'ha de fer amb _**Ctrl dreta + Supr**_. Un cop es posi la contrasenya de l'administrador, s'accedirà al sistema.

