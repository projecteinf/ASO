# Pipe (|) en Linux

* És un operador de redirecció.  

* S’escriu amb el símbol `|`.  

* Serveix per enviar la sortida d’una comanda com a entrada d’una altra.  

* És una eina de composició: fa que comandes senzilles es puguin combinar per fer coses més potents.

---

# Diferència amb `>` i `<`.  

* `>`: envia la sortida a un fitxer.  

* `<`: agafa l’entrada d’un fitxer.  

* `|`: envia la sortida a una altra comanda.  

---

# Funcionament bàsic

```bash

comanda1 | comanda2

```

* `comanda1` genera una sortida.  

* `comanda2` utilitza aquesta sortida com a entrada.  

---

# Exemples senzills

* Mostrar processos i filtrar només els que contenen "bash":

```bash

ps -ef | grep bash

```

* Comptar quants arxius hi ha en un directori:  

```bash

ls | wc -l

```

* Veure les 5 primeres línies de `/etc/passwd`:  

```bash

cat /etc/passwd | head -n 5

```

---

# Pipes múltiples

Es poden encadenar diverses comandes:  

```bash

cat /etc/passwd | grep bash | wc -l

# Idees clau a remarcar


* Funciona amb comandes que llegeixen de l’entrada estàndard i escriuen a la sortida estàndard.
* Ajuda a treballar de forma modular i eficient.

---

Vols que et faci també un esquema visual tipus infografia (imatge) amb fletxes que mostri clarament "sortida → entrada"?

```

Mostra quants usuaris tenen bash com a shell.

---

