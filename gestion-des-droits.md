# Gestion des droits

## Droits standards



<figure><img src=".gitbook/assets/Screenshot from 2024-11-10 11-31-06.png" alt=""><figcaption></figcaption></figure>

Tout est fichier sur Linux :&#x20;

* Les processus
* Les périphériques
* Les répertoires

Ils sont tous associés à des droits (RWX) respectivement associés au compte propriétaire, au groupe propriétaire et à tous les autres utilisateurs.

Modifier les droits associés à un fichier :&#x20;

```
chmod
```

Modifier le propriétaire d'un fichier :&#x20;

```
chown
```

> Seul root a les droits pour faire un chown

## Droits spéciaux

### SetUID

La commande passwd permet de modifier le mdp d'un utilisateur.\
\
Elle doit permettre à un utilisateur de modifier le fichier /etc/shadow qui contient les mdp des utilisateurs

1. Le fichier /etc/shadow n'est accessible en lecture/écriture que pour le compte root

```
ls -l /etc/shadow
-rw-r----- 1 root shadow 1067 Oct 22 18:10 /etc/shadow
```

2. La commande /etc/passwd possède à un SetUID qui permet à n'importe quel utilisateur de l'exécuter comme s'il était root

```
type passwd
passwd is /usr/bin/passwd
ls -l /usr/bin/passwd 
-rwsr-xr-x 1 root root 68248 Mar 23  2023 /usr/bin/passwd
```

Pour attribuer le SetUID :&#x20;

```
chmod 4644 fichier2
ls -li
48370394 -rwSr--r-- 1 yanis yanis 22 Nov  9 13:22 fichier2
```

### Sticky Bit

Le Sticky Bit permet de donner les droits à tous les utilisateurs mais en leur ôtant la possibilité de supprimer le fichier

Par exemple, création d'un fichier dans /tmp :&#x20;

```
yanis@work:/tmp$ ls -li
total 64
4194312 -rw-r--r-- 1 yanis      yanis        15 Nov 10 11:54 fichier1

```

Ajout de 1000 dans le chmod et le sticky-bit apparaît avec l'option T

```
yanis@work:/tmp$ chmod 1644 fichier1 
yanis@work:/tmp$ ls -li
total 64
4194312 -rw-r--r-T 1 yanis      yanis        15 Nov 10 11:54 fichier1

```





