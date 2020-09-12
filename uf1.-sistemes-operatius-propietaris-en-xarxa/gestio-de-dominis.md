# Gestió de  dominis



## Comptes d'Usuari i grups en Windows Server

#### Comptes d'usuari

Hi ha dos tipus de comptes d'usuari en el sistema Microsoft Windows Server

* **Comptes d'usuari de domini**: estan definits en l'Active Directory i poden accedir als recursos del domini mitjançant l'inici de sessió únic. Per crear aquests comptes, haureu d'utilitzar _Usuarios y equipos_ de l'Active Directory.
* **Comptes d'usuari local**: estan definits en un equip local i hi tenen accés. És necessari que aquests comptes s'autentiquin abans d'accedir a recursos de xarxa. Per crear aquests comptes haureu d'utilitzar _Usuarios y grupos locales_.

**Identificació de comptes**

En el Microsoft Windows Server, els comptes d'usuari tenen dues parts:

* **Nom d'usuari**: és una etiqueta de text que identifica el compte. Els noms locals han de ser únics per a cada equip individual, haurien de ser únics en tot un domini, no poden excedir els 64 caràcters de longitud i poden contenir caràcters alfanumèrics i especials.
* **Domini o grup de treball d'usuari**: és el domini o el grup de treball al qual pertany el compte d'usuari.

**Assignació de noms**

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

#### Comptes de grup

L'administració d'un sistema que contingui un gran nombre de comptes d'usuari genera un gran volum de treball si no es disposa d'algun mecanisme que en simplifiqui la tasca. Per això hi ha els comptes de grup.

Un **compte de grup** està format per una sèrie de comptes d'usuari amb característiques molt semblants. Un dels avantatges de poder fer servir comptes de grup és la simplificació que comporta el fet d'utilitzar-los en l'administració de comptes.

Atès que és molt usual que el nom del compte de grup es repeteixi dins d'organismes grans, l'Active Directory utilitza el format _**`DOMIMI\nomdelgrup`**_, que li permet diferenciar grups amb el mateix nom que pertanyen a diferents dominis.

**Tipus de grups**

En el sistema operatiu Microsoft Windows Server 2003 hi ha tres tipus de grups diferents:

* **Grups locals**: estan definits en un equip local i només es poden utilitzar en aquest equip. Per poder-los crear haureu de fer servir _Usuarios y grupos locales_.
* **Grups de seguretat**: són grups que poden tenir descriptors de seguretat associats. Per definir-los haureu d'utilitzar _Usuarios y equipos_ de l'Active Directory.
* **Grups de distribució**: s'utilitzen com a llistes de distribució de correu electrònic. Heu de tenir en compte que aquests tipus de grups no poden tenir cap descriptor de seguretat associat. Els haureu de definir mitjançant _Usuarios y equipos_ de l'Active Directory.

