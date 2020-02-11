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



