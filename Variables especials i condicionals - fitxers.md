# Exercici 1 — Verificador de paràmetres (`server.sh`)

L’equip de sistemes t’ha demanat desenvolupar un script anomenat `server.sh`.
La seva funció és connectar-se a un servidor i comprovar el nombre d’usuaris connectats.

La signatura del script és:

```
Usage: server.sh -s Server -u NomUsuari [-p Contrasenya]
```

La teva primera tasca és assegurar-se que el script es crida de forma correcte o bé mostrar un missatge d'error i la signatura d'utilització en cas contrari.  

---

# 2. Control de quotes d’usuaris (modificació exercici)

Necessitem controlar les quotes dels usuaris del nostre sistema. Per fer-ho, disposem d'un fitxer anomenat `usuaris.cnf` amb la següent estructura:

```
nomUsuari:quotaMB
```

Per exemple:

```
anna:5000
maria:2000
eva:10000
```

Tingues en compte que tots els usuaris guardaran la seva informació en el seu directori HOME. L'espai ocupat per altres programes o paquets no es comptabilitzaran dins la quota de l'usuari.

El programa registrarà en un fitxer quota.log quins usuaris han sobrepassat la quota amb el total de MB que s'ha sobrepassat. Tingues en compte que el fitxer pot contenir usuaris que no existeixin en el sistema. En aquest cas, es registrarà aquest fet.


```
ALERT:Data en què s'ha executat el script:UsuariQuota:SPACE:QUOTA
NORMAL:Data en què s'ha executat el script:UsuariQuota:SPACE:QUOTA
ERR:Data en què s'ha executat el script:UsuariQuota:NOTVALID
```

UsuariQuota <- usuari que estem analitzant l'ocupació d'espai.  
SPACE <- ocupació d'espai.  
QUOTA <- quota assignada a l'usuari.  

- Fitxer de configuració

    - Verificar que existeix i es disposa de permisos. Si no es dóna la situació anterior, cal mostrar un missatge d'error adecuat.

    - Verificar que el format és correcte per a cada línia (user:quota)

- Carpetes HOME usuaris

    - Cal verificar que existeixen. No podeu utilitzar $? per aquesta verificació.

---

# Exercici 3 — Informe d’errors (informe.bash)

El departament de sistemes t’ha demanat elaborar un resum automàtic dels missatges d’error registrats als fitxers de log del sistema.  

Has de desenvolupar un script anomenat `informe.bash` que:

1. Accepti un nombre variable de paràmetres, cadascun representant una paraula clau a cercar (per exemple: `error`, `fail`, `warning`, etc.).  

2. Cerqui a tots els fitxers de log del sistema totes les línies que continguin qualsevol de les paraules especificades.  

3. Generi un fitxer de sortida anomenat `informe.log`, amb el següent format:  

```
/var/log/syslog
[línies del fitxer que contenen alguna de les paraules]

/var/log/auth.log
[línies del fitxer que contenen alguna de les paraules]
```

## Exemple d’execució

```bash
$ bash informe.bash err error Error
```

Recomanació: utilitza la comanda `find` per a resoldre l'exercici.

---

# Exercici 4 - Informe d'errors amb recompte (informe.bash)

Millora el teu script per oferir informació estadística per a cada fitxer.

Afegiu-hi les següents funcionalitats:

1. Després de mostrar les línies trobades en cada fitxer, escriu una línia amb el nombre total de coincidències dins d’aquell fitxer:

   ```
   Total coincidències: X
   ```

2. Al final del fitxer `informe.log`, afegeix un resum global amb el total de coincidències trobades entre tots els fitxers:

   ```
   ====== RESUM GLOBAL ======
   Total coincidències en tots els fitxers: Y
   ====== FI DE L’INFORME ======
   ```

## Exemple de sortida esperada (`informe.log`)

```
/var/log/syslog
[ 3 línies que contenen les paraules ]
Total coincidències: 3

/var/log/auth.log
[ 5 línies que contenen les paraules ]
Total coincidències: 5

====== RESUM GLOBAL ======
Total coincidències en tots els fitxers: 8
====== FI DE L’INFORME ======
```

---
