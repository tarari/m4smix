# Directrius de grup

## Directrius de grup (GPO? <a href="#que-es-una-directriu-de-grup-gpo" id="que-es-una-directriu-de-grup-gpo"></a>

Una **directriu de grup (GPO)** és un component del servei de directori a on **es defineixen les regles** o polítiques que comparteixen  tot un grup de comptes d'usuari o equips en un domini.



* **Per exemple**: bloquejar l’accés a l’administrador de tasques, restringir l’accés a determinades carpetes, deshabilitar la descàrrega d’arxius executables, etc.

Les directrius de grup **simplifiquen molt l’administració i configuració dels ordinadors** de la nostra empresa.

Les directrius de grup sovint s’**utilitzen per restringir certes accions** que poden presentar riscos de seguretat.

Algunes configuracions que permeten les directrius de grup són:

* Polítiques de seguretat
* Instal·lació automàtica de programari
* Execució de scripts

### Herència en directrius

Les directrius del contenidors pares són heredades pels contenidors fills, resultant la suma de directrius tret del cas de directrius contradictòries. En aquest cas la directriu resultant és l'última afegida (normalment les que s'apliquen sobre OU Unitats Organitzatives).

Per evitar conflictes, s’utilitza el següent **ordre d'aplicació**: 1. Directrius de **grup local** del equip. 2. GPOs viculades a **sites**. 3. GPOs vinculats a **dominis**. 4. GPOs vinculats a **Unitats Organitzatives de primer nivell**. 5. GPOs vinculats a **Unitats Organitzatives de segon nivell**.



## Tipus de directrius de grup <a href="#tipus-de-directrius-de-grup" id="tipus-de-directrius-de-grup"></a>

* **Directrius de grup local**
  * Són aquelles que només s'apliquen **a la màquina on s'han definit**.
  * Normalment, es configuren directament en els clients, o bé, quan es treballa en una xarxa sense domini (grup de treball).
* **Directrius de grup de domini**
  * Són aquelles que s'apliquen **a un conjunt d'usuaris o equips del domini**.

## Principals polítiques incloses en un directriu de grup (GPO) <a href="#principals-politiques-incloses-en-un-directriu-de-grup-gpo" id="principals-politiques-incloses-en-un-directriu-de-grup-gpo"></a>

Cada directriu de grup (**GPO**) conté la configuració de l'**equip** i d'**usuari**.

* **Configuració de l’equip**: permet establir les directrius que s’aplicaran als equips amb independència de qui inicia la sessió.
* **Configuració d’usuari**: permet establir les directrius que s’aplicaran als usuaris, independentment de l’equip en què hagin iniciat la sessió.

Cadascuna d’aquestes es divideix en arbres iguals.![](https://gblobscdn.gitbook.com/assets%2F-M4uTdAUmhLr6lvCTUzA%2F-M4uTgeRKnmGFawpXWoQ%2F-M4uTkTCegaljVT6yEqY%2Fdirectrius\_edicio.png?alt=media)

Hi ha polítiques que estan als dos subarbres però amb significats i/o paràmetres diferents. Per exemple:

* **Configuración del Equipo** > Directivas > config Windows > script: Scripts que s'executen **quan es tanca o inicia equip**.
* **Configuración de usuario** > Directivas > config Windows > script: Scripts que s'executen **quan un usuari tanca o inicia sessió local**.

Dins cada configuració d’equip o d’usuari, hi trobem directives i preferències:

* **Directives**:
  * **Configuració de software**: instal·lació automàtica de programari.
  * **Configuració de Windows**: entre altres, aspectes de configuració de seguretat i execució de scripts.
  * **Plantilles administratives**: inclouen aquelles polítiques basades en la configuració de l'equip, com els següents: Tauler de control, escriptori, impressores, xarxa, carpetes compartides, etc.
* **Preferències**: aspectes de configuració que típicament es feien amb scripts en versions anteriors de Windows.
  * **Configuració de Windows**: Opcions de configuració com variables entorn, accessos directes, unitats xarxa
  * **Configuració del panell de control**: Instal·lació dispositius, impressores, opcions d'energia, tasques programades, serveis, etc.

## Creació de directrius de Grup (GPO) <a href="#creacio-de-directrius-de-grup-gpo" id="creacio-de-directrius-de-grup-gpo"></a>

Per introduir-nos a les directives de grup el millor és començar amb un exemple.

**Exemple: Canviar el fons de pantalla**

1. Primerament, comprovem que tenim instal·lat la característica del servidor **Administració de directrius de grup** \[_Group Policy Management_] per poder utilitzar la Consola d’administració de directrius de grup.
2. **Creem una carpeta** en el servidor. La anomenarem _**wallpaper**_ i fiquem una imatge.
3. **Compartim la carpeta** a tots els usuaris del domini. Fem botó dret sobre la carpeta i escollim "compartir".
4. Obrim la Consola d’administració de directrius de grup a través de l’_**Administrador del Servidor > Eines > Administració de directrius de grup**_.
5. Fem botó dret sobre el nostre domini i **creem la nostra directriu de grup (GPO)** i l’anomanem "Directriu Fons Pantalla".
   * Es crea una plantilla amb totes les possibles polítiques sense configurar. Si volem s'apliquin s'han de configurar.
   * Al fer botó dret sobre el nostre domini es vincula la directriu aquest contenidor (a tot el domini).
   * Els contenidors on es poden vincular GPOs són: sites, dominis, i OUs.
   * Els comptes i equips que estan dins del contenidor reben automàticament les polítiques que estiguin configurades en el GPO.
6. Fem botó dret sobre de la política que acabem de crear i escollim _**Editar...**_.
7. S’obrirà l’**Editor de directrius** de la GPO.
8. Anem a _**Configuració d'usuari > Directives > Plantilles administratives > Desktop > Desktop > Tapiz del escritorio**_.

![](https://gblobscdn.gitbook.com/assets%2F-M4uTdAUmhLr6lvCTUzA%2F-M4uTgeRKnmGFawpXWoQ%2F-M4uTkTGPePpZvixtQJB%2Fdirectriu\_escriptori.png?alt=media)

1. **Habilitem la directriu**, posem el camí de la imatge `\\NomServidor\wallpaper\imatge.jpg`
2. Tornem a la finestra de _**Administració de directives de grup**_ i fem clic a la nostra directiva i anem a la pestanya "Detalles" aquí escollim en _**Estado de GPO**_ l'opció _**Configuració d'equip deshabilitada**_ ja que ens interessa que estigui habilitat per usuari, no per equip.
3.  Per **actualitzar les directrius**, anem a la línia de comandes i executem:

    `gpupdate /force`
4.  I per últim, podem **mostrar les directrius** aplicades executant:

    `gpresult /r`
5. Ara ja podem **comprovar** que funciona, aneu al vostre equip del domini Windows 7 i comprova que s'ha canviat el fons de pantalla.

## Aplicació de directrius <a href="#aplicacio-de-directrius" id="aplicacio-de-directrius"></a>

En funció d’on s’apliquin les directrius de grup, es poden crear dos grans conjunts de directrius:

* **Directrius que s’apliquen a equips**: normalment s’apliquen durant l’arrencada de l’equip.
* **Directrius que s’apliquen a usuaris**: s’apliquen quan els usuaris inicien les sessions.

Administrador les pot forçar l’actualització de les directrius amb la comanda:

`gpupdate /force`

> No funciona amb totes les polítiques. Sempre hi ha algunes que necessiten tancar sessió o reinicia equip.



###



## Delegació del control del GPO <a href="#delegacio-del-control-del-gpo" id="delegacio-del-control-del-gpo"></a>

Qualsevol usuari o grup que tingui aplicat el **control total** sobre un GPO pot administrar-lo.

Per defecte, poden **administrar GPOs**:

* Grup administradors d'empresa
* Grup adminstradors del domini
* Creador del GPO
* El propi SYSTEM

Per defecte, només poden **crear GPOs**:

* Administradors d'empresa i domini
* Usuaris o grups membres del grup Group Policy Creator Owners

> Es pot **delegar el control** a altres usuaris i grups posant els usuaris que han de fer aquesta tasca al grup _**Group Policy Creator Owners**_.

## Directrius d'instal·lació de programari <a href="#directrius-dinstal-lacio-de-programari" id="directrius-dinstal-lacio-de-programari"></a>

Permet instal·lar programes als ordinadors clients des del servidor.

Serveix tant per instal·lar com per desinstal·lar de forma centralitzada.

Hi ha dues formes desplegar una aplicació:

* **Assignar**:
  * Permet que l’aplicació s’instal·li o bé quan l’usuari intenti accedir per primera vegada o bé automàticament.
* **Publicar**:
  * Dóna la oportunitat a l'usuari d'instal·lar l'aplicació si li interessa, però sense cap acció automàtica.
  * La llista aplicacions publicades i instal·lar-les, apareix al _**Panel de control > Añadir / eliminar programas**_.

> La publicació sempre es fa sobre els usuaris, mentre que l’assignació es pot fer sobre el usuaris o sobre els equips.

## Consells d'utilització de polítiques de grup <a href="#consells-dutilitzacio-de-politiques-de-grup" id="consells-dutilitzacio-de-politiques-de-grup"></a>

* **Separar Equips i Usuaris en OUs** diferenciades: Facilita tasca d'administració d'usuaris/equips i podem assignar a administradors diferents.
* **Minimitzar els GPOs** associades a usuaris o equips: Els temps que triga a iniciar equip o usuari depèn del número de polítiques a aplicar.
