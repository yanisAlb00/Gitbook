# Liens durs vs Liens symboliques

## Lien dur : ln

Un lien dur va créer une nouvelle référence vers le même bloc de données (numéro d'inode similaire)

```
ls -li
total 4
48372564 -rw-r--r-- 1 yanis yanis 267 Nov  9 12:56 fichier1

ln fichier1 fichier2

ls -li
total 8
48372564 -rw-r--r-- 2 yanis yanis 267 Nov  9 12:56 fichier1
48372564 -rw-r--r-- 2 yanis yanis 267 Nov  9 12:56 fichier2

```

Si on modifie fichier2 , fichier1 sera également modifié

## Lien symbolique : ln -s

Un lien symbolique contiendra l'adresse du fichier où on trouve le bloc de données (numéro dinode différent)

```
ln -s fichier1 fichier3
ls -li
total 8
48372552 -rw-r--r-- 1 yanis yanis 15 Nov  9 13:22 fichier1
48370394 -rw-r--r-- 1 yanis yanis 22 Nov  9 13:22 fichier2
48372138 lrwxrwxrwx 1 yanis yanis  8 Nov  9 13:24 fichier3 -> fichier1

```



