# Variables especials en Bash (I)

## Què són?

Quan s’executa un script Bash, el sistema crea variables automàtiques que contenen informació sobre:

* El nom de l’script
* Els arguments passats
* El nombre d’arguments
* I l’estat de sortida de les comandes

Aquestes variables no cal declarar-les; ja existeixen.

---

## `$0` — Nom de l’script

Definició: conté el nom (i sovint la ruta) amb què s’ha executat l’script.

Exemple:

```bash
#!/bin/bash
echo "Nom de l'script: $0"
```

Execució:

```bash
$ bash exemple.sh
Nom de l'script: exemple.sh

$ ./exemple.sh
Nom de l'script: ./exemple.sh
```

### Cas pràctic

* Mostrar missatges d’ajuda o errors, per exemple:

```bash
echo "Ús: $0 fitxer_entrada fitxer_sortida"
```

---

## `$1`, `$2`, `$3`, ... — Arguments posicionals

Definició: contenen els valors dels arguments passats a l’script.

* `$1` és el primer, `$2` el segon, etc.

Exemple:

```bash
#!/bin/bash
echo "Primer argument: $1"
echo "Segon argument: $2"
```

Execució:

```bash
$ bash arguments.sh hola món
Primer argument: hola
Segon argument: món
```

Observació: Si no s’han passat arguments, aquestes variables estan buides.

---

## `$#` — Nombre d’arguments

Definició: indica quants arguments s’han passat a l’script.

Exemple:

```bash
#!/bin/bash
echo "Has passat $# arguments."
```

Execució:

```bash
$ bash compte.sh hola adeu test
Has passat 3 arguments.
```

### Cas pràctic 

- Comprovar que l’usuari ha introduït prou paràmetres.

---

## `$?` — Estat de sortida

Definició: conté el codi de sortida de l’última comanda executada.

Valors possibles:

* `0` → èxit (sense errors)
* `1`, `2`, ... → error o condició especial

Exemple:

```bash
#!/bin/bash
ls /etc >/dev/null
echo "Codi de sortida (èxit): $?"

ls /no_existeix >/dev/null
echo "Codi de sortida (error): $?"
```

Resultat:

```
Codi de sortida (èxit): 0
Codi de sortida (error): 2
```

### Cas pràctic 

* Serveix per comprovar si una acció ha anat bé o no abans de continuar.

---

# Introducció `if` 

La comanda `if` serveix per prendre decisions dins d’un script.
Permet executar diferents blocs de codi segons si una condició és cert o falsa.

---

## Estructura bàsica

```bash
if [ condició ]
then
    # instruccions si la condició és certa
else
    # instruccions si la condició és falsa
fi
```

🔹 Explicació:

* Les claus quadrades `[ ]` s’utilitzen per avaluar una expressió.
* Després de `if` hi ha d’haver un espai abans i després de `[ ]`.
* Després del `then`, hi va el codi que s’executarà si la condició és cert.
* El bloc acaba amb `fi` (que és "if" al revés).

---

## Comparacions amb nombres

| Operador | Significat        | Exemple         |
| -------- | ----------------- | --------------- |
| `-eq`    | igual             | `[ $NUM1 -eq $NUM2 ]` |
| `-ne`    | diferent          | `[ $NUM1 -ne $NUM2 ]` |
| `-gt`    | més gran que      | `[ $NUM1 -gt $NUM2 ]` |
| `-ge`    | més gran o igual  | `[ $NUM1 -ge $NUM2 ]` |
| `-lt`    | més petit que     | `[ $NUM1 -lt $NUM2 ]` |
| `-le`    | més petit o igual | `[ $NUM1 -le $NUM2 ]` |

---

## Exemples

### if ... then

```bash
#!/bin/bash

MAX=18
read -r NUM

if [ "$NUM" -gt "$MAX" ]
then
    echo "Valor $NUM no és vàlid. Màxim valor permèsi $MAX"
    exit 1
fi
echo "Número introduït $NUM"
```
### if ... then ... else

```bash
#!/bin/bash

MAX=18
read -r NUM

if [ "$NUM" -ge "$MAX" ]
then
    echo "Ets major d'edat"
else    
    echo "Menor d'edat"
fi

```

---

## ⚙️ Condicions combinades

```
Operador | Significat | Exemple                                       |
-------- | ---------- | --------------------------------------------- |
&&       | AND        | [ $NUM1 -gt $NUM2 ] && [ $NUM1 -gt $NUM3 ]    |
||       | OR         | [ $NUM1 -ne $NUM2 ] || [ $NUM1 -gt $NUM3 ]    |
```

---

## Exemples

### || 

```bash
#!/bin/bash
MAX=135

read -r EDAT

if [ "$EDAT" -lt 0 ] || [ "$EDAT" -gt $MAX ]
    then
        echo "Too old for this!!"
        exit 1
fi
echo "You can go in"
```

### && 

```bash
#!/bin/bash
MAX=135
MINIM=18

read -r EDAT

if [ "$EDAT" -gt "$MIN" ] && [ "$EDAT" -lt $MAX ]
    then
        echo "You can go in"
    else
        if [ "$EDAT" -lt $MIN ]
            then
                echo "Too young!!"
        fi
fi
```

# Comparacions de text

| Operador | Significat      | Exemple            |
| -------- | --------------- | ------------------ |
| `=`      | igual           | `[ "$STR" = "$STR2" ]`  |
| `!=`     | diferent        | `[ "$STR" != "$STR2" ]` |
| `-z`     | cadena buida    | `[ -z "$STR" ]`      |
| `-n`     | cadena no buida | `[ -n "$STR" ]`      |

## Exemples

- Mostrar ajuda amb paràmetre -h

```bash

#!/bin/bash

if [ "$1" = "-h" ]
then
    echo "Help $0"
    echo -e "Paràmetres: -h ajuda \nLIMIT màxim valor del processador..."
    exit 0
fi
```

- Validar que s'ha entrat paràmetre $1

```bash

#!/bin/bash

if [ -z "$1" ]
then
    echo "S'han especificat $# paràmetres"
    exit 1
fi

```

---

# Condicions amb fitxers

| Operador | Significat           | Exemple             |
| -------- | -------------------- | ------------------- |
| `-e`     | existeix             | `[ -e fitxer.txt ]` |
| `-f`     | és un fitxer regular | `[ -f fitxer.txt ]` |
| `-d`     | és un directori      | `[ -d carpeta ]`    |
| `-r`     | és llegible          | `[ -r fitxer.txt ]` |
| `-w`     | és modificable       | `[ -w fitxer.txt ]` |
| `-x`     | és executable        | `[ -x script.sh ]`  |


## Exemples

```bash

#!/bin/bash

if [ -e "$1" ]
then
    cat "$1"
else
    echo "El fitxer no existeix"
    exit 2
fi


```

# Variables especials - II

| Variable    | Significat                                                                                                           | Exemple pràctic                                                                 |                                           |
| ----------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------- |
| `$*`        | Tots els arguments rebuts com **una sola cadena** (tots junts).  El podem utilitzar per unir tots els arguments en una única cadena   | `echo "$*"` → mostra `arg1 arg2 arg3`                                           |                                           |
| `$@`        | Tots els arguments rebuts com **llista individual** (cada argument per separat).                                     | <pre>for a in "$@"; do echo "$a"; done</pre> mostra cada argument en línia nova |                                           |

---

## Exemples 

- $*

```bash

#!/bin/bash

ENTRADA="$*"

TOTALC=$(echo -n $ENTRADA | wc -c)  # -n elimina el \n
echo -e "$ENTRADA \nTOTAL CARACTERS: $TOTALC "

```

- $@

```bash

#!/bin/bash

TOTAL=0

for PARAM in "$@"
do
  TOTAL=$(( TOTAL + 1 ))
  echo "$TOTAL $PARAM"
done

```

---

# Comanda shift

- La comanda shift desplaça els paràmetres posicionals cap a l’esquerra.

    - $2 passa a ser $1

    - $3 passa a ser $2

- El primer paràmetre anterior es perd (ja no és accessible)

## Exemple

```bash

#!/bin/bash
while [ $# -gt 0 ]; do
  echo "Total paràmetres $#"
  echo "\$1 $1"
  shift
done

```

# Estructura `case` en Bash

- Comparar un valor amb diferents opcions possibles.

- Alternativa més neta i llegible a múltiples `if` / `elif`.

---

## Estructura bàsica

```bash
case VARIABLE in
    valor1)
        # Ordres a executar si VARIABLE = valor1
        ;;
    valor2)
        # Ordres a executar si VARIABLE = valor2
        ;;
    *)
        # Ordres per defecte (cap coincidència)
        ;;
esac
```

- `;;` indica el final de cada bloc de cas.

- `*` (asterisc) és l’opció per defecte, com el “else” d’un `if`.

---

## Exemple pràctic amb paràmetres

Podem combinar `case` amb **variables especials** (`$1`, `$2`, etc.):

```bash
#!/bin/bash

opcio=$1

case $opcio in
    start | Start )
        echo "Iniciant el servei..."
        ;;
    stop | Stop )
        echo "Aturant el servei..."
        ;;
    restart | Restart )
        echo "Reiniciant el servei..."
        ;;
    *)
        echo "Ús: $0 {start|Start|stop|Stop|restart|Restart}"
        exit 1
        ;;
esac
```

---
