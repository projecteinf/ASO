# Llibreries en bash

- Conjunt de fitxers amb funcions reutilitzables (i, si cal, constants/variables comunes).

- Objectiu: evitar duplicar lògica en múltiples scripts (validacions, logs, parseig, utilitats…).  

# Avantatges

- El codi existeix una sola vegada:

    - Correccions i millores en un únic punt,

    - Menys duplicació i inconsistències,

    - Scripts principals més curts i llegibles,

    - Manteniment i extensió més fàcils.

- Permet estandarditzar coses com errors i logs (tots els scripts treballen igual).

# A tenir en compte

- Les llibreries es carreguen amb source i comparteixen el mateix procés.

- Una llibreria defineix funcions (i constants o variables). Evita executar accions en carregar-se.

- Evita exit, canvis de directori o variables globals; usa local.

- No barregis dades i errors: stdout per resultats, stderr per errors, i codis de sortida per indicar èxit/fallada.  

- Utilitza rutes absolutes per a la comanda `source`.

## Exemple

- Utilitzem una carpeta lib per emmagatzemar la llibreria.

### Llibreria (./lib/wlog.sh)

```bash

#!/bin/bash

WRITE_LOGFILE() {
  if [ $# -ne 1 ]
    then
	    echo "Ús: WRITE_LOGFILE LOGFILE" >&2  
        return 1    # Error en la crida de la funció. No hauria de passar mai!
  fi

  local LOGFILE="$1"
  local USEREX="$USER"

  if [ -n "$SUDO_USER" ]
  then
    USEREX="$SUDO_USER"
  fi
  
  local ts="$(date '+%F %T')"
  
  echo "$ts:$USEREX:Acció registrada" >> "$LOGFILE"
}


```

###  Programa principal

```bash

#!/bin/bash

SCRIPT_DIR="$(pwd)" # El SCRIPT_DIR només és vàlid si executem el script des del directori on resideix
                    # Execució CORRECTE: bash script.sh 
                    # Execució INCORRECTE: bash SCRIPTS/script.sh

source "$SCRIPT_DIR/lib/wlog.sh" # Incloure/Afegir llibreria al script

WRITE_LOGFILE "error.log"

```

## Exercici

1. Executa el script amb sudo i sense. Observa el contingut del fitxer. Substitueix el codi utilitzant la comanda `whoami` i observa el resultat. Quina és la millor opció? Raona la teva resposta.

1. Modifica el codi anterior per tal que el missatge de l'acció ("Acció registrada") sigui configurable des del programa principal.

1. Modifica el script per tal que funcioni en el cas que el fitxer de log només el pugui modificar l'usuari root.



