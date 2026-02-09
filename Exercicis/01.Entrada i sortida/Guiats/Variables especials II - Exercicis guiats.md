# PRÀCTICA GUIADA: Variables especials II i comanda `shift`

## Recordatori teòric breu

| Variable | Significat                                                   | Exemple resumit                       |
| -------- | ------------------------------------------------------------ | ------------------------------------- |
| `$*`     | Tots els arguments junts en una sola cadena.                 | `echo "$*"` → mostra `arg1 arg2 arg3` |
| `$@`     | Tots els arguments com a llista independent.                 | `for a in "$@"; do echo "$a"; done`   |
| `shift`  | Mou els paràmetres cap a l’esquerra. `$2` passa a `$1`, etc. | Es perd l’anterior `$1`               |

---

## Activitat 1: Prova amb `$*`

### Crea un fitxer anomenat `cadena.sh`

```bash
#!/bin/bash

ENTRADA="$*"

TOTALC=$(echo -n $ENTRADA | wc -c)  # Comptem caràcters sense salt de línia
echo -e "$ENTRADA \nTOTAL CARACTERS: $TOTALC "
```

### Executa el programa amb arguments:

```bash
./cadena.sh hola món des de bash
```

### Fes una captura o enganxa la sortida:

[Captura de pantalla]

### Respòn a...

1. Quants arguments hem passat al programa?
2. El resultat que mostra `$*` està separat per espais o per línies?
3. Quin és el total de caràcters mostrat? Què passaria si hi afegim cometes a l’entrada?

---

## Activitat 2: Prova amb `$@`

### Crea un fitxer anomenat `llista.sh`

```bash
#!/bin/bash

TOTAL=0
for PARAM in "$@"
do
  TOTAL=$(( TOTAL + 1 ))
  echo "$TOTAL $PARAM"
done
```

### Executa el programa amb diferent nombre d'arguments

[Captura de pantalla]

### Respòn a...

1. En quin ordre apareixen els arguments?
2. Quin paper juga el comptador `$TOTAL`?
3. Si utilitzessis `$*` en lloc de `$@`, què canviaria en la sortida?

---

## Activitat 3: Experiment amb `shift`

### Crea un fitxer anomenat `desplaça.sh`

```bash
#!/bin/bash
while [ $# -gt 0 ]; do
  echo "Total paràmetres: $#"
  echo "\$1: $1"
  shift
done
```

### Executa el programa amb diferent nombre d'arguments

[Captura de pantalla]

### Respòn a...

1. Què passa amb el valor de `$#` després de cada iteració?
2. Què mostra `$1` després de cada `shift`?
3. Quin paràmetre es perd cada vegada que es fa `shift`?

