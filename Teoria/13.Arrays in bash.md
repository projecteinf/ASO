# 1. Què és un array en Bash

Un array en Bash és una variable que pot emmagatzemar **diversos valors indexats** (IPs, noms de servidors, ports, etc.).

```bash
SERVERS=(192.168.1.10 192.168.1.11 192.168.1.12)
echo ${SERVERS[0]}
echo ${SERVERS[1]}
echo ${SERVERS[2]}
```
---

# 2. Declaració explícita d’un array

`declare -a` permet indicar explícitament que una variable és un array.

```bash
declare -a backup_SERVERS
backup_SERVERS=(10.0.0.5 10.0.0.6)
echo ${backup_SERVERS[1]}
```

---

## Exercici

Tenim el següent array:

```bash

SERVERS=("192.168.1.10 HostA" "192.168.1.11 HostB" "192.168.1.12 HostC")

```

1. Escriu la comanda que mostra tot l’element associat a la IP 192.168.1.11.

1. Quina limitació té aquesta estructura de dades si el nombre de servidors augmenta?

1. Si utilitzem dos arrays, quina avantatge(s) tenim?

---

# 3. Accedir a tots els elements

`"${array[@]}"` retorna **tots els elements**, respectant cada IP o valor complet.

Exemple

```bash
HOSTS=("server web" "db server" "ldap server")
for h in "${HOSTS[@]}"; do
  echo "$h"
done
```

## Exercici

Donat el següent codi, 

```bash
HOSTS=("server web" "db server" "ldap server" "db server")
for h in "${HOSTS[@]}"; do
  echo "$h"
done
```

Fes els canvis necessaris per tal que em mostri per pantalla el número de vegades que un element està contingut en l'array.

- Crida

```bash

bash script.sh "server web"

```

- Sortida

```
server web: 1
```

- Crida

```bash

bash script.sh "db server"

```

- Sortida

```
db server: 2
```

- Crida

```bash

bash script.sh "mail server"

```

- Sortida

```
mail server: 0
```

---

# 4. Nombre d’elements

`${#array[@]}` indica quants elements hi ha (quants servidors, quantes IPs…).

```bash
ips=(192.168.0.1 192.168.0.2 192.168.0.3)
echo "Nombre d'IPs: ${#ips[@]}"
```

---

# 5. Recorregut amb índex  

`${!array[@]}` permet obtenir els índexs.
Útil quan vols mostrar posició + valor.

```bash
SERVICES=(ssh nginx docker)
for i in "${!SERVICES[@]}"; do
  echo "Servei $i → ${SERVICES[$i]}"
done
```

---

## 6. Modificar un element existent

Es pot actualitzar un valor concret (per exemple, canvi d’IP).

```bash
SERVERS=(192.168.1.10 192.168.1.11)
SERVERS[1]=192.168.1.20
echo "${SERVERS[@]}"
```

---

## 7. Cerca d’elements en un array

- Bash no disposa de cap instrucció específica per cercar elements dins d’un array.

- La cerca s’ha de fer recorrent l’array element a element i comparant cada valor amb el criteri de cerca.

```bash
SERVERS=("192.168.1.10" "192.168.1.11" "192.168.1.12")

SEARCH_IP="192.168.1.11"
I=0
TROBAT=0

while [ $I -lt ${#SERVERS[@]} ] && [ $TROBAT -eq 0 ]
do
  if [ ${SERVERS[$I]} = "$SEARCH_IP" ]
  then
        TROBAT=1
  fi
  I=$(( I+1 ))
done

if [ $TROBAT -eq 0 ]; then
  echo "$SEARCH_IP No trobada"
else
  echo "$SEARCH_IP Trobada"
fi
```

## Exercici

Disposem de dos arrays:

- un array amb adreces IP.  

- un segon array amb el servei associat a cada IP.  

Els dos arrays estan alineats per índex, de manera que:

- la IP de la posició "I" està associada amb el servei de la posició "I".   

Cada IP només pot tenir un únic servei associat.

Escriu un script en Bash que:

1. Demani a l’usuari una IP i un servei.

1. Comprovi si la IP existeix dins de l’array d’IPs.

1. En cas afirmatiu, verifiqui si el servei associat a aquella IP coincideix amb el servei introduït.

1. Mostri un missatge indicant si la relació IP–servei és correcta o no.


## 8. Afegir nous elements (nous HOSTS)

`+=` permet afegir nous elements sense redefinir l’array.


```bash
firewall_rules=(22 80)
firewall_rules+=(443)
echo "Ports oberts: ${firewall_rules[@]}"
```

## Exercici

Modifica el script anterior, de la següent manera.

Si la relació IP -> Servei no és correcta, s'afegirà la nova relació en l'array.  

---

## 9. Eliminar un element (punt crític!)

`unset` elimina un element, però **NO reindexa** l’array.

```bash
ports=(22 80 443)
unset ports[1]
echo "Ports actius: ${ports[@]}"
echo "Índexs: ${!ports[@]}"
```

## Exercici

Modifica el script anterior, de la següent manera.

L'usuari decideix si vol crear una nova relació IP -> Servei o bé eliminar-la.


---

## 10. Arrays amb entrada de l’usuari

`read -a` permet llegir múltiples valors (IPs, usuaris…) en una sola línia.

```bash
echo "Introdueix IPs separades per espais:"
read -a ip_list
echo "IPs introduïdes: ${ip_list[@]}"
```

---

## 11. Convertir una cadena en array (configuració típica)

Amb `IFS` podem dividir una cadena (fitxers de config, variables d’entorn).

```bash
export SERVERS="web01,db01,ldap01"
IFS=',' read -ra server_list <<< "$SERVERS"
echo "${server_list[@]}"
```

---
