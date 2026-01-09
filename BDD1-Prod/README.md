# Script de backup



#!/bin/bash
# Nom du fichier avec la date
DATE=$(date +%Y-%m-%d_%Hh%M)
BACKUP_NAME="backup_bdd1_$DATE.sql"

# Commande de sauvegarde via Docker
docker exec mariadb_prod mariadb-dump -u root -p'1234' --all-databases > ./backups/$BACKUP_NAME

# Optionnel : Supprimer les sauvegardes de plus de 30 jours pour gagner de la place
find ./backups/ -name "*.sql" -type f -mtime +30 -delete
