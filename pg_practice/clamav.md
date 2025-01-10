# clamav

```
exegol start pg_clamav nightly --vpn /home/yanis/Downloads/universal.ovpn 
```

## Setup

```
firefox &> /dev/null & 
```

```
tmux capture-pane -pS - > tmux_history.log
```

## Scanning

### Nmap

```
nmap -sC -sV -oA nmap  192.168.110.42  --stats-every=5s
```

```
[Jan 04, 2025 - 10:21:10 (CET)] exegol-pg_clamav /workspace # nmap -sC -sV -oA nmap  192.168.110.42  --stats-every=5s
Starting Nmap 7.93 ( https://nmap.org ) at 2025-01-04 10:21 CET
Stats: 0:00:06 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 33.33% done; ETC: 10:21 (0:00:12 remaining)
Stats: 0:00:11 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 83.33% done; ETC: 10:21 (0:00:02 remaining)
Nmap scan report for 192.168.110.42
Host is up (0.015s latency).
Not shown: 994 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 3.8.1p1 Debian 8.sarge.6 (protocol 2.0)
| ssh-hostkey:
|   1024 303ea4135f9a32c08e46eb26b35eee6d (DSA)
|_  1024 afa2493ed8f226124aa0b5ee6276b018 (RSA)
25/tcp  open  smtp        Sendmail 8.13.4/8.13.4/Debian-3sarge3
| smtp-commands: localhost.localdomain Hello [192.168.45.160], pleased to meet you, ENHANCEDSTATUSCODES, PIPELINING, EXPN, VERB, 8BITMIME, SIZE, DSN, ETRN, DE
LIVERBY, HELP
|_ 2.0.0 This is sendmail version 8.13.4 2.0.0 Topics: 2.0.0 HELO EHLO MAIL RCPT DATA 2.0.0 RSET NOOP QUIT HELP VRFY 2.0.0 EXPN VERB ETRN DSN AUTH 2.0.0 START
TLS 2.0.0 For more info use "HELP <topic>". 2.0.0 To report bugs in the implementation send email to 2.0.0 sendmail-bugs@sendmail.org. 2.0.0 For local informa
tion send email to Postmaster at your site. 2.0.0 End of HELP info
80/tcp  open  http        Apache httpd 1.3.33 ((Debian GNU/Linux))
|_http-server-header: Apache/1.3.33 (Debian GNU/Linux)
|_http-title: Ph33r
| http-methods:
|_  Potentially risky methods: TRACE
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
199/tcp open  smux        Linux SNMP multiplexer
445/tcp open  netbios-ssn Samba smbd 3.0.14a-Debian (workgroup: WORKGROUP)
Service Info: Host: localhost.localdomain; OSs: Linux, Unix; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)
| smb-os-discovery:
|   OS: Unix (Samba 3.0.14a-Debian)
|   NetBIOS computer name:
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-01-04T09:21:27-05:00
|_clock-skew: mean: 7h29m58s, deviation: 3h32m07s, median: 4h59m58s
|_nbstat: NetBIOS name: 0XBABE, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
| smb-security-mode:
|   account_used: guest
|   authentication_level: share (dangerous)
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.61 seconds
```



### Browse website

01101001 01100110 01111001 01101111 01110101 01100100 01101111 01101110 01110100 01110000 01110111 01101110 01101101 01100101 01110101 01110010 01100001 01101110 00110000 0011 0000 01100010

ifyoudontpwnmeuran00b



## Footprinting

### SMB

```
[Jan 04, 2025 - 10:31:55 (CET)] exegol-pg_clamav /workspace # netexec smb 192.168.110.42 -u guest -p "" --shares
SMB         192.168.110.42  445    NONE             [*] Unix (name:) (domain:) (signing:False) (SMBv1:True)
SMB         192.168.110.42  445    NONE             [+] \guest: (Guest)
SMB         192.168.110.42  445    NONE             [*] Enumerated shares
SMB         192.168.110.42  445    NONE             Share           Permissions     Remark
SMB         192.168.110.42  445    NONE             -----           -----------     ------
SMB         192.168.110.42  445    NONE             print$                          Printer Drivers
SMB         192.168.110.42  445    NONE             IPC$                            IPC Service (0xbabe server (Samba 3.0.14a-Debian) brave pig)
SMB         192.168.110.42  445    NONE             ADMIN$                          IPC Service (0xbabe server (Samba 3.0.14a-Debian) brave pig)
[Jan 04, 2025 - 10:33:26 (CET)] exegol-pg_clamav /workspace # smbclient --no-pass --user 'guest' "//192.168.110.42/ADMIN"
protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED
[Jan 04, 2025 - 10:33:48 (CET)] exegol-pg_clamav /workspace # smbclient --no-pass --user 'guest' "//192.168.110.42/ADMIN$"
protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED
[Jan 04, 2025 - 10:33:55 (CET)] exegol-pg_clamav /workspace # smbclient --no-pass --user 'guest' "//192.168.110.42/"
[Jan 04, 2025 - 10:33:59 (CET)] exegol-pg_clamav /workspace # smbclient --no-pass --user 'guest' "//192.168.110.42"

\\192.168.110.42: Not enough '\' characters in service
Usage: smbclient [-?EgqBNPkV] [-?|--help] [--usage] [-M|--message=HOST] [-I|--ip-address=IP] [-E|--stderr] [-L|--list=HOST] [-T|--tar=<c|x>IXFvgbNan]
        [-D|--directory=DIR] [-c|--command=STRING] [-b|--send-buffer=BYTES] [-t|--timeout=SECONDS] [-p|--port=PORT] [-g|--grepable] [-q|--quiet]
        [-B|--browse] [-d|--debuglevel=DEBUGLEVEL] [--debug-stdout] [-s|--configfile=CONFIGFILE] [--option=name=value] [-l|--log-basename=LOGFILEBASE]
        [--leak-report] [--leak-report-full] [-R|--name-resolve=NAME-RESOLVE-ORDER] [-O|--socket-options=SOCKETOPTIONS] [-m|--max-protocol=MAXPROTOCOL]
        [-n|--netbiosname=NETBIOSNAME] [--netbios-scope=SCOPE] [-W|--workgroup=WORKGROUP] [--realm=REALM] [-U|--user=[DOMAIN/]USERNAME[%PASSWORD]]
        [-N|--no-pass] [--password=STRING] [--pw-nt-hash] [-A|--authentication-file=FILE] [-P|--machine-pass] [--simple-bind-dn=DN]
        [--use-kerberos=desired|required|off] [--use-krb5-ccache=CCACHE] [--use-winbind-ccache] [--client-protection=sign|encrypt|off] [-k|--kerberos]
        [-V|--version] [OPTIONS] service <password>
[Jan 04, 2025 - 10:34:03 (CET)] exegol-pg_clamav /workspace # smbclient --no-pass --user 'guest' "//192.168.110.42/"
[Jan 04, 2025 - 10:34:10 (CET)] exegol-pg_clamav /workspace # smbmap -u guest -H "192.168.110.42"

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.6 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB connections(s) and 0 authenticated session(s)
[*] Closed 1 connections
[Jan 04, 2025 - 10:34:47 (CET)] exegol-pg_clamav /workspace # smbmap -u guest -H "192.168.110.42" -r ADMIN

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.6 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB connections(s) and 0 authenticated session(s)
[*] Closed 1 connections
[Jan 04, 2025 - 10:35:07 (CET)] exegol-pg_clamav /workspace # smbmap -u guest -H "192.168.110.42" -r ADMIN$

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.6 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB connections(s) and 0 authenticated session(s)
[*] Closed 1 connections
[Jan 04, 2025 - 10:35:10 (CET)] exegol-pg_clamav /workspace # netexec smb 192.168.110.42 -u guest -p "" --shares
SMB         192.168.110.42  445    NONE             [*] Unix (name:) (domain:) (signing:False) (SMBv1:True)
SMB         192.168.110.42  445    NONE             [+] \guest: (Guest)
SMB         192.168.110.42  445    NONE             [*] Enumerated shares
SMB         192.168.110.42  445    NONE             Share           Permissions     Remark
SMB         192.168.110.42  445    NONE             -----           -----------     ------
SMB         192.168.110.42  445    NONE             print$                          Printer Drivers
SMB         192.168.110.42  445    NONE             IPC$                            IPC Service (0xbabe server (Samba 3.0.14a-Debian) brave pig)
SMB         192.168.110.42  445    NONE             ADMIN$                          IPC Service (0xbabe server (Samba 3.0.14a-Debian) brave pig)
[Jan 04, 2025 - 10:35:16 (CET)] exegol-pg_clamav /workspace # netexec smb 192.168.110.42 -u guest -p "" --users
SMB         192.168.110.42  445    NONE             [*] Unix (name:) (domain:) (signing:False) (SMBv1:True)
SMB         192.168.110.42  445    NONE             [+] \guest: (Guest)
SMB         192.168.110.42  445    NONE             -Username-                    -Last PW Set-       -BadPW- -Description-
SMB         192.168.110.42  445    NONE             -Username-                    -Last PW Set-       -BadPW- -Description-
```

### SMTP

#### telnet

```
[Jan 04, 2025 - 10:46:41 (CET)] exegol-pg_clamav /workspace # telnet 192.168.110.42 25
Trying 192.168.110.42...
Connected to 192.168.110.42.
Escape character is '^]'.
220 localhost.localdomain ESMTP Sendmail 8.13.4/8.13.4/Debian-3sarge3; Sat, 4 Jan 2025 09:49:19 -0500; (No UCE/UBE) logging access from: [192.168.45.160](FAIL
)-[192.168.45.160]
VRFY root
503 5.0.0 I demand that you introduce yourself first
VRFY Ph33r
503 5.0.0 I demand that you introduce yourself first
USER gust
500 5.5.1 Command unrecognized: "USER gust"
USER guest
500 5.5.1 Command unrecognized: "USER guest"
USER Ph33r
500 5.5.1 Command unrecognized: "USER Ph33r"
USER root
500 5.5.1 Command unrecognized: "USER root"
^C^E*
```

SMTP userenum

```
[Jan 04, 2025 - 10:57:24 (CET)] exegol-pg_clamav /workspace # smtp-user-enum -U /opt/seclists/Usernames/top-usernames-shortlist.txt 192.168.110.42 25
Connecting to 192.168.110.42 25 ...
220 localhost.localdomain ESMTP Sendmail 8.13.4/8.13.4/Debian-3sarge3; Sat, 4 Jan 2025 09:57:25 -0500; (No UCE/UBE) logging access from: [192.168.45.160](FAIL
)-[192.168.45.160]
250 localhost.localdomain Hello [192.168.45.160], pleased to meet you
Start enumerating users with VRFY mode ...
[SUCC] root          250 2.1.5 <root@localhost.localdomain>
[----] admin         550 5.1.1 admin... User unknown
[----] test          550 5.1.1 test... User unknown
[----] guest         550 5.1.1 guest... User unknown
[----] info          550 5.1.1 info... User unknown
[----] adm           550 5.1.1 adm... User unknown
[----] mysql         550 5.1.1 mysql... User unknown
[----] user          550 5.1.1 user... User unknown
[----] administrator 550 5.1.1 administrator... User unknown
[----] oracle        550 5.1.1 oracle... User unknown
[SUCC] ftp           250 2.1.5 <ftp@localhost.localdomain>
[----] pi            550 5.1.1 pi... User unknown
[----] puppet        550 5.1.1 puppet... User unknown
[----] ansible       550 5.1.1 ansible... User unknown
[----] ec2-user      550 5.1.1 ec2-user... User unknown
[----] vagrant       550 5.1.1 vagrant... User unknown
[----] azureuser     550 5.1.1 azureuser... User unknown

```











