# Activitat pràctica de Winbind

## Unir-se a un domini de Windows Server amb AD

Dades:

* Hostname: srv1.domini.com
* Nom NETBIOS: SRV1
* Domini: domini.com
* Realm: DOMINI.COM

```
root@smb:~# apt -y install winbind libpam-winbind libnss-winbind krb5-config samba-dsdb-modules samba-vfs-modules
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
# specify hostname of AD DS
     +--------------+ Configuring Kerberos Authentication +---------------+
     | Enter the hostnames of Kerberos servers in the SRV.WORLD           |
     | Kerberos realm separated by spaces.                                |
     |                                                                    |
     | Kerberos servers for your realm:                                   |
     |                                                                    |
     | srv1.domini.com____________________________________________________ |
     |                                                                    |
     |                               <Ok>                                 |
     |                                                                    |
     +--------------------------------------------------------------------+
# specify hostname of AD DS
 +------------------+ Configuring Kerberos Authentication +------------------+
 | Enter the hostname of the administrative (password changing) server for   |
 | the SRV.WORLD Kerberos realm.                                             |
 |                                                                           |
 | Administrative server for your Kerberos realm:                            |
 |                                                                           |
 | srv1.domini.com___________________________________________________________ |
 |                                                                           |
 |                                  <Ok>                                     |
 |                                                                           |
 +---------------------------------------------------------------------------+
```

## Configurar winbind

```
root@smb:~# pico /etc/samba/smb.conf
# line 29: change NetBIOS Name to AD DS's one and add like follows
   workgroup = DOMINI
   realm = DOMINI.COM
   security = ads
   idmap config * : backend = tdb
   idmap config * : range = 3000-7999
   idmap config DOMINI : backend = rid
   idmap config DOMINI : range = 10000-999999
   template homedir = /home/%U
   template shell = /bin/bash
   winbind use default domain = true
   winbind offline logon = false

root@smb:~# vi /etc/nsswitch.conf
# line 7: add like follows
passwd:         files systemd winbind
group:          files systemd winbind

root@smb:~# pico /etc/pam.d/common-session
# afegir si és necessari (auto creat de directori personal en login inicial)
session optional        pam_mkhomedir.so skel=/etc/skel umask=077
root@smb:~# pico /etc/resolv.conf
# canviar DNS al que té el AD
nameserver 10.0.1.**
```

## Unió al domini Windows

```
# unió al  Active Directory (net ads join -U [AD's admin user])
root@smb:~# net ads join -U Administrator (o Administrador)
Enter Administrator's password:
Using short domain name -- DOMINI
Joined 'SMB' to dns domain 'domini.com'
No DNS domain configured for smb. Unable to perform DNS Update.
DNS update failed: NT_STATUS_INVALID_PARAMETER
root@smb:~# systemctl restart winbind
# show domain users info
root@smb:~# wbinfo -u
administrator
guest
sshd
krbtgt
debian
ldapusers
# prova de canviar a un usuari Windows
root@smb:~# su - alumnes
Creating directory '/home/alumnes'.
serverworld@smb:~$ id
uid=11103(alumnes) gid=10513(domain users) groups=10513(domain users),11103(alumnes)
```

