# Swap

- Espai del disc destinat a swapping.

```bash
free -h # -h Human readable
```

|      |total    |  used  |  free   |  shared  | buff/cache  | available |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|Mem:  |  7.7Gi  |  2.8Gi |  3.4Gi  |  242Mi   |    2.0Gi    |   4.9Gi   |
|Swap: |  8.0Gi |     0B  |  8.0Gi   |             |           |     |

- Swap used determina la saturació de la memòria.

## Operacions

### Deshabilitar/Habilitar swap

```bash
swapoff -a  # Deshabilitar
swapon  -a  # Habilitar
```

### Crear swap (fitxer - no partició disc)

```bash
dd if=/dev/zero of=/swapfile bs=1M count=4096   # 4 GB 
mkswap /swapfile
chmod 0600 /swapfile 
echo "/swapfile   none    swap    sw    0   0" >> /etc/fstab 
swapon -a
free
```

```
               total        used        free      shared  buff/cache   available
Mem:         2976992      885088     1089624       11508     1251776     2091904
Swap:        4194300           0     4194300
```

- Explicació comanda dd

    - dd if=/dev/zero of=/swapfile bs=1G count=4 

        - Llegeix 4 blocs de 1 GB cadascun des de /dev/zero

        - Els escriu en el fitxer /swapfile

        - Resultat: crea un fitxer de 4 GB ple de zeros

### Crear swap (partició de disc)

```bash

sudo swapoff -a # -a = ALL
sudo mkswap /dev/sdb2 
sudo swapon /dev/sdb2

UUIDSWAP=$(sudo blkid /dev/sdb2 | cut -d"=" -f2 | cut -d" " -f1)   # agafar UUID

echo "UUID=$UUIDSWAP   none   swap   sw   0  0" >> /etc/fstab   # Activar partició al reiniciar el sistema

# Verificació de funcionament
free -h
swapon --show

```

## Canviar mida assignada a swap (fitxer)

```bash
cat /etc/fstab
```

```
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/vda1 during installation
UUID=67e92ae5-44a6-40bc-a13f-ced670bea7fd /               ext4    defaults,noatime,discard,errors=remount-ro 0       1
/dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0
/swapfile   none    swap    sw    0   0

```

```bash
swapoff /swapfile
rm /swapfile
dd if=/dev/zero of=/swapfile bs=1M count=1024
chmod 600 /swapfile
mkswap /swapfile
# echo "/swapfile   none    swap    sw    0   0" >> /etc/fstab  # Només si no existeix!!
swapon /swapfile
free # Per a comprovar
```
-----

# Comanda `vmstat` (Estadístiques de Memòria Virtual)

- La comanda `vmstat` (Virtual Memory Statistics) proporciona estadístiques detallades sobre l'activitat de la memòria, processos i I/O.

- Informació sobre swapping (si / so)

## Swap-in (si) / Swap-out (so)

- Swap-in (si)
    
    - Memòria (`KB/s`) que està sent carregada des del disc cap a la RAM per a cada segon.  

    - Quan es produeix un page fault.

- Swap-out (so)

    - Memòria (`KB/s`) que està sent escrita des de la RAM cap al disc per a cada segon.

    - No hi ha prou espai de memòria per a tots els processos. 

- Objectiu

    - Valors de si i so sigui 0.

```bash
vmstat 
```

- Podem executar la comanda en temps real

```
vmstat 2
```

```
root@debian:~# vmstat 2
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 877844  35864 1429020    0    0    37   325   54   98  0  0 99  1  0
 1  0      0 877844  35864 1429020    0    0     0     0  236  407  1  1 97  0  0
 1  0      0 877844  35864 1429020    0    0     0     0   99  182  1  0 99  0  0
 1  0      0 877844  35864 1429020    0    0     0     0  103  205  1  1 99  0  0
```

-----

## `top` / `htop` (Per Procés)

```bash
top
```

### Informació

| Camp | `top` / `htop` | Descripció |
| :--- | :--- | :--- |
| Memòria Virtual | `VIRT` | Mida total de la memòria virtual del procés. Espai lògic d'adreces virtual.  |
| Memòria Resident | `RES` | Quantitat de memòria física (RAM) que el procés està utilitzant realment ara mateix. |
| Memòria Compartida | `SHR` | Mida de la memòria compartida utilitzada pel procés (memòria que altres processos també poden utilitzar). |
| Memòria Swap | `SWAP` (a `htop`) | Quantitat de memòria del procés que està assignada a l'espai de swap (al disc). És la diferència entre `VIRT` i `RES` (sense comptar les llibreries compartides no residents). |

-----

[Tornar](../../../../../Diari/Guio.md)