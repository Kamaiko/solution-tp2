Remise du travail pratique 2
Travail individuel.

Identification

Cours      : Utilisation et administration des systèmes informatiques
Sigle      : INF1070
Session    : Hiver 2023
Groupe     : Numéro du cours-groupe

Enseignant : <nom de votre enseignant>

Auteur     : <votre nom> (<votre code permanent>)


 
Solution de la mission M01

État de la mission : résolue

Démarche

Voici le résultat du script tp2/checkM01.sh : "Tout fonctionne correctement."







Solution de la mission M02

État de la mission : résolue

Démarche
M02.1 : 
Afficher la liste de tous les containers Docker présent sur la machine :  'docker ps -a'
Savoir combien de containers dockers sont présent sur la machine : 'docker ps -aq | wc -l' 
Réponse : 5

Pour savoir combien de container Docker sont en train de tourner : 'docker ps --filter "status=running" | wc -l'
Réponse : 4


M02.2 : 
La conduite : 'docker network inspect tp2_default | grep '"IPv4Address": "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]+\/[0-9]\+"' | cut -d '"' -f 4
La réponse : 
172.18.0.3/16
172.18.0.2/16






Solution de la mission M03

État de la mission : résolue, partiellement résolue, non résolue

Démarche

M03.1
Obtenir les en-têtes de la réponse HTTP : 'curl -kI hhtps://localhost'
Réponse : "Caddy"




M03.2
La commande utilisée : 'ssh -p 2222 root@localhost'



M03.3
Il y a un seul processus actif sur le serveur web, qui est Caddy.
Trouvé par la commande : 'ps aux | grep caddy' 




M03.4
Le chemin du fichier de configuration : /etc/caddy/Caddyfile




Contenu du fichier : 

{
    local_certs
}

nextcloud.localhost {
    reverse_proxy localhost:8080
}

localhost {
    respond "Hello, world!"
}


Methode utilisée : 'cat /etc/caddy/Caddyfile'







Solution de la mission M04

État de la mission : résolue, partiellement résolue, non résolue

Démarche
M04.1
L'adresse IPv4 de la base de donnée mariadb est : '172.18.0.2'

La commande ping : 'ping 172.18.0.2 -c 4' nous affiche que les 4 paquets envoyés ont été reçu avec succès. Cela signifie que la machine peut communiquer avec le serveur de base de données.

Résultat obtenu : "4 paquets transmis, 4 reçus, 0% paquets perdus, temps 3058 ms"


M04.2
J'ai installé mysql à l'aide de la commande 'sudo apt-get install mysql-client'
En utilisant la commande 'ls /usr/bin/mysql', je prouve que l'executable est bien présent.



M04.3
Je me suis connecté à la base de donnée mariadb avec le client MySQL. Voici la commande utilisé : 'mysql -h 172.18.0.2 -u root -p'



M04.4
'create database nextcloud;' : 'Query OK, 1 row affected (0,00 sec)
'create user 'patrickpatenaude'@'%' identified by 'qwerty';  :  'Query OK, 0 rows affected (0,00 sec)'
'grant all privileges on nextcloud.* to 'patrickpatenaude'@'%';  :  'Query OK, 0 rows affected (0,00 sec)'
'flush privileges' ;  :  'Query OK, 0 rows affected (0,00 sec)'



M04.5






Solution de la mission M05

État de la mission : résolue, partiellement résolue, non résolue

Démarche
À compléter






Solution de la mission M06

État de la mission : résolue, partiellement résolue, non résolue

Démarche
À compléter







Solution de la mission M07

État de la mission : résolue, partiellement résolue, non résolue

Démarche
À compléter







Solution de la mission M08

État de la mission : résolue, partiellement résolue, non résolue

Démarche
À compléter








Solution de la mission M09

État de la mission : résolue, partiellement résolue, non résolue

Démarche
À compléter
