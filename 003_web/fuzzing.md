# Fuzzing

Directory fuzzing :&#x20;

```bash
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://192.168.207.120/FUZZ
```

Extension fuzzing :&#x20;

```
ffuf -w /opt/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ
```

Page fuzzing :&#x20;

```
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php

```
