# Linux Administration CheatSheet

## Informations relatives au système

```bash
# Linux System Information
uname -a

# Linux Kernel information
uname -r

# Operating system : distribution et version
cat /etc/os-release

# Afficher depuis combien de temps le système est démarré
uptime

# Nom d'hôte
hostname

# Toutes les adresses IP locales de l'hôte
hostname -I

# Derniers reboots du système
last reboot

# Horodatage local
date

# Utilisateurs en ligne
w

# Utilisateur qu'on utilise
whoami

```

## Informations Hardware

```bash
# Informations du CPU
cat /proc/cpuinfo

# Informations mémoire
cat /proc/meminfo

# Affichage des informations dans le "Ring Buffer" Kernel 
# --> il s'agit de l'espace mémoire dédié au Kernel
dmesg

# Espace mémoire occupé et libre
free -h

# Affichage des équipements PCI
# Les équipements PCI sont branchés directement sur le PCI Bus de la carte mère
# Il s'agit des équipements utilisés pour le son, la vidéo ou le réseau
lspci -tv 

# Affichage des équipements USB
lsusb -tv

# Affichage des infos Hardware contenues dans le DMI/SMBIOS
dmidecode

# Informations concernant le disque dur (monté sur le répertoire /dev/sda)
hdparm -i /dev/sda

# Réaliser un test rapide en lecture sur le disque dur 
hdparm -tT /dev/sda

# Tester les blocks illisibles sur le disque
badblocks -s /dev/sda
```

## Informations sur les performences

```bash
// Some code
```
