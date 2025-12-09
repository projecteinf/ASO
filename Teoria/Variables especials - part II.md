
# Variables especials - part II

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
