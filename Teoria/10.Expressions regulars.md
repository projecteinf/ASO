# Expressions regulars

- Patró que serveix per 
    - Buscar i/o transformar text.

- Instruccions bàsiques que permeten expressions regulars

    * grep  
    * sed
    * awk
    * find


```bash
grep -E "error" /var/log/boot.log  # Cerca text "error" a cada línia de boot.log
grep -i "error" /var/log/boot.log
```

- Les expressions regulars incorporen símbols especials que ens permeten construir el patró de cerca.  

---

# Símbols especials bàsics

## "^"

- Inici de línia 

### Exemples

```bash
grep -E "^Error" /var/log/boot.log    # Totes les línies que comencin per Error

# Match amb
echo "Error" | grep -E "^Error"
echo "Error garrafal!" | grep -E "^Error"

# No fa match amb

echo "Ha comés un Error" | grep -E "^Error"
```
---

## "$"

- Final de línia

### Exemples

```bash
grep -E "Error$" /var/log/boot.log    # Totes les línies que acabin per Error

# Match amb
echo "Error" | grep -E "Error$"
echo "Un gran Error" | grep -E "Error$"
echo "Un mestre del TError" | grep -E "Error$"

# No match amb
echo "Error!" | grep -E "Error$"
```

## Combinació "^" i "$"

- Ens permet buscar el text exacte.

### Exemples

```bash
grep -E "^Error$" /var/log/boot.log    # Linies que només tinguin Error

# Match amb
echo "Error" | grep -E "^Error$"

# No match amb
echo "Un Error" | grep -E "^Error$"
echo " Error" | grep -E "^Error$"
```

---

## "."

- Qualsevol caràcter  

### Exemples

```bash
grep -E ".rror" /var/log/boot.log    # Totes les paraules que contenen rror

# Match amb
echo "Error" | grep -E ".rror"
echo "error" | grep -E ".rror"
echo "Terror" | grep -E ".rror"
echo "Terrorific" | grep -E ".rror"

# No match amb

echo "rror" | grep -E ".rror"      # Falta caràcter associat al .
echo "rrors" | grep -E ".rror"     # Falta caràcter associat al .
```

---

## "*"

- 0 o més repeticions del caràcter anterior al símbol 

### Exemples


```bash
grep -E "Er*or" /var/log/boot.log    

# Match amb
echo "Eor" | grep -E "Er*or"
echo "Eror" | grep -E "Er*or"
echo "Error" | grep -E "Er*or"
echo "Errrrrror" | grep -E "Er*or"
echo "Ha comès un nou Error" | grep -E "Er*or"

# No match amb

echo "Un gran Esforç" | grep -E "Er*or" # Després de la E només hi pot haver r(s) o una "o"
```
---


## "+"

- 1 o més repeticions del caràcter anterior al símbol 

### Exemples

```bash
### Exemples

```bash
grep -E "Er+or" /var/log/boot.log    

# Match amb
echo "Eror" | grep -E "Er+or"
echo "Error" | grep -E "Er+or"
echo "Errrrrror" | grep -E "Er+or"
echo "Ha comès un nou Error" | grep -E "Er+or"

# No match amb
echo "Eor" | grep -E "Er+or"
echo "Un gran Esforç" | grep -E "Er+or" # Després de la E només hi pot haver r(s) 
```
---

## "?"

- 0 o 1 repetició del caràcter anterior al símbol (opcional)

### Exemples

```bash
### Exemples

```bash
grep -E "Warn?ing" /var/log/boot.log    

# Match amb
echo "Warning" | grep -E "Warn?ing"
echo "Waring" | grep -E "Warn?ing"
echo "Un gran Warning" | grep -E "Warn?ing"
echo "Un gran Waring" | grep -E "Warn?ing"

# No match amb
echo "Warring" | grep -E "Warn?ing" # Després de la "r" hi ha d'haver una "n" o una "i"
```
---


## "|"

- Equival a una O

### Exemples

```bash
### Exemples

# Match amb
echo "ERROR" | grep -E "ERROR|Error|error"  
echo "Error" | grep -E "ERROR|Error|error"  
echo "error" | grep -E "ERROR|Error|error"  
echo "Hi ha un error" | grep -E "ERROR|Error|error"  

# No match amb
echo "ERror" | grep -E "ERROR|Error|error"  
```

---

## "[ ]"

- Conjunt de caràcters.

    - [A-Z] qualsevol lletra majúscula
    - [a-z] qualsevol lletra minúscula
    - [0-9] qualsevol dígit
    - [0-2] 0,1,2
    - [aeiou] qualsevol vocal

### Exemples

```bash
### Exemples

# Match amb
echo "ERROR" | grep -E "[A-Z]"  
echo "Error" | grep -E "[A-Z]"  
echo "error" | grep -E "[a-z]"   
echo "ERROR" | grep -E "[A-Za-z]"   # Qualsevol lletra majúscula o minúscula
echo "error" | grep -E "[A-Za-z]"   # Qualsevol lletra majúscula o minúscula
echo "192.168.25.32" | grep -E "[0-9]+\.+" 

# No match amb
echo "error" | grep -E "[A-Z]" 
echo "ERROR" | grep -E "[a-z]" 
echo "F192A.A168F.D25C.E32D" | grep -E "[0-9]+\.+" 
```
---

## "[^ ]"

- Caràcters que no corresponen al conjunt especificat.

    - [^A-Z] Hi ha algun caràcter diferent a una lletra majúscula

    - [^0-9] Hi ha algun caràcter que no és un dígit

### Exemples

```bash
### Exemples

# Match amb
echo "error" | grep -E "[^A-Z]"             # Hi ha caràcters diferents a lletres majúscules
echo "error 3225" | grep -E "[^a-z]"        # Hi ha caràcters diferents a lletres minúscules
echo "Error" | grep -E "[^0-9]"             # Hi ha caràcters diferents a dígits
echo "err" | grep -E "[^aeiou]"             # Hi ha caràcters diferents a les vocals
echo "192.168.25.32" | grep -E "[^0-9]"     # Hi ha caràcters "." diferents a dígits


# No match amb
echo "ERROR" | grep -E "[^A-Z]"             # Tots els caràcters són Majúscules
echo "3225" | grep -E "[^0-9]"              # Tots els caràcters són dígits
echo "err" | grep -E "[^er]"                # Tots els caràcters són "e" o "r"
```
---

## "{}"

- Ens permet especificar el número de repeticions.

    - {n} : número de vegades que s'ha de repetir l'expressió

    - {min,} : mínim de vegades que s'ha de repetir l'expressió

    - {min,max} : número mínim i màxim de vegades que s'ha de repetir l'expressió

### Exemples

```bash
### Exemples

# Match amb
echo "error" | grep -E "r{2}"                  
echo "error i un altre error" | grep -E "r{2}"                  
echo "ERROR: linia 25" | grep -E "[A-Z]{4}" 
echo "11 de desembre de 2025: ERROR: linia 25" | grep -E "[0-9]{3,}" # Almenys 3 dígits seguits
echo "85.15." | grep -E "[0-9]{1,3}"     
echo "11/12/2025: ERROR: linia 25" | grep -E "[0-9]{2}/[0-9]{2}/[0-9]{4}"

# No match amb
echo "eror" | grep -E "r{2}"                  
echo "11/12/25: ERROR: linia 25" | grep -E "[0-9]{3,}" # Almenys 3 dígits seguits
echo "11/12/2025: ERR: linia 25" | grep -E "[A-Z]{4}" 
echo "11/12/25: ERRO linia 25" | grep -E "[0-9]{2}/[0-9]{2}/[0-9]{4}"
```
---


## "()"

- Ens permet agrupar les expressions.

### Exemples

```bash
### Exemples

# Match amb
echo "192.168.15.3" | grep -E "([1-9]{1}[0-9]{0,2}\.){3}[1-9]{1}[0-9]{0,2}"
echo "IP: 192.168.15.3" | grep -E "([1-9]{1}[0-9]{0,2}\.){3}[1-9]{1}[0-9]{0,2}"
echo "IP: 21.168.15.3.6" | grep -E "([1-9]{1}[0-9]{0,2}\.){3}[1-9]{1}[0-9]{0,2}"

# No match amb

echo "IP: 092.168.15.3" | grep -E "([1-9]{1}[0-9]{0,2}\.){3}[1-9]{1}[0-9]{0,2}"
echo "IP: 0921.168.15.3" | grep -E "([1-9]{1}[0-9]{0,2}\.){3}[1-9]{1}[0-9]{0,2}"
echo "IP: 21.168.15.3.6" | grep -E "^([1-9]{1}[0-9]{0,2}\.){3}[1-9]{1}[0-9]{0,2}$"
```
---

Perfecte —aquí tens **4 exemples** de com *regex* avança pas a pas amb el **cursor**, explicats de manera visual i entenedora.
Els faig **molt clars i curts** perquè els puguis posar directament a la documentació.

---

# Resolució del patró

## Paraula: “error”

Patró: `r{2}`

Cadena:

```
e r r o r
0 1 2 3 4
```

### Timeline del cursor

```
Posició 0: e     → no és 'r' → no pot començar r{2}
Posició 1: r r   → coincideix 'rr' → MATCH
Posició 2: r o   → no és 'rr'
Posició 3: o r   → no és 'rr'
Posició 4: r ?   → falta una segona r
```

✔️ Resultat: hi ha *match* per `rr` a posició 1.

---

## Número mòbil (format simple)

Cadena: `612345678`
Patró: `^6[0-9]{8}$`
(Regla: ha de començar per 6 i tenir 9 dígits)

### Timeline del cursor

```
^ 6 1 2 3 4 5 6 7 8 $
↑
```

* `^` → el cursor és a inici (OK)
* `6` → coincideix amb el primer “6”
* `[0-9]{8}` → els següents 8 caràcters són dígits:

  ```
  1 2 3 4 5 6 7 8  → OK
  ```
* `$` → hem arribat exactament al final (OK)

✔️ Resultat: tota la cadena compleix el patró.

## Número de mòbil NO vàlid**

Cadena: `512345678`
Patró: `^6[0-9]{8}$`
(Regla: ha de començar per **6** i tenir **9 dígits**)

### Timeline del cursor

```
5 1 2 3 4 5 6 7 8
^
```

* `^6` → al començament de la línia esperem un 6, però hi ha un 5

Resultat: NO match.


---

## Data simple

Cadena: `12/05/2024`
Patró: `[0-9]{2}/[0-9]{2}/[0-9]{4}`

### Timeline del cursor

```
1 2 / 0 5 / 2 0 2 4
↑ ↑   ↑ ↑   ↑ ↑ ↑ ↑
2 dígits / 2 dígits / 4 dígits
```

* `[0-9]{2}` → `12` ✔️
* `/` → coincideix `/` literal ✔️
* `[0-9]{2}` → `05` ✔️
* `/` → coincideix `/` literal ✔️
* `[0-9]{4}` → `2024` ✔️

✔️ Resultat: la data coincideix perfectament amb el patró.

---

## IP molt simple (només “dígits + punt”)**

Cadena: `192.168.25.3`
Patró simple: `([0-9]{1,3}\.){3}[0-9]{1,3}`
(no comprova rangs, només format)

### Timeline del cursor

```
1 9 2 . 1 6 8 . 2 5 . 3
↑↑↑ ↑ ↑↑↑ ↑ ↑↑ ↑ ↑
```

Per cada bloc `[0-9]{1,3}\.`:

- Primer bloc

```
192.
↑↑↑↑
3 dígits i punt → OK
```

- Segon bloc

```
168.
↑↑↑↑
3 dígits i punt → OK
```

- Tercer bloc

```
25.
↑↑↑
2 dígits i punt → OK
```

- Quart bloc `[0-9]{1,3}`:

```
3
↑
1 dígit → OK
```

✔️ Resultat: l’IP simple coincideix.

---

