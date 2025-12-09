# 1. Comptador de processos actius

Compta quants processos hi ha executant-se i compara’ls amb un límit passat com a argument al script.
Escriu al fitxer `alerta.log` la següent informació

```
ALERT:Data en què s'ha executat el script UsuariExecució Processos actuals: $NUMPROC , Limit: $LIMIT
OK:Data en què s'ha executat el script UsuariExecució Processos actuals: $NUMPROC , Limit: $LIMIT
```

on NUMPROC són els processos que s'estan executant i LIMIT el valor màxim de processos que volem que s'executin.  

---

# 2. Sistema sobrecarregat

Abans de fer aquest exercici, tingues en compte:

```
Linux només treballa amb valors enters. Per tant, per a fer els següents exercicis, convertirem els valors que obtenim del sistema a valors enters.


NUM=3.14
RESULTAT=$(echo "$NUM * 100" | bc)
echo "$RESULTAT"

Executa aquest codi per a comprovar què fa. Tingues en compte que resultat continua essent un valor real, però podem eliminar els dígits que hi ha després del "." decimal. 
```

Fes un script que llegeixi la càrrega del sistema (load average). Utilitza la comanda `uptime` per a obtenir la informació. El script rebrà com a argument la càrrega màxima que pot suportar el sistema: per exemple, 2.75. 

El script cal que registri la següent informació en el arxiu `sistema.log`:

```
ALERT:Data en què s'ha executat el script UsuariExecució Càrrega actual: CURLOAD , Limit: LIMIT
NORMAL:Data en què s'ha executat el script UsuariExecució Càrrega actual: CURLOAD , Limit: LIMIT
```

Segons si la càrrega del sistema (CURLOAD) sobrepassa (ALERT) o no la capacitat especificada (NORMAL). 

---

# 3. Sistema sobrecarregat

El script proposat és una variació de l'anterior, però l'usuari indicarà el nom de l'equip que vol examinar la càrrega, enlloc d'especificar la càrrega. Per fer-ho, disposem d'un fitxer de nom "servers"  amb la següent estructura. 

```
nomServidor:númerodecores:percentatgecarrega
```

Per exemple, 
```
web:16:75
mail:32:50
```

on el 75% de web significa que no volem que la càrrega teòrica del sistema sobrepassi el 75% de la capacitat total de l'ordinador.

Tingues en compte que no cal controlar que existeixi el fitxer "servers". Sempre existirà i estarà ben referenciat.


El script cal que registri la següent informació en el arxiu `sistema.log`:

```
ALERT:Data en què s'ha executat el script UsuariExecució Càrrega actual: CURLOAD , Limit: LIMIT
NORMAL:Data en què s'ha executat el script UsuariExecució Càrrega actual: CURLOAD , Limit: LIMIT
```

Segons si l'execució del sistema sobrepassa o no la capacitat especificada en el fitxer per l'equip indicat.

En el cas que el nom especificat com a servidor sigui incorrecte, el script avisarà a l'usuari d'aquesta circumstància i finalitzarà l'execució.

---

# 4. Control de quotes d’usuaris

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

