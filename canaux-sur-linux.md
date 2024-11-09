# Canaux sur Linux

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Dans la majorité des cas, tous les programmes exécutés sous Linux disposent de 3 canaux de données :

1. **`stdin`**(pour standard input) : c'est le canal de l'entrée standard, et par défaut, lorsque vous lancez une commande, c'est votre clavier. La commande sera éventuellement capable de lire les informations saisies avec le clavier via ce canal. Il porte le descripteur de fichier **numéro 0** ;
2. **`stdout`**(pour standard output) : c'est le canal de la sortie standard, et lorsque vous lancez une commande depuis un terminal, c'est l'écran par défaut. Le résultat et les données affichées par la commande sont diffusés sur l'écran. Il porte le descripteur de fichier **numéro 1** ;
3. **`stderr`**(pour standard error) : c'est le canal du flux concernant les erreurs, et par défaut, lorsque vous lancez une commande, c'est aussi l'écran. La commande va différencier les données “normales” des données “erreur” et peut changer de canal pour diffuser ces informations.

## Canal d'entrée (implicitement le premier argument)

```
// tous équivalent
cat < /etc/os-release
cat 0</etc/os-release
cat /etc/os-release
```

## Canal de sortie (par défaut c'est le terminal)

```
cat /etc/os-release > /home/yanis/os-release
cat /etc/os-release 1> /home/yanis/os-release
```

## Canal d'erreur

```
cat /etc/os-releaseee 1> /home/yanis/os-release 2> /home/yanis/stderr
cat /home/yanis/stderr 
cat: /etc/os-releaseee: No such file or directory

```









