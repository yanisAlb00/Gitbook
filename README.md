# Kernel

Les Kernel Linux offrent aux programmes 2 types de ressources

1. Les ressources d'exécutions (flots d'instructions tels que c = f(a + b))
2. Les fichiers

## Ressources d'exécutions

### Threads

Un thread correspond à un flot d'instruction.

Pour faire du multitâche sur un OS, il est nécessaire de passer d'un thread à un autre : **changement de contexte**

Un changement de contexte est effectué à l'initiative :

* Du noyau : multitâche préemptif
* D'un programme : multitâche coopératif

\-> Le changement de contexte se fait en photographiant l'état du premier thread et en restaurant un second Thread en repartant de l'état photographié.

### Ordonnancement et synchronisation

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>Entrelacement d'exécution</p></figcaption></figure>

Les synchronisations permettent d'éviter les entrelacements qui peuvent fausser un calcul multi-threads.

Principaux types de synchronisations :

* Exclusions mutuelles (mutex)
* Sémaphores
* Conditions
* Files de message

Les synchronisations permettent par exemple de protéger une ressource matérielle en bloquant l'exécution d'instructions grâce à une ressource logique qui la précède dans la file d'exécution.

### Espace d’adressage et mémoire virtuelle

On distingue 2 types d'adresses :

* **Les adresses physiques** : manipulées en lecture/écriture par le processeur
* **Les adresses effectives** : utilisées par le processeur pour ses instructions et ses données

**Le MMU (Memory Management Unit)** intégré dans le processeur permet de faire la traduction adresses effectives < - > adresses physiques

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>Espaces d'adressage et traduction d'adresse</p></figcaption></figure>

\-> ce fonctionnement simplifie le multi-tâche

Il est possible de partager des zones de mémoire physique entre différentes applications.

La gestion **VMM (Virtual Memory Management)** repose sur le mécanisme de pagination du MMU :

L'espace d'adressage est potentiellement supérieur à la mémoire physique de la machine et certaines adresses virtuelles peuvent correspondre à une donnée sur disque plutôt qu'en mémoire physique.

La mémoire physique fait office de cache vers l'espace d'adressage (accès plus rapide mais taille inférieure à celle du disque).

Une application correspond à deux données :

* Une configuration de son espace d'adressage
* Un ensemble de threads

**Un processus** = La combinaison de ces deux données

## Fichiers

### Opérations sur les fichiers

Sous UNIX, on accède à un fichier du système par l'intermédiaire d'une **interface de programmation unique** associée à différentes opérations :

* read
* write
* seek = déplacement de la tête de lecture
* select = attente d'opérations externes (lecture/écriture) par d'autres applications
* stat = consultation des infos du fichier

Cette interface unique permet d'accéder aussi bien à une carte son qu'un fichier de type /etc/passwd

Les fichiers ne supportent pas forcément tous les mêmes opérations :

* **Fichiers de type bloc** (fonction seek supportée)
  * Fichiers stockés sur le disque
  * Disque lui-même
* **Fichiers en mode caractère** (lecture/écriture séquentielle)
  * Socket réseau
  * Imprimante

Sous UNIX, certains fichiers supportent d'autres opérations (interfaces étendues) :

* Opérations `accept` ou `bind` pour les sockets
* Opérations `readdir` pour les répertoires
* Opérations `ioctl` pour les interactions directes entre les applications et les pilotes de périphérique
  * Permet par exemple de modifier la fréquence d'échantillonnage d'une carte son ou la résolution d'une carte graphique
* ...

### Espaces de nommage

L'espace de nommage rassemble la plupart des fichiers accessibles dans une arborescence unique et globale à tout le système.

Un fichier est identifiable par un chemin unique :

```bash
/home/toto/.bashrc
```

Les fichiers spéciaux sont également présents dans l'espace de nommage.

Certains fichiers n'apparaissent pas dans l'espace de nommage comme, par exemple, les sockets réseau.

L'espace de nommage est unique mais il peut être constitué de plusieurs sous arborescences (système de fichier monté ou file system) :

* Données physiques (disque FAT ou ext2)
* Données virtuelles (/proc)

{% file src=".gitbook/assets/linux_kernel.pdf" %}
Source
{% endfile %}

