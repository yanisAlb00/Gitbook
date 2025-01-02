# eBPF

## Définition

eBPF = Extented Berkeley Packet Filter

eBPF est utilisé pour étendre les capacités du Kernel en toute sécurité.

L'espace utilisateur doit toujours passer par le Kernel pour interagir avec les éléments physiques du système.

Par exemple, une application n'a pas besoin de se soucier des interfaces réseau qui sont présentes sur la machine. Elle a juste besoin de communiquer avec le Kernel qui effectue le Mapping.

## Cas d'usages d'eBPF

* Filtrage réseau
* Observabilité
* Politiques de sécurité

[Vidéo Youtube expliquant le fonctionnement d'eBPF](https://www.youtube.com/watch?v=eVsMkXDE_5I)

## Avantages d'eBPF

L'intérêt de eBPF réside dans le fait qu'il s'agisse d'une extension au niveau Kernel :

* Haut niveau de privilège
* Haut niveau de performance (eBPF tourne au niveau de la jonction logique entre le Kernel et les composants physiques)

## Représentation schématique du positionnement d'eBPF

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

