# Shell

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

## **Bash, le shell standard de Linux** <a href="#r-7551561" id="r-7551561"></a>

### Afficher le shell par défaut associé à un compte utilisateur

```
grep yanis /etc/passwd
yanis:x:1000:1000:yanis,,,:/home/yanis:/bin/bash

```

Le shell lancé par défaut est /bin/bash

Si un utilisateur est associé à /usr/bin/nologin ou /dev/zero ou /dev/null , il ne pourra pas lancer de shell lors de sa connexion.

### Afficher le shell sur lequel on est connecté

```
yanis@work:~$ echo $SHELL
/bin/bash
```

### Gérer le prompt du shell

```
echo $PS1
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$

```

### Obtenir des informations sur les fichiers

```
file Downloads/*
Downloads/Annonce - Splunker.docx:                                                                Microsoft Word 2007+
Downloads/Conditions Générales de location.pdf:                                                   PDF document, version 1.4, 9 pages
Downloads/Conditions Générales de réservation.pdf:                                                PDF document, version 1.4, 13 pages

```

### Obtenir des informations sur une commande

```
type cat
cat is hashed (/usr/bin/cat)
```



### Afficher les répertoires dans le PATH

```
echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

Par défaut, BASH va vérifier si la commande est présente dans le répertoire courant et si elle n'est pas présente il va vérifier les répertoires dans l'ordre de gauche à droite

Pour ajouter un chemin au $PATH :&#x20;

```
export PATH=$PATH:/path/to/custom/command
```

Pour faire persister la modification, il faut ajouter cette commande au fichier `.bashrc` du /home ou \~ de l'utilisateur

### Bash History

#### Rappel de commande

```
history
   17  ls -l
   18  file Downloads/
   19  file Downloads/*
   20  echo $PATH
   21  cat .bashrc 
   22  history
yanis@work:~$ !17
ls -l
total 32
drwxr-xr-x 2 yanis yanis 4096 Oct 22 18:12 Desktop
drwxr-xr-x 2 yanis yanis 4096 Oct 22 18:12 Documents
drwxr-xr-x 2 yanis yanis 4096 Nov  6 18:03 Downloads
drwxr-xr-x 2 yanis yanis 4096 Oct 22 18:12 Music
drwxr-xr-x 2 yanis yanis 4096 Oct 22 18:12 Pictures
drwxr-xr-x 2 yanis yanis 4096 Oct 22 18:12 Public
drwxr-xr-x 2 yanis yanis 4096 Oct 22 18:12 Templates
drwxr-xr-x 2 yanis yanis 4096 Oct 22 18:12 Videos

```

#### Recherche de commande

```
CTRL +R
(reverse-i-search)`Down': file Downloads/*

```

## Alias d'une commande

```
vi .bashrc

alias ll='ls -lrtha';
```

## Lister les alias

```
alias
alias ll='ls -lrtha'
alias ls='ls --color=auto'

```













