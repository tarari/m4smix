# Integració de sistemes amb Windows - Linux com a estació de treball

## 1. Sistemes heterogenis. Integració de sistemes amb Windows

En aquest punt desenvolupem el concepte de sistema heterogeni i la importància de la integració dels sistemes existents en aquest tipus d'escenari. Així mateix, es mostren les diverses possibilitats d'integració entre diferents tipus de sistemes operatius i introduïm els mecanismes d'integració proporcionats pel sistema Windows Server.

Microsoft Windows és el sistema operatiu amb llicència propietària de programari més important del món, mentre que les diferents distribucions Linux són els sistemes operatius amb llicència lliure més distribuïts arreu del món. És clara, doncs, la importància de fer treballar junts tots dos sistemes i aprofitar al màxim les respectives característiques.

El codi obert dels sistemes Linux i la seva gratuïtat es contraposen al codi tancat i de pagament dels sistemes Windows. Segons Microsoft, els seus sistemes són realment més barats que els sistemes Linux, ja que són més fàcils d'utilitzar i això implica a llarg termini un abaratiment del producte, mentre que els sistemes Linux requereixen una “mà” més experta, cosa que encareix un producte que en un principi era gratuït.

 

### 1.1. Sistemes heterogenis

La gran majoria d’organitzacions existents actualment, degut a les tendències del món de la informàtica i les telecomunicacions, fan servir gran quantitat de maquinari i programari amb característiques, arquitectures o fabricants diferents. Aquestes diferències conformen un sistema o escenari heterogeni, que podem definir com:

Un **sistema heterogeni** és aquell que es troba compost per maquinari amb característiques físiques distintes entre si, i programari amb característiques operatives distintes entre si, però que es poden comunicar utilitzant mitjans comuns.

Molts dels àmbits referents a la comunicació i la compatibilitat entre equips amb diferent maquinari i programari estan estandarditzats, per exemple protocols de comunicacions, estàndards de maquinari, etc. Però existeixen altres àmbits, com són el del funcionament i gestió dels sistemes operatius, en el qual cada distribució o empresa té implementacions, comportaments o funcionalitats diferents. Aquestes implementacions no estandarditzades o no consensuades fan que en molts casos els diferents sistemes operatius siguin incompatibles entre ells. Aquestes diferències són majors sobretot entre sistemes operatius lliures i propietaris, ja que la filosofia de negoci de cadascú dificulta de vegades el desenvolupament d’estàndards comuns i, per tant, la integració dels sistemes.

El fet que existeixen diferents màquines amb sistemes operatius en una mateixa organització indica que cada màquina gestionarà una quantitat d’informació i una sèrie de recursos com ara impressores, discos durs, unitats de CD-ROM que pot ser necessari que siguin compartits amb la resta de màquines de la xarxa o l’organització.

Així resulta necessari l’ús de serveis o aplicacions que permetin i facilitin l’intercanvi de recursos i informació en les organitzacions i que generin un entorn de xarxa comú entre els diferents sistemes operatius.

Independència

Quan utilitzem el terme _independència_ no volem dir que les aplicacions que implementen els serveis de xarxa siguin independents del sistema operatiu, sinó que quan els sistemes utilitzen un determinat servei, per exemple accedir a un fitxer en un servidor FTP, és indiferent que la màquina on es troba funcionant el servei tingui instal·lat un sistema operatiu o un altre, davant de l’usuari.

Existeixen gran varietat de serveis que ens permeten fer la compartició de recursos de manera transparent a l’usuari i amb independència del sistema operatiu que utilitzem a través de la xarxa. Aquesta independència es deu al fet que aquests serveis utilitzen protocols de comunicació estandarditzats, per exemple FTP, HTTP, DNS, etc. Però si el que volem és generar un entorn de xarxa comú en el qual integrem el sistema d’autenticació d’usuaris en una xarxa local, amb la compartició de recursos i la gestió de la informació de les màquines connectades a la xarxa, necessitem fer servir altres tipus de serveis com són els serveis de directori, serveis d’autenticació centralitzada, serveis de compartició de fitxers, serveis d’impressió, etc. que funcionin de manera conjunta i integrada.

La figura 1 il·lustra la idea de sistema heterogeni. Es pretén fer conviure sistemes molt diferents en un mateix espai.

Figura 1. Un sistema heterogeni![integracio](https://agora.xtec.cat/escolesnuria/moodle/file.php/102//uf4/t1/imatges/integracio.png)

 

#### 1.1.1. Integració de sistemes heterogenis

Una vegada entenem en què consisteix un sistema heterogeni i la importància de la compartició de recursos dins de les organitzacions, podem adonar-nos de la necessitat d'integrar els diferents sistemes operatius perquè puguin funcionar en conjunt en àmbits comuns i utilitzar els recursos de què es disposa.

Definim la integració de sistemes com la creació d’estructures formades per ordinadors de diferent tipus i amb sistemes operatius diferents que interoperin entre si de manera transparent per a l’usuari.

Existeixen diferents possibilitats d’integració entre sistemes lliures i propietaris per a la compartició de recursos, la possibilitat més comuna és mitjançant dominis Windows. Per implementar un domini Windows tenim diferents opcions:

* Utilitzar una màquina Windows com a controlador de domini amb Active Directory
* Utilitzar una màquina GNU/Linux com a controlador de domini amb Samba 4.

En els dos casos anteriors els clients podran ser màquines amb sistemes Windows o GNU/Linux.

En el cas d'utilitzar una màquina Windows com a controlador de domini, les màquines clients Windows no tindran cap problema per accedir al domini. Per a les màquines clients GNU/Linux, ens caldrà especificar un mecanisme d’autenticació i proporcionar la manera d'obtenir els atributs específics \(UID; GID; shell, etc.\) per a usuaris i grups. D’aquesta manera podem optar per dues solucions:

**1.** **No utilitzar Directori Actiu**, fent servir winbind, aquesta opció ens permet:

* Resoldre l’autenticació i l’obtenció d’atributs comuns mitjançant mecanismes Windows \(Kerberos + LDAP\).
* Integrar el client GNU/Linux com a membre del domini Windows.
* Proporcionar atributs UNIX des del mateix client GNU/Linux quan sigui necessari.

**2.** **Utilitzar Directori Actiu**, fent servir IDMU, aquesta opció ens permet:

* Modificar l’esquema d'_Active Directory_ per incloure atributs UNIX.
* Resoldre l’autenticació i l’obtenció d’atributs comuns mitjançant mecanismes Windows \(Kerberos + LDAP\).
* No integrar el client GNU/Linux com a membre del domini Windows.
* Configurar el client GNU/Linux perquè pugui accedir al Directori Actiu.

Independentment de l'opció que utilitzem haurem de configurar el client GNU/Linux per utilitzar NSS \(especificació de la base de dades d'usuaris\), PAM \(autenticació de comptes\) i, si cal, windbind \(integració de clients GNU/Linux en dominis Windows\).

En el cas d'utilitzar una màquina GNU/Linux com a controlador de domini, la màquina haurà de tenir instal·lat el paquet Samba i estar configurada com a controlador primari de domini. Aquesta solució permet emular un domini Windows NT amb una màquina GNU/Linux, la qual cosa suposa utilitzar programari lliure, evitant així, haver de pagar llicències per la seva utilització. Els clients Windows podran accedir al domini i als recursos accessibles des del domini sense cap problema. Els clients GNU/Linux podran accedir als recursos del domini, si els usuaris existents als clients estan donats d'alta també com a usuaris Samba. Si volem que els clients GNU/Linux, a més, s'autentiquen al domini creat pel servidor Samba, haurem d'utilitzar i configurar eines com NSS, PAM, i windbind o LDAP.

Existeix una altra opció molt més senzilla per a la simple compartició de recursos entre màquines Windows i GNU/Linux, sense necessitat d'implementar un domini. Aquesta opció consisteix a utilitzar Samba a les màquines GNU/Linux i crear un grup de treball comú entre les màquines amb sistemes Windows i les màquines amb sistemes GNU/Linux.

 

### 1.2. Integració de sistemes lliures i propietaris amb Windows Server

Tot i que Microsoft domina clarament el mercat domèstic, cada vegada més el sistema Linux està present a les llars compartint màquina amb una versió Windows. En el món de les supercomputadores les dades canvien: Microsoft és el segon sistema utilitzat per darrere de Linux.

Des del punt de vista tècnic, el més important és saber identificar les febleses de cadascun dels sistemes i cobrir-les amb un altre sistema operatiu. És aquí on té un paper molt important saber integrar diferents sistemes operatius.

Microsoft Windows Server  ofereix diverses opcions d'integració vers diferents sistemes operatius, com ara les distribucions Linux o els sistemes operatius d'Apple.

 

#### 1.2.1. Subsistema per aplicacions UNIX

En les distribucions Linux existeixen paquets que permeten executar en Linux aplicacions dissenyades per a Windows. L'empresa Microsoft no tenia la possibilitat d'executar aplicacions basades en UNIX, cosa que es podia interpretar com una situació de desavantatge, fins que es va presentar el SUA \(_Subsystem Unix Applications_\).

**›SUA** ofereix la infraestructura bàsica per executar aplicacions i scripts UNIX en Microsoft Windows Server.

El SUA resideix sobre el nucli, igual que el subsistema win32. Contempla la semàntica i les crides del sistema UNIX completes.

Per utilitzar el _SUA_ l'haureu d'instal·lar com una característica de Windows Server.

**Utilitats del subsistema d'aplicacions UNIX**

El _SUA_ inclou una sèrie d'utilitats que el posicionen com una eina molt potent i amb un ampli camp d'aplicació. Algunes de les utilitats que inclou el _SUA_ són:

* **Shells**: Korn i C.
* **Jobs**: ps, nice i kill.
* **Batchs**: at, cron i batch.
* **Desenvolupament**: gcc, gdb i make.
* **Processament de text**: grep, less, awk, sed, pr i tr.
* **Gràfics**: xterm, xrdb, xset i xclock.
* **Connectivitat**: bind, sendmail i ftp.

El SUA també inclou un ampli suport d'APIs que permeten dur a terme la portabilitat d'aplicacions UNIX sense haver de fer gaires canvis en el codi font, com ara C, C++, Fortran, Perl, Math, RPC, Socket, Curse, Crypt, Termcap, Lex, Yacc, Pthread, X11R6.6 i suport a Dynamic linking/shared object.

**Portabilitat d'aplicacions**

De manera similar a com es fan les portabilitats d'aplicacions entre diferents distribucions de Linux, _SUA_ s'encarrega de portar a Windows _scripts_ i aplicacions d'UNIX. Els _scripts_ els copia a _scripts_ per a Windows Server i els executa, mentre que les aplicacions les ha de recompilar.

La portabilitat es fan de manera segura en un gran nombre de casos, tot i que us podeu trobar amb problemes durant la compilació de les aplicacions. En determinats casos es produeixen petits canvis en l'aspecte que presenten les aplicacions deguts a les diferències en les llibreries d'ambdós sistemes.

 

#### 1.2.2. Gestió d'identitats per a UNIX

La gestió d'identitats s'instal·la com una part del _Role_ de _Directory Services_ de Windows Server.

_**SNIS**_ \(_Server for NIS_\) integra les xarxes de Windows i la de _Network Information Service_ \(_NIS_\).

Un controlador de domini Windows té la possibilitat d'actuar como un NIS mestre per a un o diversos dominis NIS. SNIS emmagatzema les dades dels _NIS maps_ en el _Directorio Activo_ utilitzant un esquema compatible amb l'RFC 2307.

Altres característiques pròpies són:

* La gestió d'identitats pot utilitzar _NIS maps_ com estàndard, per exemple hosts, group, protocols, o com no estàndard.
* Els clients UNIX i Linux accedeixen a _AD_ utilitzant _LDAP_.
* _SNIS_ pot instal·lar-se en altres _DC_ del domini per a que actuïn com a subordinats.
* Els servidors _NIS_ UNIX poden seguir actuant com a subordinats del domini _NIS_.

**Eines i utilitats SNIS**

_SNIS_ agrupa una sèrie d'eines i utilitats que faciliten molt les tasques de l'usuari. En resum, SNISS ofereix:

* Un assistent de migració i utilitats en la línia d'ordres per migrar i gestionar els _NIS maps_ d'UNIX al servidor executant SNIS: nis2ad, nisadmin, nismap, ypcat, ypclear, ypmatch, yppush.
* Els clients poden utilitzar moltes funcions i crides de procediment remot per connectar al servei de xarxa: yp\_match, yp\_first, yp\_next, yp\_all, yp\_order, yp\_master, yperr\_string, and ypprot\_err.
* Un llarg etcètera com: yppoll, ypset, domainname.

**Sincronització de contrasenyes**

La possibilitat de sincronitzar les contrasenyes tant en un sistema UNIX com en un sistema Windows facilita molt la tasca de l'administrador i la dels usuaris.

La sincronització de contrasenyes:

* S'instal·la com una part del _Role_ de _Directory Services_ de Windows Server.
* Permet als usuaris mantenir un únic parell usuari/contrasenya per a tot el domini Windows i els equips UNIX, i sincronitzar les contrasenyes quan es canvien en algun dels sistemes.
* La sincronització pot ser en un o en els dos sentits.
* Sincronitza les contrasenyes en equips Windows _Stand-Alone_ o per a un domini de Windows.
* Gestiona les contrasenyes d'equips UNIX individuals o de tots els equips d'un domini _NIS_.

 

### 1.3. Utilització d'NFS en Windows Server

Windows Services For UNIX queda actualitzat a les noves versions, per tant, us recomano mirar els següents enllaços:

[Instal·lació de NFS](https://www.server-world.info/en/note?os=Windows_Server_2016&p=nfs&f=1)

[Compartir en NFS en Windows](https://www.server-world.info/en/note?os=Windows_Server_2016&p=nfs&f=2)

[Configurar client NFS en Windows](https://www.server-world.info/en/note?os=Windows_Server_2016&p=nfs&f=4)



\_\_

#### 

 

#### 

#### 1.3.4. Administració de la funció de serveis d'arxius

El servidor d'arxius és un equip que resulta imprescindible en moltes configuracions de xarxa. És imprescindible configurar almenys un servidor de fitxers si molts usuaris necessiten accedir als mateixos fitxers i dades d'aplicació. Microsoft Windows Server  està preparat per donar aquest accés a màquines que funcionin sobre sistemes UNIX i distribucions Linux.

En sistemes anteriors a Windows Server es tenia la possibilitat d'instal·lar un servidor de fitxers bàsic, però amb l'actual sistema operatiu és necessari configurar un servidor perquè actuï com a servidor de fitxers afegint la funció Serveis d'arxiu.

En la taula 1 es mostren els serveis de funció per a servidors de fitxers.

Taula 1. Descripció dels serveis de funció per a servidors de fitxers.

| Servei de funció | Descripció |
| :--- | :--- |
| Administració de recursos compartits i emmagatzemament | Permet que els administradors s'ocupin de les carpetes compartides i permet que els usuaris accedeixin a les carpetes compartides mitjançant la xarxa. |
| Servei de cerca de Windows | Permet buscar fitxers ràpidament en el servidor des de màquines client. |
| DFS, sistema de fitxers distribuït | Proporciona eines i serveis per als espais de noms i la replicació DFS. |
| Espais de noms DFS | Permet agrupar carpetes compartides ubicades en diferents servidors dins d'una estructura lògica formada per un o més espais de noms. |
| Replicació DFS | Permet sincronitzar carpetes de diversos servidors mitjançant les connexions de xarxa d'àrea local o extensa. |
| FSRM, administrador de recursos del servidor de fitxers | Agrupa un conjunt d'eines que els administradors poden utilitzar per gestionar les dades emmagatzemades. |
| Serveis per a NFS | Permet compartir fitxers en entorns mixtos. Els usuaris podran transferir fitxers entre Windows Server  i UNIX o distribucions Linux. |
| Serveis d'arxiu de Windows Server 2003 | Proporciona serveis de fitxer compatibles amb Windows Server 2003. |
| FRS, serveis de replicació d'arxius | Permet sincronitzar carpetes amb servidors d'arxius que utilitzin FRS en comptes de DFS. |
| Serveis d'Index Server | Permet la indexació d'arxius i carpetes per poder trobar-los ràpidament. |

Per afegir la funció _Serveis de fitxer_ i poder treballar seguiu els passos següents:

1. Obriu l'administrador del servidor.
2. Dins el node _Funcions_ cliqueu a _Afegir funcions_.
3. Quan estigueu a _Seleccionar funcions del servidor_ seleccioneu _Serveis de fitxer_ i progresseu amb _Següent_.
4. Marqueu _Serveis per a Network File System_ per poder treballar amb sistemes UNIX.
5. Completeu els passos opcionals i cliqueu a _Següent_.
6. Confirmeu l'inici de la instal·lació amb _Confirmar seleccions d'instal·lació_.
7. Cliqueu a _Instal·lar_ per iniciar el procés d'instal·lació.

 

#### 1.3.5. Configuració de l'accés a impressores

Una impressora accessible des de la xarxa pot concentrar un gran número de tasques d'impressió que poden venir tant de sistemes Windows com de sistemes UNIX. L'alta càrrega de treball fa que calgui fer un treball d'administració força acurat.

Els treballs que s'acumulen a la cua d'impressió es copien a la carpeta _%SystemRoot%\system32\spool\PRINTERS_. Els usuaris que hagin d'accedir a la impressora hauran de tenir permisos per modificar aquesta carpeta.

Reviseu els permisos de la carpeta seguint els passos següents:

1. Obriu el servidor d'impressió.
2. Cliqueu a _Propietats_.
3. Seleccioneu la fitxa _Opcions avançades_.
4. La carpeta a revisar està descrita com _Carpeta de cua d'impressió_.
5. Utilitzeu l'explorador de Windows per accedir a la carpeta descrita en el punt anterior.
6. Cliqueu amb el botó dret a sobre de la carpeta i premeu _Propietats_.
7. Dins la fitxa _Seguretat_ comproveu que els permisos són els adients.

 

#### 1.3.6. Configuració d'Active Directory per autenticar màquines UNIX

Abans de l'aparició de Microsoft Windows Server 2000 els controladors de domini de Windows NT proporcionaven serveis d'autenticació per a clients utilitzant _NTLM_ \(gestor de xarxes d'NT\). Aquest protocol era capaç de mantenir duplicats de comptes d'usuari en diversos servidors de xarxa, tot i que tenia importants problemes de seguretat.

És a partir de Microsoft Windows Server 2000 que Microsoft decideix deixar de banda el protocol _NTLM_ i començar a treballar amb _Active Directory_ i els serveis d'autenticació integrats d'autenticació Kerberos.

Kerberos és un sistema molt més segur que NTLM i que gestiona millor l'escalabilitat. Els sistemes Linux i UNIX també utilitzen Kerberos, per tant es converteix en molt real la possibilitat d'integrar els diferents sistemes operatius.

Existeix un problema amb l'autenticació d'usuaris de Linux i UNIX amb _Active Directory_: els identificadors d'usuaris i grups. Internament, ni Linux ni Windows fan referència als usuaris pel nom, sinó que utilitzen els identificadors interns únics.

Els sistemes Microsoft utilitzen el _SID_ \(identificador de seguretat\), que és una estructura de longitud variable que identifica sense marge d'error els diferents usuaris dins d'un domini de Windows. El _SID_ també conté un identificador únic de domini per tal que el sistema operatiu pugui distingir entre els usuaris en diferents dominis.

En sistemes UNIX i distribucions Linux cada usuari té un identificador d'usuari \(_UID_\) que és un nombre enter de 32 bits únic en el sistema. L'àmbit de l'_UID_ està limitat en l'equip, sense que es garanteixi que un altre usuari en una màquina diferent pugui tenir el mateix nombre enter. Això fa que un usuari ha d'iniciar sessió en cada equip on hagi de tenir accés.

Aquest problema se soluciona proporcionant autenticació de xarxa amb el sistema d'informació de xarxa \(_NIS_\) o un directori compartit d'_LDAP_. El sistema d'autenticació de xarxa proporciona l'_UID_ per a l'usuari i tots els equips Linux o UNIX utilitzen aquest sistema d'autenticació compartint el mateix usuari i els identificadors de grup.

És recomanable utilitzar _Active Directory_ per proporcionar un usuari únic i els identificadors de grup. Es pot crear un _UID_ per a cada usuari i grup i emmagatzemar aquest identificador amb l'objecte corresponent en _Active Directory_, així, quan un usuari s'autentica pot cercar l'UID per a l'usuari i proporcionar-lo per al sistema operatiu com l'identificador de l'usuari intern.

Aquesta solució, però, té un inconvenient. És necessari proporcionar un mecanisme per garantir que cada usuari i grup tenen un identificador i que aquests identificadors són únics en el bosc.

Una altra estratègia per assignar identificadors és utilitzar l'identificador relatiu \(_RID_\). L'identificador relatiu és un número enter de 32 bits que identifica l'usuari dins el domini. El _RID_ forma part del _SID_. La solució consisteix en extreure el _RID_ quan l'usuari inicia la sessió i fer-lo servir com a _UID_ intern únic. Només cal remarcar que no podreu utilitzar aquesta estratègia en aquells entorns on existeixin diversos dominis, ja que existeix la possibilitat que els usuaris de diferents dominis tinguin el mateix valor _RID_.

És necessari proporcionar els identificadors de Linux o UNIX per a tots els usuaris, i els grups als quals pertanyen, que poden iniciar sessió. S'han de definir valors per als atributs _uidNumber_ i _gidNumber_ dels usuaris i grups. No oblideu que:

* Els sistemes UNIX i distribucions Linux necessiten un UID para a cadascun dels usuaris que autentiquen. Cada compte d'usuari que iniciarà una sessió en un equip Linux o UNIX ha de tenir un atribut _uidNumber_ únic. El valor específic que utilitzi per a un _uidNumber_ no és important, però ha de ser únic entre tots els usuaris que poden iniciar sessió en l'equipo de Linux o UNIX.
* Cada usuari de Linux també ha de tenir un identificador de grup predeterminat, per a cada usuari d'_Active Directory_ que s'iniciarà en una sessió en un equipo Linux o UNIX requereix un valor per a l'atribut _gidNumber_. Aquest valor no ha de ser únic entre tots els usuaris, però ha d'identificar el grup.
* Cada grup en _Active Directory_ ha de tenir un valor únic per a l'atribut _gidNumber_.

 

### 1.4. Interoperabilitat amb equips Mac

És possible intercanviar arxius i compartir impressores i la connexió a Internet dins una xarxa amb equips Mac.

L'adopció del d'Ethernet i el protocol TCP/IP com estàndard en els últims PC i Mac ha simplificat enormement la interoperativitat entre les dues plataformes.

Els sistemes Macintosh es poden connectar a totes les principals plataformes de servidor, com ara AppleShare, Unix, Linux i Windows \(NT/2000/XP/2003/Vista/20xx\). El sistema operatiu Mac OS X és compatible amb:

* Els protocols d'arxius compartits estàndard AFP \(_Apple Filing Protocol_\).
* SMB / Samba \(_Service Message Block_\).
* WebDAV.
* Unix NFS \(_Network File System_\).

Per connectar-se a xarxes, Mac OS X disposa d'un client AFP, un altre SMB per connectar-se a servidors de fitxers Windows i també un NFS per als servidors Unix. La integració de clients Macintosh en aquestes plataformes de servidor és senzilla i molt intuïtiva.

 

#### 1.4.1. Equips Mac i Windows dins d'una mateixa xarxa

El sistema operatiu Mac OS X disposa d'un client i un servidor SMB incorporat, fet que permet a l'equip que està funcionant amb Mac OS X pertànyer a la xarxa exactament igual que qualsevol PC per als usuaris de Windows i viceversa. A l'equip amb Mac OS X, heu de seleccionar _Connectar al servidor …_ al menú _Anar_ per navegar per diversos ordinadors de grups de treball i dominis Windows.Amb aquesta implementació de client i servidor SMB, els usuaris de Mac OS X poden compartir les carpetes i impressores connectades per USB a una xarxa Windows.

Des d'una estació de treball Windows s'accedeix de la mateixa manera a tots els recursos ubicats en un servidor Mac OS X.

Mac OS X Server és un sistema operatiu per a servidor d'última generació. Gràcies al seu alt rendiment. Heu de tenir en compte que el sistema operatiu Mac OS X Server pot atendre un gran nombre d'estacions de treball, tant Mac com PC amb Windows o estacions de treball Unix.

El programari inclós en el sistema operatiu Mac proporciona serveis per compartir arxius Macintosh \(AFP sobre IP\), Windows \(SMB / CIFS\) i Unix \(NFS\). També inclou eines d'administració per a ordinadors Macintosh anteriors i les aplicacions necessàries per implantar un servidor d'Internet complet \(DNS, web, correu i un llarg etcètera\). A més, el programari proporciona una interfície d'administració unificada per organitzar els privilegis d'accés dels usuaris.

Mac OS X Server és compatible amb els arxius compartits Windows des de qualsevol punt d'accés definit, no solament les carpetes compartides i públiques del directori d'inici de l'usuari. També és compatible amb el protocol WINS \(_Windows Internet Naming Service_\), que permet a clients Windows de diverses subxarxes negociar noms i adreces.

La interoperavilitat amb Novell també és possible. Novell sempre ha treballat per proporcionar als seus servidors Netware solucions de compatibilitat amb Macintosh \(des de la versió 4.x. Netware inclou Netware per a Macintosh, una extensió del programari que permet compartir arxius i impressores amb Macintosh\). Des de Mac OS X, els servidors Netware 5.xi 6 es veuen com un veritable servidor AppleShare sobre IP.

 

#### 1.4.2. Connexió a volums NFS

Si la xarxa on pertany l'equip Mac té servidors NFS, podeu utilitzar l'opció _Connectar al servidor …_ del menú _Anar_ per accedir als arxius compartits. A diferència dels servidors SMB, haureu d'indicar el camí NFS fins als arxius en qüestió.

La sintaxi correcta per l'URL de servidors NFS és: nfs: / / nomdelservidor.com / ruta /.

Cal tenir molt en compte que no podeu utilitzar les utilitats de la línia d'ordres ni _Serveis per a NFS_ per administrar versions anteriors.

Utilitzeu _Servidor per a NFS_ per controlar l'accés dels usuaris i dels grups als recursos de _Serveis per a NFS_ mitjançant _Active Directory_. Per aconseguir-ho, us caldrà l'identificador d'usuari o l'identificador de grup segons quin sigui el cas.

Si agregueu una nova unitat en l'equip que és el servidor d'NFS, tingueu en compte que haureu de modificar permisos. El directori arrel de la unitat haurà de tenir els permisos adequats perquè no tothom hi pugui escriure.

 

#### 1.4.3. Mac OS X i Active Directory

El sistema operatiu Mac OS X és compatible amb _Active Directory_, cosa que permet integrar més fàcilment els Mac a les xarxes basades en Windows. És possible adoptar el mateix sistema d'autenticació mitjançant contrasenya que s'utilitza en els sistemes Windows, i emmagatzemar el vostre directori d'inici en un servidor Windows remot.

Un aspecte a tenir molt en compte és utilitzar un nom de màquina i contrasenya correctes. Aquestes dades han de contenir caràcters vàlids per a Windows i no tenir una longitud de més de setze caràcters. Amb la utilitat _dsconfigad_ podeu utilitzar noms de màquines i contrasenyes no vàlids per a _Active Directory_. Amb _dsconfigad_ es canvien de forma automàtica les dades per tal que siguin vàlides per a _Active Directory_.

Per poder utilitzar recursos de Windows Server  en màquines amb sistemes operatius Mac, caldrà que el protocol _AppleTalk_ estigui funcionant correctament. Per activar el protocol _AppleTalk_ seguiu els passos següents:

1. Seleccioneu _Preferències del sistema_ al menú _Apple_.
2. Cliqueu a sobre de la icona _Xarxa_.
3. Al menú _Mostrar_ seleccioneu _Ethernet integrada_ o bé _Airport_.
4. Cliqueu a la fitxa _AppleTalk_.
5. Marqueu la casella _Activar AppleTalk_.
6. Cliqueu a _Aplicar ara_.

#### 1.4.4. Mac OS X i Open Directory

Apple ha integrat Samba i _Open Directory_ per l'autenticació d'usuaris. No és necessari mantenir bases de dades de serveis de directori separades per als sistemes Windows, de manera que els usuaris poden accedir als seus arxius de xarxa des de sistemes tant Windows com Macintosh amb el mateix nom d'usuari i contrasenya.

Des del punt de vista del manteniment, resulta de gran utilitat treballar amb _Open Directory_, ja que el tècnic només ha de preocupar-se de mantenir un únic directori, independentment de les plataformes que es trobin dins la xarxa informàtica.

Els passos per crear un domini basat en _Open Directory_ són:

1. Obrir la utilitat _MacOS X Server Admin_.
2. Al menú _Ordinadors i serveis_, que trobareu a l'esquerra de la pantalla, seleccioneu _Open Directory_.
3. A la part central de la pantalla, dins la fitxa _General_, seleccioneu el rol _Open Directory_.
4. Amb el botó _Configuració_, que trobareu a la part inferior de la pantalla, podreu fer-ne la configuració a mida.
5. Per finalitzar, haureu de crear un compte d'usuari local no compartit que permeti la gestió administrativa d'_Open Directory_.

 

#### 1.4.5. Emmagatzematge en xarxa

Actualment s'utilitzen servidors d'emmagatzematge en xarxa per a emmagatzemar els arxius d'un grup de treball o d'un departament. Aquest sistema es pot usar amb transparència pels Mac.

La majoria dels servidors d'emmagatzemament en xarxa de nivell bàsic són compatibles amb diversos protocols d'arxius compartits, com ara AppleShare. Això vol dir que l'equipament pot utilitzar-se per a compartir arxius sense esforç en entorns heterogenis constituïts per ordinadors Mac OS X / PC. La majoria tenen una interfície web d'administració i poden configurar i administra-se des d'un equip Mac.

Quan els servidors d'emmagatzematge no són compatibles amb AppleShare, el sistema Mac OS X ofereix diverses alternatives igual d'efectives mitjançant sistemes d'arxiu NFS d'Unix i de web estàndard WebDAV. El sistema operatiu Mac OS X garanteix l'accés a l'emmagatzematge en xarxa, fins i tot a servidors com els de Network Appliance o EMC.

 

#### 1.4.6. Mac i PC: impressores compartides en LAN

L'ús compartit d'impressores és una de les tasques més desitjades en els escenaris heterogenis.

Amb Mac OS X, les opcions d'impressió s'han ampliat considerablement. Aquest sistema operatiu és compatible amb totes les impressores PostScript del mercat. Les impressores gestionades i compartides amb Mac OS X Server es veuen des de les estacions Windows com impressores de PC i com impressores Mac per les estacions amb Mac OS X. Des de Windows, les cues d'impressió de Mac OS X Server es veuen com cues SMB/CIFS estàndard. Amb el controlador adient, apareixen com impressores compartides en l'entorn de xarxa Windows.

Les estacions de treball Mac imprimeixen a través de Mac OS X Server amb les seves eines habituals \(el _Centre d'impressió_\). Els serveis d'impressió de Mac OS X també estan a l'abast de les estacions de treball Unix via LPD/LPR. De la mateixa manera, no cal utilitzar servidors d'impressió compatibles amb AppleTalk, ja que es pot utilitzar el sistema d'impressió LPD/LPR d'Unix.

### 1.5. Utilització dels diferents servidors de fitxers

És molt important que comproveu la possibilitat d'utilitzar determinats sistemes de fitxers en el sistema operatiu en xarxa que necessiteu implantar. Heu de saber els problemes de compatibilitat que es poden manifestar en els vostres dissenys abans d'implementar cap solució.

En la taula 2 es presenta un resum on es relacionen alguns dels sistemes operatius en xarxa dels servidors més comuns amb els diferents sistemes de fitxers que us podeu trobar, i s'indica si és possible utilitzar-los.

Taula 2. Possibilitat s'ús de diferents servidors de fitxers segons el sistema operatiu.

| Sistema operatiu | AFP en AppleTalk | AFP sobre IP | SMB/CIFS | NFS |
| :--- | :--- | :--- | :--- | :--- |
| Mac OS X Server | Sí | Sí | Sí | Sí |
| Windows  Server | Sí | Sí | Sí | Sí |
| Netware 6 | Sí | Sí | Sí | Sí |
| Linux | Sí | Sí | Sí | Sí |
| Solaris | Sí |  | Sí | Sí |

