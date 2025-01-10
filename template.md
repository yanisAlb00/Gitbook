# template

## Passive information gathering

* whois
* Google search
* crt.sh
*

## Scanning

### IF Network

* ping sweep

```
nmap -v -sn 192.168.50.1-253 -oG ping-sweep.txt --stats-every=5s
```

* put list of alive hosts in hosts.lst

```
grep Up ping-sweep.txt | cut -d " " -f 2 > hosts.lst
```

### ELSE IF Single Host

* put single host in hosts.lst

```
export TARGET = 192.168.50.1
ping $TARGET
echo $TARGET > hosts.lst
```

### FOR EACH host IN hosts.lst

* SYN-SCAN TCP of common ports

```
nmap -sS -iL hosts.lst --stats-every=5s
```

* Scan UDP of common ports

```
nmap -sS -iL hosts.lst --stats-every=5s
```

* SYN-SCAN TCP of all ports

```
nmap -sS -p- -iL hosts.lst --stats-every=5s
```

* Scan UDP of all ports

```
nmap -sU -p- -iL hosts.lst --stats-every=5s
```

## Client-side attack

### Information gathering

#### Analyzing metadata of a file (w/o interaction with target)

```
exiftool -a -u brochure.pdf
```

#### By Interacting with the target

```
gobuster dir -u http://$TARGET -w /opt/seclists/Discovery/Web-Content/big.txt  -x .pdf -o results.txt

```

Or using [https://github.com/v0re/dirb/blob/master/wordlists/common.txt](https://github.com/v0re/dirb/blob/master/wordlists/common.txt)

### Microsoft Office





