# PRÀCTICA GUIADA: La comanda `case` en Bash

## Estructura `case` 

```bash
case VARIABLE in
    valor1)
        # Ordres
        ;;
    valor2)
        # Ordres
        ;;
    *)
        # Per defecte
        ;;
esac
```

* Cada bloc acaba amb `;;`.
* El `*` actua com a "else" (cap coincidència).
* Es poden unir diversos patrons amb `|` (p. ex. `start | Start`).

---

## 🧪 Activitat 1: Prova bàsica amb text fix

### Crea un fitxer anomenat `colors.sh`

```bash
#!/bin/bash

color="blau"

case $color in
  vermell)
    echo "Color càlid"
    ;;
  blau)
    echo "Color fred"
    ;;
  verd)
    echo "Color natural"
    ;;
  *)
    echo "Color desconegut"
    ;;
esac
```

### Executa'l per a cada una de les situacions

[Captura de pantalla]

### Preguntes de reflexió

1. Quina variable s’està avaluant dins del `case`?
2. Quina línia indica el final de cada bloc?
3. Què fa el bloc amb `*`?

---

## 🧩 Activitat 2: `case` amb paràmetre

### Crea `accio.sh`

```bash
#!/bin/bash

opcio=$1

case $opcio in
  start | Start)
    echo "Iniciant el servei..."
    ;;
  stop | Stop)
    echo "Aturant el servei..."
    ;;
  restart | Restart)
    echo "Reiniciant el servei..."
    ;;
  *)
    echo "Ús: $0 {start|stop|restart}"
    exit 1
    ;;
esac
```

###  Executa el programa amb diferent nombre d'arguments (cada un dels casos)

[Captura de pantalla]

### Preguntes de reflexió

1. Què passa quan l’opció no coincideix amb cap cas?
2. Per què hi ha una línia amb `exit 1`?
3. Quina diferència hi ha entre `start` i `Start`?
4. Com s’indica més d’un patró en una mateixa línia?
