# Introducció als sistemes operatius lliures en xarxa

## **Conceptes previs:** 

Abans de començar amb els sistemes propis de Linux, convé repassar la teoria sobre Particions, Volums i Sistemes de fitxers.

Igual que en Windows, en Linux també es poden crear volums senzills \(formats per una única partició\) i volums compostos \(formats per més d'una partició\).

La principal diferència està en com s'anomenen els volums:

* En Windows, tant els volums senzills com els volums compostos, tenen assignada una lletra: C:, D:, E:, ...
* En Linux se'ls assigna una ruta que comença a /dev, el directori on es troben tots els dispositius:
* Volums senzills: la ruta depén del disc i de la partició: /dev/sda1, /dev/sdb5, ...

### **Volums senzills**

Els volums senzills estan formats per una única partició.

Cada partició té assignada una ruta: /dev/sdXY.

* X indica el disc i és una lletra començant per la a: /dev/sda, /dev/sdb, ...
* Y indica el número de la partició i és un número començant per 1: /dev/sda1, /dev/sda5, ...

#### MBR Master Boot Record

En el sistema de particionat MBR, les particions primàries i l'estesa tenen els números de l'1 al 4.

**Les particions lògiques sempre comencen a partir del 5.**  
  
![](https://lh5.googleusercontent.com/JuovaMngYQ7OMO-k30wOqFk_XczHgikBNtKmfo8m3p1G70zVqW0CsHI6iY5Mnucz3HuM0u5d6Yni11l_myXxMhS-06GHt96ec1CMwHvCtcuYvWGLxv8LuCdfwDHZqrzmBCApFFSnx_0)  
****

###  **Sistemes de fitxers habituals en Linux**

#### **ext4**

* Particions de fins a 1 EiB \(1 exbibyte = 260 Bytes = 1.152.921.504.606.846.976 Bytes\).
* Arxius de fins a 16 TiB.
* És compatible amb ext3, i ext3 també ho és amb ext4.
* Permet gestionar permisos per diferents usuaris.
* No cal desfragmentar manualment.
* És un sistema de fitxers transaccional.

**Reiser4**: Característiques similars a ext4, excepte amb la compatibilitat amb ext3.

**Swap** \(o memòria d'intercanvi\): És un sistema de fitxers especial que s'utilitza quan es necessita més memòria RAM de la que té el maquinari.Altres:També accepta molts altres tipus: ReiserFS, JFS, XFS



