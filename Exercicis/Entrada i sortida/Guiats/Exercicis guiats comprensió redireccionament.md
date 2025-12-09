 # Objectius

* Comprendre i practicar la redirecció d’errors (`2>`, `2>>`)
* Utilitzar `/dev/null` per ignorar sortides no desitjades
* Aplicar pipes (`|`) per enviar sortides d’una comanda com a entrada d’una altra

---

 # Exercicis

 ## 1. Separar sortida i errors

Executa:

```bash

ls /etc /carpeta_inexistent

```

1. Què mostra per pantalla?
      ____________________________________________________________
2. Repeteix-ho, però guarda:

   * La sortida correcta a `sortida.txt`
   * Els errors a `errors.txt`
        Escriu la comanda completa:

---

---

 ## 2. Afegir errors a un registre

Executa tres comandes que donin error (per exemple directoris inexistents) i afegeix els errors al fitxer `errors.log` sense sobreescriure’l.
   Escriu una de les comandes:

---

---

 ## 3. Ignorar errors

Executa:

```bash

ls /etc /carpeta_inexistent > resultats.txt 2> /dev/null

```

1. Què conté `resultats.txt`?
      ____________________________________________________________
2. Què ha passat amb els errors?
      ____________________________________________________________

---

 ## 4. Només errors

Fes una comanda que mostri un error i redirigeix-lo a un fitxer `error.log`.
   Escriu la comanda:

---

---

 ## 5. Pipes i errors

Executa:

```bash

ls /etc /carpeta_inexistent | wc -l

```

1. Quin resultat obtens?
      ____________________________________________________________
2. Per què `wc -l` no compta la línia d’error?
      ____________________________________________________________
3. Repeteix-ho redirigint els errors a `/dev/null`:

   ```bash
   ls /etc /carpeta_inexistent 2> /dev/null | wc -l
   ```

   Quina diferència hi ha?
      ____________________________________________________________

---

 ## 6. Pipes + redireccions

Fes una comanda que compti quants arxius contenen la paraula “conf” dins `/etc`, i que:

* Guardi la sortida correcta en `resultat.txt`
* Guardi els errors en `errors.txt`

   Escriu la comanda:

---

---

 ## 7. Pipes múltiples

Executa:

```bash

ps aux | grep root | wc -l

```

1. Explica què fa cada part de la comanda:
      ____________________________________________________________
2. Afegeix una redirecció per guardar el resultat en `processos.txt`.
3. Afegeix una redirecció per guardar els errors en `error_proc.txt`.
      Comanda final:

---

---

 ## 8. Provar `/dev/null` amb pipes

Executa:

```bash

find / -name "*.conf" 2> /dev/null | wc -l

```

1. Per què no es mostren errors de permisos?
      ____________________________________________________________
2. Què fa realment la comanda?
      ____________________________________________________________

---

 ## 9. Combinació total — Script

Crea un script `informe.sh` que:

1. Escrigui la data al fitxer `informe.log`
2. Afegeixi quants fitxers hi ha al directori `/etc`
3. Afegeixi també el nombre d’errors trobats en intentar llistar `/etc` i `/carpeta_inexistent`


   Esbossa aquí el teu codi:

```bash
#!/bin/bash

# Codi de l’script
```

---
