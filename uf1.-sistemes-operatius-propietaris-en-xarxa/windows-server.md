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

## Tasques de l’administrador de sistemes

Algunes de **tasques d'un administrador de sistemes** d'informació:

* Instal·lació, configuració i reconfiguració del Sistema Operatiu.
* Instal·lació, actualització dels controladors i firmwares.
* Instal·lació de les **actualitzacions** crítiques i/o recomenables del sistema operatiu.
  * **Crítiques** és **important** instal·lar-les, sempre que no afecti al funcionament del sistema d'informació.
  * Recomanables depen de si ens soluciona un problema o no.
* Gestió i comprovació de les **còpies de seguretat**.
* **Monitorització i vigilar:**
  * l’estat del sistema comprovant el rendiment de l'equip
  * l’estat de la xarxa
  * l’estat dels serveis i recursos
  * l’espai físic d’emmagatzematge
  * etc

### Eines de supervisió <a id="eines-de-supervisi&#xF3;"></a>

El sistema operatiu Microsoft Windows Server us proporciona una sèrie d’**eines** perquè pugueu **administrar el sistema** de manera efectiva i ràpida:

* **Administrador del servidor**
* **Administració de tasques**
* **Administració de serveis**
* **Administrador de discs**
* **Registre d’esdeveniments**

### Administrador del servidor _\[Server Manager\]_

L'**Administrador del servidor** permet veure, en una única pantalla, la informació del sistema, opcions de configuració de seguretat i els seus possibles problemes de configuració.

![](https://seicoll.gitbooks.io/sox/content/.gitbook/assets/servermanager.png)

L'**Administrador del servidor**, amb una única eina, permet als administradors:

* Veure i modificar els **rols i característiques instal·lats** al servidor.
* Realitzar tasques d'administració com **iniciar o aturar serveis** i administrar comptes d'usuari locals.
* Determinar l'**estat del servidor**.
* Identificar **esdeveniments crítics**.
* Analitzar i solucionar **problemes o errors de configuració**.

### Administrador de tasques _\[Task Manager\]_

L’**administrador de tasques** és l’eina principal per administrar processos i aplicacions del sistema.

Disposa de cinc pestanyes. Aquestes pestanyes us ajudaran a administrar **processos, rendiment, usuaris i serveis**.

* **Processos** _**\[Processes\]**_: aplicacions d'usuari i processos que s'estan executant en segon pla \(processos del sistema\), i la utilització què estan fent del sistema \(CPU i memòria\)
* **Detalls** _**\[Details\]**_: programes associats a les aplicacions. Permet canviar la prioritat de cada un
* **Usuaris** _**\[Users\]**_: quins usuaris estan connectats i quines aplicacions estan utilitzant; també es poden desconnectar usuaris.
* **Serveis** _**\[Services\]**_: quins serveis estan engegats o aturats. Permet aturar o iniciar serveis. També es pot obrir l'administrador de serveis per gestionar els serveis de forma més detallada.
* **Rendimient** _**\[Performance\]**_: veure la utilització global de la CPU, la memòria i la xarxa. Des d'aquest apartat es pot obrir el Monitor de recursos, que permet veure amb més detall la utilització de la CPU, la memòria, la xarxa i els discos.

![](https://seicoll.gitbooks.io/sox/content/.gitbook/assets/taskmanager.png)

Teniu **quatre vies per accedir** a l’administrador de tasques:

* Combinar les tecles _**Control, Majúscules i Esc**_.
* Combinar les tecles **Control, Alt i Supr** i escollir l’opció Iniciar l’administrador de tasques.
* Clicar a Inicio, escriure ``_**`taskmgr`**_ ``en el quadre de text, clicar a Iniciar la cerca i prémer Enter.
* Clicar amb el botó dret del ratolí a la barra d’eines i seleccionar l’opció _**Administrador de tasques**_.

#### Administrador de serveis _\[Services\]_ <a id="administrador-de-serveis-services"></a>

> Els **serveis** són programes que funcionen sense interactuar directament amb l'usuari.

Normalment són programes que s'arranquen amb el sistema operatiu.

L'eina _**Serveis**_ mostra l'estat dels serveis i permet gestionar-los.

Cada servei el podem configurar: Ens posem a sobre &gt; boto dret ratolí &gt; propietats:

![](https://seicoll.gitbooks.io/sox/content/.gitbook/assets/services.png)

#### **Configuració d'inici d'un servei**

És important que feu la **configuració d’inici** dels serveis més escaient.

> Que un servei estigui instal·lat no vol dir que s'estigui executant.

![](https://seicoll.gitbooks.io/sox/content/.gitbook/assets/sevicesstart.png)

Existeixen quatre **tipus d’arrencada d’un servei**:

* **Automàtic:** El servei que s’iniciarà quan l’equip s’engegui.
* **Automàtic \(inici retardat\):** el servei s’inicia quan l’equip està en marxa i tots els serveis automàtics \(marcats amb l’opció anterior\) estan funcionant.
* **Manual:** el servei s’haurà d’iniciar manualment.
* **Deshabilitat:** el servei estarà desactivat i no es pot engegar.

> Tenir executant-se serveis que no fem servir consumirà recursos innecessaris.

#### **Configuració de la recuperació d'un servei**

El sistema Microsoft Windows Server permet fer accions si detecta que un servei específic falla.

![](https://seicoll.gitbooks.io/sox/content/.gitbook/assets/serveisrecuperacio.png)

#### Existeixen tres opcions de **recuperació d’un servei**:

* **No fer cap acció:** el sistema no intentarà recuperar-se d’aquest error, però sí de la resta.
* **Reiniciar el servei:** atura el servei, fa una pausa i el torna a iniciar.
* **Executar un programa:** si es produeix un error en aquest servei, es llançarà un script o un programa.

#### Administrador de discs _\[Disk Management\]_ <a id="administrador-de-discs-disk-management"></a>

L'**administrador de discs** permet administrar els discs, particions i volums.

#### Registre d’esdeveniments _\[Event Viewer\]_

El **registre d’esdeveniments** mostra a l’administrador informació sobre els error o avisos que s'han produït al sistema.

Els principals són:

* Registres de Windows
  * Aplicacions
  * Seguretat
  * Instal·lació
  * Sistema
* Registres d'aplicacions i serveis
  * Esdeveniments de hardware

![](https://seicoll.gitbooks.io/sox/content/.gitbook/assets/eventviewer.png)

### Actualitzacions <a id="actualitzacions"></a>

Una de les tasques que ha de fer l'administrador de sistemes és assegurar-se que els sistemes operatius s'**actualitzen** correctament.

En **versions anteriors de Windows Server 2016**, les actualitzacions venien deshabilitades de forma predeterminada. En canvi, en **Windows Server 2016 i posteriors**, la configuració predeterminada estableix que les actualitzacions es descarreguin però sigui l'administrador qui decideixi quan instal·lar-les.

#### En versions anteriors a Windows Server 2016 <a id="en-versions-anteriors-a-windows-server-2016"></a>

En el **Windows Update** \(en Windows Server 2012 i Windows 8\) existeixen **quatre configuracions** possibles:

* **Instal·lar actualitzacions automàticament \(recomenat\)**.
  * Les actualitacions requereix reiniciar, per aquesta, raó si triem l'opció automàtica, **configurarem el dia i la hora** que no afecti al funcionanament de l'empresa o als processos del servidor o sistema informàtic.
* **Descarregar actualitzacions, però deixar-me triar quan instal·lar-les**.
* **Buscar actualitzacions, però deixar triar si vull descarregar-es i intalar-les**.
  * Quan volem controlar quines actualitzacions s’instal·len.
* **No buscar actualitzacions \(no recomenat\)**
  * Podem instal·lar manualment les actualitzacions des de la web oficial de Windows.

#### En Windows Server 2016 i posteriors <a id="en-windows-server-2016"></a>

En el **Windows Update** \(en Windows Server 2016 i Windows 10\) podem configurar:

* **Canviar hores actives** Un cop instal·lades actualitzacions, si és necessari reiniciar, tenim dos opcions:
  * reiniciar el servidor manualment
  * esperar un reinici automàtic fora de les hores de major activitat del sistema.

    De forma predeterminada, les hores de major activitat estan definides entre les 8:00 i las 17:00. Si aquests valors no s'adapten a l'horari de la teva empresa els podem modificar amb l'opció _**Cambiar horas activas**_.
* **Opcions de reinici** Quan es programa un reinici automàtic després d'instal·lar actualitzacoins, amb aquesta opció podrem canviar l'hora i el dia en el qual es realitzarà el reinici.
* **Opcions avançades**
  * **Ofrecer actualizaciones para otros productos de Microsoft cuando actualice Windows**. Permet mantenir actualitzades la resta d'aplicacions que hàgim adquirit de Microsoft al mateix temps que actualitzem el sistema operatiu.
  * **Aplazar actualizaciones de características**. Permet deixar d'instal·lar aplicacions del sistema operatiu que no tinguin a veure amb la seguretat. Això redueix els temps necessari en instal·lar actualitzacions, però deixes de disposar de les últimes funcions que s'incorporin al sistema operatiu.

#### Cas d'exemple: Impedir actualitzacions a Windows <a id="cas-dexemple-impedir-actualitzacions-a-windows"></a>

Com s'ha pogut veure, amb l'arribada de **Windows 10** el servei d'actualitzacions va fer un canvi significatiu de paradigma. Es va pensar més en els usuaris inexperts que no pas en els professionals. S'ha volgut evitar que per manca de coneixement o d'atenció, es deixi d'actualitzar el sistema amb el gran risc de seguretat que pot comportar.

Per això, quan un pedaç de seguretat crític és emès pels servidors d'actualització de Microsoft, el servei d'actualització el descarregarà, l'**instal·larà i reiniciarà el sistema automàticament** si és necessari o es **reservarà la instal·lació en fred per la següent vegada de aturem o reiniciem manualment el sistema**.

Tot això ho farà sense demanar consentiment a l'usuari i, si ens descuidem, ens reiniciarà el sistema sense tenir en compte el que estiguem fent en aquell moment.

En el nostre cas una actualització forçada ens podria destorbar durant la realització d'alguna pràctica o control. Per això, farem servir algunes eines administratives per impedir-ho.

**A\) Deshabilitar el servei d'actualitzacions**

El servei responsable de les actualitzacions de Windows s'anomena _**wuausrv**_ però en l'_Administrador de serveis_ el podrem trobar com _**Windows Update**_. A partir d'aquí només haurem de deshabilitar el servei.

_**Administrador de serveis &gt; "Windows Update" &gt; Propietats &gt; Tipus d'inici &gt; Escollir Deshabilitat**_ _**Administrador de serveis &gt; "Windows Update" &gt; Propietats &gt; Estat del servei &gt; Detenir**_

**B\) Deshabilitar la directiva de grup \(GPO\) responsable de les actualitzacions**

La _**Directiva de grup \(GPO\)**_ és un conjunt de regles de Windows que ens permeten gestionar i configurar el sistema operatiu, les aplicacions i els usuaris. I això ho podrà fer de manera local o en un domini i, distingint-los entre regles vinculades a usuaris i a equips. Però aquest és un tema que tractarem àmpliament més endavant. El que ens interessa saber ara mateix és com utilitzar una de les directives responsables de les actualitzacions automàtiques.

Accedim a l'editor de directives. Tres opcions:

* _\[Win + R\]_ -&gt; Escriu "`gpedit.msc`" per executar-lo
* _"Botó dret sobre Inici de Windows" &gt; Executar_
* _\[Clica sobre el botó d'inici -&gt; Escriu "Editar directiva de grupo"_

Dins de l'editor seguirem la següent ruta:

_**Configuració de l'equip &gt; Plantilles administratives &gt; Components de Windows &gt; Windows Update**_

A la dreta, trobarem un llarg llistat de regles. Entre elles, **deshabilitarem** almenys una de les següent regles:

* _Configurar Actualitzacions automàtiques_
* _No reiniciar automàticament amb usuaris que hagin iniciat sessions en instal·lacions d'actualitzacions automàtiques_

Per forçar l'aplicació de directives editades haurem d'executar \[Win + R\]: **`gpupdate /force /boot /logoff`**  


