# Aide configuration DNS 

### Attention au conflit "systemd-resolved"

Si vous êtes sur Ubuntu/Debian, Docker ne pourra pas démarrer le conteneur car le port 53 est déjà pris par le système.

La solution rapide :

    Stoppez le service local : sudo systemctl stop systemd-resolved

    Désactivez-le : sudo systemctl disable systemd-resolved

    Modifiez votre /etc/resolv.conf pour pointer vers 127.0.0.1.



### Rappel important : Le Serial

Chaque fois que vous modifiez ce fichier, vous devez incrémenter le "Serial" (dans l'exemple, il est à 3). Si vous ne le faites pas, Bind9 pensera que le fichier n'a pas changé et ne rechargera pas les modifications.


## Rappel sur les droits d'écriture (Le piège classique)

Sur un serveur Slave, Bind9 doit pouvoir écrire le fichier de zone qu'il reçoit du Master dans le dossier /var/cache/bind.

Sur ta machine hôte (Succursale), lance cette commande :
Bash

Donne les droits au porteur du processus bind (généralement UID 104 ou 105 dans le conteneur)

sudo chown -R 104:104 ./cache-slave

Sans cela, le transfert de zone échouera car le conteneur n'aura pas le droit de créer le fichier db.mon-entreprise.lan.