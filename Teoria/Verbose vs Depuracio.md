# Verbose vs depuració en scripts Bash

## Necessitat

Quan un script fa diverses passes (llegir configuració, validar, connectar-se a serveis, crear fitxers, aplicar canvis…), sovint necessites:

* Seguir el flux d’execució: en quin punt està i què està intentant fer.  

* Entendre decisions: per què ha entrat a un `if`, per què ha saltat una part.  

* Detectar on falla: quin pas concret ha anat malament.  

* Reduir la incertesa: “què ha passat realment?” quan el resultat no és l’esperat.  

Per això existeixen dos enfocaments complementaris:

1. Verbose: el script explica què fa.

1. Depuració (debug/tracing): el shell mostra què executa.

---

## Conceptes

### Opció vs argument

```bash

ls -l *.log
grep --extended-regexp "root" *.log

```

- Les opcions sempre comencen per - o bé -- . -l i --extended-regexp són opcions

- Els arguments és tota la informació que enviem al script que no comença per - ni per --. root i *.log són arguments de les comandes.

- Els paràmetres posicionals són el conjunt d'opcions i arguments.

### Verbose (mode informatiu)

* És un mode d’informació extra pensat per a l’usuari o per entendre el flux d’execució.  

* Tu decideixes: què s’imprimeix i quan.  

* Objectiu: diagnòstic “humà” (què està fent el script, en quin pas va, quines decisions pren).  Per exemple, si el teu programa té sentències (if -> then -> else) podem saber si la condició es compleix o no.

### Depuració (debug / tracing)

* És un **mode tècnic** per veure què executa realment el shell.  

* Tu no decideixes el detall; el dona el shell.  

* Objectiu: entendre errors d’expansions, variables, comandes fallides, condicions, etc.

---

## 2) Com fer “verbose” 

### Regles simples

* Cridem el nostre script amb una opció (`-v` o `--verbose`). 

    * Indica que volem obtenir més informació sobre el que s'està executant.

* Tots els missatges associats a l'execució verbose s'han de redireccionar a la sortida estàndard d'error (stderr).

    * Permet diferenciar els missatges "normals" del script dels missatges propis generats pel mode verbose.


### Patró 

- En aquest cas, per simplicitat, tenim un únic valor de verbose (log).  

```bash
#!/usr/bin/env bash

VERBOSE=0
EXTRACT=0
ERROR=0

processar_opcions() {
  
  while getopts "vx" OPC; do    # si cridem amb -xv o -vx ho interpreta com a -x i -v
    case "$OPC" in
      v) VERBOSE=1 ;;
      x) EXTRACT=1 ;;
      \?) echo "Opció no vàlida: -$OPTARG" >&2; ERROR=1 ;;
    esac
  done

}

log() { [ "$VERBOSE" -eq 1 ] && echo "[INFO] $*" >&2; }
```

### Exemple d’ús

```bash
echo "ABANS  -> $# : $*"

processar_opcions "$@"

[ $ERROR -eq 1 ] && exit 1

shift $(( OPTIND - 1 ))   # OPTIND és un índex a la següent posició de les opcions del script

echo "DESPRÉS -> $# : $*"
echo -e "OPCIONS ACTIVADES:\n Verbose: $VERBOSE\n Extract: $EXTRACT"

log "Això només surt amb -v"
```


### Exercici


1. Executa: `./script.sh -n`.  

   * Com es gestionen els errors dins el programa? Modifica el codi per tal que un argument incorrecte no sigui motiu de finalització del mateix.

1. Modifica `getopts` i el `case` per acceptar també `-z` com a opció vàlida. Al final, el script ha de mostrar l'estat de totes les opcions (ja ho fa per -v i -x).

1. Comenta la línia `shift $(( OPTIND - 1 ))` i consumeix les opcions al final de la funció `processar_opcions`.

   * Després, imprimeix `$#` i `$*` al main. Ha canviat? Per què?

1. Explica de forma tècnica la diferència de comportament quan el `shift` es fa dins la funció o bé en el programa principal.


---

## Com depurar en Bash

### `set -x` (xtrace)

Mostra cada comanda amb expansions abans d’executar-se.

```bash
set -x   # activa
# ... codi ...
set +x   # desactiva
```

### Recomanació

* En producció, el debug ha d'estar sempre desactivat. Si es vol depurar, cal modificar el script

---

## Comparativa (què és important)

| Aspecte                  | Verbose                        | Depuració (set -x / -x)                  |
| ------------------------ | ------------------------------ | ---------------------------------------- |
| Qui controla el missatge | Tu                             | Bash                                     |
| Detall                   | Moderat, útil per humans       | Molt detall tècnic                       |
| Objectiu                 | Entendre el flux i diagnòstic  | Trobar l’arrel d’un bug tècnic           |
| Sortida recomanada       | stderr (per no trencar stdout) | stderr (per defecte)                     |
| Risc en producció        | Baix si no exposes secrets     | Alt: pot filtrar secrets i dades         |
| Ús típic                 | “Què està fent el script?”     | “Quina comanda exacta s’està executant?” |

---
