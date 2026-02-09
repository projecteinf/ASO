# Comanda read i stdin

L'objectiu d'aquest document és entendre la diferència quan es rediregeix l'entrada en la crida del script i quan es rediregeix dins el script. Aquesta diferència és important quan el nostre script executa comandes remotes de ssh.

Podem resumir la idea amb aquestes dues sentències

* `while read ... < fitxer`

* `script.sh < fitxer`

---

# Pràctica guiada: Entrada estàndard (STDIN) en Bash

## Part 1 — Teoria

### 1.1 Què és STDIN (entrada estàndard)

En Bash:

* `STDIN` és el flux d’entrada (descriptor 0). Informació d'entrada del script.

* pot venir de:

  * teclat
  * un fitxer
  * una canonada (`|`)
  * una redirecció (`<`)

- En un moment donat, només hi ha un STDIN, i qui el llegeix, el consumeix. Per tant, no podem llegir (consumir) dos cops el mateix STDIN. L'única manera que hi ha de recuperar una informació ja llegida és guardar-la en una variable.

---

## Part 2 — Cas A: `while read ... < fitxer`

### 2.1 Fitxer de prova

#### 2.1.1 Comprova què fa aquesta comanda.

```bash
cat > noms.txt << EOF
Anna
Bernat
Carla
EOF
```
---

**Exercici** 

- Aconseguir el mateix objectiu/resultat utilitzant la comanda echo.  

- Demostrar que s'ha assolit l'objectiu (utilitza la comanda diff).  

---

### 2.2 Script A

```bash
#!/bin/bash

while read nom; do
    echo "Nom: $nom"
done < noms.txt

echo "Fi de l'script"
```

Executa:

```bash
chmod +x scriptA.sh
./scriptA.sh
```

### 2.3 Què passa realment?

* El `while` rep un STDIN privat (el fitxer).

* El script no toca l’STDIN general del procés.  

* El bucle consumeix només el fitxer.  

- Aquest patró és segur. No hi ha interferències entre el STDIN del script (teclat) i el STDIN utilitzat per la comanda read.  

- Ideal quan:

    * vols llegir un fitxer concret.  

    * no vols interferir amb altres lectures.  

---

## Part 3 — Cas B: `script.sh < fitxer`

### 3.1 Script B

```bash
#!/bin/bash

while read nom; do
    echo "Nom: $nom"
done

echo "Introdueix un altre nom:"
read altre 
echo "Has escrit: $altre"
```

- Executa:

```bash
chmod +x scriptB.sh
./scriptB.sh < noms.txt
```

---

**Reflexió** 

- Per què no em deixa escriure res?

---

### 3.2 Problemes de `script.sh < fitxer`

- Guarda aquest script amb el nom de `script.sh`.  

```bash
#!/bin/bash

echo "Processant fitxer..."

while read linia; do
    echo "→ $linia"
done

read -p "Prem ENTER per continuar..."
echo "Continuem"
```

- Executa el script amb

```bash
./script.sh < noms.txt
```

---

**Pregunta**

* Per què no espera ENTER?

---

## Part 5 — Comparació.  

| Aspecte              | `while read < fitxer` | `script.sh < fitxer`    |
| -------------------- | --------------------- | ----------------------- |
| Qui consumeix STDIN  | Només el bucle        | Tot el script           |
| Afecta altres `read` | ❌ No                  | ✅ Sí                    |
| Control del flux     | Alt                   | Baix                    |
| Patró recomanat      | ✅ Sí                  | ⚠️ Només casos concrets |

---

## Part 6 — Solució simple 

- Separem el que és el STDIN del script del STDIN de la comada read (dins el bucle)

```bash
#!/bin/bash

while read linia; do
    echo "→ $linia"
done < noms.txt

read -p "Prem ENTER per continuar..."
echo "Continuem"
```

- El script i la comanda read(del bucle) tenen origens diferents.

- El resultat del script, no depèn de com s’executa

---

##  Part 7 — Idea clau 

- `script.sh < fitxer`

    - NO és “una altra manera” de llegir un fitxer

    - És canviar el STDIN de tot el procés

---

# Part 8 — El problema real: SSH i STDIN

## 8.1 Concepte  

- Quan executes:

```bash

./script.sh < fitxer

```

i  dins del script hi ha un `ssh`: **SSH hereta l’STDIN del procés pare**.

- No “crea” un STDIN nou.

---

## 8.2 — Exemple  

- Guarda el següent script amb el nom de scriptSSH.sh
```bash
#!/bin/bash

while read ip; do
    echo "Connectant a $ip"
    ssh ec2-user@"$ip" "hostname"
done < hosts.txt

echo "Fi"

```
### Fitxer `hosts.txt`

- Adapta a IPs de màquines que tinguis

```
10.0.0.1
10.0.0.2
10.0.0.3
```

- Execució:

```bash
chmod +x  scriptSSH.sh
./scriptSSH.sh
```

---

Respòn a les següents preguntes

- Què hauria de fer el script?  

- Què fa realment?  

---

## Què fa `ssh` per defecte?

* Llegeix STDIN per  

  * autenticació  

  * comandes remotes  

  * canals interactius

- Conseqüència

    * `ssh` **consumeix l’STDIN**.  

    * El `while read` es queda sense dades. Les ha consumit ssh!!  

- Conclusió: el bucle no falla: **li han robat l’entrada**.

---

## Solució 

- Diem de forma explícita quina és l'entrada estàndard (STDIN) de ssh.

```bash
ssh ec2-user@"$ip" "hostname" < /dev/null
```

---

# Reflexió

- En la majoria de les pràctiques que hem fet fins el moment, he demanat que el script es pogués executar utilitzant el redireccionament d'entrada. Aquest fet, respòn bàsicament a:  

    - Més flexible, jo decideixo quin fitxer utilitzar.  

    - Facilita poder testejar diferents fitxers que representen diferents situacions.

