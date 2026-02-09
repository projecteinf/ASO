# Funcions en Bash I

## Definició.  

Una funció en Bash és un bloc de comandes amb nom que pots cridar tants cops com calgui. 

---

## Avantatges

* Evitar duplicar codi.  

* Script més llegible.   

    - Noms descriptius: `valida_args`, `comprimir`, ...).  

* Aïllar lògica i reduir errors. 

    - Si hi ha un error en la lògica de la funció, només has de canviar la funció.  

* Reutilitzar el mateix comportament amb diferents entrades (arguments).

---

## Declaració

```bash

hello() {
  echo "Hola!"
}

# o bé
function hello {
  echo "Hola!"
}

```

- Crida

```bash

hello

```

- Punts crítics

    * No hi ha “tipus” de retorn com en Python.

    * Tot és text i resultat de l'execució.

---

## Pas de paràmetres: `$1`, `$2`, `$@`, `$#`

- Exemple:

```bash

#!/bin/bash

mostrar_args() {
  for a in "$@"; do
    echo "[$a]"
  done
}

mostrar_args 1 2 3

```

Claus per no fer errors:

* `$#` = nombre d’arguments.  

* `$1`, `$2`, ... = posicions.  

* `"$@"` és el correcte si vols preservar arguments amb espais.  

* `"$*"` i `$*` tendeixen a ajuntar-ho tot i fer mal quan hi ha espais.

### Exercici.  

```bash

#!/bin/bash
total_args() {
    echo "Total arguments $#"
}

mostrar_args() {
  local a
  for a in "$@"; do
    echo "[$a]"
  done
  total_args "$@"
  total_args "$*"
}

mostrar_args 1 2 3

```

1. Quin és el resultat de l'execució del script anterior.  

1. Com expliques la diferència entre $@ i $*?

---

## Variables dins funcions: `local` i efectes col·laterals

- Les variables d'una funció, mai han de canviar el valor de les variables del programa principal.  

### Exercici.  

Executa el següent codi

```bash

x=1
canvia() { x=99; }
canvia
echo "$x"   

```

1. Quin és el resultat de l'execució?  

1. Quin és el problema?

## Instrucció `local`.  

- Declara una nova variable que només existeix dins la funció.

### Exercici

```bash
x=1

canvia_segur() {
  local x=99
  echo "dins: $x"
}

canvia_segur
echo $x

```

1. Quin és el resultat de l'execució?  

1. Quina és la diferència amb l'execució de l'exercici anterior?  

---
