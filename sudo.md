# Sudo

Sudo = Substitue User do

La commande `sudo` permet d'exécuter une commande entant qu'un autre utilisateur (**par defaut root**)

Les membres du groupe sudo ont le droit de lancer toutes les commandes si la ligne suivante est présente dans le fichier `/etc/sudoers` :

```bash
%sudo     ALL=(ALL:ALL)  ALL
```

\
