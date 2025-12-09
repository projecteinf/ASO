# Scripts amb redireccionament

# 1. “Informe de comprovació”

Crea un que generi un informe a partir d’un fitxer d’entrada.

L’script ha de:

* Llegir un fitxer de text que conté noms d’arxius, un per línia.  

* Obtenir les propietats de cada un dels fitxers llegits.  

* Desar en un fitxer `resultats.txt` les dades obtingudes dels arxius existents.  

* Desar en un fitxer `errors.txt` els missatges d’error que es produeixin durant l'obtenció de les propietats del fitxer.  

* Inclou en el script un exemple d'execució.  

---

# 2. Informe permisos arxius

Disposem d'un arxiu amb diferents noms d'arxius, un per línia. Volem guardar en un arxiu, de nom "permisos", el seu nom, el tipus d'arxiu (l/-) i els permisos del propietari del fitxer. 

Suposem que disposem de la següent informació de cada un dels fitxers

```
-rw-rw-r-- 1 mboada mboada   53 Jan 10 09:54 arxius.txt
-rw-rw-r-- 1 mboada mboada   10 Jan 10 09:48 errors.txt
lrwxrwxrwx 1 mboada mboada   10 Jan 10 10:06 lerrors.txt -> errors.txt

```

El contingut del fitxer permisos hauria de ser:

```
-rw- arxius.txt
-rw- errors.txt
lrwx lerrors.txt -> errors.txt

```

* No es pot visualitzar cap error per pantalla.  

* En el cas que hi hagi errors, no es volen guardar (forat negre).  

* Utilitzeu l'opció -c de la comnada cut per a obtenir la informació.  

* Utilitzeu fitxers amb el mateix propietari i grup propietari.  

* Utilitzeu fitxers amb mides semblants.  

* Cal incloure en l'entrega, l'exemple dels arxius que s'ha utilitzat per a fer les proves i el resultat d'aplicar el script a l'exemple. Tal i com teniu en l'enunciat.

---

# 3. Informe de còpies

- Crea un script que copïi els arxius especificats en les carpetes especificada. 

- Passarem la informació al script amb un fitxer format per dues columnes.

    - 1a -> Nom del fitxer a copiar. Pot especificar-se en ruta relativa o absoluta.   

    - 2a -> Path on es vol copiar el fitxer.  

Exemple de fitxer

```
"arxius.txt" "/tmp/"
"arxius.txt" "/home/mboada/Documents/"
"errors.txt" "/tmp/"
"lerrors.txt" "/tmp/"
"Horaris_25-26_DOCENTS (Provisionals v.3).pdf" "/home/mboada/Documents/"
```

* El script l'ha de poder executar un usuari regular (no root).  

* Els problemes en la còpia es registren a (/var/log/copia/error.log).  

    * Creeu el directori amb l'usuari root.  

* No s'ha de visualitzar cap error per pantalla.  

* Abans d'intentar resoldre l'exercici comprova què fan les següents comandes.  

```bash

DIRACT=$(pwd)
echo "$DIRACT"
QUISOC=$(whoami)
echo "$DIRACT"
ROOTUSER=$(cat /etc/passwd | grep "root" )
echo "$ROOTUSER"

```

- Entrega

    - Cal afegir en el script, com a comentaris

        * Permisos de la carpeta "/var/log/copia".  

        * Permisos del fitxer script.  

        * Exemple d'execució del script.  

---

# 4. Tot instal·lat

Crea un script que:

- Instal·li totes les aplicacions/paquets especificats en un fitxer. Cada paquet s'especificarà en una línia pròpia.  

- Faci 2 anotacions en el registre del sistema en una carpeta que crearà el propi script "aptaut".  

    - apt.log : es registren totes les operacions. Per a cada operació es registra la data, l'usuari i el nom del programa/paquet instal·lat.

    - apt.err : es registren els errors que s'han produït en la instal·lació.  

- Només executarà el script l'usuari root o usuaris amb permisos sudo adequats.  

- Cal que el script mostri el mínim d'informació possible per pantalla.  

- Exemple contingut fitxers de registre

## Fitxer: apt.log 
```
Fri Oct 10 12:06:19 PM CEST 2025 mboada paquetNoValid
Fri Oct 10 12:06:20 PM CEST 2025 mboada wget
```

## Fitxer: apt.err 
```
E: Unable to locate package paquetNoValid
```

- Entrega

    - Cal afegir en el script, com a comentaris

        * Permisos de la carpeta "aptaut".  

        * Permisos del fitxer script.  

        * Exemple d'execució del script.    

---

# 5. Informe de mida dels arxius

Disposem d'un arxiu amb diferents noms d'arxius, un per línia. Volem mostrar per pantalla els arxius ordenats, de major a menor mida. Si hi ha un arxiu que no existeix, cal que es guardi la informació en un fitxer de nom notFound.

* Utilitza la comanda du per a obtenir la mida de cada un dels arxius. 

* No es pot visualitzar cap error per pantalla.

