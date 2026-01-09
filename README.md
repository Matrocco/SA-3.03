# SA-3.03
SAÉ 3.03 faite en coopération avec 3 camarade de classe 


Voici une présentation structurée du projet **SAÉ 3.03 : Projet pépinière multi-sites**, basée sur les documents fournis :

## Introduction au Projet

Le projet consiste à gérer la migration et l'extension informatique d'une entreprise qui s'agrandit par l'ouverture d'une **succursale distante**. En tant qu'administrateurs réseaux et télécoms, votre mission est de redéployer l'intégralité de l'infrastructure (équipements, postes clients et serveurs) tout en sécurisant les échanges et les connexions Internet entre les sites.

---

## Architecture et Services à Déployer

<img width="1264" height="905" alt="SchemaPKTSAE303" src="https://github.com/user-attachments/assets/a6b1d822-2411-4671-b79a-cedc9dca7001" />

### Infrastructure Réseau

L'infrastructure doit être segmentée en plusieurs réseaux spécifiques pour isoler et sécuriser les flux :

* **ADMIN** : Unique réseau autorisé à accéder aux équipements en SSH ou Web.


*  **PERSONNEL** : Pour la communication entre les postes des employés.


*  **VIDEO** : Flux prioritaire (QoS) pour la diffusion sur téléviseurs connectés.


*  **PRODUCTION** : Réseau isolé pour couper le trafic entre les halls de production et les employés.


*  **WIFI** : Deux réseaux distincts (Personnel sécurisé et Invité avec accès Web uniquement).



### Services Serveurs

Le déploiement s'appuie sur un domaine **societeX.pepiniere.rt** et comprend:

*  **Active Directory** avec espaces personnels et partagés.


*  **Messagerie et Web** : Serveur de mail interne et serveur Web public interrogeant un réplicat de base de données.


*  **Bases de Données (BDD)** : Une BDD "production" au siège et un réplicat en lecture seule pour le Web, avec des utilisateurs distincts pour la sécurité.


*  **Gestion des Logs** : Un serveur **Syslog centralisé** pour collecter et organiser les journaux de tous les équipements.


*  **Filtrage et DNS** : Un proxy au siège pour tout le trafic extérieur et un DNS principal avec son réplicat en succursale.

### Tableau d'adressage :
Endroit,Machine,Vlan,IP,Masque,Gateway,DNS,Plage DHCP
Siège,Routeur,X,10.X.1.1,/8,X,X,X
Siège,Routeur,Trunk,192.168.X.254,/24,X,X,X
Siège,PC1,10 Vidéo,192.168.10.100,/24,192.168.10.254,192.168.10.253,X
Siège,Serveur 1,10 Vidéo,192.168.10.253,/24,192.168.10.254,Source,192.168.X.100-200
Siège,PC2,20 Vidéo,192.168.20.100,/24,192.168.20.254,192.168.10.253,X
Siège,PC3,30 Vidéo,192.168.30.100,/24,192.168.30.254,192.168.10.253,X
Siège,PC4,40 Prod,192.168.40.100,/24,192.168.40.254,192.168.10.253,X
Siège,BDD 1,40 Prod,192.168.60.253,/24,192.168.40.254,X,X
Siège,AP 1,100 WIFI,192.168.100.100,/24,192.168.1000.254,X,X
,,,,,,,
Succursale,Routeur,X,10.X.2.1,/8,X,X,X
Succursale,Routeur 2,Trunk,192.168.X.254,/24,X,X,X
Succursale,PC5,10 Vidéo,192.168.10.101,/24,192.168.10.254,192.168.10.253,X
Succursale,Serveur 2,10 Vidéo,192.168.10.252,/24,192.168.10.254,Réplicat,X
Succursale,PC6,20 Vidéo,192.168.20.101,/24,192.168.20.254,192.168.10.253,X
Succursale,PC7,30 Vidéo,192.168.30.101,/24,192.168.30.254,192.168.10.253,X
Succursale,PC8,40 Prod,192.168.40.101,/24,192.168.40.254,192.168.10.253,X
Succursale,BDD 2,40 Prod,192.168.60.252,/24,192.168.40.254,X,X
Succursale,AP 2,100 WIFI,192.168.100.101,/24,192.168.1000.254,X,X

---

## Organisation et Travail à Réaliser

Le projet se déroule en plusieurs phases chronologiques :

1.  **Autonomie (30h - Groupes de 2)** : Définition du plan détaillé, configuration des équipements réseaux via simulation (Packet Tracer, GNS3) et création des machines virtuelles.


2.  **Encadrement (4h)** : Aide aux étudiants de RT1 sur leur SAÉ 12, ce qui servira également à votre propre infrastructure.


3.  **Réalisation et Mise en production (2 jours - Groupes de 4)** : Câblage physique, injection des configurations et déploiement réel des services.


4.  **Soutenance (20 min)** : Évaluation individuelle sur l'ensemble du projet.



---

## Données Techniques Clés

*  **Matériel** : Routeurs Cisco série c800, switches type 2960/3560.


*  **Adressage WAN** : 10.X.1.1/8 pour le siège et 10.X.2.1/8 pour la succursale (VLAN 800).


*  **Redondance** : Obligatoire au siège avec deux switches de niveau 3, optionnelle en succursale.


*  **Synchronisation** : L'espace partagé de la succursale doit se synchroniser quotidiennement avec celui du siège.



---

## Évaluation (Total sur 20 points)

*  **Préparation & Réalisation** : 12 points (6 + 6).


*  **Soutenance & Questions** : 4 points.


*  **Rendu final & Aide aux RT1** : 4 points (2 + 2).



Souhaitez-vous que je vous aide à détailler le plan d'adressage IP ou à lister les étapes prioritaires pour la configuration des serveurs ?
