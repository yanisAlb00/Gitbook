# twiggy

## Setup

```
exegol start pg_clamav nightly --vpn /home/yanis/Downloads/universal.ovpn 
```

```
firefox &> /dev/null & 
```

```
tmux capture-pane -pS - > tmux_history.log
```

## Scanning

### Nmap

```
nmap -sC -sV -oA nmap  192.168.110.62  --stats-every=5s
Starting Nmap 7.93 ( https://nmap.org ) at 2025-01-04 11:25 CET
Stats: 0:00:10 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 25.00% done; ETC: 11:25 (0:00:18 remaining)
Stats: 0:00:15 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 75.00% done; ETC: 11:25 (0:00:04 remaining)
Stats: 0:00:16 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 95.51% done; ETC: 11:25 (0:00:00 remaining)
Stats: 0:00:21 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.82% done; ETC: 11:25 (0:00:00 remaining)
Nmap scan report for 192.168.110.62
Host is up (0.014s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 447d1a569b68aef53bf6381773165d75 (RSA)
|   256 1c789d838152f4b01d8e3203cba61893 (ECDSA)
|_  256 08c912d97b9898c8b3997a19822ea3ea (ED25519)
53/tcp   open  domain  NLnet Labs NSD
80/tcp   open  http    nginx 1.16.1
|_http-server-header: nginx/1.16.1
|_http-title: Home | Mezzanine
8000/tcp open  http    nginx 1.16.1
|_http-server-header: nginx/1.16.1
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Site doesn't have a title (application/json).

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.18 seconds

```

### Searchsploit

#### NLNET Labs NSD

Search for NLnet Labs NSD vulnerabilities -> Nothing interesting

```
searchsploit NSD                                       
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                              |  Path
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
AIX 4.1/4.2 - 'pdnsd' Remote Buffer Overflow                                                                                | aix/remote/21093.c
iMessage - NSKeyedUnarchiver Deserialization Allows file Backed NSData Objects                                              | multiple/dos/47194.txt
Multiple OEM - 'nsd' Remote Stack Format String (PoC)                                                                       | multiple/dos/43998.txt
SGI IRIX 6.5.2 - 'nsd' Information Gathering                                                                                | irix/remote/19316.c
Symantec Enterprise Firewall 7.0/8.0 - DNSD DNS Cache Poisoning                                                             | windows/remote/24218.cpp
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

[CVE-2020-28935](https://nvd.nist.gov/vuln/detail/cve-2020-28935) -> does not seem to be exploitable

#### NGINX

```
searchsploit nginx
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                              |  Path
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Nginx 0.6.36 - Directory Traversal                                                                                          | multiple/remote/12804.txt
Nginx 0.6.38 - Heap Corruption                                                                                              | linux/local/14830.py
Nginx 0.6.x - Arbitrary Code Execution NullByte Injection                                                                   | multiple/webapps/24967.txt
Nginx 0.7.0 < 0.7.61 / 0.6.0 < 0.6.38 / 0.5.0 < 0.5.37 / 0.4.0 < 0.4.14 - Denial of Service (PoC)                           | linux/dos/9901.txt
Nginx 0.7.61 - WebDAV Directory Traversal                                                                                   | multiple/remote/9829.txt
Nginx 0.7.64 - Terminal Escape Sequence in Logs Command Injection                                                           | multiple/remote/33490.txt
Nginx 0.7.65/0.8.39 (dev) - Source Disclosure / Download                                                                    | windows/remote/13822.txt
Nginx 0.8.36 - Source Disclosure / Denial of Service                                                                        | windows/remote/13818.txt
Nginx 1.1.17 - URI Processing SecURIty Bypass                                                                               | multiple/remote/38846.txt
Nginx 1.20.0 - Denial of Service (DOS)                                                                                      | multiple/remote/50973.py
Nginx 1.3.9 < 1.4.0 - Chuncked Encoding Stack Buffer Overflow (Metasploit)                                                  | linux/remote/25775.rb
Nginx 1.3.9 < 1.4.0 - Denial of Service (PoC)                                                                               | linux/dos/25499.py
Nginx 1.3.9/1.4.0 (x86) - Brute Force                                                                                       | linux_x86/remote/26737.pl
Nginx 1.4.0 (Generic Linux x64) - Remote Overflow                                                                           | linux_x86-64/remote/32277.txt
Nginx (Debian Based Distros + Gentoo) - 'logrotate' Local Privilege Escalation                                              | linux/local/40768.sh
PHP-FPM + Nginx - Remote Code Execution                                                                                     | php/webapps/47553.md
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

-> Nothing obvious on 1.16 version

CVE-2021-23017 : Only Denial of service ?

#### Mezzanine

```
searchsploit mezzanine
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                              |  Path
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Mezzanine 4.2.0 - Cross-Site Scripting                                                                                      | python/webapps/40799.txt
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

#### OpenSSH

```
searchsploit openssh
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                              |  Path
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Debian OpenSSH - (Authenticated) Remote SELinux Privilege Escalation                                                        | linux/remote/6094.txt
Dropbear / OpenSSH Server - 'MAX_UNAUTH_CLIENTS' Denial of Service                                                          | multiple/dos/1572.pl
FreeBSD OpenSSH 3.5p1 - Remote Command Execution                                                                            | freebsd/remote/17462.txt
glibc-2.2 / openssh-2.3.0p1 / glibc 2.1.9x - File Read                                                                      | linux/local/258.sh
Novell Netware 6.5 - OpenSSH Remote Stack Overflow                                                                          | novell/dos/14866.txt
OpenSSH 1.2 - '.scp' File Create/Overwrite                                                                                  | linux/remote/20253.sh
OpenSSH 2.3 < 7.7 - Username Enumeration                                                                                    | linux/remote/45233.py
OpenSSH 2.3 < 7.7 - Username Enumeration (PoC)                                                                              | linux/remote/45210.py
OpenSSH 2.x/3.0.1/3.0.2 - Channel Code Off-by-One                                                                           | unix/remote/21314.txt
OpenSSH 2.x/3.x - Kerberos 4 TGT/AFS Token Buffer Overflow                                                                  | linux/remote/21402.txt
OpenSSH 3.x - Challenge-Response Buffer Overflow (1)                                                                        | unix/remote/21578.txt
OpenSSH 3.x - Challenge-Response Buffer Overflow (2)                                                                        | unix/remote/21579.txt
OpenSSH 4.3 p1 - Duplicated Block Remote Denial of Service                                                                  | multiple/dos/2444.sh
OpenSSH < 6.6 SFTP - Command Execution                                                                                      | linux/remote/45001.py
OpenSSH < 6.6 SFTP (x64) - Command Execution                                                                                | linux_x86-64/remote/45000.c
OpenSSH 6.8 < 6.9 - 'PTY' Local Privilege Escalation                                                                        | linux/local/41173.c
OpenSSH 7.2 - Denial of Service                                                                                             | linux/dos/40888.py
OpenSSH 7.2p1 - (Authenticated) xauth Command Injection                                                                     | multiple/remote/39569.py
OpenSSH 7.2p2 - Username Enumeration                                                                                        | linux/remote/40136.py
OpenSSH < 7.4 - agent Protocol Arbitrary Library Loading                                                                    | linux/remote/40963.txt
OpenSSH < 7.4 - 'UsePrivilegeSeparation Disabled' Forwarded Unix Domain Sockets Privilege Escalation                        | linux/local/40962.txt
OpenSSH < 7.7 - User Enumeration (2)                                                                                        | linux/remote/45939.py
OpenSSHd 7.2p2 - Username Enumeration                                                                                       | linux/remote/40113.txt
OpenSSH/PAM 3.6.1p1 - 'gossh.sh' Remote Users Ident                                                                         | linux/remote/26.sh
OpenSSH/PAM 3.6.1p1 - Remote Users Discovery Tool                                                                           | linux/remote/25.c
OpenSSH SCP Client - Write Arbitrary Files                                                                                  | multiple/remote/46516.py
Portable OpenSSH 3.6.1p-PAM/4.1-SuSE - Timing Attack                                                                        | multiple/remote/3303.sh
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

## Web pentesting

### Fuzzing

#### Global website on port 80

```
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://192.168.
110.62/FUZZ/

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://192.168.110.62/FUZZ/
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

# on at least 3 different hosts [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 113ms]
# This work is licensed under the Creative Commons [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 201ms]
#                       [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 225ms]
# Attribution-Share Alike 3.0 License. To view a copy of this [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 288ms]
#                       [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 311ms]
blog                    [Status: 200, Size: 6252, Words: 1054, Lines: 314, Duration: 366ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/ [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 476ms]
# Priority-ordered case-sensitive list, where entries were found [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 552ms]
# or send a letter to Creative Commons, 171 Second Street, [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 656ms]
contact                 [Status: 200, Size: 7922, Words: 1292, Lines: 355, Duration: 798ms]
#                       [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 819ms]
about                   [Status: 200, Size: 6091, Words: 1013, Lines: 267, Duration: 1012ms]
# directory-list-2.3-small.txt [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 1041ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 1169ms]
# Copyright 2007 James Fisher [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 1253ms]
#                       [Status: 200, Size: 6927, Words: 1080, Lines: 274, Duration: 1287ms]
search                  [Status: 200, Size: 6138, Words: 1015, Lines: 278, Duration: 1426ms]
gallery                 [Status: 200, Size: 22187, Words: 2370, Lines: 583, Duration: 1234ms]
static                  [Status: 403, Size: 153, Words: 3, Lines: 8, Duration: 22ms]
admin                   [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 1180ms]
edit                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 1242ms]

```

/edit leads to /admin&#x20;

user="admin" password="admin" does not work

user="admin" password"default" does not work as well

brute-force ?



<figure><img src="../.gitbook/assets/Screenshot from 2025-01-04 11-46-13.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot from 2025-01-04 11-43-49.png" alt=""><figcaption></figcaption></figure>

#### Other website on port 8080 that seems to be an API



