#  🚀 TP2: Solutions

Remise du travail pratique 2
Travail individuel.

Identification
```
Cours      : Utilisation et administration des systèmes informatiques
Sigle      : INF1070
Session    : Hiver 2023
Groupe     : 11
```
Enseignant : Johnny Tsheke Shele

Auteur     : Patrick Patenaude (PATP01129302)


 
## Solution de la mission M01

État de la mission : résolue

Démarche

Voici le résultat du script tp2/checkM01.sh : "Tout fonctionne correctement."



## Solution de la mission M02

État de la mission : résolue

Démarche
#### M02.1 : 
Afficher la liste de tous les containers Docker présent sur la machine :  
``` docker ps -a ```
Savoir combien de containers dockers sont présent sur la machine : 
```docker ps -aq | wc -l ```
- Réponse : 5

Pour savoir combien de container Docker sont en train de tourner :
```docker ps --filter "status=running" | wc -l ```
- Réponse : 4


#### M02.2 : 
La conduite : 
```
docker network inspect tp2_default | grep '"IPv4Address": "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]+\/[0-9]\+"' | cut -d '"' -f 4
```

La réponse : 
```
172.18.0.3/16
172.18.0.2/16
```





## Solution de la mission M03

État de la mission : résolue, partiellement résolue, non résolue

Démarche

#### M03.1
Obtenir les en-têtes de la réponse HTTP : ```curl -kI hhtps://localhost```
- Réponse : "Caddy"




#### M03.2
La commande utilisée : ```ssh -p 2222 root@localhost```



#### M03.3
Il y a un seul processus actif sur le serveur web, qui est Caddy.
Trouvé par la commande : ```ps aux | grep caddy```




#### M03.4
Le chemin du fichier de configuration : ```/etc/caddy/Caddyfile```




Contenu du fichier : 
```
{
    local_certs
}

nextcloud.localhost {
    reverse_proxy localhost:8080
}

localhost {
    respond "Hello, world!"
}
```

- Commande utilisée : ```cat /etc/caddy/Caddyfile```







## Solution de la mission M04

État de la mission : résolue

Démarche
#### M04.1
- L'adresse IPv4 de la base de donnée mariadb est : ```172.18.0.2```

- La commande ping : ```ping 172.18.0.2 -c 4``` nous affiche que les 4 paquets envoyés ont été reçu avec succès. Cela signifie que la machine peut communiquer avec le serveur de base de données.

- Résultat obtenu : "4 paquets transmis, 4 reçus, 0% paquets perdus, temps 3058 ms"


#### M04.2
J'ai installé mysql à l'aide de la commande ```sudo apt-get install mysql-client```
En utilisant ensuite la commande ```ls /usr/bin/mysql```, je prouve que l'executable est bien présent.



#### M04.3
Je me suis connecté à la base de donnée mariadb avec le client MySQL. Voici la commande utilisé : ```mysql -h 172.18.0.2 -u root -p```



#### M04.4
- 'create database nextcloud;' : ```Query OK, 1 row affected (0,00 sec)```
- 'create user 'patrickpatenaude'@'%' identified by 'qwerty';  :  ```Query OK, 0 rows affected (0,00 sec)```
- 'grant all privileges on nextcloud.* to 'patrickpatenaude'@'%';  :  ```Query OK, 0 rows affected (0,00 sec)```
- 'flush privileges' ;  :  ```Query OK, 0 rows affected (0,00 sec)```



#### M04.5
```
+-----------------------------------------------------------------------------------------------------------------+
| Grants for patrickpatenaude@%                                                                                   |
+-----------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `patrickpatenaude`@`%` IDENTIFIED BY PASSWORD '*AA1420F182E88B9E5F874F6FBE7459291E8F4601' |
| GRANT ALL PRIVILEGES ON `nextcloud`.* TO `patrickpatenaude`@`%`                                                 |
+-----------------------------------------------------------------------------------------------------------------+
2 rows in set (0,00 sec)

```





## Solution de la mission M05

État de la mission : résolue
Démarche
À compléter

#### M05.1
 Adresse IP du serveur nextcloud : ```172.18.0.3```
 Adresse IP du serveur mariadb : ```172.18.0.2```
 
#### M05.2
 Port de Nextcloud : ```80```
 Port de MariaDB :  ```3306```.
 
 
#### M05.3
 Le script 'occ' se trouve dans le repertoire '/var/www/html/occ'. Il est trouve par la commande ```find / -name occ```
 
 Le shebang de ce script est : ```#!/usr/bin/env php```. Il est trouve par la commande ```head -n 1 /usr/src/nextcloud/occ```
 
 
 
#### M05.4
 La commande affiche les instructions du script, tel que l'usage, les options, les commandes disponibles, etc.
 
#### M05.5
 
 VOici la commandde qui utilise le script occ pour convertir la base de donnees Nextcloud qui etait auparavant dans le format "mysql" en un format "mariadb" : 
 ```
 sudo -u "#33" php occ db:convert-type --port=3306 --password=qwerty mysql patrickpatenaude mariadb nextcloud
 ```

-  ```sudo -u "#33"``` : exécute la commande en tant qu'utilisateur #33, qui est l'utilisateur sous lequel Nextcloud s'exécute.
- ```php occ``` : exécute le script occ pour gérer les opérations de Nextcloud.
- ```db:convert-type``` : commande à exécuter pour convertir le type de la base de données.
- ```--port=3306``` : spécifie le port utilisé par la base de données (3306 est le port par défaut pour MariaDB/MySQL).
- ```--password=qwerty``` : spécifie le mot de passe de l'utilisateur de la base de données (dans ce cas, patrickpatenaude).
- ```mysql``` : spécifie le type de base de données avant la conversion.
- ```patrickpatenaude``` : nom d'utilisateur de la base de données.
- ```mariadb``` : spécifie le type de base de données après la conversion.
- ```nextcloud``` : nom de la base de données utilisée par Nextcloud.
 





## Solution de la mission M06

État de la mission : résolue

#### M06.1 : 
```
DRIVER    VOLUME NAME
local     tp2_caddy_data
local     tp2_mariadb
local     tp2_nextcloud
```

#### M06.2 :
```
#!/bin/bash

# Vérifier qu'il y ait un argument
if [ -z "$1" ]; then
  echo "Utilisation : $0 container_id"
  exit 1
fi

# Récupérer le nom du conteneur et le chemin à sauvegarder
container="$1"
container_name=$(docker inspect --format '{{.Name}}' "$container" | sed 's/\///g')
chemin=$(docker inspect --format='{{range .Config.Env}}{{println .}}{{end}}' "$container" | grep '^PATH' | cut -d= -f2)

# Vérifier qu'un chemin a été trouvé
if [ -z "$chemin" ]; then
  echo "Aucun chemin trouvé pour $container"
  exit 1
fi

# Récupérer le nom du volume
volume=$(docker inspect --format='{{range .Mounts}}{{if eq .Destination "'"${chemin}"'"}}{{.Name}}{{end}}{{end}}' "$container")

# Vérifier si un volume est monté
if [ -n "$volume" ]; then
  # Sauvegarder le volume
  docker run --rm --volumes-from "$container" -v "$PWD":/backup alpine tar czf "/backup/${container_name}_${volume}.tar.gz" -C "$chemin" .
  echo "Sauvegarde du volume $volume terminée"
else
  # Sauvegarder le répertoire
  docker run --rm -v "$PWD":/backup "$container" tar czf "/backup/${container_name}.tar.gz" -C "$chemin" .
  echo "Sauvegarde du répertoire $chemin terminée"
fi
```


#### M06.3 :
```
-rw-r--r-- root/root   6059546 2023-04-22 23:00 backup/mariadb_mysql.tar.gz
-rw-r--r-- root/root 169369137 2023-04-22 23:00 backup/nextcloud_html.tar.gz
```



#### M06.4 :
Apres avoir installe et executer la commande ```shellcheck backup.sh```, rien n'est rien affiche au terminal apres la gestion des erreurs.





## Solution de la mission M07

État de la mission : résolue

Démarche

#### M07.1
Voici la tache cron qui doit etre executee : 
```
0 17 * * * /home/patp01129302/Téléchargements/tp2/backup.sh 045ebed38c01
```

#### M07.2
D'apres la sortie de la commande ```systemctl status cron.service```, le service cron est active sur la machine virtuelle car le statut du service est 'active (running)'






## Solution de la mission M08

État de la mission : résolue

Démarche

#### M08.1 :
- Pour trouver le repertoire de l'utilisateur 'admin', apres avoir effectue la commande : 'docker exec -it nextcloud /bin/bash',
il faut naviguer au repertoire de donnees Nextcloud ou sont stockes les fichiers de l'utilisateur admin avec la commande : ```cd /var/www/html/data/admin/files```


- A l'aide de la commande ```df -h /var/www/html/data/admin/files```, nous apercevons que les fichiers sont montes sur le point de montage ```/var/www/html```

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda5        24G   19G  4.3G  82% /var/www/html
```



#### M08.2 :
Une fois conecte sur Nextcloud, nous pouvons retrouver tous les fichiers en cliquant sur l'icone "Fichiers" dans le menu en haut a gauche de l'interface. Cela nous mene a l'explorateur de fichiers de Nextcloud.






## Solution de la mission M09

État de la mission : non-résolue








