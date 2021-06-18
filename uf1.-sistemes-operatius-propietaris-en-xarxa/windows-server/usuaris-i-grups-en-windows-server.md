# Usuaris i grups en Windows Server

## Comptes d'usuari

Bàsicament hi ha dos tipus de comptes d'usuari en el sistema Microsoft Windows Server:

* **Comptes d'usuari de domini**: estan definits en l'Active Directory i poden accedir als recursos del domini mitjançant l'inici de sessió únic. Per crear aquests comptes, haureu d'utilitzar _Usuarios y equipos_ de l'Active Directory.
* **Comptes d'usuari local**: estan definits en un equip local i hi tenen accés. És necessari que aquests comptes s'autentiquin abans d'accedir a recursos de xarxa. Per crear aquests comptes haureu d'utilitzar _Usuarios y grupos locales_.

### **Identificació de comptes**

En el Microsoft Windows Server, els comptes d'usuari tenen dues parts:

* **Nom d'usuari**: és una etiqueta de text que identifica el compte. Els noms locals han de ser únics per a cada equip individual, haurien de ser únics en tot un domini, no poden excedir els 64 caràcters de longitud i poden contenir caràcters alfanumèrics i especials.
* **Domini o grup de treball d'usuari**: és el domini o el grup de treball al qual pertany el compte d'usuari.

### **Assignació de noms**

Els noms d'inici de sessió no poden excedir els 256 caràcters de longitud. No són vàlids els caràcters següents: `”, /, \, [, ], ;, |, =, +, *, ?, <, >`

**Estratègies per assignar noms**

Per assignar noms hi ha diverses estratègies:

* Nom i primera lletra dels cognoms de l'usuari.
* Primera lletra del nom i cognoms de l'usuari.
* Primera lletra del nom, inicials i cognoms de l'usuari.
* Nom i cognoms de l'usuari.

**Identificadors de seguretat**

Els comptes d'usuari tenen una sèrie d'atributs, alguns dels quals són molt importants.

Els **SID** \(_security identifiers_, identificadors de seguretat\) són uns identificadors únics que es generen automàticament durant la creació dels comptes.

Quan es crea un compte d'usuari nou es genera un SID que no es modificarà encara que canviï el nom d'usuari lligat al compte. Així, es pot estudiar el recorregut dels comptes d'usuari amb independència del nom d'usuari emprat.

## Comptes de grup

L'administració d'un sistema que contingui un gran nombre de comptes d'usuari genera un gran volum de treball si no es disposa d'algun mecanisme que en simplifiqui la tasca. Per això hi ha els comptes de grup.

Un **compte de grup** està format per una sèrie de comptes d'usuari amb característiques molt semblants. Un dels avantatges de poder fer servir comptes de grup és la simplificació que comporta el fet d'utilitzar-los en l'administració de comptes.

Atès que és molt usual que el nom del compte de grup es repeteixi dins d'organismes grans, l'Active Directory utilitza el format _**DOMIMI\nomdelgrup**_, que li permet diferenciar grups amb el mateix nom que pertanyen a diferents dominis.

### **Tipus de grups**

En el sistema operatiu Microsoft Windows Server  hi ha tres tipus de grups diferents:

* **Grups locals**: estan definits en un equip local i només es poden utilitzar en aquest equip. Per poder-los crear haureu de fer servir _Usuarios y grupos locales_.
* **Grups de seguretat**: són grups que poden tenir descriptors de seguretat associats. Per definir-los haureu d'utilitzar _Usuarios y equipos_ de l'Active Directory.
* **Grups de distribució**: s'utilitzen com a llistes de distribució de correu electrònic. Heu de tenir en compte que aquests tipus de grups no poden tenir cap descriptor de seguretat associat. Els haureu de definir mitjançant _Usuarios y equipos_ de l'Active Directory.

Com es mostra en la figura, el sistema operatiu Microsoft Windows Server  simplifica molt la tasca de lligar diferents objectes a usuaris o grups.

Figura . La selecció dels objectes és una tasca fàcil amb aquest entorn.

![comptes](https://moodle.escolesnuria.cat/moodle/file.php/3/imatges/comptes.png)

**Àmbit de grup**

Podeu utilitzar els grups en diferents àrees, segons convingui. Hi ha quatre àmbits:

* **Grups locals de domini**: aquests grups s'utilitzen per assignar permisos d'accés a recursos, però només dins d'un únic domini. Per exemple, faríeu servir aquests grups per administrar una carpeta compartida.
* **Grups locals integrats**: aquests grups tenen un àmbit especial de grup amb permisos d'àmbit local de domini. No es poden crear ni eliminar, només modificar.
* **Grups globals**: s'utilitzen per definir conjunts d'usuaris o equips del mateix domini amb característiques molt semblants. Un exemple d'ús de grup global seria utilitzar-lo per donar permís d'accés a un recurs, ja que només caldria fer membre del grup local de domini al grup global.
* **Grups universals**: són grups que s'utilitzen per definir conjunts d'usuaris o equips que haurien de tenir permisos aplicables a tot un domini o bosc.

## Comptes predeterminats d'usuari i de grup

La instal·lació del Microsoft Windows Server instal·la usuaris i grups predeterminats. Mitjançant aquests comptes predeterminats el sistema pot construir la xarxa.

Els tipus de comptes predeterminats són els següents:

* **Integrat**: comptes d'usuari i grup instal·lats en el sistema operatiu, en les aplicacions i en els serveis.
* **Predefinit**: comptes d'usuari i de grup instal·lats en el sistema operatiu.
* **Implícit**: grups creats implícitament en accedir a recursos de xarxa.

### **Comptes d'usuari integrats**

En el sistema operatiu Microsoft Windows Server, els comptes d'usuari integrats tenen una sèrie de capacitats especials:

* **LocalSystem**: l'ús rau en l'execució de processos i la gestió de tasques de sistema. Si configureu aplicacions o serveis perquè utilitzin aquest compte, els processos tindran accés complet al servidor.
* **LocalService**: aquest compte té privilegis limitats. Només permet accedir al sistema local. És recomanable utilitzar-lo en aplicacions i serveis que no necessitin accedir a altres servidors.
* **NetworkService**: aquest compte s'utilitza per executar serveis que necessiten privilegis addicionals i drets d'inici de sessió en un sistema local i en la xarxa.

### **Comptes d'usuari predefinits**

Amb la instal·lació del Microsoft Windows Server  s'instal·len els comptes següents:

* **Administrador**: proporciona accés complet a arxius, carpetes, serveis i altres recursos. Aquest compte té privilegis i accés a escala de domini en l'Active Directory.
* **Convidat**: aquest compte té privilegis de sistema limitats. És membre d'_Invitados de dominio_ i _Invitados_. Té accés a una sèrie d'arxius i carpetes de manera predeterminada.

#### Capacitats dels comptes

Un compte pot tenir diverses capacitats. Les assigna el sistema operatiu. El Microsoft Windows Server pot assignar les capacitats següents:

* **Privilegis**: permeten als usuaris fer determinades tasques.
* **Drets d'inici de sessió**: permet als usuaris iniciar una sessió.
* **Capacitats integrades**: estan predefinides i no es poden modificar. S'assignen als administradors i als operadors de compte. Per exemple, permeten crear, eliminar i administrar comptes d'usuari.
* **Permisos d'accés**: defineixen les operacions que es poden fer en els recursos de xarxa.

#### Ús d'usuaris i grups locals

{% hint style="info" %}
L'ús d'usuaris i grups **locals** ens permet accedir als recursos de la pròpia màquina i sistema.
{% endhint %}

Per crear un compte d'usuari local cal seguir aquestes indicacions:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Usuarios_.
5. Dins el menú _Acción_ premeu _Usuario nuevo_.
6. Empleneu el quadre de diàleg amb la informació necessària.
7. Activeu els quadres en funció de la configuració que voleu fer.
8. Tanqueu les finestres amb _Crear_.

Per **restablir** la **contrasenya** d'un usuari local cal fer el següent:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Usuarios_.
5. Cliqueu amb el botó dret al damunt del compte d'usuari que necessiteu i premeu _Establecer contraseña_.
6. Premeu _Continuar_.
7. Empleneu els camps _Contraseña nueva_ i _Confirmar contraseña nueva_ amb la contrasenya nova.
8. Cliqueu a _Aceptar_.

{% hint style="info" %}


Normes a seguir en l'establiment de la **contrasenya**:

* S'ha d'exigir l'historial de contrasenyes per tal de no deixar utilitzar sempre el mateix grup de contrasenyes.
* La vigència màxima de la contrasenya no pot excedir els trenta dies.
* La vigència mínima de la contrasenya ha d'oscil·lar entre els tres i els set dies.
* La contrasenya ha de tenir com a mínim vuit caràcters, o catorze com a mínim si necessiteu més seguretat.
* La contrasenya no ha de contenir el nom de l'usuari.
* La contrasenya ha de contenir minúscules, majúscules, números i símbols.
{% endhint %}

Per **deshabilitar** o **activar** un compte d'usuari local heu de seguir els passos següents:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Usuarios_.
5. Cliqueu amb el botó dret al damunt del compte d'usuari que necessiteu i premeu _Propiedades_.
6. Aquí podeu activar o deshabilitar el compte.

Per **eliminar** un compte d'usuari local heu de seguir aquestes indicacions:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Usuarios_.
5. Cliqueu amb el botó dret al damunt del compte d'usuari que necessiteu i premeu _Eliminar_.

**Eviteu problemes amb els compte**s

Abans d'eliminar un compte d'usuari és recomanable deshabilitar-lo. No es poden recuperar comptes d'usuari eliminats. Tampoc no es poden eliminar els comptes _Administrador_ i _Convidat_.

Per canviar el nom d'un compte d'usuari local cal que feu el següent:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Usuarios_.
5. Cliqueu amb el botó dret al damunt del compte d'usuari que necessiteu i premeu _Cambiar nombre_.
6. Escriviu el nou nom i feu clic a _Enter_.

Per assignar una carpeta principal \(PERSONAL\)  a un compte d'usuari local heu de fer el següent:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Usuarios_.
5. Cliqueu amb el botó dret al damunt del compte d'usuari que necessiteu i cliqueu a _Propiedades_.
6. Accediu a la fitxa _Perfil_.
7. Especifiqueu la ruta de la carpeta a _Ruta de acceso local_.
8. Si la carpeta és en un recurs compartit, indiqueu l'adreça a _Conectar_.

Per crear un grup local cal que seguiu aquestes indicacions:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Grupos_.
5. Dins el menú _Acción_ cliqueu a _Grupo nuevo_.
6. Escriviu el nom del grup nou a _Nombre de grupo_.
7. Escriviu una descripció del grup a _Descripción_.
8. Agregueu els usuaris al grup mitjançant _Agregar_.
9. Apareix el quadre de diàleg _Seleccionar usuarios, equipos o grupos_.
10. Per agregar al grup un compte d'usuari indiqueu-lo a _Escriba los nombres de objeto que desea seleccionar_ i premeu _Aceptar_.
11. Si voleu agregar un compte d'equip al grup cliqueu a _Tipos de objetos_, activeu _Equipos_, premeu _Aceptar_ i feu el pas anterior.

Per identificar els membres d'un grup local heu de seguir aquests passos:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Grupos_.
5. Cliqueu amb el botó dret al damunt del grup que necessiteu i cliqueu a _Propiedades_.

**Eliminació de grups**

Els grups eliminats no es poden recuperar. Quan s'elimina només el grup no s'eliminen els comptes d'usuari, els comptes d'equip ni els comptes de grup que eren membres del grup. Si s'elimina un grup i se'n crea un de nou amb el mateix nom, aquest grup nou no hereta els permisos del grup anterior.

Per **eliminar** un grup local heu de fer el següent:

1. Obriu _Administración de equipos_.
2. Cliqueu a _Herramientas del sistema_.
3. Cliqueu a _Usuarios y grupos locales_.
4. Cliqueu a _Grupos_.
5. Cliqueu amb el botó dret al damunt del grup que necessiteu i premeu _Eliminar_.

