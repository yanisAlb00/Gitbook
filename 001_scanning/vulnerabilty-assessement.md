# Vulnerabilty Assessement

NSE Scripts :

HTTPS :

```bash
sudo nmap -sV -p 443 --script "vuln" 192.168.50.124 --stats-every=5s
```

HTTP :&#x20;

```bash
sudo nmap -sV -p 80 --script "vuln" 192.168.50.124 --stats-every=5s
```
