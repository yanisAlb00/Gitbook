---
description: >-
  https_www.it-connect.fr/?url=https%3A%2F%2Fwww.it-connect.fr%2Fcommande-sudo-comment-configurer-sudoers-sous-linux%2F
---

# Sudo

Sudo = Substitue User do

La commande `sudo` permet d'exécuter une commande entant qu'un autre utilisateur (**par defaut root**)

Les membres du groupe sudo ont le droit de lancer toutes les commandes si la ligne suivante est présente dans le fichier `/etc/sudoers` :

```bash
%sudo     ALL=(ALL:ALL)  ALL
```

\
