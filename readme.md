# <u> Présentation du réseau informatique de l'entreprise RTequila </u>

## <u>Sommaire :</u> 
* ## Introduction 
* ## Partie réseau IUT
* ## Partie réseau DMZ
* ## Partie réseau Virtualisé

----

## <u>Introduction :</u>

<br>

![schém2](assets/schém2.png)

<br>

### Notre réseau d'entreprise est donc divisé en 3 parties. Nous avions un contrôle total sur le réseau de la DMZ ainsi que le réseau Virtualisé, que ce soit sur le choix d'adressage, de technologies à chosir, le nombre de machines et ainsi de suite. Cependant nous avions pas de modification possible à faire au niveau de l'IUT car nous gérons pas le réseau.

<br>
 
### Technologies utilisées et Matériel choisi :
* ### **Routeurs** : Mikrotik, Image Cisco c3775 (GNS3)
* ### **Switchs** : Mikrotik, Switch de base inclus dans GNS3
* ### **Machines / Serveur Web** : Images docker personalisées, machines de l'IUT
  
----

## <u>Partie réseau IUT :</u>

<br>

### Le réseau de l'IUT est la porte d'entrée et de fin de nos deux réseaux locaux, il nous permettra d'accéder à Internet.
<br>

### Nous avons chosi de nous adresser en 10.214.150.50/16. Il n'y a pas de raison particulière hormis le fait qu'elle n'était pas. La liaison se fait avec un routeur **Mikrotik**
<br>

-----

## <u>Partie réseau DMZ :</u>

<br>

### Pour ce qui est du réseau de la DMZ, il s'agit d'un réseau local, qu'on a chosi d'adresser en **172.16.0.0/29** car nous avions très peu de machines à l'intérieur mais surtout pour des raisons de sécurité car cette partie du réseau doit être accesible sur Internet. **Cela nous laisse donc la possibilité d'adresser 6 machines.** 

<br>

### Dans ce réseau nous avons une machine de l'IUT **qui fait office de serveur Web** (fonctionne avec le package apache2) mais aussi de **service DNS** pour le réseau entier (avec le package bind9). Les fichiers de configuration sont trouvables ici dans le dossier nommé "Configuration". 

<br>

### Nous avons mis sur le schéma au milieu du réseau de la DMZ un switch mais en réalité ce rôle est également effectué par le routeur Mikrotik du début de réseau car il le permet.

<br>

### Le routeur Mikrotik joue le rôle de firewall sur le réseau entier et permet de filtrer les paquets et donc sécuriser le réseau de la DMZ ainsi que le réseau virtualisé de l'entreprise. Il bloque les requêtes 'ping' venant de l'extérieur mais autorise celles venant de l'intérieur. Il fait office de NAT et permet donc de convertir les différentes adresses privées en adresses publiques utilisables pour SuRfEr sur l'Internet.

<br>

### Parmis les 6 adresses possibles, 3 sont déjà prises par les différents routeurs et les 3 autres sont prises lorsqu'un Vlan (donc venant du réseau virtualisé) sort sur la DMZ pour aller sur Internet par exemple. Chaque VLAN a une adresse spécifique qu'elle peut utiliser sur le réseau de la DMZ et qui est statique et unique.

<br>

### Finalement, le routeur Cisco est émulé sur GNS3 et sera expliqué dans la partie suivante. 

<br>

----

## <u>Partie réseau Virtualisé :</u>

<br>

### *L'entièreté du réseau virtuel a été réalisé sur le logiciel GNS3, les fichiers de projets sont disponibles dans le dossier GNS3.*
<br>

### Cette partie du réseau est la plus complète. Elle comporte 4 Vlan pour chacuns des services de l'entreprise, un serveur web, un routeur cisco qui joue également le rôle de service DHCP pour l'adressage automatique ainsi qu'un switch principal qui permet de séparer les communications en plusieurs VLAN : 
<br>

* ### Pôle administratif, Vlan numéro 10 
* ### Pôle commercial, Vlan numéro 20
* ### Service Informatique, Vlan numéro 30
* ### Serveurs, Vlan numéro 40
<br>

### Les Vlan ne peuvent pas communiquer ensemble sauf à destination du Vlan 40 pour accéder au serveur web interne mais également si une communication SSH est initié depuis le service informatique.

<br>

### Les machines sont des dockers que nous avons créés à la main, ils sont tous composés d'un script bash qui permet d'établir le service SSH au démarrage, mais également qui change l'adresse MAC de la machine et enfin fait une requête DHCP.

<br>

### Le service DHCP fonctionne sur le routeur Cisco pour des raisons pratiques, notamment avec la gestion des ACL. Il est d'ailleurs intéressant de noter que le service fonctionne en prenant en compte les **adresses MAC**. Cela signifie donc que chaque machine **aura toujours la même adresse IP**, ce qui facilite d'autant plus l'administration du réseau pour le service Informatique et notamment pour les accès SSH (on peut cibler une personne en particulier directement et ainsi de suite).

<br>

### Le serveur Web est un docker apache2 récupéré sur le site ***Dockerhub*** et n'a pas de stockage persistent, il faut donc faire une manipulation à chaque démarrage de GNS3. Le contenu du docker a été vérifié à la main pour voir s'il n'y avait pas de malware ou de contenu invoulu.

<br>

### Le switch principal est le switch de base proposé sur GNS3. Il permet de faire des Vlan sans entrer de commandes. Il n'y aura donc pas de commandes dans les fichiers de compte-rendu. Bien évidemment, des recherches ont quand même été faites sur la configuration de Vlan Cisco, notamment en révisant ce que nous avions fait dans la ressource R103.

<br>

### Le routage inter-Vlan est effectué par le routeur Cisco émulé. Le switch principal permet juste de marquer les paquets du bon Vlan-ID.

<br>
<br>
<br>
<br>


#### ***Plusieurs détails techniques sont disponibles ici : https://github.com/TimFlp/Carnet-de-bord/blob/master/Journal-de-bord.md***

<br>

Merci d'avoir lu jusqu'ici.   
*~Timothée Fulop* 
