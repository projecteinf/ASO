# Canvi remot de nom d’equips

Crea un script en Bash que permeti canviar de forma remota el nom (hostname) de diversos equips.

L’script ha de llegir un fitxer de text amb una llista d’equips i el nom de l'equip.

## Format de l'arxiu

```
IP NOU_NOM
```

## Exemple 

```
192.168.1.10 servidor-web
192.168.1.11 servidor-db
172.20.1.1 router-lab
```

Registra les operacions i errors associats a l'execució del script.

# Registre usuaris

Crea un script en Bash que permeti verificar que tots els usuaris estan creats. En cas que hi hagi algun usuari no creat, caldrà registrar la incidència en el sistema.

L’script ha de llegir un fitxer de text on s'especifica la IP dels equips i els usuaris que hauria de tenir cada equip.

## Format de l'arxiu

```
IP USUARIS (separats per :)
```

## Exemple 

```
192.168.1.10 root:anna
192.168.1.11 root:maria
```

Registra les operacions i errors associats a l'execució del script.
