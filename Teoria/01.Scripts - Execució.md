# Execució scripts

## 1. **Execució directa amb ruta relativa o absoluta**

```bash

./script.sh
/home/usuari/scripts/script.sh

```

* Necessita permisos d’execució (`chmod +x`).
* Fa servir el shebang (#!) del fitxer per a la seva execució.

---

## 2. **Invocació com a comanda**

```bash

script.sh

```

* Només funciona si la carpeta on és l’script està dins la variable **\$PATH**.


---

## 3. **Execució passant-lo explícitament a l’intèrpret**

```bash

bash script.sh
sh script.sh

```

* No cal que tingui permisos d’execució.
* No fa servir el shebang (#!)  del fitxer, sinó el binari que has escrit (`bash`, `sh`, `dash`, etc.).

---

## 4. **Execució en el shell actual**

```bash

. script.sh
source script.sh

```

* No es crea cap procés nou.
* Les variables i funcions queden disponibles després de l’execució.
* Nom habitual: **sourcing** o **execució en el shell actual**.
* No fa servir el shebang (#!) 

