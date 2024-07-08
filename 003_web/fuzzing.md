# Fuzzing

Directory fuzzing :&#x20;

```bash
ffuf -w /opt/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://192.168.207.120/FUZZ
```
