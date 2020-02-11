# Compartir recursos en xarxa i seguretat en sistemes lliures

La compartició de recursos en xarxa és una de les utilitats o raons principals perquè els sistemes operatius es connectin en xarxa. En l'àmbit del sistemes operatius, podem entendre el concepte de compartició de recursos des de diversos punts de vista.

Des del punt de vista del maquinari, compartir recursos fa referència a l'ús del maquinari per dos o més processos dins del sistema operatiu.

En aquesta unitat ens centrarem en la compartició des del punt de vista de les xarxes d'ordinadors.

**Compartir recursos en xarxa** implica configurar la xarxa de tal manera que els ordinadors que hi estan connectats puguin utilitzar els recursos de la resta, fent servir la xarxa com a mitjà de comunicació.

Es poden compartir tot tipus de recursos, encara que els més habituals són directoris i impressores. En els sistemes operatius lliures hi ha diversos protocols i aplicacions que ens permeten compartir recursos en xarxa, com el protocol NFS i el paquet de programari Samba.

 

### 2.1. Protocol NFS

El protocol NFS \(_network file system_ o sistema de fitxers per xarxa\) és un protocol del nivell d'aplicació que s'utilitza en l'àmbit de les xarxes locals per compartir fitxers entre màquines de la mateixa xarxa. El protocol NFS es pot fer servir per defecte en la majoria de sistemes operatius GNU/Linux. Així, NFS fa possible que diversos sistemes GNU/Linux connectats a una mateixa xarxa puguin accedir a fitxers remots, en altres equips de la xarxa, com si es tractés de fitxers locals.

El sistema GNU/Linux només pot treballar amb una jerarquia de directoris. Per tant, si volem accedir a diferents sistemes d'arxius, particions de discos o CD-ROM, entre altres, primer hem de muntar aquests elements en algun punt de la jerarquia.

L'NFS ens proporciona un servei de xarxa que permet a un ordinador client muntar un sistema d'arxius remot, exportat per un altre ordinador servidor, i accedir-hi. Per tant, el protocol NFS funciona clarament sota una arquitectura client-servidor.

 

#### 2.1.1. Usos del protocol NFS

El protocol NFS pot ser utilitzat amb múltiples finalitats, sempre dins de l'àmbit de compartició de recursos. Per això, en general, el protocol NFS és molt flexible i admet diferents possibilitats o escenaris de funcionament com, per exemple, els següents:

* Un servidor NFS pot exportar més d'un directori i atendre simultàniament diversos clients.
* Un client NFS pot muntar directoris remots exportats per diferents servidors.
* Qualsevol sistema GNU/Linux pot ser alhora client i servidor NFS.

Tenint en compte això, hi ha diversos usos típics de l'NFS en què aquest servei mostra la utilitat que té. En podem destacar les utilitats tradicionals següents:

**1. Centralització dels directoris de connexió dels usuaris \(**_**home directory**_**\)**. Quan en una xarxa local de màquines GNU/Linux es vol que els usuaris puguin treballar indistintament en qualsevol, és adequat situar els directoris de connexió de tots els usuaris en una mateixa màquina i fer que les altres muntin aquests directoris mitjançant NFS. Si aquest ús es combina amb l'autenticació d'usuaris en xarxa per mitjà de l'LDAP, en els sistemes GNU/Linux es pot implementar quelcom similar a un domini del Windows.

**2. Compartició de directoris d'ús comú**. Si diversos usuaris des de diferents màquines treballen amb els mateixos arxius, per exemple, d'un projecte comú, també és útil compartir els directoris en què hi ha aquests arxius. Aquesta opció comporta un estalvi de disc en les màquines locals, ja que les dades estan centralitzades en un lloc, de manera que diversos usuaris hi poden accedir i les poden modificar. Per tant, no és necessari replicar la informació.

**3. Situar programari en un sol ordinador de la xarxa**. És possible instal·lar programari en un directori del servidor NFS i compartir aquest directori via NFS. Si configurem els clients NFS perquè muntin aquest directori remot en un directori local, aquest programari estarà disponible per a tots els ordinadors de la xarxa.

**4. Compartició per mitjà de la xarxa de dispositius d'emmagatzematge**. És possible compartir dispositius com ara particions de discos durs, CD-ROM, etc. Això pot reduir la inversió en aquests dispositius i millorar l'aprofitament del maquinari que hi ha en l'organització.

Totes les operacions sobre fitxers són síncrones. Això significa que l'operació només torna un resultat quan el servidor ha completat tot el treball associat a aquesta operació.

Per exemple, en cas d'una sol·licitud d'escriptura, el servidor escriurà físicament les dades en el disc i, si és necessari, actualitzarà l'estructura de directoris abans de tornar una resposta al client. Això garanteix la integritat dels fitxers.

 

#### 2.1.2. Funcionament de l'NFS

En el sistema client, el funcionament de l'NFS es basa en la capacitat de traduir els accessos de les aplicacions a un sistema d'arxius en peticions al servidor corresponent per mitjà de la xarxa. Normalment aquesta funcionalitat del client està programada en el nucli de GNU/Linux, de manera que no necessita cap tipus de configuració.

Respecte al servidor, l'NFS s'implementa mitjançant dos serveis de xarxa, el **mountd** i l'**nfsd**. Vegem quines accions controla cadascun d'aquests serveis:

* **El servei mountd s'encarrega d'atendre les peticions remotes de muntatge, efectuades per l'ordre** _**mount**_ **del client**. Entre altres coses, aquest servei s'encarrega de comprovar si la petició de muntatge és vàlida i de controlar sota quines condicions s'accedirà al directori exportat \(només lectura, lectura/escriptura, etc.\). Una petició es considera vàlida quan el directori sol·licitat ha estat explícitament exportat i el client té prou permisos per muntar aquest directori.
* **El servei nfsd s'encarrega**, una vegada un directori remot ha estat muntat amb èxit, **d'atendre i resoldre les peticions d'accés del client als arxius que hi ha en el directori**.

 

#### 2.1.3. Instal·lació i configuració del client NFS

El client NFS no requereix ni instal·lació ni configuració, ja que els directoris remots es poden importar mitjançant l'ordre _**mount**_. També es pot fer servir el fitxer _**/etc/fstab**_ si es vol que el directori es munti a l'inici. Les opcions de muntatge de cada directori es poden establir tant amb l'ordre _**mount**_ com amb el fitxer _**etc/fstab**_. Amb aquestes opcions es particularitza el comportament que tindrà el sistema d'arxius una vegada s'hagi muntat en el directori corresponent.

Per exemple, per muntar un sistema de fitxers NFS mitjançant l'ordre _mount_, podem fer servir la línia següent:

```text
sudo mount -t nfs 192.168.1.15:/home (servidor) /home (client)
```

Si es vol que el directori _/home_ es munti a l'inici de sessió, es pot afegir aquesta entrada permanent al fitxer _/etc/fstab_:

```text
192.168.1.15:/home /home nfs soft,users,suid,exec
```

L'opció _**exec**_ permet executar programes a la carpeta muntada i l'opció _**users**_ permet que els usuaris puguin utilitzar _mount_ per muntar i desmuntar aquest recurs.

Per **instal·lar el servidor NFS**, cal executar l'ordre següent:

```text
sudo apt-get install nfs-kernel-server
```

El paquet _**nfs-kernel-server**_ depèn del paquet _**nfs-common**_, el qual alhora depèn del paquet _**portmap**_. Per tant, tots tres s'instal·laran conjuntament amb l'ordre anterior.

El servidor necessita, a més dels dos dimonis _mountd_ i _nfsd_, un altre dimoni anomenat _**portmap**_. El funcionament dels dimonis _mountd_ i _nfsd_ es basa en el dimoni _**portmap**_. Així, doncs, la configuració d'un servidor NFS només necessita tenir disponibles aquests serveis i iniciar-los en el nivell d'execució 3 \(o 5 o bé tots dos\) de la màquina.

Per tal de parar, reiniciar, etc. el servei NFS, es poden fer servir els _scripts_ del servei que hi ha a la carpeta _/etc/init.d_.

Una vegada actius els serveis NFS, el servidor ha d'indicar explícitament quins directoris vol que s'exportin, a quines màquines s'han d'exportar i amb quines opcions s'ha de fer. Per això hi ha un fitxer de configuració denominat _**/etc/exports**_. Vegem un exemple d'aquest fitxer:

```text
# /etc/exports: the access control list for filesystems which may be exported
#to NFS clients. See exports(5).
# Example for NFSv2 and NFSv3:
#/srv/homes hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
# Example for NFSv4:
# /srv/nfs4 gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes gss/krb5i(rw,sync,no_subtree_check)
```

Cada línia del fitxer _**/etc/exports**_ especifica un directori a exportar, juntament amb una llista d'autoritzacions. És a dir, determina quins ordinadors podran muntar aquest directori i amb quines opcions ho podran fer. Cada element de la llista d'ordinadors pot especificar un sol ordinador \(mitjançant un nom simbòlic o una adreça IP\) o un grup d'ordinadors \(mitjançant l'ús de caràcters comodí, com \* o ?\). Quan no s'especifica l'ordinador o el rang, significa que el directori corresponent s'exporta a tots els ordinadors de la xarxa.

Les opcions de muntatge més importants que es poden especificar entre parèntesis per a cada ordinador o grup són les següents:

* **\(\)**: aquesta opció estableix les opcions que l'NFS assumeix per defecte.
* **ro**: el directori s'exporta com un sistema d'arxius només de lectura \(opció per defecte\).
* **rw**: el directori s'exporta com un sistema d'arxius de lectura/escriptura.
* **root\_squash**: els accessos des del client amb UID = 0 \(_root_\) es converteixen en el servidor en accessos amb UID d'un usuari anònim \(opció per defecte\).
* **no\_ root\_squash**: es permet l'accés des d'un UID = 0 sense conversió. És a dir, els accessos d'arrel \(_root_\) en el client es converteixen en accessos d'arrel en el servidor.
* **all\_squash**: tots els accessos des del client, amb qualsevol UID, es transformen en accessos d'usuari anònim.
* **anonuid, anongid**: quan s'activa l'opció _root\_squash_ o _allsquash_, els accessos anònims utilitzen normalment l'UID i el GID primari de l'usuari denominat _nobody_ si aquest usuari existeix en el servidor \(opció per defecte\). Si es volen utilitzar altres formes d'identificació, els paràmetres _anonuid_ i _anongid_ estableixen, respectivament, quins UID i GID tindrà el compte anònim que el servidor utilitzarà per accedir al contingut del directori.
* **noaccess**: impedeix l'accés al directori especificat. Aquesta opció és útil per impedir que s'accedeixi a un subdirectori d'un directori exportat.

És important destacar que cada vegada que es modifica aquest fitxer, el servidor NFS s'ha d'actualitzar mitjançant l'ordre _**exportfs –ra**_ a fi que s'activin els canvis.

Control NFS

El mecanisme per controlar quines màquines de la xarxa poden disposar dels fitxers compartits i quines no consistiria a utilitzar els fitxers _/etc/hosts.allow_ i _/etc/hosts.deny_.

En la pàgina de manual _exports_ hi ha una llista completa de les opcions de muntatge i el significat que tenen. Per accedir-hi, cal utilitzar l'ordre _**man exports**_.

És molt important remarcar que **no hi ha cap procés d'acreditació d'usuaris en l'NFS, de manera que l'administrador ha de decidir amb cautela a quins ordinadors exporta un determinat directori.**

Un directori sense restriccions s'exporta, en principi, a qualsevol altre ordinador connectat al servidor per mitjà de la xarxa \(també Internet\). Si en un ordinador client NFS hi ha un usuari amb un UID igual a _X_, aquest usuari accedirà al servidor NFS, per defecte, amb els permisos de l'usuari amb l'UID igual a _X_ del servidor, encara que es tracti d'usuaris distints.

La manca d'autenticació d'usuaris és un dels majors inconvenients del protocol NFS i per aquesta raó cada vegada s'utilitzen més altres sistemes de compartició de fitxers com, per exemple, el Samba.

 

### 2.2. Què és el Samba?

Podem definir el Samba de la manera següent:

Paquet de programari que implementa en sistemes basats en Unix, com GNU/Linux, una dotzena de serveis i una dotzena de protocols, entre els quals hi ha el NetBIOS sobre TCP/IP i l'SMB. Aquests serveis i protocols permeten que els equips d'una xarxa local comparteixin fitxers i impressores.

Els dos protocols més importants que formen el paquet de programari Samba són el NetBIOS i l'SMB.

 

#### 2.2.1. Protocol NetBIOS

NetBIOS sobre NetBEUI

Als anys noranta, els sistemes Windows van utilitzar el protocol NetBIOS sobre el NetBEUI com a sistema que proporcionava serveis de noms, serveis de sessió i serveis de distribució de datagrames sense connexió. El Windows no el suporta des de la versió 2000, tot i que s'hi pot instal·lar.

El NetBIOS \(_network basic input/output system_\) és un protocol del nivell de sessió \(model OSI\) que s'utilitza en xarxes locals i que s'encarrega de garantir l'accés a serveis de xarxa entre màquines, independentment del maquinari de xarxa que facin servir.

Principalment, el NetBIOS s'utilitza per identificar amb un nom els equips connectats per mitjà de xarxes locals, amb la finalitat d'establir una sessió i mantenir la connexió entre equips de la xarxa. El NetBIOS no pot transportar per si mateix les dades entre els nodes de la LAN. Per això ha de funcionar amb altres protocols com el TCP/IP, l'IPC/IPX i el NetBEUI.

El NetBIOS al Windows

En els sistemes Windows el protocol NetBIOS s'ha fet servir principalment per compartir arxius i impressores, i també per veure els recursos disponibles en “Entorn de xarxa”. Bona part de les crítiques de seguretat cap als entorns Windows se centren en el protocol NetBIOS. Per motius de seguretat, aquest protocol s'ha de deshabilitar sempre que no sigui imprescindible.

El NetBIOS ha de ser transportat per altres protocols perquè, en operar en la capa cinc del model OSI, no proveeix un format de dades per a la transmissió. Aquest format, doncs, el proveeixen aquests altres protocols.

El NetBIOS permet comunicació orientada a connexió \(TCP\) i no orientada a connexió \(UDP\). Suporta tant difusió \(_broadcast_\) com transmissió a grups \(_multicast_\), a més de quatre tipus de serveis diferents:

* Serveis generals
* Servei de noms
* Servei de sessió
* Servei de datagrames

Quan un programa d'aplicació necessita els serveis NetBIOS, el programa executa una interrupció de programari específica. Aquesta interrupció adreça el control del microprocessador al programari de l'adaptador de xarxa, el qual processa l'ordre. Quan un programa d'aplicació emet una interrupció NetBIOS, el servei NetBIOS requereix un servei de xarxa. La interfície NetBIOS defineix exactament la manera com els programes d'aplicació poden fer servir la interrupció NetBIOS i els serveis que proporciona.

 

#### 2.2.2. Protocol SMB

**El protocol SMB** \(_server message block_\) el va inventar originalment IBM, però avui dia la versió més comuna és la que Microsoft ha modificat àmpliament. El 1998 Microsoft va reanomenar aquest protocol CIFS \(_common Internet file system_\) i hi va afegir més característiques.

L'SMB és un protocol del nivell d'aplicació de tipus client-servidor en què l'ordinador que fa de servidor ofereix recursos \(arxius, impressores, etc.\) que els ordinadors clients poden fer servir remotament per mitjà de la xarxa.

L'SMB forma part dels protocols anomenats petició-resposta, ja que les comunicacions sempre s'inicien des del client com una petició de servei al servidor. El servidor la processa i torna una resposta a aquest client. La resposta del servidor pot ser positiva o negativa, en funció del tipus de petició, la disponibilitat del recurs, els permisos del client, etc.

El Samba incorpora tres nivells més de seguretat que l'SMB.

El protocol SMB incorpora dos nivells de seguretat. Són els següents:

* **Share-level**: protecció en el recurs compartit. S'assigna una contrasenya a cada recurs compartit. L'accés a cada recurs es permet en funció del coneixement d'aquesta contrasenya. Va ser el primer sistema de seguretat utilitzat amb l'SMB \(propi del Windows 3.11/95\).
* **User-level:** la protecció s'aplica a cada recurs compartit i es basa en drets d'accés de l'usuari. Els usuaris s'han d'autenticar en el servidor. Un cop identificat el client, se li assigna un UID que s'utilitza en els subsegüents accessos al servidor \(propi dels dominis Windows NT o 2000\).

El protocol SMB s'implementa habitualment amb el NetBIOS sobre el TCP/IP. Aquesta alternativa s'ha convertit en l'estàndard de fet per compartir recursos entre sistemes Windows.

 

#### 2.2.3. Característiques del Samba

El Samba és una implementació lliure del protocol SMB amb les extensions de Microsoft.

Nom Samba

Inicialment el Samba s'anomenava smbserver, però li van haver de canviar el nom a causa de problemes amb una altra empresa que tenia aquesta marca registrada. Per obtenir el nom nou van buscar una paraula al diccionari de GNU/Linux que contingués les lletres SMB:

```text
grep -i '^s.*m.*b' /usr/dict/words
salmonberry
samba
sawtimber
scramble
```

Essencialment, el Samba el constitueixen dos dimonis anomenats _**smbd**_ i _**nmbd**_. També fa servir el protocol **windbindd**, encara que no és essencial per al funcionament de l'aplicació. Els dimonis utilitzen el protocol NetBIOS per accedir a la xarxa, de manera que poden conversar amb ordinadors Windows.

El dimoni _**smbd**_ s'encarrega d'oferir els serveis d'accés remot a fitxers i impressores \(per fer-ho, implementa el protocol SMB\), a més d'autenticar i autoritzar usuaris. El dimoni _smbd_ ofereix les dues maneres de compartició de recursos del Windows: compartició basada en usuaris \(_user-level_\) o compartició basada en recursos \(_share-level_\).

El dimoni _**nmbd**_ permet que el sistema GNU/Linux participi en els mecanismes de resolució de noms propis del Windows, la qual cosa inclou el següent:

* L'anunci en el grup de treball.
* La gestió de la llista d'ordinadors del grup de treball.
* La contestació a peticions de resolució de noms.
* L'anunci dels recursos compartits.

D'aquesta manera, els sistemes GNU/Linux poden aparèixer en l'“Entorn de xarxa” de les màquines Windows, com qualsevol altre sistema Windows, i publicar la llista de recursos que ofereix a la resta de la xarxa.

El dimoni _windbindd_ proporciona el servei windbind, el qual resol els problemes d'inici de sessió unificats. El servei windbind es fa servir per resoldre informació d'usuaris i grups corresponent a un servidor Windows NT. El Winbind proporciona tres funcions separades:

* Autenticació d'usuaris \(via PAM\).
* Resolució d'identitat \(via NSS\).
* Manteniment d'una base de dades anomenada _winbind\_idmap.tdb_, en la qual emmagatzema les associacions entre usuaris UNIX UID/GID i NT SID. Aquesta associació només s'utilitza per a usuaris i grups que no tenen UID/GID locals.

El funcionament en conjunt d'aquests serveis permet transferir fitxers i informació entre sistemes GNU/Linux i Windows, els quals donen suport als protocols SMB/CIFS.

Gràcies al Samba, en una xarxa hi pot haver equips amb Windows i equips amb GNU/Linux de manera que puguin intercanviar informació en carpetes compartides i compartir impressores, tal com es faria si tots els equips fossin Windows o GNU/Linux.

L'avantatge principal del paquet de programari Samba és que és pràcticament equivalent a qualsevol servidor SMB/CIFS \(Windows NT o 2000, servidor Netware, servidor NFS UNIX, etc.\) i, a més, és programari lliure i gratuït. En la taula 1 es mostren els diversos rols o serveis per als quals es pot utilitzar el Samba.

Taula 1. Possibles rols d'un servidor Samba

| Usos del Samba |  |
| :--- | :--- |
| Servidor de fitxers | Sí |
| Servidor d'impressores | Sí |
| Servidor DFS de Microsoft | Sí |
| Controlador primari de domini \(PDC\) | Sí |
| Controlador de còpia de seguretat o _backup_ del domini \(BDC\) | No |
| Controlador de domini directori actiu | No |
| Autenticació Windows 95/98/Me | Sí |
| Autenticació Windows NT/2000/XP | Sí |
| Local master browser | Sí |
| Local backup browser | Sí |
| Master browser de domini | Sí |
| Servidor WINS primari | Sí |
| Servidor WINS secundari | No |
| Autenticació GNU/Linux | Sí |
| Integració LDAP | Sí |

Primordialment, el Samba permet que màquines GNU/Linux i Windows coexisteixin en la mateixa xarxa. De totes maneres, també es pot fer servir per compartir recursos en una xarxa formada íntegrament per màquines amb sistemes GNU/Linux.

 

### 2.3. Seguretat en el Samba

Abans d'iniciar el procés d'instal·lació i configuració del servidor i del client Samba, analitzarem els mecanismes de seguretat que ens proporciona el paquet de programari.

El Samba, a més dels nivells de seguretat que proporciona el protocol SMB \(_share_ i _user_\), incorpora tres subnivells de seguretat en el nivell d'usuari. Així, en total podem fer servir els nivells de seguretat següents:

* **share:** cada recurs compartit utilitza una paraula de pas. Tothom que sàpiga aquesta paraula de pas pot accedir al recurs.
* **user:** cada recurs compartit del grup de treball està configurat per permetre l'accés a un grup específic d'usuaris. En cada connexió inicial a un servidor Samba autentica l'usuari.
* **server:** el sistema és idèntic a l'anterior, però s'utilitza un altre servidor per obtenir la informació dels usuaris.
* **domain:** el Samba es converteix en membre d'un domini del Windows NT i utilitza un PDC \(_primary domain controller_\) o un BDC \(_backup domain controller_\) per implementar l'autenticació. Un cop autenticat l'usuari manté un testimoni o _token_ amb la informació de l'usuari, a partir de la qual es podrà determinar a quins recursos té accés.
* **ADS:** el Samba es comporta com a membre d'un domini de directori actiu i, per tant, requereix un servidor W2000 Server o W2003 Server.

 

#### 2.3.1. Share-level security

En el nivell de seguretat _share_, el client s'autentica de manera separada per a cada recurs al qual vol accedir. El funcionament d'aquest nivell de seguretat determina que cada recurs compartit tingui associat una paraula de pas amb independència de l'usuari que es connecti.

Tot i que els sistemes Windows associen la contrasenya a un recurs, el Samba utilitza l'esquema d'autenticació de GNU/Linux, en què la parella a autenticar és usuari-contrasenya i no pas recurs-contrasenya.

El client envia una paraula de pas cada cop que vol accedir a un recurs, però no envia cap usuari. Els sistemes GNU/Linux, no obstant això, sempre han d'utilitzar un usuari per autenticar-se. Així, doncs, quin usuari s'envia per fer la connexió?

Depèn dels paràmetres especificats en la configuració global del servidor, de manera que hi ha diverses possibilitats. Per exemple:

* Si s'utilitza el paràmetre _**guest only**_ en la configuració del recurs compartit al Samba, aleshores només es fa servir l'usuari convidat especificat amb el paràmetre _**guest account**_. Per defecte, _nobody_.
* Els sistemes Windows moderns i el Samba envien com a usuari per defecte l'usuari que està utilitzant, en el moment d'accedir al recurs, la màquina client.
* També es poden enviar a altres usuaris, com un inici de sessió previ, el nom del recurs al qual es vol accedir, el nom NetBIOS del client o els usuaris del paràmetre _user list_, depenent de les diferents configuracions.

El _guest only_ i el _guest account_ són paràmetres que hi ha en el fitxer de configuració _/etc/samba/smb.conf_ , els quals analitzem en l'apartat “Configuració d'un servidor Samba”.

Per tant, en iniciar una sessió, els clients passen un usuari al servidor \(sense contrasenya\). El Samba emmagatzema aquest usuari en una llista de possibles usuaris. Quan el client especifica una contrasenya i accedeix a un recurs concret, el Samba apunta el nom del recurs juntament amb els usuaris vàlids que apareguin en el fitxer _/etc/samba/smb.conf_ en la llista anterior. Seguidament, es comprova la paraula de pas de cadascun dels possibles usuaris. Si hi ha coincidència, aleshores s'autentica amb aquest usuari.

Quan aquesta llista no està disponible, aleshores el Samba envia una petició al sistema GNU/Linux per trobar l'usuari a qui correspon la contrasenya. Això es fa mitjançant l'NSS i la configuració del fitxer _**/etc/nsswitch.conf**_.

 

#### 2.3.2. User-level security

L'opció _user_ és l'opció per defecte i també és la més simple. El client s'identifica en el nivell de sessió amb un usuari i una paraula de pas. El servidor pot acceptar o denegar la sessió, però no necessàriament ha de saber a quins recursos vol accedir el client. En aquest nivell, doncs, per controlar l'accés als recursos el servidor només es pot basar en els elements següents:

* L'usuari i la paraula de pas.
* El nom de la màquina client.

Si s'accepta la sessió, el client pot accedir als recursos remots sense haver de tornar a especificar la paraula de pas. En aquest mètode és imprescindible que els usuaris apareguin com a usuaris de GNU/Linux i del Samba.

Aquest mètode de seguretat no és gaire recomanable si es volen compartir recursos de xarxa de manera anònima. De totes maneres, hi ha la possibilitat de generar un mode híbrid entre els nivells _user_ i _share_ mitjançant el paràmetre _**map to guest**_ de les opcions globals del fitxer _/etc/samba/smb.conf_. Aquest paràmetre pot tenir diversos valors i, per tant, el servidor també pot tenir diversos comportaments. Així, si establim un valor com _**Bad User**_, estem configurant un mètode en el qual, si la contrasenya de l'usuari falla, se'l rebutjarà, però si l'usuari no existeix, passarà automàticament a ser un usuari convidat, és a dir, s'activarà com a usuari _guest_.

 

#### 2.3.3. Domain security mode \(user-level security\)

En el cas _domain_, la base de dades d'usuaris està centralitzada en un controlador de domini i tots els membres d'un domini la comparteixen. Un servidor controlador primari de domini \(PDC\) és el responsable de mantenir la integritat de la base de dades de comptes de seguretat. Els _backup domain controllers_ \(BDC\) només proveeixen serveis d'autenticació i inici de sessió o _logon_.

Aleshores, amb aquest sistema de seguretat el servidor Samba és converteix en un servidor membre del domini, controlador primari de domini \(PDC\) o no. Per aquesta raó, totes les màquines que participen en el domini han de tenir un compte de màquina en la base de dades de seguretat.

El nivell de seguretat de domini utilitza un sistema de seguretat basat en l'usuari \(_user-level security_\), en el qual fins i tot les màquines s'han de validar en l'arrencada del sistema. El compte de màquina és un compte d'usuari més del Samba. L'única diferència que hi ha respecte al compte d'usuari és que el de màquina acaba en _$_. D'aquesta manera, el nom del compte serà _NETBIOS\_NAME$_.

La contrasenya es genera de manera aleatòria i només la coneixen els controladors de domini i la màquina membre. Si la màquina no es pot validar en iniciar el sistema, els usuaris no podran entrar al domini mitjançant aquesta màquina, ja que es considerarà que no és de confiança \(_not trusted machine_\).

Hi ha tres configuracions possibles de membres de domini:

* _Primary domain controller_ \(PDC\).
* _Backup domain controller_ \(BDC\).
* _Domain member server_ \(DMS\).

#### 2.3.4. ADS security mode \(user-level security\)

Les màquines amb una versió del Samba posterior a la 2.2 es poden unir a un domini de directori actiu. Això és possible si el servidor funciona en mode natiu, ja que el directori actiu en aquest mode accepta perfectament membres de domini de l'estil NT4.

A partir de la versió 3 del Samba, a més, el servidor Samba es pot afegir com a membre natiu de directori actiu. Això pot ser útil si hi ha una política de seguretat que prohibeix els protocols d'autenticació d'NT.

 

#### 2.3.5. Server security \(user-level security\)

Aquest mode es manté per compatibilitat enrere i existeix perquè abans era el mode que s'utilitzava quan el Samba no podia actuar com un PDC. No és gens recomanable fer servir aquesta opció perquè té moltes deficiències.

#### 2.3.6. Opcions de seguretat en l'apartat GLOBALS del fitxer de configuració

En el fitxer de configuració del Samba _**/etc/samba/smb.conf**_ es poden especificar una gran quantitat de paràmetres per garantir tant la seguretat d'accés al servidor com la seguretat d'accés als recursos compartits.



### 2.4. Instal·lació del servidor i del client Samba

Si utilitzem l'ordre _**apt-cache search samba**_ trobarem tots els paquets que conté el Samba.

El paquet de programari Samba es compon de moltes aplicacions i molts paquets amb diverses finalitats. Els paquets més utilitzats són els següents:

* **samba**: servidor d'arxius i impressores de xarxa local per a Unix/GNU/Linux.
* **smbclient**: client simple de xarxa local per a Unix/GNU/Linux.
* **samba-common**: arxius comuns del Samba que utilitzen els clients i els servidors.
* **samba-doc**: documentació del Samba.
* **smbfs**: ordres per muntar i desmuntar unitats de xarxa Samba.
* **winbind**: servei per resoldre informació d'usuaris i grups de servidors Windows.

Tots aquests paquets es poden trobar en els dipòsits del Debian.

#### smbclient

El més probable és que, per defecte, el gestor de paquets ja estigui instal·lat en el sistema. Si el tornem a instal·lar, ens ho dirà i potser l'actualitzarà si en troba una versió més recent.

Per instal·lar el servei i el client Samba  hi ha dues possibilitats. La primera consisteix a fer-ho de la manera tradicional, és a dir, utilitzar els gestors de paquets del sistema directament. Farem servir l'ordre següent:

```text
$sudo apt-get install samba smbclient smbfs
```

La segona possibilitat consisteix a utilitzar l'entorn gràfic. 

////Gràfic gnome compartir carpeta

L'ús d'aquest mecanisme d'instal·lació ens permet, a més, seleccionar algunes opcions de configuració del recurs a compartir.

Aquestes opcions ens permeten especificar si els usuaris que accedeixen al directori poden escriure \(crear elements a dins\) o no i si es permet l'accés a usuaris convidats, sense cap compte d'usuari Samba. Més endavant veurem la manera com especificar aquestes opcions en el fitxer de configuració del servei Samba.

Així, aquest serà el mètode que utilitzarem per compartir carpetes fàcilment mitjançant l'entorn gràfic.

Una vegada instal·lats els paquets relacionats amb el Samba mitjançant alguna de les possibilitats anteriors, podem consultar les ordres o les aplicacions que s'instal·len en el sistema amb l'ordre següent:

```text
dpkg -L samba | grep bin
/usr/bin
/usr/bin/eventlogadm
/usr/bin/smbstatus
/usr/bin/smbcontrol
/usr/bin/profiles
/usr/bin/tdbbackup
/usr/bin/pdbedit
/usr/sbin
/usr/sbin/smbd
/usr/sbin/nmbd
/usr/sbin/mksmbpasswd
```

```text
dpkg -L smbclient | grep bin
/usr/bin
/usr/bin/findsmb
/usr/bin/smbclient
/usr/bin/smbget
/usr/bin/smbtar
/usr/bin/rpcclient
/usr/bin/smbspool
/usr/bin/smbtree
/usr/bin/smbcacls
/usr/bin/smbcquotas
```

```text
dpkg -L smbfs | grep bin
/sbin
/sbin/mount.cifs
/sbin/umount.cifs
/usr/bin
/usr/bin/smbmount
/usr/bin/smbumount
/usr/bin/smbmnt
/sbin/mount.smbfs
/sbin/mount.smb
```

Després d'instal·lar el paquet Samba, el servei Samba arrenca automàticament. Per comprovar-ho, podem consultar amb l'ordre _nmap_ si l'equip escolta els ports que utilitza el Samba.

```text
sudo nmap localhost
Starting Nmap 4.76 ( http://nmap.org ) at 2010-02-21 10:14 CET
Warning: Hostname localhost resolves to 2 IPs. Using 127.0.0.1.
Warning: RateMeter::update: negative time delta; now=1266743641.46884;
last_update_tv=1266743641.46961
Interesting ports on localhost (127.0.0.1):
Not shown: 991 closed ports
PORT STATE SERVICE
139/tcp open netbios-ssn
445/tcp open microsoft-ds
```

Per defecte, el servidor Samba fa servir els ports 139 i 445.

Per aturar o reiniciar el servidor, farem servir les ordres

```text
/etc/init.d/samba stop/
```

o

```text
/etc/init.d/samba restart/
```

respectivament. Aquestes ordres reiniciaran els dimonis _nmbd_, _smbd_ i _windbindd_, necessaris per al funcionament del servei Samba. Haurem de reiniciar el servei cada vegada que vulguem que algun canvi de configuració es faci efectiu.

Per altra banda, podem configurar l'arrencada automàtica del servei Samba quan iniciem el sistema amb l'ordre següent:

```text
sudo update-rc.d samba defaults
```

Abans de veure el procés de configuració del servidor Samba, observarem com gestiona els usuaris, els grups i els permisos.

### 

### 2.5. Gestió d'usuaris, grups i permisos del Samba

El Samba és un servei que requereix l'administració dels usuaris per poder-ne gestionar els permisos.

En funció de l'usuari que hi accedeixi, el Samba es comportarà d'una manera o d'una altra. Quan hi accedeix un usuari normal, generalment té uns permisos limitats. En canvi, quan hi accedeix un usuari administrador, ha de disposar de tots els permisos.

Per tal que aquesta administració sigui possible, el Samba disposa de la seva pròpia base de dades d'usuaris Samba. No obstant això, com que els usuaris utilitzen altres recursos del servidor, com carpetes i impressores, cal que aquests usuaris també estiguin creats en el sistema GNU/Linux.

Per poder ser usuari del Samba, cal disposar d'un compte d'usuari a GNU/Linux i d'un compte d'usuari al Samba.

#### 2.5.1. Gestió d'usuaris Samba

La gestió d'usuaris Samba es fa amb l'ordre _**smbpasswd**_. Amb aquesta ordre podem crear i eliminar usuaris, canviar-ne la contrasenya i unes quantes coses més. Vegem-ne les diferents possibilitats.

**1. Creació d'usuaris Samba**. Per crear un usuari Samba hem d'utilitzar l'ordre _**smbpasswd**_. Abans de crear un usuari, però, aquest usuari ha d'existir en el sistema GNU/Linux.

Per exemple, suposem que volem que l'usuari _lluis_, gaudeixi dels serveis del Samba. Primerament haurem de crear l'usuari a l'Ubuntu amb l'ordre següent:

```text
sudo adduser lluis
```

Després, per habilitar-lo al Samba, executarem aquesta ordre:

```text
sudo smbpasswd –a lluis
```

L'opció _-a_ serveix per indicar al Samba que ha d'afegir l'usuari a la llista d'usuaris Samba. Tot seguit ens preguntarà dues vegades la contrasenya que volem establir a l'usuari. El més raonable és que aquesta contrasenya sigui la mateixa que l'usuari té a GNU/Linux.

**2. Eliminació d'usuaris Samba**. Per eliminar usuaris Samba també hem d'utilitzar l'ordre _smbpasswd_. Aquesta vegada, però, l'opció és la _–x_. Per exemple, per eliminar l'usuari _lluis_, executarem aquesta ordre:

```text
sudo smbpasswd -x lluis
```

L'usuari desapareixerà immediatament de la base de dades d'usuaris Samba, però continuarà essent un usuari de GNU/Linux.

**3. Altres opcions de** _**smbpasswd**_. L'ordre _smbpasswd_ disposa d'altres opcions que considerem interessants. Són les següents:

* **-d**: deshabilitar un usuari.
* **-i**: habilitar un usuari.
* **-n**: establir un usuari sense contrasenya. \(Necessita paràmetre _null passwords = yes_ en secció _GLOBAL_ de l'arxiu de configuració del Samba\).
* **-m**: indicar que és un compte de màquina.

Per a més informació es pot consultar la pàgina del manual del _smbpasswd_ amb l'ordre _**man smbpasswd**_.

Abans de veure la manera com el Samba gestiona els permisos d'usuaris i grups, cal tenir clar la diferència que hi ha entre permís i dret en els sistemes operatius.

#### 

#### 2.5.2. Permisos i drets Samba

Després d'identificar cada usuari amb accés al servei Samba, es poden especificar els permisos i els drets que té a la xarxa. L'administrador s'encarrega de determinar l'ús de cada recurs de la xarxa o les operacions que cada usuari pot dur a terme en cada estació de treball.

Per exemple, un usuari pot tenir el dret a accedir a un servidor per mitjà de la xarxa, a forçar l'apagada o el reinici d'un equip remotament, a canviar el sistema d'arrencada, etc. Alhora, cada recurs, servei o utilitat té una informació associada que li indica qui pot utilitzar-lo o executar-lo i qui no. Així, doncs, no hem de confondre dret amb permís. Vegem-ne la diferència:

Un **dret** autoritza un usuari o grup d'usuaris a fer determinades operacions sobre un servidor o una estació de treball.

Un **permís o privilegi** és una marca associada a cada recurs de xarxa \(fitxers, directoris, impressores, etc.\) que regula quins usuaris i de quina manera hi tenen accés.

D'aquesta manera, els drets fan referència a operacions pròpies del sistema operatiu com, per exemple, el dret a fer còpies de seguretat o a canviar l'hora del sistema. En canvi, els permisos fan referència a l'accés als diferents objectes de xarxa com, per exemple, el permís de llegir un fitxer concret.

Així, doncs, cada recurs té associat un grup de marques \(bits\) que determina els permisos que té cada usuari depenent del grup al qual pertanyi o de si és el propietari del recurs o no.

Els drets els determinen les accions que cada usuari pot desenvolupar en el sistema. Per exemple, si pertany al grup _root_ o al grup _sudo_.

Els drets prevalen sobre els permisos. Per exemple, un operador de consola pot tenir dret a fer una còpia de seguretat de tot un disc, però és possible que no pugui accedir a determinats directoris d'usuaris perquè no tingui permís per fer-ho. D'aquesta manera, podrà fer la còpia de seguretat, perquè el dret de còpia de seguretat preval sobre la restricció dels permisos, però no podrà llegir la informació que hi ha en els directoris si no té permís per fer-ho.

Dret i permís al Samba

Al Samba, l'opció _valid users_, per exemple, determina els usuaris que tenen dret a accedir al recurs, mentre que l'opció _read only_ determina els permisos que té l'usuari amb relació al recurs.

L'assignació de permisos en una xarxa es fa en dues fases:

1. En primer lloc, es determina el dret d'accés sobre el servei de xarxa. Per exemple, es pot assignar el dret a connectar-se al servidor Samba. Això evita que es puguin obrir unitats remotes de xarxa sobre les quals després no es tingui privilegis d'accés als fitxers que conté, cosa que podria sobrecarregar el servidor.
2. En segon lloc, s'han de configurar els permisos dels fitxers i els directoris que conté aquest servei de xarxa. Depenent del sistema operatiu de xarxa, les marques associades als recursos varien, encara que en general hi ha les de lectura, escriptura, execució, esborrament, etc. En xarxes en què coexisteixen sistemes operatius de xarxa de distints fabricants, cal determinar els permisos per a cada sistema.

 

#### 2.5.3. Gestió de grups i permisos

La gestió de grups, usuaris i permisos és diferent en sistemes GNU/Linux i en sistemes Microsoft Windows. En els sistemes GNU/Linux, la gestió dels permisos que els usuaris i els grups tenen sobre els arxius es fa mitjançant un esquema senzill de tres tipus de permisos \(lectura, escriptura i execució\) aplicables a tres tipus d'usuaris \(propietari, grup propietari i resta d'usuaris\).

Aquest esquema es va desenvolupar als anys setanta i avui encara és adequat per a la gran majoria dels sistemes en xarxa que hi ha en qualsevol tipus d'organització, tant si es tracta de xarxes petites com de xarxes grans.

És cert que té algunes limitacions, però té l'avantatge de ser senzill. Això fa que sigui fàcil d'administrar i que el rendiment sigui molt elevat.

En els sistemes Windows, la gestió dels permisos que els usuaris i els grups tenen sobre els arxius es fa mitjançant un esquema complex de llistes de control d'accés \(_access control lists_, ACL\) per a cada directori i arxiu. El sistema ACL té l'avantatge de ser molt més flexible que el sistema GNU/Linux, ja que es poden establir més tipus de permisos, donar permisos només a uns quants usuaris i grups, denegar permisos, etc. Com s'ha comentat anteriorment, però, en la majoria de casos n'hi ha prou amb les prestacions del sistema GNU/Linux.

D'altra banda, el sistema ACL és més complex d'administrar i més lent, ja que abans d'accedir a les carpetes o als arxius el sistema ha de comprovar les llistes. En sistemes de GNU/Linux, en canvi, es fa una operació lògica dels bits que especifiquen els permisos, de manera que és molt més ràpid.

Per defecte, el Samba utilitza el sistema de permisos de GNU/Linux. Tot i que també pot implementar el sistema ACL i gestionar les llistes mitjançant l'ordre _**smbcacls**_, és més recomanable fer servir el sistema de gestió de permisos de GNU/Linux.

Quan compartim directoris amb el Samba, en última instància sempre imperen els permisos GNU/Linux.

Per exemple, si tenim compartida una carpeta anomenada _professors_ amb permisos d'escriptura per al grup _professors_, tots els usuaris que pertanyin al grup _professors_ podran efectuar canvis en la carpeta. No obstant això, si dins d'aquesta carpeta n'hi ha una altra que s'anomena _confidencial_, a la qual el grup _professors_ no té permís per entrar, cap professor en podrà veure el contingut, encara que sigui dins d'una carpeta compartida.

Per fer una gestió eficaç d'usuaris, grups i permisos, es recomana fer servir els permisos GNU/Linux, els quals permeten assignar permisos de lectura, escriptura i execució a l'usuari propietari de l'arxiu, al grup propietari de l'arxiu i a la resta d'usuaris del sistema.

Pot ser que hi hagi alguna contradicció entre els permisos del sistema GNU/Linux i els permisos del recurs compartit a Samba.

Per exemple, podem tenir un directori compartit anomenat _magatzem_ amb permisos GNU/Linux de lectura, escriptura i execució per a tots els usuaris del sistema. Tanmateix, si en l'arxiu de configuració del Samba aquest recurs té el paràmetre _read only = yes_, no s'hi podran efectuar canvis, ja que està compartit amb permís només de lectura.

Quan els permisos GNU/Linux es contradiuen amb els permisos Samba, el permís efectiu és el més restrictiu.

Per simplificar l'administració dels permisos, es recomana no ser restrictius en els permisos de recurs compartit amb el Samba i aplicar els permisos en el sistema GNU/Linux. D'aquesta manera, a més de ser efectius quan accedim al recurs per mitjà del Samba, també ho serem quan hi accedim d'una altra manera, com per SSH, FTP o mitjançant la consola del servidor.

 

### 2.6. Configuració del servidor Samba

La configuració del servidor Samba es fa a partir del fitxer _**/etc/samba/smb.conf**_.

La sintaxi de l'arxiu de configuració del Samba és bastant senzilla, ja que està dividit en seccions que es limiten a establir el valor d'uns quants paràmetres i a determinar quines són les carpetes i les impressores compartides, i també els permisos que hi ha. A més, l'arxiu va donant exemples de com hauríem de configurar alguns recursos per compartir-los, com perfils, CD-ROM, etc.

**Amb l'edició de l'arxiu** _**/etc/samba/smb.conf**_ **es poden configurar més de tres-cents paràmetres**, cosa que dóna lloc a milers de configuracions. Nosaltres ens limitarem a analitzar els paràmetres més rellevants per garantir la seguretat i establir la compartició d'arxius i impressores del servidor.

#### Ordres de configuració del Samba



En l'arxiu _/etc/samba/smb.conf_ hi ha tres seccions predefinides \(_global_, _homes_ i _printers_\) i tantes seccions addicionals com recursos extra es vulguin compartir. La utilitat i alguns dels paràmetres d'aquestes seccions predefinides es descriuen breument, ja que resulta necessari coneixer-los, per a configurar correctament Samba:

**1. \[global\]** . Defineix els paràmetres a escala global del servidor Samba, a més d'alguns dels paràmetres que s'establiran per defecte en la resta de les seccions.

En la taula 2 es mostren els paràmetres més significatius amb el valor per defecte i exemples:

Taula 2. Opcions principals de la configuració global del Samba

| Opció | Significat | Valor per defecte | Exemple |
| :--- | :--- | :--- | :--- |
| **netbios name** | Nom \(NetBIOS\) de l'ordinador Samba | Primer component del nom DNS de l'ordinador. | UBUNTUSRV |
| **workgroup** | Nom del domini \(o grup de treball\) al qual pertany el Samba. | WORKGROUP | SOX |
| **security** | Nivell de seguretat. Permet determinar el mode de compartició de recursos. Hi ha cinc valors possibles: _share_, _user_, _domain_, _server_ i _ads_. | user | user |
| **encrypt passwords** | Ús de contrasenyes xifrades. L'opció més recomanable és que les contrasenyes s'enviïn xifrades per impedir que altres usuaris puguin descobrir-les, per exemple, capturant paquets de dades \(_sniffing_\). Les contrasenyes encriptades del Samba s'emmagatzemen en un altre arxiu. Per defecte, _/etc/samba/smbpasswd_. | yes | yes |
| **hosts allow** | Permet especificar des de quines adreces IP es podrà accedir al servei. | buit | Si posem 10.0.2. permetem l'accés a totes les màquines de la xarxa l'adreça IP de les quals comenci per 10.0.2 |
| **host deny** | Igual que _hosts allow_, però per especificar els rangs d'adreces no permesos. | buit | Si posem 10.10. deneguem l'accés a totes les màquines de la xarxa l'adreça IP de les quals comenci per 10.10. |
| **map to guest** | Estableix en quines condicions un accés al Samba s'ha de considerar en mode convidat. Pot tenir quatre valors possibles. _**Never**_: es rebutjarà l'usuari amb una contrasenya incorrecta. _**Bad User**_: es rebutjarà l'usuari amb una contrasenya incorrecta, però si l'usuari no existeix, passarà a ser un usuari convidat. _**Bad Password**_: l'usuari amb una contrasenya incorrecta es tractarà com a convidat. És perillós en possibles equivocacions a l'hora d'introduir la contrasenya. _**Bad UID**_: només s'utilitza si els modes de seguretat són _domain_ o _ads_. L'usuari que s'autentiqui correctament, però que no sigui un usuari GNU/Linux, es tractarà com a convidat. | Never | Bad User, implementa un sistema híbrid si el mode de seguretat és _user_. |
| **usershare allow guests** | Permet la compartició de recursos amb usuaris convidats. | yes | yes |
| **guest account** | Nom d'usuari amb el qual els usuaris convidats es validaran en el sistema. | nobody | nobody |
| **valid users** | Defineix quins usuaris o grups poden accedir als recursos compartits. Es poden especificar múltiples usuaris separats per comes o noms de grup amb l'arrova \(@\) al davant. | buit | raul,lluis, @administradors |
| **invalid users** | Igual que _valid users_, però per als usuaris que no poden accedir als recursos. | buit | ana, angela,@alumnes |
| **admin users** | Defineix quins usuaris poden accedir amb permisos d'administració \(superusuaris\) als recursos compartits. | buit | toni,@administradors |
| **write list** | Defineix quins usuaris poden accedir amb permisos d'escriptura als recursos compartits. | buit | lluis, @professors |
| **log file** | Fitxer en què s'emmagatzemen els registres del Samba. | /var/log/samba/log.%m \(%m significa el nom NetBIOS de la màquina client\). | ídem |

**2. \[homes\]**. En aquesta secció es defineix automàticament un recurs de xarxa per a cada usuari conegut pel servidor Samba. Aquest recurs, per defecte, està associat al directori de connexió de cada usuari en l'ordinador en el qual el servidor Samba està instal·lat, és a dir, el directori de l'usuari \(_home directory_\). Aquesta secció és opcional, és a dir, si no existeix, no es compartiran les carpetes dels usuaris del servidor. S'utilitza quan es volen crear perfils mòbils per tal que, quan l'usuari s'identifiqui en qualsevol dels equips de la xarxa, el perfil s'escanegi automàticament.

El funcionament del servei Samba determina que, quan es faci una sol·licitud de connexió a un recurs compartit, s'escanegin les seccions que hi hagi en el fitxer _**/etc/samba/smb.conf**_ mitjançant la cerca del nom del recurs. Si es troba una coincidència, s'utilitzen els paràmetres de la secció, amb el mateix nom que el recurs sol·licitat, per determinar les propietats i la configuració del recurs.

Si no es troba cap coincidència, el nom de la secció sol·licitada es tracta com un nom d'usuari i se'l cerca en l'arxiu de contrasenyes locals. Si el nom existeix i la contrasenya és correcta, es crea un recurs amb el nom d'usuari, el directori de l'usuari s'estableix com a camí o _path_ del recurs i la resta de paràmetres del recurs es copien dels que s'han especificat en la secció \[homes\], si n'hi ha. Si l'usuari no es troba en l'arxiu de contrasenyes locals, es rebutja la connexió al recurs.

La configuració normal de la carpeta de l'usuari \(_home_\) serà la següent:

```text
[home]
path=/home/%u
read only=no
```

Aquí, _%u_ és el nom de l'usuari amb el qual ens hem connectat al recurs.

Aquesta és una manera ràpida i senzilla de donar accés als directoris a un gran nombre de clients amb un esforç mínim.

Aquesta secció pot especificar tots els paràmetres de les seccions dels recursos nous a compartir, encara que alguns paràmetres tindran més sentit que d'altres.

Hem de tenir en compte que si permetem que usuaris convidats accedeixin a la secció \[homes\], tots els directoris d'inici seran visibles i/o modificables si no hem especificat el paràmetre que només permet la lectura, cosa que, des del punt de vista de la seguretat, és poc recomanable. Per tant, per accedir al directori de l'usuari, hem d'especificar directament que ens volem connectar al directori d'inici concret de l'usuari, ja que cal, per seguretat, que els usuaris no puguin veure els directoris de la resta \(_browsable = no_\).

**3. \[printers\]**. Aquesta secció funciona com \[homes\], però per a les impressores.

Si es troba una secció \[printers\] en l'arxiu de configuració, es permet que els usuaris es connectin a qualsevol impressora especificada en el fitxer _**/etc/printcap**_ de l'ordinador central o _host_ local.

Quan es fa una sol·licitud de connexió a un recurs, s'escanegen les seccions que hi ha en el fitxer _/etc/samba/smb.conf_. Si es troba alguna coincidència amb el nom del recurs sol·licitat, s'utilitzen els paràmetres de la secció amb el mateix nom per determinar les propietats i la configuració del recurs. Si no hi ha coincidències, però hi ha una secció \[homes\], se segueix el procés que s'ha descrit en la secció anterior. En cas que no hi hagi cap secció \[homes\], el nom de la secció \(recurs\) sol·licitada es tracta com un nom d'impressora i s'analitza l'arxiu _/etc/printcap_ per comprovar si aquest nom és un nom vàlid d'impressora compartida. Si es troba una coincidència, es crea una secció nova amb el nom de la secció buscada i amb els paràmetres especificats en la secció \[printers\] per defecte.

Si no es troba cap coincidència, la connexió al recurs es rebutja.

Normalment el path especificat en la secció \[printers\] és un directori de cua d’escriptura accessible per tots els usuaris amb un sticky bit.

A fi que el comportament sigui aquest, el paràmetre _**printable**_ de la secció \[printers\] ha de tenir el valor _yes_, ja que si s'especifica el contrari, és a dir, el valor _no_, el servidor es negarà a carregar el fitxer de configuració.

Un exemple típic de configuració d'aquesta secció és el següent:

```text
[printers]
path = /var/spool/samba
guest ok = yes
printable = yes
```

**4. Recursos nous a compartir**. Cada vegada que es vol compartir un recurs \(un directori, una impressora, etc.\), cal crear una secció nova amb un encapçalament entre claudàtors. L'encapçalament d'aquesta secció es correspondrà amb el nom que el recurs tindrà a la xarxa \(el nom mitjançant el qual es podrà accedir al recurs des d'una altra màquina\). Generalment es fa servir el mateix nom de la impressora o la carpeta a compartir per tal que sigui més aclaridor.

Per exemple, si volem compartir la carpeta _/home/samba/alumnes_, crearem una secció \[alumnes\] en què aquest recurs compartit es configurarà amb els paràmetres específics.

En la taula 3 es descriuen unes quantes opcions aplicables a cada recurs compartit. També es poden establir en la secció global, cas en què s'utilitzaran com a valors per defecte per a cada recurs compartit:

Taula 3. Opcions principals de la configuració dels recursos compartits \(//shares//\) del Samba

| Opció | Significat | Valor per defecte | Exemple |
| :--- | :--- | :--- | :--- |
| **read only/writable** | Defineix si es permet l'escriptura en el recurs o no, només té sentit en carpetes compartides. | yes | yes |
| **browseable / public** | Determina si el recurs apareix en la llista de recursos compartits en explorar el servidor Samba. | yes | yes |
| **path** | Especifica la ruta absoluta al directori compartit pel recurs. | buit | /home/ioc/professors |
| **comment** | Descriu el recurs mitjançant una cadena de caràcters. | buit | Directori en què els professor deixen els apunts. |
| **guest ok** | Determina si es permet accedir com a convidat al recurs. | no | yes |
| **guest only** | Especifica que tots els accessos al recurs s'accepten en mode convidat. | no | no |
| **create mask** | Estableix la màscara de creació d'arxius, que determinarà els permisos dels usuaris. Té la mateixa funció que l'opció _directory mask_ per a la creació de directoris. | 0770 | 0775 |
| **hosts allow** | Proporciona una llista d'ordinadors des dels quals es permet accedir al recurs. S'hi afegeixen els que s'han especificat en la secció \[global\]. | buit\(vol dir tots els ordinadors\) | 192.169. |
| **hosts deny** | Proporciona una llista d'ordinadors des dels quals no es permet accedir al recurs. En cas de conflicte, preval el que s'indica en l'opció _hosts allow_ de la secció \[global\]. | buit\(vol dir cap ordinador\) | 10.0. |
| **valid users** | Proporciona una llista d'usuaris que poden accedir al recurs. En cas de conflicte, preval el que s'indica en la secció \[global\]. | buit\(vol dir tots els usuaris\) | pere, @pas |
| **invalid users** | Proporciona una llista d'usuaris que no poden accedir al recurs. En cas de conflicte, preval el que s'indica en la secció \[global\]. | buit\(vol dir cap usuari\) | profe,@admin |
| **read list** | Proporciona una llista d'usuaris que només tindran permisos de lectura en el recurs. Si el paràmetre és _read only = yes_, per defecte tots els usuaris només tindran permís de lectura. | buit | @alumnes |
| **write list** | Proporciona una llista d'usuaris que tindran permís de lectura i escriptura en el recurs. Ignora l'opció _read only = yes_ | buit | @professors |
| **printable** | Permet determinar, en cas d'impressores, si el client pot obrir, escriure i enviar fitxers de gestió o _spool_ de cues al directori especificat per tal d'imprimir-los. | yes | yes |
| **printing** | Aquesta opció determina l'estil o el sistema d'impressió utilitzat pel Samba. | cups | cups |

Recomanacions durant la configuració del Samba:

* És convenient crear en el directori _/home_ una carpeta anomenada _samba_ que contingui totes les carpetes compartides. La finalitat és tenir totes les dades d'usuari dins del directori de l'usuari i que fer les còpies de seguretat sigui senzill.
* És convenient crear una còpia de seguretat de l'arxiu _/etc/samba/smb.conf_ abans de fer cap canvi. La finalitat és poder tornar a l'estat anterior en cas que fem una modificació incorrecta de l'arxiu que impedeixi que el servei arrenqui. El Samba analitza cada 60 segons l'arxiu _/etc/samba/smb.conf_ i, si hi ha hagut canvis, es fan efectius.
* Per comprovar que el nostre arxiu _/etc/samba/smb.conf_ és correcte, podem utilitzar l'ordre _testparm_, la qual analitza cada línia per localitzar-hi errors.
* Per tenir una descripció detallada de tots els paràmetres, es pot consultar la pàgina del manual d'_/etc/samba/smb.conf_ amb l'ordre _man smb.conf_.











### 2.7. Utilització del client Samba

El client Samba ens proporciona una ordre per accedir als recursos compartits dels servidors Samba disponibles per mitjà de la xarxa: _smbclient_. L'ordre _smbclient_ és una petita aplicació que ens permet accedir als servidors Samba com a clients, com si es tractés d'una mena d'accés FTP. S'utilitza sobretot per saber quins recursos Samba ens ofereix una màquina remota. Per exemple, la sintaxi per llistar els recursos d'una màquina remota és la següent:

```text
smbclient -U usuario -L NET_BIOS_NAME
```

En cas que no hi tinguem accés, ens mostrarà el missatge següent:

```text
$smbclient -U luis -L SRV
Password:
session setup failed: NT_STATUS_LOGON_FAILURE
```

També hi podem accedir de manera anònima:

```text
smbclient -N -L SRV
Anonymous login successful
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.3.2]
Sharename Type Comment
--------- ---- -------
material APUNTS Disk
apunts Disk
IPC$ IPC IPC Service (UBUNTUsrv-laptop server (Samba, Ubuntu))
print$ Disk Printer Drivers
homes Disk
Anonymous login successful
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.3.2]
Server Comment
-------- -------
SOX UBUNTUsrv-laptop server (Samba, Ubuntu)
Workgroup Master
-------- -------
WORKGROUP SOX
```

Per connectar-nos al recurs que ens interessa, haurem de fer servir aquesta ordre:

```text
smbclient //NETBIOS_NAME/Recurs
```

Si el recurs està protegit amb contrasenya, hi haurem d'afegir l'opció _–U_ amb el nom d'usuari. Després d'executar l'ordre, ens demanarà la contrasenya. L'ordre quedarà així:

```text
smbclient -U clientsamba //UBUNTUSRV/apunts
Enter clientsamba's password:
Domain=[UBUNTUSRV] OS=[Unix] Server=[Samba 3.3.2]
smb: \>
```

Quan accedim al recurs compartit, disposem d'una línia d'ordres. Tot i així, també podem executar les ordres típiques del servei FTP, com _put_ o _get_, entre altres. Per tal que ens mostri totes les ordres que podem utilitzar, hem d'executar l'ordre _help_:

```text
smb: \> help
? altname archive blocksize cancel
case_sensitive cd chmod chown close
del dir du exit get
getfacl hardlink help history lcd
link lock lowercase ls mask
md mget mkdir more mput
newer open posix posix_open posix_mkdir
posix_rmdir posix_unlink print prompt put
pwd q queue quit rd
recurse reget rename reput rm
rmdir showacls setmode stat symlink
tar tarmode translate unlock volume
vuid wdel logon listconnect showconnect
```

També podem fer servir les ordres de navegació per al sistema de fitxers de GNU/Linux \(_cd_, _ls_\) i algunes de les ordres habituals de modificació de fitxers \(_rm_, _mkdir_, _del_ o _rename_\), sempre que tinguem permisos.

Suposem, per exemple, que ens volguéssim connectar a la carpeta de l'usuari \(_home_\) _clientsamba_ i que tinguéssim la secció _HOMES_ habilitada en el servidor Samba. En aquest cas, hauríem de fer servir l'ordre següent:

```text
 smbclient -U clientsamba //IOC/clientsamba
 Enter clientsamba's password:
 Domain=[IOC] OS=[Unix] Server=[Samba 3.3.2]
 smb: \> ls
 . D 0 Sun Mar 21 19:16:12 2010
 .. D 0 Mon Mar 22 11:09:38 2010
 .profile H 675 Sun Mar 21 19:16:12 2010
 examples.desktop 357 Sun Mar 21 19:16:12 2010
 .bash_logout H 220 Sun Mar 21 19:16:12 2010
 .bashrc H 3115 Sun Mar 21 19:16:12 2010
 61335 blocks of size 131072. 26464 blocks available
```

Tot i que l'ordre _smbclient_ és molt útil, aquesta manera de treballar pot resultar una mica enutjosa. Hi ha, però, la possibilitat de muntar les unitats de xarxa a les quals volem accedir en directoris del nostre sistema, com si es tractés de directoris locals. Per això haurem de tenir instal·lat el paquet _**smbfs**_.

 

### 2.8. Muntar unitats de xarxa

GNU/Linux disposa de suport per al sistema de fitxers SMB. Així, GNU/Linux, de la mateixa manera que pot muntar un directori exportat via NFS en un directori local, pot muntar un recurs SMB ofert per un servidor SMB, com un sistema Windows o un servidor Samba.

No obstant això, com ja hem comentat, hi ha una diferència entre l'NFS i l'SMB. L'NFS no requereix que l'usuari que fa la connexió s'autentiqui. Aquest servidor fa servir el UID de l'usuari de l'ordinador client per accedir als fitxers i als directoris exportats.

Realment, l'ordre **mount -t smbfs** es reencamina a _smbmount_.

Un servidor SMB, per contra, sí que requereix que l'usuari s'autentiqui, i per això necessita un nom d'usuari i una contrasenya. Per muntar un recurs SMB podem fer servir les ordres _smbmount_ o, directament, l'ordre _mount_ si li indiquem un tipus de sistema d'arxius específic \(en aquest cas, _smbfs_\). La sintaxi d'aquestes dues ordres seria la següent:

```text
smbmount -username=usuari -password=contrasenya -workgroup=MEUGRUP //servidor_Samba/recurs
/punt_de_muntatge/

mount -t smbfs –o username=usuari,password=contrsenya,workgroup=MEUGRUP
//servidor_Samba/recurs /punt de muntatge
```

Si el servidor no requereix que l'usuari s'autentiqui \(permet accés a convidats\), els paràmetres _username_, _password_ i _workgroup_ es poden obviar.

Si en les ordres anteriors s'omet l'opció _password_, el sistema sol·licita a l'usuari que introdueixi una contrasenya. Si el servidor SMB permet l'accés a l'usuari, s'aconsegueix accedir al recurs \(en aquest cas, _servidor\_Samba/recurs_\) a partir del directori local que hem establert com a punt de muntatge.

En el muntatge de sistemes d'arxius, també podem optar per registrar el muntatge en el fitxer _**/etc/fstab**_. Així, els directoris es poden muntar automàticament en l'arrencada del sistema. No obstant això, en el cas del sistema d'arxius _smbfs_, aquest registre presenta un problema, ja que el muntatge sempre implica la petició d'una contrasenya. Aquesta contrasenya es pot especificar en les opcions de muntatge o bé es pot sol·licitar per teclat en el moment de fer el muntatge.

Òbviament, aquesta última opció dificulta el muntatge automàtic en l'arrencada, tret que escrivim la contrasenya en el fitxer _/etc/fstab_. Això, per motius de seguretat, no és gaire recomanable, ja que qualsevol usuari pot llegir aquest arxiu. L'alternativa consisteix a utilitzar un fitxer d'identificacions d'usuaris \(opció de muntatge _credentials=FITXER_\) en què s'haurà d'escriure el nom i la contrasenya de l'usuari.

El nom típic del fitxer d'identificacions d'usuaris és _.smbpasswd_ i s'emmagatzema a la carpeta de l'usuari \(_home_\).

A pesar que en aquest fitxer la contrasenya també s'escriu en text pla, constitueix una mesura de seguretat suficient, ja que aquest fitxer només el pot llegir l'usuari que fa el muntatge, com ara el primari \(_root_\).

Vegem un exemple del fitxer _/etc/fstab_ configurat per muntar recursos Samba remots en l'arrencada del sistema:

* **Muntar de manera permanent un recurs anònim**.

```text
 //UBUNTUSRV/apunts /mnt smbfs user,auto,guest,ro,gid=100 0 0
```

* **Muntar de manera permanent un recurs protegit**.

```text
 //UBUNTUSRV/Materials /mnt smbfs username=clientsamba,password=smix 0 0
```

* **Muntar un recurs protegit amb el fitxer d'identificadors d'usuaris**.

```text
 //UBUNTUSRV/professor /mnt smbfs credentials=/home/clientsamba/.smbpasswd 0 0**
```

Un inconvenient addicional és que, si les unitats es munten d'aquesta manera, l'únic usuari que hi podrà escriure serà el primari. Si volem que múltiples usuaris tinguin permís de lectura i escriptura en la unitat muntada, haurem de crear un grup \(anomenat, per exemple, _sambausersgroup_\) i afegir-hi els usuaris. El fitxer _/etc/fstab_ quedarà així:

```text
//UBUNTUSRV/professor /mnt smbfs credentials=/home/clientsamba/
.smbpasswd,gid=sambausersgroup 0 0
```

Per altra banda, també pot ser molt útil permetre que els usuaris que no tinguin permisos de superusuari puguin muntar unitats Samba remotes. Per fer-ho, haurem de seguir una sèrie de passos:

**1.** Crear un grup i afegir-hi els usuaris.

```text
sudo groupadd samba
sudo adduser user samba
```

**2.** Editar _sudo_ per permetre que els usuaris del grup puguin muntar unitats Samba.

```text
sudo visudo
## Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL
%samba ALL=(ALL) /bin/mount,/bin/umount,/sbin/mount.cifs,/sbin/umount.cifs
```

Ara tots els usuaris del grup afegit podran muntar unitats Samba remotes.

Es recomana consultar la pàgina de manual _smbmount_ per obtenir més detalls sobre les opcions de muntatge. Ho podem fer mitjançant l'ordre _**man smbmount**_.

torna a dalt

### 2.9. Accés gràfic als recursos compartits

L'Ubuntu ens permet accedir gràficament als recursos disponibles dels grups de treball \(paràmetre _workgroup_ del Samba\) que hi ha a la xarxa local amb el navegador Nautilus, per mitjà del menú _**Llocs**_ **&gt;** _**Xarxa**_.

En seleccionar aquesta opció del menú, se'ns obrirà una finestra del Nautilus en què ens apareixeran tots els grups de treball \(dominis\) que hi hagi a la xarxa local. Si fem doble clic a cadascun dels grups, ens mostrarà els servidors Samba disponibles. Per veure els recursos que comparteix cada servidor, haurem de fer doble clic al damunt de la icona amb el nom. Aleshores, o bé hi podrem accedir lliurement perquè el servidor permet l'accés a usuaris convidats o bé haurem d'especificar el nom d'usuari Samba i la contrasenya adequada. En accedir a qualsevol servidor Samba, automàticament es muntaran totes les carpetes compartides del servidor, cosa que ens permetrà gestionar més fàcilment els recursos als quals tinguem accés. L'accés a cada recurs pot ser lliure o pot requerir un nom d'usuari Samba i una contrasenya.

Per moure'ns per les carpetes, els servidors i els grups també podem utilitzar, a més dels clics, la barra de cerques _Ubicació_. La sintaxi de les adreces en la barra _**Ubicació**_ és la següent:

```text
smb://nom_servidor/recurs
```

 

### 2.10. Servidor d'impressió CUPS

En els sistemes GNU/Linux hi ha un sistema d'impressió estàndard que ens permet centralitzar, compartir i gestionar impressores instal·lades en una màquina que fa les tasques de servidor d'impressió. L'eina que ens proporciona aquest sistema d'impressió és el CUPS.

El CUPS \(_common Unix printing system_, sistema d'impressió comú d'Unix\) és un sistema d'impressió modular per a sistemes operatius de tipus Unix/GNU/Linux que permet que un ordinador actuï com a servidor d'impressió.

El CUPS es basa en el protocol IPP \(_Internet printing protocol_, protocol d'impressió per Internet\), el qual permet compartir impressores per mitjà de xarxes TCP/IP. El CUPS és programari lliure i es distribueix sota llicència GPL i LGPL.

![Esquema](https://agora.xtec.cat/escolesnuria/moodle/file.php/102//uf3/t2/CUPSeSQUEMA.png)

Esquema CUPS

La gran majoria de distribucions GNU/Linux utilitzen el CUPS com a sistema d'impressió per defecte.

Un ordinador que executa el CUPS actua com un servidor que pot acceptar tasques d'impressió des d'altres ordinadors clients, les processa i les envia a la impressora apropiada.

El CUPS és format per una cua d'impressió amb un planificador, un sistema de filtres, el qual converteix les dades que s'han d'imprimir en formats que la impressora conegui, i un sistema de suport \(_back-end_\) que envia les dades al dispositiu d'impressió.

El CUPS, a més d'utilitzar el protocol IPP com a base per al maneig de tasques d'impressió i cues d'impressió, també proveeix les ordres tradicionals de línia d'ordres d'impressió dels sistemes GNU/Linux.

El CUPS també disposa d'un suport limitat d'operacions sota el protocol SMB, el quals s'utilitza, en aquest cas, per compartir impressores.

 

#### 2.10.1. Funcionament del CUPS

El CUPS proveeix un mecanisme que permet enviar treballs d'impressió a impressores de manera estandarditzada. La informació s'envia al planificador, el qual envia el treball a un sistema de filtres que el converteix a un format que la impressora pugui comprendre. Després, el sistema de filtres envia les dades a un _back-end_, un filtre especial que envia les dades destinades a la impressora a un perifèric o una connexió de xarxa. El sistema fa un ús extensiu del llenguatge PostScript i de tramatge de les dades a fi de convertir-les a un format que la impressora accepti.

El CUPS té com a avantatge principal que és un sistema d'impressió estandarditzat i modularitzat, capaç de processar diferents formats de dades en el servidor d'impressió. El CUPS permet als fabricants d'impressores i als desenvolupadors de controladors crear més fàcilment controladors que funcionin nativament en el servidor d'impressió.

El processament de la informació a imprimir ocorre en el servidor, motiu pel qual permet sistemes d'impressió basats en xarxa molt més senzills que altres sistemes d'impressió Unix. Quan el CUPS es fa servir amb el Samba, les impressores també es poden utilitzar en ordinadors Windows remots per imprimir per mitjà de la xarxa.

 

#### 2.10.2. Instal·lació i configuració del CUPS

La instal·lació de l'eina CUPS la podem fer mitjançant les ordres de gestió de paquets del sistema amb els paquets adequats. Així, doncs, l'ordre d'instal·lació seria la següent:

```text
sudo apt-get install cupsys cupsys-client cups-pdf
```

**cupsys**, és el paquet que ens instal·la el servidor CUPS.

**cupsys-client**, és el paquet que ens instal·la el client CUPS.

**cups-pdf**, és el paquet que ens instal·la una eina que ens permet crear fitxers PDF a partir del CUPS, com si fos una impressora. És similar al PDFCreator del Windows.

Per poder-lo configurar i administrar, el servidor CUPS disposa, a més de les ordres de l'intèrpret d'ordres, d'una interfície web que funciona sobre el port 631. Aquesta interfície web ens permet de manera gràfica afegir, cercar i eliminar impressores i classes d'impressores, controlar els treballs en les cues d'impressió i gestionar diversos paràmetres del servidor. En la figura 10 es mostra la pantalla que apareix en carregar la interfície web del CUPS.

Figura 10. Pantalla incial de CUPS![Home Cups](https://agora.xtec.cat/escolesnuria/moodle/file.php/102//uf3/t2/CupsHome.png)

 

#### 2.10.3. Compartir impressores gràficament amb el CUPS

Per tal de compartir impressores amb el servidor CUPS disposem d'un entorn gràfic, al qual ens podem connectar mitjançant l'explorador d'Internet, que ens facilita el procés. Així doncs, el procés de compartició d'impressores per mitjà del CUPS consta dels passos següents:

PostScript

El PostScript és un llenguatge de descripció de pàgines que s'utilitza en moltes impressores i, de manera usual, també es fa servir com a format de transport d'arxius gràfics en tallers d'impressió professional.

**1. Instal·lació en el servidor d'impressió de les impressores a compartir**. Per instal·lar una impressora en el servidor CUPS hem de seleccionar l'opció _**Add printers**_ de la pestanya _Administration_.

Apareixerà una pantalla, com la que veiem en la figura 11, en què ens demanarà un nom, la localització i una descripció per a la impressora que volem instal·lar.

Figura 11. Pantalla inicial per afegir una nova impressora amb CUPS![Afegir Imp.](https://agora.xtec.cat/escolesnuria/moodle/file.php/102//uf3/t2/cupsImp.png)

Si continuem, una vegada especificats els paràmetres anteriors, ens demanarà el dispositiu o _back-end_ al qual volem associar la impressora. Aquest paràmetre es fa servir per transferir correctament les ordres d'impressió als dispositius d'impressió.

Seguidament ens mostrarà la pantalla de la figura 12, en què ens demanarà l'URI \(_uniform resource identifier_, identificador uniforme de recursos\) del dispositiu per poder especificar on és la impressora en el sistema. Hem d'anar en compte perquè, si no especifiquem bé aquest paràmetre, la impressora romandrà inaccessible. Podem consultar en la documentació del CUPS com hem d'especificar l'URI, segons el model de la impressora.

Figura 12. Pantalla d'especificació del lloc de l'impressora al sistema amb CUPS![URI](https://agora.xtec.cat/escolesnuria/moodle/file.php/102//uf3/t2/cups_uri.png)

_PostScript printer description_

El PPD és un arxiu que crea el fabricant de la impressora per descriure les característiques disponibles per a les seves impressores PostScript. Un PPD conté el codi de PostScript necessari per fer servir les característiques d'una impressora. D'aquesta manera, funciona com un controlador de dispositiu per a les impressores PostScript i proveeix una interfície unificada.

La pantalla següent ens demana pel fabricant de la impressora a fi d'instal·lar els controladors de dispositiu adequats. També ens permet especificar aquests controladors mitjançant un fitxer de text en format PPD \(_PostScript printer description_\). Seguidament, ens demanarà pel model d'impressora per instal·lar els controladors més adequats.

Una vegada seleccionat el model d'impressora, la instal·larà. Si s'instal·la correctament, apareixerà una pantalla, que ens permetrà configurar les opcions generals d'impressió de la impressora.

CUPS i PPD

El CUPS fa servir el PPD per a totes les seves impressores. Redirigint la sortida per mitjà d'un filtre, ha estès el concepte per a permetre impressió PostScript en impressores que no són PostScript. Aquest filtre ja no és un PPD estàndard, sinó que més aviat és un “CUPS-PPD”.

**2. Configuració del servidor per compartir impressores i fer que siguin visibles per mitjà de la xarxa**. Per tal que les màquines de la xarxa local pugin accedir a les impressores que gestiona el servidor CUPS, és a dir, per tal que el servidor comparteixi les impressores, haurem de seleccionar, en la secció _Server_ de la pestanya _Administration_, les opcions _**Show printers shared by other systems**_ \(d'aquesta manera ens mostrarà les impressores per mitjà de la xarxa\) i _**Share published printers connected to this system**_ \(d'aquesta manera ens permetrà compartir impressores públiques connectades al sistema\). Si es pot accedir al servidor des d'Internet i volem que es pugui imprimir, marcarem l'opció _**Allow printing from Internet**_.

Per altra banda, si volem administrar remotament el servidor, és a dir, accedir a la configuració via web des d'un altra màquina, haurem de marcar l'opció _**Allow remote administration**_, que també és en la secció _Server_ de la pestanya _Administration_. Es mostra en la figura 14.

Figura 14. Exemple de secció Server de CUPS

**3. Configuració del client per detectar i utilitzar les impressores compartides pel servidor CUPS**. Per tal que el client utilitzi les impressores remotes instal·lades en el servidor CUPS, des de l'Ubuntu, instal·lat en la màquina client, anirem al menú _**Sistema**_ **&gt;** _**Administració**_ **&gt;** _**Impressió**_. En seleccionar l'opció _Impressió_ del menú, apareixerà una pantalla en què ens mostrarà les impressores que ja tenim configurades. En aquesta pantalla haurem de seleccionar la icona _**Nou**_ **&gt;** _**Impressora**_. Aleshores començarà un procés de cerca de les impressores que detecti l'equip. Al final d'aquest procés se'ns obrirà el quadre _**Impressora nova**_ que veiem en la figura 20.

Figura 15. Quadre per afegir una impressora nova

Si ens ha detectat la impressora que volem instal·lar, només caldrà que premem el botó _**Endavant**_.

A continuació, apareixerà un quadre en què ens demanarà el nom mitjançant el qual volem que es reconegui la impressora en el sistema. De manera opcional, també ens demanarà una ubicació i una descripció d'aquesta impressora. Després d'especificar les dades anteriors, haurem de fer clic a _**Aplica**_ i així conclourà el procés d'instal·lació.

Finalment, el sistema ens proposarà imprimir una pàgina de prova perquè puguem estar segurs que la instal·lació s'ha fet correctament.

Si, per contra, en el quadre _**Impressora nova**_ no es mostra la impressora que volem instal·lar, tenim dues possibilitats:

* Seleccionar l'opció _**Altre**_ i especificar tot l'URI de la impressora que volem instal·lar en el format següent:

```text
ipp://nom_o_ip_servidor/printers/nom_impresora
```

* Seleccionar, en la part dreta del quadre, l'opció _**Internet printing protocol**_ **\(IPP\)** i especificar en cada quadre de text els valors dels paràmetres demanats: ordinador \(nom o adreça IP del servidor\), cua \(afegir el nom de la impressora\).

Una vegada especificats els paràmetres de la impressora que volem instal·lar en una de les dues opcions anteriors, clicarem a _**Endavant**_.

A continuació, ens mostrarà una pantalla com la de la figura 21, en què ens donarà la possibilitat d'escollir entre tres opcions: seleccionar una marca d'impressora de la base de dades de controladors del sistema, proporcionar un fitxer PPD en què especifiquem el controlador o cercar el controlador a baixar. Normalment escollirem la marca de la impressora que volem instal·lar i polsarem el botó _**Endavant**_.

Figura 16. Opcions de selecció en instal·lar una impressora nova

Seguidament haurem d'escollir el model concret dins dels controladors de la marca seleccionada anteriorment. Escollirem el més adequat per a la impressora concreta i farem _**Endavant**_.

A continuació apareixerà un quadre en què ens demanarà el nom mitjançant el qual volem que es reconegui la impressora en el sistema. De manera opoional, també ens demanarà una ubicació i una descripció de la impressora. Després d'especificar aquestes dades, polsarem el botó _**Aplica**_ i així conclourà el procés d'instal·lació.

Finalment, el sistema ens proposarà imprimir una pàgina de prova perquè puguem estar segurs que la instal·lació s'ha fet correctament.

 

#### 2.10.4. Samba i CUPS

El servei Samba integra el servei d'impressió CUPS i està configurat per tal que, per defecte, faci servir aquest sistema. Tot i que el Samba també suporta altres estils o sistemes d'impressió, el paràmetre _**printing**_, el qual determina el sistema d'impressió, té el valor _cups_ per defecte.

La combinació del Samba i el CUPS permet la compartició d'impressores entre màquines GNU/Linux i màquines Windows. En xarxes que són formades totalment per màquines GNU/Linux, proporciona al servei CUPS més control de l'accés a les impressores compartides. Això és degut a les polítiques de seguretat i de gestió d'usuaris del Samba, ja que la configuració global d'aquest servidor, a diferència del CUPS, permet determinar quins usuaris poden accedir als seus recursos i quins no.

Per exemple, si fem servir la compartició d'impressores via CUPS, podrem determinar si una màquina de la xarxa pot accedir o no a les impressores, però no quins usuaris concrets de cada màquina hi tenen accés i quins no. Si utilitzem el Samba, en canvi, podrem distingir entre els usuaris i/o els grups d'una màquina que tenen accés a les impressores del servidor CUPS i els que no hi tenen accés.

**Compartir impressores amb el Samba fent servir el CUPS**

La compartició d'impressores via Samba entre sistemes GNU/Linux és molt similar a la compartició d'impressores amb el CUPS. El procés de compartició també consta d'una sèrie de passos.

**1. Instal·lació de les impressores a compartir en el servidor Samba**. La instal·lació de les impressores la podem fer de la mateixa manera que es fa en el servidor CUPS. Tanmateix, si tenim les impressores connectades físicament al servidor Samba, també la podem fer localment. Se suposa que aquests dos tipus d'instal·lació ja es coneixen.

Per defecte, el Samba treballa sobre el sistema CUPS.

**3. Configuració del client per detectar i utilitzar les impressores compartides pel servidor Samba**. La configuració s'assembla molt a la del servidor CUPS, però hi ha unes quantes diferències. Un cop siguem en el sistema Ubuntu instal·lat en la màquina client, anirem al menú _**Sistema**_ **&gt;** _**Administració**_ **&gt;** _**Impressió**_. Apareixerà una pantalla en què ens mostrarà les impressores que ja tenim configurades. En aquesta pantalla seleccionarem la icona _**Nou**_ **&gt;** _**Impressora**_ i aleshores començarà un procés de cerca de les impressores detectades per l'equip. Al final d'aquest procés, s'obrirà el quadre _**Impressora nova**_.

Una vegada obert el quadre, tenim dues possibilitats:

* Seleccionar l'opció _**Altre**_ i especificar tot l'URI de la impressora que volem instal·lar en el format següent:

```text
smb://[grup_de_treball]nom_o_ip_servidor[:port]/nom_impressora
```

* Seleccionar, en la part dreta del quadre, l'opció _Windows Printer via SAMBA_ i especificar en cada quadre de text els valors dels paràmetres demanats.

En el quadre de text smb especificarem les dades corresponents amb el format següent:

```text
smb://[grup_de_treball]nom_o_ip_servidor[:port]/nom_impressora
```

A continuació, determinarem si cal que, a l'hora d'imprimir, el sistema demani les dades d'autenticació a l'usuari o utilitzi les dades especificades en aquesta pantalla \(figura 18\).

Figura 18. Pantalla de configuració d'una impressora nova al client

Una vegada especificats els paràmetres de la impressora que volem instal·lar, premerem el botó _**Endavant**_.

A continuació, ens mostrarà una pantalla en què ens demanarà que seleccionem una marca d'impressora de la base de dades de controladors del sistema, que li proporcionem el controlador en un fitxer PPD o que cerquem el controlador a baixar.

Normalment escollirem la marca de la impressora que volem instal·lar i farem _**Endavant**_.

A continuació, haurem d'escollir el model concret dins dels controladors de la marca seleccionada anteriorment. Escollirem el més adequat per a la impressora, com veiem a la figura 19 i farem _**Endavant**_.

Figura 19.Pantalla per escollir el tipus d'impressora

Seguidament, apareixerà un quadre en què ens demanarà el nom mitjançant el qual volem que es reconegui la impressora en el sistema. De manera opcional, també ens demanarà una ubicació i una descripció d'aquesta impressora. Després d'especificar aquestes dades, haurem de prémer el botó _**Aplica**_ i així conclourà el procés d'instal·lació.

Finalment, el sistema ens proposarà imprimir una pàgina de prova perquè puguem estar segurs que la instal·lació s'ha fet correctament.

### A7. Activitat a classe: Compartir recursos en sistemes lliures

#### NFS i serveis associats

Llegeix al recurs de compartir en Linux la part de NFS.  
1. Prepara un esquema del funcionament del NFS tant a nivell client com a servidor  
2. Instal·la el servei nfs al sistema Debian  
3. Crea una carpeta al teu directori amb permisos _drwxr-xr-x_ amb el nom "documents".  
  
4. Exporta la carpeta "documents" amb permís de lectura/escriptura per als components de la xarxa 10.0.2.0/255.255.255.0  
  
5. El teu equip també  ha de poder accedir a l'exportació de la carpeta, com si fos un client qualsevol, indica i demostra de forma detallada quines operacions cal fer?  
6. Instal·la el servei i el client de impressores "**cups**", i configura la impressora virtual "**cups-pdf**", perquè tothom de la xarxa de classe hi pugui accedir  


#### SAMBA i serveis associats

  
Llegiu al recurs de compartir en Linux la part de Samba.  
  


7. Instal·leu el samba per a client i servidor

8. Modifica el servidor Samba del vostre servidor Linux per a què:

* En la secció \[globals\]:
  * Permeti l'accés únicament a IP's 10.0.2.0/24
  * Denegui l'accés a una IP dels vostres companys/nyes.
  * Indiqui seguretat user, és a dir compartir recursos amb permisos d'accés per usuari o grups d'usuaris
  * Els passwords s'utilitzaran encriptats
  * El servidor ha de suportar **wins**
  * ha de compartir automàticament totes les impressores del sistema
  * l'indicarem que el fitxer d'usuaris samba és /etc/samba/smbpasswd

9. Crea la carpeta /home/samba/alumnesM4  


* Compartiu la carpeta amb el nom de recurs \[alumnesM4\]
  * Que sigui explorable
  * Que permeti l'accés a l'usuari que teniu en Windows \(alumnes\) i també als usuaris del grup smix.
  * Que els usuaris puguin llegir i escriure.



10. Comprova que tot funciona correctament, documentant tota l'activitat.  


