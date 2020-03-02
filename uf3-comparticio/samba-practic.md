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

Imagina que volem tenir una carpeta amb accés restringit, només aquells que pertanyn a un grup podran accedir-hi i amb uns permisos determinats

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

Editem i afegim línies si cal \(En pico o nano ^C per mirar el núm. de la  línia\):

```text
# línia 25, afegir
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



