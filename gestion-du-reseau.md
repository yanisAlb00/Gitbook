# Gestion du réseau

## Nom réseau de la machine

Information présente dans un fichier maintenu par le Kernel

```
//equivalent

cat /proc/sys/kernel/hostname 
work

hostnamectl
 Static hostname: work
       Icon name: computer-convertible
         Chassis: convertible
      Machine ID: bbb005e373484ac2b6a19e2b2a57d057
         Boot ID: 6622c4ef8c2b4e1bac616f266e59038a
Operating System: Debian GNU/Linux 12 (bookworm)  
          Kernel: Linux 6.1.0-26-amd64
    Architecture: x86-64
 Hardware Vendor: ASUSTeK COMPUTER INC.
  Hardware Model: Zenbook UN5401QAB_UN5401QA
Firmware Version: UN5401QAB_UN5401QA.308

```

Modifier le nom du serveur

```
hostnamectl set-hostname twinkle

// ou modifier le fichier /etc/hostname
```

## Interfaces réseau

### Vérifier que le noyau a bien reconnu le matériel

La commande dmesg permet d'affichier les messages qui ont été reçus par le noyau pendant le boot

```
dmesg | grep Network
```

On peut aussi consulter les liens symboliques faire les fichiers référencés par le noyau :&#x20;

```
ls -l /sys/class/net
total 0
lrwxrwxrwx 1 root root 0 Nov  9 11:36 docker0 -> ../../devices/virtual/net/docker0
lrwxrwxrwx 1 root root 0 Nov  9 11:36 lo -> ../../devices/virtual/net/lo
lrwxrwxrwx 1 root root 0 Nov  9 11:36 wlp2s0 -> ../../devices/pci0000:00/0000:00:02.2/0000:02:00.0/net/wlp2s0

```





