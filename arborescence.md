# Arborescence

L'arborescence de fichiers Linux suit un standard : FHS (File Hierachy Standard)

{% embed url="https://fr.wikipedia.org/wiki/Filesystem_Hierarchy_Standard" %}

{% embed url="https://refspecs.linuxfoundation.org/fhs.shtml" %}

Arborescences faisant partie de la norme :&#x20;

* /boot : répertoire contenant tout le nécessaire pour démarrer le système : la configuration du bootloader, le noyau, les fichiers ramfs… ;
* /etc pour les fichiers de configuration
* **`/bin`**: ce sont les commandes critiques pour le bon fonctionnement du système, quel que soit son objectif. Elles sont lancées par le système et par l'administrateur ;
* **`/sbin`**: ce sont les commandes uniquement à destination de l'administrateur pour la gestion du système.
* **`/usr/bin`**: ce répertoire contient majoritairement les commandes à destination de tous les utilisateurs du système, privilégiés ou non.
* **`/usr/sbin`**: ce sont des commandes à nouveau à destination unique de l'administrateur, mais non critiques pour le bon fonctionnement du système.

Arborescences ne faisant pas partie de la norme :&#x20;

* /home : fichiers personnels de l'utilisateur
* /root : tâches d'administration

> Déconseillé d'associer une boite mail ou des fichiers à l'utilisateur root

Arborescence non présente sur le disque (vitrines virtuelles gérées en mémoire par Linux) :&#x20;

* /proc
* /sys



