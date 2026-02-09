# PRÀCTICA GUIADA: Comparacions de text i comprovacions de fitxers

## Recordatori teòric

### Comparacions de text

| Operador | Significat      | Exemple                 |
| -------- | --------------- | ----------------------- |
| `=`      | Igual           | `[ "$STR" = "$STR2" ]`  |
| `!=`     | Diferent        | `[ "$STR" != "$STR2" ]` |
| `-z`     | Cadena buida    | `[ -z "$STR" ]`         |
| `-n`     | Cadena no buida | `[ -n "$STR" ]`         |

---

### Comparacions amb fitxers

| Operador | Significat           | Exemple             |
| -------- | -------------------- | ------------------- |
| `-e`     | Existeix             | `[ -e fitxer.txt ]` |
| `-f`     | És un fitxer regular | `[ -f fitxer.txt ]` |
| `-d`     | És un directori      | `[ -d carpeta ]`    |
| `-r`     | És llegible          | `[ -r fitxer.txt ]` |
| `-w`     | És modificable       | `[ -w fitxer.txt ]` |
| `-x`     | És executable        | `[ -x script.sh ]`  |

---

## Activitat 1: Missatge d’ajuda amb `-h`

### Crea un script anomenat `ajuda.sh`

```bash
#!/bin/bash

if [ "$1" = "-h" ]
then
  echo "Ús: $0 <nom_fitxer>"
  echo "Mostra informació bàsica d’un fitxer."
  exit 0
fi

echo "No s’ha demanat ajuda. Continua l’execució..."
```

### Executa'l per a cada una de les situacions

[Captura de pantalla]


### Respòn a...

1. Què fa l’script quan el primer paràmetre és `-h`?
2. Què passa si no es posa cap argument?
3. Quina variable s’està comparant i amb quin operador?

---

## Activitat 2: Validar si hi ha argument amb `-z` i `$#`

### Crea `valida.sh`

```bash
#!/bin/bash

if [ -z "$1" ]
then
  echo "Error: no s’ha especificat cap fitxer."
  echo "S’han rebut $# paràmetres."
  exit 1
fi

echo "Fitxer o paràmetre rebut: $1"
```

### Executa'l per a cada una de les situacions

[Captura de pantalla]

### Respòn a...

1. Quin operador detecta si la cadena està buida?
2. Quina diferència hi ha entre `$1` i `$#`?
3. Si passes diversos arguments, què mostrarà `$1`?

---

## Activitat 3: Comprovació de fitxer amb `-e`

### Crea `comprova.sh`

```bash
#!/bin/bash

if [ -z "$1" ]; then
  echo "Cal indicar un fitxer!"
  exit 1
fi

if [ -e "$1" ]; then
  echo "El fitxer '$1' existeix!"
else
  echo "El fitxer '$1' NO existeix!"
fi
```

### Executa'l per a cada una de les situacions

[Captura de pantalla]

### Respòn a...

1. Quin operador comprova si el fitxer existeix?
2. Si en comptes d’un fitxer passes un directori, què passa?
3. Com ho canviaries perquè només acceptés fitxers regulars?

---

##  Activitat 4: És fitxer, directori o executable?

### Crea `tipus.sh`

```bash
#!/bin/bash

for ITEM in "$@"; do
  if [ -d "$ITEM" ]; then
    echo "$ITEM és un directori."
  elif [ -f "$ITEM" ]; then
    echo "$ITEM és un fitxer regular."
  elif [ -x "$ITEM" ]; then
    echo "$ITEM és executable."
  else
    echo "$ITEM no existeix o no és accessible."
  fi
done
```

### Executa'l per a cada una de les situacions

[Captura de pantalla]

### Preguntes de reflexió

1. Què passa si passes més d’un argument?
2. Per què aquí s’utilitza `"$@"` en comptes de `$*`?
3. Quin tipus d’estructura de control s’utilitza per diferenciar casos?
