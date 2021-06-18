# Els sistemes de fitxers Windows

## Introducció

> Per poder utilitzar una partició, cal donar-li format per tal de gestionar els arxius i directoris que s'hi guardaran.

Els sistemes de fitxers divideixen l'espai de la partició en diferents volums.

**Característiques** 

* Han de permetre la gestió de permisos per diferents usuaris i grups.
* Han de ser **transaccionals** \(en anglès, _**journaling**_\): aquesta característica disminueix molt el perill de perdre arxius o de que el sistema de fitxers quedi fet malbé en cas que falli una operació d'escriptura, ja que són capaços de tornar a la situació abans de l'operació.

## Sistemes de fitxers Windows

### FAT32

Particions de fins a **2 TiB**. Arxius de fins a **4 GiB**. No permeten gestionar permisos per diferents usuaris. **No és transaccional.**

### exFAT

Particions de fins a **512 TiB** \(màxim recomanat\). Arxius de fins a **16 EiB** \(límit teòric\). No permeten gestionar permisos per diferents usuaris. Per defecte, **no és transaccional**, però se li pot afegir suport per ser-ho.

### NTFS

Particions de fins a **16 TiB** \(màxim 256 TiB\). Arxius tan grans com la partició. És més eficient que **FAT32**. Permet gestionar permisos per diferents usuaris. Sistema de fitxers **transaccional**.

## 

