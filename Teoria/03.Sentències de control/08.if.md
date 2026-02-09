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
