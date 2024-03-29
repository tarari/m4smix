# Activitat pràctica Controlador de domini Debian

Per configurar un servidor Debian  com a Controlador de domini, seguirem una sèrie de passos. Recordem que un controlador de domini permet la **gestió unificada d'usuaris i grups** proporcionant un únic punt d'inici de sessió NETLOGON a través, en el cas de Samba 4.0, d'un sistema d'autenticació Kerberos.

Seguirem el següent patró:

Apunta quin serà el vostre domini a crear, quin serà el regne d'autenticació kerberos (REALM), i quin serà el nom dns del vostre servidor administratiu.

{% hint style="info" %}
Cal respectar les majúscules i minúscules!
{% endhint %}



* Domini:   **DOMINI**
* Realm:   **DOMINI.COM**
* Hostname:  **srv.domini.com**

{% hint style="warning" %}
Hem de ser curosos a l'hora de seguir l'activitat, repasem tots els missatges del sistema, observem tots els requeriments, i si ens equivoquem mirem el pas anterior.
{% endhint %}

## Instal·lació

```
 sudo apt -y install samba krb5-config winbind smbclient
# if using DHCP. answer [Yes], if static IP, answer [No]
 +----------------------+ Samba server and utilities +-----------------------+
 |                                                                           |
 | If your computer gets IP address information from a DHCP server on the    |
 | network, the DHCP server may also provide information about WINS servers  |
 | ("NetBIOS name servers") present on the network.  This requires a change  |
 | to your smb.conf file so that DHCP-provided WINS settings will            |
 | automatically be read from /var/lib/samba/dhcp.conf.                      |
 |                                                                           |
 | The dhcp-client package must be installed to take advantage of this       |
 | feature.                                                                  |
 |                                                                           |
 | Modify smb.conf to use WINS settings from DHCP?                           |
 |                                                                           |
 |                    <Yes>                       <No>                       |
 |                                                                           |
 +---------------------------------------------------------------------------+
# specify Realm
 +------------------+ Configuring Kerberos Authentication +------------------+
 | When users attempt to use Kerberos and specify a principal or user name   |
 | without specifying what administrative Kerberos realm that principal      |
 | belongs to, the system appends the default realm.  The default realm may  |
 | also be used as the realm of a Kerberos service running on the local      |
 | machine.  Often, the default realm is the uppercase version of the local  |
 | DNS domain.                                                               |
 |                                                                           |
 | Default Kerberos version 5 realm:                                         |
 |                                                                           |
 | DOMINI.COM________________________________________________________________ |
 |                                                                           |
 |                                  <Ok>                                     |
 |                                                                           |
 +---------------------------------------------------------------------------+
# specify own hostname
 +------------------+ Configuring Kerberos Authentication +------------------+
 | Enter the hostnames of Kerberos servers in the DOMINI.COM Kerberos         |
 | realm separated by spaces.                                                |
 |                                                                           |
 | Kerberos servers for your realm:                                          |
 |                                                                           |
 | srv.domini.com____________________________________________________________ |
 |                                                                           |
 |                                  <Ok>                                     |
 |                                                                           |
 +---------------------------------------------------------------------------+
 
# specify own hostname
 +------------------+ Configuring Kerberos Authentication +------------------+
 | Enter the hostname of the administrative (password changing) server for   |
 | the DOMINI.COM Kerberos realm.                                             |
 |                                                                           |
 | Administrative server for your Kerberos realm:                            |
 |                                                                           |
 | srv.domini.com____________________________________________________________ |
 |                                                                           |
 |                                  <Ok>                                     |
 |                                                                           |
 +---------------------------------------------------------------------------+
```

## Configurar Samba AD DC

Utilitzarem l'eina **samba-tool,** una eina d'administració de domini

<pre><code>#renomenar o eliminar configuració per defecte
root@smb:~# mv /etc/samba/smb.conf /etc/samba/smb.conf.orig
root@smb:~# samba-tool domain provision
#especificar REALM
Realm [<a data-footnote-ref href="#user-content-fn-1">DOMINI.COM</a>]: 
# especificar nom de domini
 Domain [DOMINI]: DOMINI 
#per defecte rol dc
 Server Role (dc, member, standalone) [dc]:
#Per defecte utilitza DNS-intern de samba 
 DNS backend (SAMBA_INTERNAL, BIND9_FLATFILE, BIND9_DLZ, NONE) [SAMBA_INTERNAL]:
# si tenim un  DNS forwarder, l'especifiquem , si no, especificar [none]
 DNS forwarder IP address (write 'none' to disable forwarding) [10.0.0.10]: 8.8.8.8
# set admin password
# Password amb certa complexitat, sino donarà error de configuració
Administrator password:
Retype password:
Looking up IPv4 addresses
Looking up IPv6 addresses
No IPv6 address will be assigned
Setting up share.ldb
Setting up secrets.ldb
Setting up the registry
Setting up the privileges database
Setting up idmap db
Setting up SAM db
Setting up sam.ldb partitions and settings
Setting up sam.ldb rootDSE
Pre-loading the Samba 4 and AD schema
Unable to determine the DomainSID, can not enforce uniqueness constraint on local domainSIDs

Adding DomainDN: DC=domini,DC=com
Adding configuration container
Setting up sam.ldb schema
Setting up sam.ldb configuration data
Setting up display specifiers
Modifying display specifiers and extended rights
Adding users container
Modifying users container
Adding computers container
Modifying computers container
Setting up sam.ldb data
Setting up well known security principals
Setting up sam.ldb users and groups
Setting up self join
Adding DNS accounts
Creating CN=MicrosoftDNS,CN=System,DC=domini,DC=com
Creating DomainDnsZones and ForestDnsZones partitions
Populating DomainDnsZones and ForestDnsZones partitions
Setting up sam.ldb rootDSE marking as synchronized
Fixing provision GUIDs
A Kerberos configuration suitable for Samba AD has been generated at /var/lib/samba/private/krb5.conf
Merge the contents of this file with your system krb5.conf or replace it with this one. Do not create a symlink!
Once the above files are installed, your Samba AD server will be ready to use
Server Role:           active directory domain controller
Hostname:              srv
NetBIOS Domain:        DOMINI
DNS Domain:            domini.com
DOMAIN SID:            S-1-5-21-3772837808-1505251784-1375148484

root@smb:~# cp /var/lib/samba/private/krb5.conf /etc/
root@smb:~# systemctl stop smbd nmbd winbind
root@smb:~# systemctl disable smbd nmbd winbind
root@smb:~# systemctl unmask samba-ad-dc
root@smb:~# systemctl start samba-ad-dc
root@smb:~# systemctl enable samba-ad-dc
# verificar  status
root@smb:~# smbclient -L localhost -U%

        Sharename       Type      Comment
        ---------       ----      -------
        netlogon        Disk
        sysvol          Disk
        IPC$            IPC       IPC Service (Samba 4.9.5-Debian)
Reconnecting with SRV for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            NAS
</code></pre>

Si tot ha anat bé, tenim ja configurat un servidor amb el rol de controlador de domini.

Si no ha anat bé, hem de reconfigurar el SAMBA, afegim aquestes línies just després de la definicició de WORKGROUP:

```
client min protocol = NT1
server min protocol = NT1
```

Ara reiniciem el nou servei samba-ad-dc i ja estarà, tornem a comprovar amb:

```
smbclient -L localhost -N
```

### Possibles errors:

#### Falta de zona inversa en DNS intern

Quan fem nslookup des de l'equip Windows, no troba el servidor DNS del nostre equip, cal crear la zona inversa i els registres associats

{% embed url="https://wiki.samba.org/index.php/Setting_up_Samba_as_an_Active_Directory_Domain_Controller#Create_a_reverse_zone" %}
Crear zona inversa en DNS interna
{% endembed %}

## Confirmació de nivell de DC

```
# confirmar domain level
root@smb:~# samba-tool domain level show
Domain and forest function level for domain 'DC=domini,DC=com'

Forest function level: (Windows) 2012 R2
Domain function level: (Windows) 2012 R2
Lowest function level of a DC: (Windows) 2012 R2

# add a domain user
root@smb:~# samba-tool user create debian-cli
New Password:   # set password
Retype Password:
User 'debian' created successfully
```

A part de confirmar, hem acabat creant amb les samba-tool un client, amb el que podrem iniciar sessió en Windows.

## Accions a les estacions de treball Windows

Com a condició final per a aquest client Windows, dir-vos que cal afegir-lo al domini, i recordem que això significa que:

* Windows 7 o 10 ha de ser client DNS del servidor Debian (DNS preferit)
* Fer les comprovacions pertinents (**nslookup**)

Un cop unit al domini, podem iniciar sessió amb l'usuari creat.

[^1]: Nom dns del domini, en majúscules
