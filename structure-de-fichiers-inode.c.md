# Structure de fichiers inode.c

La strcuture inode.c est un fichier caractérisé par différentes informations :&#x20;

* Sa taille
* Son propriétaire
* Le groupe propriétaire
* Ses permissions
* La date de ses derniers accès, etc.

Les données présentes dans une structure inode.c peuvent être des adresses directes ou indirectes :&#x20;

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Dans cet inode, il y a :

* **les adresses de blocs directes**. Ces 12 champs contiennent les adresses directes des données du fichier sur les périphériques de type blocs les contenant (disque dur, périphériques externes type clé USB, etc.) :

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

* **les adresses de blocs indirects**. Ces champs contiennent simplement des pointeurs vers d'autres inodes dont la totalité des champs est adressée pour des blocs de données.

L’adresse contenue dans la ligne 13 renvoie à un nouvel inode ou bloc de pointeurs. Ce nouvel inode contient, lui, **128 lignes.** Chacune des adresses contenues dans ces lignes renvoient vers un bloc de données. On dit que ces blocs de donnés sont indirects :

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

L’adresse contenue dans la ligne 14 renvoie à un nouvel inode ou bloc de pointeurs de **128 lignes.**  Chacune des adresses contenues dans ces lignes renvoient vers un nouvel inode contenant à nouveau 128 lignes (oui c’est énorme !) Et chaque adresse contenue dans chaque ligne renvoie vers un bloc de données. On dit que ces blocs de donnés sont doublement indirects :&#x20;

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Pour l'adresse 15 , les bloc de données sont triplement indirects...
