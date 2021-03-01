# SAMBA pràctic

## Accés complet a directori compartit \(_Fully access_\)

Assegurem-no de tenir instal·lat el paquet samba.

```text
sudo apt -y install samba
```

Imaginem que volem crear un directori compartit de forma general \(independentment dels usuaris i els seus permisos\), Creem la carpeta i deminim permisos:

```text
sudo mkdir /home/shares
sudo chmod 777 /home/shares
```

Modifiquem o millor, creem de nou el fitxer de configuració de samba _/etc/samba/smb.conf_:

```text
[Globals]
unix charset = UTF-8
workgroup = GRUP_TREBALL
interfaces = 127.0.0.0/8 10.0.2.0/24
bind interfaces only = yes
map to guest = Bad User


[Share]
    # shared directory
    path = /home/share
    # writable
    writable = yes
    # guest OK
    guest ok = yes
    # guest only
    guest only = yes
    # fully accessed
    create mode = 0777
    # fully accessed
    directory mode = 0777
```

Reiniciem el servei smbd i el nmbd

```text
sudo systemctl restart smbd
sudo systemctl restart nmbd
```

* [x] Prova el seu funcionament accedint a la ruta UNC de la carpeta des de l'explorador d'un altre equip del mateix grup de treball



## Accés restringit a directori

En aquest cas ens recolzem en la seguretat que donem a través dels permisos sobre fitxers i carpetes.

Imagina que volem tenir una carpeta amb accés restringit, només aquells que pertanyin a un grup \(aquí li diem security\) podran accedir-hi i amb uns permisos determinats

```text
root@smb:~# groupadd security
root@smb:~# mkdir /home/security
root@smb:~# chgrp security /home/security
root@smb:~# chmod 770 /home/security
```

Creem el grup d'accés, **security** per exemple, la carpeta a la qual donarem permisos _/home/security_,  donem permisos a **root** i **security** sobre la carpeta 

```text
# ls -la /home/security
drwxrwx---  2 root  security 4096 de març   2 15:17 security
```

Modifiquem el fitxer de configuració de samba 

```text
# pico /etc/samba/smb.conf
```

Editem i afegim línies si cal \(En pico o nano **^C** per mirar el núm. de la  línia\):

```text
# línia 25 del smb.conf, afegir
unix charset = UTF-8
# linia 30: canviar grup de treball 
workgroup = WORKGROUP
# línia 37: descomentar i canviar la IPs que permets
interfaces = 127.0.0.0/8 10.0.0.0/24
# línia 44: descomentar
bind interfaces only = yes
# afegir al final
# el nom amb el que vols compartir la carpeta
[Security]
    path = /home/security
    writable = yes
    create mode = 0770
    directory mode = 0770
    # no permetem convidat
    guest ok = no
    # només usuaris del grup security
    valid users = @security
```

Reiniciem el servei

```text
# systemctl restart smbd
```

Cal afegir un **usuari SAMBA,** debi per exemple, un usuari que també ha de ser usuari de linux :

```text
sudo adduser debi
sudo smbpasswd -a debi
```

i el col·loquem dins el grup  secundari **security**

```text
usermod -aG security debi
```

## Client en línia amb smbclient

### Instal·lació

```text
$ sudo apt install smbclient
$ dpkg -l smbclient
Desitjat=desconegUt/Instal·la/supRimeix/Purga/retín(H)
| Estat=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Estat,Err: majúsc.=dolent)
||/ Nom            Versió                 Arquitectura Descripció
+++-==============-======================-============-=========================
ii  smbclient      2:4.9.5+dfsg-5+deb10u1 amd64        command-line SMB/CIFS cli
```

### Comprova recursos compartits per un equip

```text
$ smbclient   -L host 


	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	Security        Disk      
	IPC$            IPC       IPC Service (Samba 4.9.5-Debian)
Reconnecting with SMB1 for workgroup listing.

	Server               Comment
	---------            -------

	Workgroup            Master
	---------            -------
	AS2                  DEBIM4
	WORKGROUP            SRV-A

```

Si volem veure la llista d'usuaris completa de Samba

```text
$ sudo pdbedit -L -v
---------------
Unix username:        secu
NT username:          
Account Flags:        [U          ]
User SID:             S-1-5-21-90188730-1240831525-1088045497-1000
Primary Group SID:    S-1-5-21-90188730-1240831525-1088045497-513
Full Name:            
Home Directory:       \\debim4\secu

```

### Accedir a recursos a través de smb:&gt;

Això és com el ftp, si ja coneixem l'usuari, podem accedir perfectament: `smbclient //equip/recurs -U usuari_samba`

```text
$ smbclient   //debiM4/Security -U secu
Unable to initialize messaging context
Enter AS2\secu's password: 
Try "help" to get a list of possible commands.
smb: \> 

```

Aquí teniu el llistat de comandos disponibles:

```text
Try "help" to get a list of possible commands.
smb: \> help
?              allinfo        altname        archive        backup         
blocksize      cancel         case_sensitive cd             chmod          
chown          close          del            deltree        dir            
du             echo           exit           get            getfacl        
geteas         hardlink       help           history        iosize         
lcd            link           lock           lowercase      ls             
l              mask           md             mget           mkdir          
more           mput           newer          notify         open           
posix          posix_encrypt  posix_open     posix_mkdir    posix_rmdir    
posix_unlink   posix_whoami   print          prompt         put            
pwd            q              queue          quit           readlink       
rd             recurse        reget          rename         reput          
rm             rmdir          showacls       setea          setmode        
scopy          stat           symlink        tar            tarmode        
timeout        translate      unlock         volume         vuid           
wdel           logon          listconnect    showconnect    tcon           
tdis           tid            utimes         logoff         ..             
!              

```

