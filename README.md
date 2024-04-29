# Kernel



[https://inf3173.uqam.ca/610-pagination.pdf](https://inf3173.uqam.ca/610-pagination.pdf)[https://inf3173.uqam.ca/610-pagination.pdf](https://inf3173.uqam.ca/610-pagination.pdf)

{% file src=".gitbook/assets/linux_kernel.pdf" %}
[https://www.irif.fr/\~carton/Enseignement/Architecture/Cours/Virtual/linux.pdf](https://www.irif.fr/\~carton/Enseignement/Architecture/Cours/Virtual/linux.pdf)
{% endfile %}

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

## Espaces de nommage

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



## Interaction entre le noyau et les applications utilisateur

Certaines opérations sont dites **privilégiées** et ne peuvent être réalisées que par le **noyau**.

Par exemple, une application ne peut pas modifier son espace d'adressage ni accéder à l'espace d'adressage d'une autre application.

On distingue 2 modes d'exécution des instructions supportées par le processeur :

* Le mode privilégié
* Le mode utilisateur

\-> Pour faire appel aux services du noyau, les applications passent par des **appels système (ou syscall)**.

appel système = opération réalisée par une application avec changement de privilèges

## Gestion de la mémoire

### Mémoire physique

L'OS est chargé de gérer la RAM (mémoire physique et tangible) en permettant son allocation et sa désallocation par pages physique en utilisant le mécanisme de pagination du MMU.

Dans un système x86 , la taille fixée pour les pages physiques est de 4 Ko.



{% file src=".gitbook/assets/610-pagination.pdf" %}
[https://inf3173.uqam.ca/610-pagination.pdf](https://inf3173.uqam.ca/610-pagination.pdf)
{% endfile %}

Une page physique peut être référencée par plusieurs pages virtuelles.

Chaque page physique est associée à une structure qui maintient un compteur (justement pour gérer le fait que plusieurs pages virtuelles peuvent accéder à la même page physique). Le reste de la structure est géré par le sous-système concerné par la page physique :&#x20;

* Système de fichiers
* Système de swapping

L'allocation des pages physiques se fait par la fonction mm/page alloc.c: alloc pages()  qui repose sur l'algorithme de type buddy system. Elle fait appel au thread noyau kswapd (mm/vmscan.c) pour swapper des pages physiques lorsqu’il n’y a plus assez de pages physiques disponibles.

L'algorithme slab allocator est implémenté au-dessus de cette fonction pour gérer les plus petites tailles d'objets à allouer (quelques octets).

Le noyau Linux découpe la mémoire physique en zone :

* La zone DMA (16 Mo)  pour les transferts directs entre les périphériques et la mémoire
* Une zone pour les diverses allocations dynamiques (en-dessous de 1 Go)
* Une zone HIGHMEM pour la mémoire physique au-delà de 1 Go

Ces zones sont représentées par la structure linux/mmzone.h:zone t

### Mémoire virtuelle

Chaque processus dispose de son propre espace d'adressage.

Dans un système x86, les adresses de cet espace d'adressage sont codées sur 32 bits ce qui correspond à une taille de 4 Go.

L'espace d'adressage est découpé en pages qui possédent ou non une correspondance avec une page physique (selon les tables de traductionn de la MMU).

#### **Traduction d'adresses**

Linux utilise un mécanisme de pagination à 3 niveaux :&#x20;

* PGD : répertoire de pages global
* PMD : Pages du milieu dans lequel chaque page globale correspond à une table PTE
* PTE : Table de pages

Dans un système x86, la pagination n'est que de 2 niveaux (les répertoires de pages du milieu ont une taille de 1 entrée).

Linux fonctionne toujours comme si la pagination était de 3 niveaux indépendemment du fonctionnement du processeur (x86 ne supporte que 2 niveaux).

#### Découpage de l'espace d'adressage

L'espace d'adressage de chaque processus est découpé en 2 parties :

1. **Espace utilisateur de l'espace d'adressage** : De 0 à include/asm/page.h:PAGE OFFSET (fixé à 3 Go sur x86)
2. **Espace noyau de l'espace d'adressage** : De PAGE\_OFFSET à la fin de la mémoire (fixé à 4Go sur x86)

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

L'adresse PAGE\_OFFSET + x en virtuel correspond à l'adresse x en physique.

C'est l'**Identity Mapping** de la mémoire physique dans l'espace noyau. Lorsque la quantité de mémoire physique excède 1 Go, c'est le système HIGHMEM qui est utilisé pour accéder à la mémoire physique au-delà de 1 Go.

