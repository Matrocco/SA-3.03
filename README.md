# SA-3.03
SAÉ 3.03 faite en coopération avec 3 camarades de classe : CUBIZOLLES Estéban, KARBOWSKI Géraud, ALLART Baptiste


Voici une présentation structurée du projet **SAÉ 3.03 : Projet pépinière multi-sites**, basée sur les documents fournis :

## Introduction au Projet

Le projet consiste à gérer la migration et l'extension informatique d'une entreprise qui s'agrandit par l'ouverture d'une **succursale distante**. En tant qu'administrateurs réseaux et télécoms, votre mission est de redéployer l'intégralité de l'infrastructure (équipements, postes clients et serveurs) tout en sécurisant les échanges et les connexions Internet entre les sites.

---

## Architecture et Services à Déployer

<img width="1631" height="747" alt="Capture d’écran 2026-01-19 160749" src="https://github.com/user-attachments/assets/21349ed3-1689-4807-b5e2-f14e3e7528aa" />


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

# Tableau d'Adressage Maquette Réseau

## 1. Site Siège (Droite)

| Zone / VLAN | ID VLAN | Adresse Réseau   | Masque (CIDR) | Passerelle (Gateway) |
| :---        | :---:   | :---             | :---:         | :---                 |
| **ADMIN** | 10      | `192.168.10.0`   | /24           | `192.168.10.254`       |
| **PROD** | 20      | `192.168.20.0`   | /24           | `192.168.20.254`       |
| **PERSO** | 30      | `192.168.30.0`   | /24           | `192.168.30.254`       |
| **VIDEO** | 40      | `192.168.40.0`   | /24           | `192.168.40.254`       |
| **GUEST** | 50      | `192.168.50.0`   | /24           | `192.168.50.254`       |
| **SERVEUR** | 60      | `192.168.60.0`   | /24           | `192.168.60.254`       |

## 2. Site Succursale (Gauche)

| Zone / VLAN | ID VLAN | Adresse Réseau   | Masque (CIDR) | Passerelle (Gateway) |
| :---        | :---:   | :---             | :---:         | :---                 |
| **ADMIN** | 10     | `192.168.11.0`  | /24           | `192.168.11.254`      |
| **PROD** | 20      | `192.168.21.0`   | /24           | `192.168.21.254`       |
| **PERSO** | 30      | `192.168.31.0`   | /24           | `192.168.31.254`       |
| **VIDEO** | 40      | `192.168.41.0`   | /24           | `192.168.41.254`      |
| **GUEST** | 50     | `192.168.51.0`  | /24           | `192.168.51.254`      |
| **SERVEUR** | 60     | `192.168.61.0`  | /24           | `192.168.61.254`      |


## 3. Services Hébergés (Détail Serveurs)

| Site           | Nom Serveur       | IP (Statique)    | Rôles / Services                                  |
| :---           | :---              | :---             | :---                                              |
| **Siège** | `DOCKER`  | `192.168.60.1`  |   Conteneurs : DNS, SYSLOG, Proxy, Mail, WEB, BDD                         |
| **Succursale** | `Windows Serveur` | `192.168.110.2`  | Active Directory, Fichiers                        |
| **Succursale** | `Replicat - DNS`          | `192.168.110.1` |  DNS Secondaire/Réplicat  | 
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

