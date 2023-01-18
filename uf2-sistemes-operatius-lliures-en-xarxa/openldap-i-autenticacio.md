# OpenLDAP i autenticació

## Servei de directori

El servei de directori permet accedir a través de protocol lleuger  (LDAP) a recursos de sistema. Estem parlant per exemple de serveis de consulta però també permet en general operacions CRUD (Crear, llegir, actualitzar i esborrar), com ara la consulta d'usuaris al servei d'autenticació.

## Operació

Si volem configurar un servidor OpenLDAP en Debian 11 per al domini "smix.local" i permetre l'autenticació PAM per als usuaris de sistema i els incorporats al servei de directori, es pot seguir els següents passos:

* Instal·lar OpenLDAP i els paquets relacionats utilitzant el gestor de paquets apt:

```
sudo apt-get update
sudo apt-get install slapd ldap-utils libpam-ldapd
```

* Configura OpenLDAP en el moment d'instal·lació, establint el nom de domini, la contrasenya de l'administrador i altres opcions.
* Crear o no, un fitxer de configuració de slapd.conf
* Crear un fitxer de base de dades de ldif
* Configurar PAM per a l'autenticació LDAP
* Reiniciar els serveis slapd i pam per aplicar els canvis
* Provar la connexió a l'LDAP amb ldapsearch i altres eines per assegurar-se de que està configurat correctament.

Aquests passos són una guia general, i pots necessitar adaptar-los a les teves necessitats específiques. Es recomana seguir una guia detallada per a la configuració d'OpenLDAP i l'autenticació PAM per obtenir una configuració funcional.



### Consultes del servei

Per connectar a un servidor OpenLDAP utilitzant ldapsearch i realitzar consultes, podem fer servir la següent sintaxi:

```
ldapsearch -x -H ldap://<hostname or IP> -b <baseDN> -D <bindDN> -w <bind password> <search filter> <attributes>
```

* `-x`: Utilitza l'autenticació simple (username and password)
* `-H`: Especifica l'URL del servidor LDAP. Pots utilitzar "ldap://" o "ldaps://" per a connexions no segures o segures, respectivament.
* `-b`: Especifica el DN de la base de dades des de la qual es realitzarà la cerca
* `-D`: Especifica el DN de l'usuari que es connectarà al servidor
* `-w`: Especifica la contrasenya de l'usuari que es connectarà al servidor
* `<search filter>`: Especifica el filtre de cerca que s'utilitzarà per cercar entrades. Per exemple, "(objectClass=inetOrgPerson)" per cercar totes les entrades de tipus inetOrgPerson
* `<attributes>`: Especifica les atributs que es volen retornar per cada entrada. Si no especifica cap atribut, ldapsearch retornarà tots els atributs per defecte.

Exemple:

```
ldapsearch -x -H ldap://ldap.smix.local -b dc=smix,dc=local -D cn=admin,dc=smix,dc=local -w mypassword "(objectClass=inetOrgPerson)"
```

Aquest exemple cerca totes les entrades de tipus **inetOrgPerson** en el servidor OpenLDAP en _ldap.smix.local_, amb un DN de base dc=smix,dc=local, utilitzant l'usuari amb DN cn=admin,dc=smix,dc=local i la contrasenya "mypassword".

Es recomana fer-lo amb un usuari amb suficients permisos per fer cerques en els directoris i que no exposi informació sensible.



### PAM i autenticació

PAM

Si volem configurar PAM per a l'autenticació LDAP, s'han de modificar els fitxers de configuració de PAM corresponents als serveis que vulguis configurar per a l'autenticació LDAP. Els fitxers de configuració de PAM es troben en el directori /etc/pam.d/.

Per exemple, per configurar l'autenticació LDAP per al servei SSH, s'ha  de modificar el fitxer /etc/pam.d/sshd.

Les línies que hauries d'afegir depenen de la versió del  sistema operatiu i dels paquets instal·lats, però en general, aquestes són les línies que s'haurien d'afegir o modificar al fitxer:

```
auth sufficient pam_ldap.so
account sufficient pam_ldap.so
password sufficient pam_ldap.so
session sufficient pam_ldap.so
```

Aquests exemples, són per a sistemes on PAM i libpam-ldapd estan instal·lats, hauries de fer una revisió de la documentació dels teus paquets per assegurar-te que estan correctament configurats.

També hauries de configurar els paràmetres de connexió a LDAP en el fitxer /etc/ldap.conf, per exemple:

```
uri ldap://ldap.smix.local
base dc=smix,dc=local
ldap_version 3
rootbinddn cn=admin,dc=smix,dc=local
bind_policy soft
```

En aquest exemple, es connecta al servidor OpenLDAP en ldap.smix.local, utilitzant el protocol LDAP versió 3, amb un DN de base dc=smix,dc=local, i l'usuari amb DN cn=admin,dc=smix,dc=local, i una politica de bind "soft"

Després de modificar els fitxers de configuració, cal reiniciar els serveis PAM i els serveis que volguem configurar per a l'autenticació LDAP perquè els canvis tinguin efecte.

Es recomana fer una prova d'autenticació amb usuaris existents en el directori per assegurar-se que tot esta configurat correctament.

