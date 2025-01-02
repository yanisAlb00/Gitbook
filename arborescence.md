# Arborescence

## Norme FHS



L'arborescence de fichiers Linux suit un standard : FHS (File Hierachy Standard)

{% embed url="https://fr.wikipedia.org/wiki/Filesystem_Hierarchy_Standard" %}

{% embed url="https://refspecs.linuxfoundation.org/fhs.shtml" %}

## Arborescences faisant partie de la norme

* /boot : répertoire contenant tout le nécessaire pour démarrer le système : la configuration du bootloader, le noyau, les fichiers ramfs… ;
* /etc pour les fichiers de configuration
* **`/bin`**: ce sont les commandes critiques pour le bon fonctionnement du système, quel que soit son objectif. Elles sont lancées par le système et par l'administrateur ;
* **`/sbin`**: ce sont les commandes uniquement à destination de l'administrateur pour la gestion du système.
* **`/usr/bin`**: ce répertoire contient majoritairement les commandes à destination de tous les utilisateurs du système, privilégiés ou non.
* **`/usr/sbin`**: ce sont des commandes à nouveau à destination unique de l'administrateur, mais non critiques pour le bon fonctionnement du système.
* **`/var/log`**: répertoire contenant l'arborescence de toutes les traces systèmes et applicatives. C'est dans ce répertoire qu'il est possible de consulter les traces des historiques de démarrage du système, de connexion des comptes utilisateurs, d'activité des services réseaux (SSH, HTTPD, SMTP, etc.) ainsi que les traces du noyau. Généralement les applications installées sur le système disposent de leur propre sous-répertoire (**`/var/log/apache2`**&#x70;ar exemple).
* **`/var/run`**: répertoire contenant toutes les données relatives aux processus en cours d'exécution, les sémaphores, les données applicatives, les fichiers numéro de processus, etc.
* **`/var/spool`**: répertoire contenant des données stockées de manière temporaire entre processus. Souvent, ce répertoire est utilisé pour stocker des données relatives à des actions ou tâches à effectuer dans un futur proche par les processus en cours d'exécution.
* **`/var/mail`**: c'est le répertoire de stockage des messageries électroniques locales des comptes utilisateurs du système.

## Arborescences ne faisant pas partie de la norme

* /home : fichiers personnels de l'utilisateur
* /root : tâches d'administration

> Déconseillé d'associer une boite mail ou des fichiers à l'utilisateur root

## Shareable vs unshareable

Les fichiers présents dans les dossiers shareable peuvent être partagés entre plusieurs dossiers.

Exemple de dossier shareable : `/home`

Exemple de dossier unshareable : `/boot`

## Static vs variable

Les répertoires de type static nécessitent une élévation de privilèges pour en modifier le contenu.

Exemple de répertoires static : /boot

Exemple de répertoires variables : /var/www

## Arborescence non présente sur le disque (vitrines virtuelles gérées en mémoire par Linux) :&#x20;

* /proc
* /sys

Les fichiers et répertoires présents dans ces emplacements ne sont pas stockés sur un périphérique de type bloc.

Il s'agit d'informations maintenues en temps réel par le noyau Linux.

Exemple de fichiers intéressants :&#x20;

```
// Informations sur le CPU
yanis@work:~$ cat /proc/cpuinfo
processor	: 0
vendor_id	: AuthenticAMD
cpu family	: 25
model		: 80
model name	: AMD Ryzen 9 5900HX with Radeon Graphics
stepping	: 0
microcode	: 0xa50000d
cpu MHz		: 1197.592
cache size	: 512 KB
physical id	: 0

// Informations sur la version du noyau
yanis@work:~$ cat /proc/version
Linux version 6.1.0-26-amd64 (debian-kernel@lists.debian.org) (gcc-12 (Debian 12.2.0-14) 12.2.0, GNU ld (GNU Binutils for Debian) 2.40) #1 SMP PREEMPT_DYNAMIC Debian 6.1.112-1 (2024-09-30)

// Informations sur la mémoire vive du noyau
yanis@work:~$ cat /proc/meminfo 
MemTotal:       15756444 kB
MemFree:        11352776 kB
MemAvailable:   12405444 kB
Buffers:           65752 kB
Cached:          1357152 kB

// Paramètres passés au démarrage du noyau
yanis@work:~$ cat /proc/cmdline
BOOT_IMAGE=/boot/vmlinuz-6.1.0-26-amd64 root=UUID=4865beae-e479-4069-8bdf-3df7679b4014 ro quiet

// Premier processus du noyau
yanis@work:~$ cat /proc/1/cmdline
/sbin/init
```

* les périphériques de type bloc ou caractères dans les répertoires **`/sys/block`** ou **`/sys/dev`**,
* les drivers dans **`/sys/devices`**,
* les différents systèmes de fichiers dans **`/sys/fs`**,
* les modules du noyau dans **`/sys/module`**.

