# Services management on Linux

### List all opened ports on the server

```
ss -lptun
Netid     State      Recv-Q      Send-Q                                Local Address:Port            Peer Address:Port     Process                            
udp       UNCONN     0           0                                           0.0.0.0:5353                 0.0.0.0:*                                           
udp       UNCONN     0           0                                           0.0.0.0:44142                0.0.0.0:*                                           
udp       UNCONN     0           0                                              [::]:5353                    [::]:*                                           
udp       UNCONN     0           0                                              [::]:39214                   [::]:*                                           
udp       UNCONN     0           0                [fe80::18b1:98d2:c493:5d72]%wlp2s0:546                     [::]:*                                           
tcp       LISTEN     0           128                                       127.0.0.1:631                  0.0.0.0:*                                           
tcp       LISTEN     0           1                                           0.0.0.0:1234                 0.0.0.0:*         users:(("nc",pid=76000,fd=3))     
tcp       LISTEN     0           128                                           [::1]:631                     [::]:*  
```

Following values list the process, pid and file descriptor associated to listening ip:port

```
users:(("nc",pid=76000,fd=3))
```

OR

```
lsof -Pi
COMMAND     PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
firefox-e  3039 yanis  115u  IPv6 429774      0t0  TCP work:36982->g2a02-26f0-9100-0003-0000-0000-6010-7a2e.deploy.static.akamaitechnologies.com:443 (ESTABLISHED)
firefox-e  3039 yanis  136u  IPv4 414345      0t0  TCP work:38398->93.243.107.34.bc.googleusercontent.com:443 (ESTABLISHED)
firefox-e  3039 yanis  152u  IPv6 429752      0t0  TCP work:38784->[2606:4700:4400::ac40:92a7]:443 (ESTABLISHED)
firefox-e  3039 yanis  169u  IPv6 431157      0t0  TCP work:40088->[2606:4700:4400::6812:2959]:443 (ESTABLISHED)
firefox-e  3039 yanis  172u  IPv4 431161      0t0  TCP work:55272->a1370dc23e25e46ce.awsglobalaccelerator.com:443 (ESTABLISHED)
firefox-e  3039 yanis  177u  IPv6 431159      0t0  TCP work:39066->[2606:4700:4400::ac40:92a7]:443 (ESTABLISHED)
firefox-e  3039 yanis  180u  IPv4 430229      0t0  TCP work:45686->104.18.41.89:443 (ESTABLISHED)
firefox-e  3039 yanis  193u  IPv4 372370      0t0  TCP work:55532->mad01s26-in-f170.1e100.net:443 (ESTABLISHED)
firefox-e  3039 yanis  194u  IPv6 414476      0t0  TCP work:42526->[2606:4700:4400::ac40:92a7]:443 (ESTABLISHED)
firefox-e  3039 yanis  198u  IPv4 425692      0t0  TCP work:60118->104.18.41.89:443 (ESTABLISHED)
firefox-e  3039 yanis  200u  IPv6 414477      0t0  TCP work:42538->[2606:4700:4400::ac40:92a7]:443 (ESTABLISHED)
firefox-e  3039 yanis  213u  IPv4 399277      0t0  TCP work:38686->104.18.41.89:443 (ESTABLISHED)
firefox-e  3039 yanis  222u  IPv4 410606      0t0  TCP work:33218->server-18-161-111-124.mrs52.r.cloudfront.net:443 (ESTABLISHED)
gvfsd-smb 38693 yanis   10u  IPv4 172621      0t0  TCP work:33284->_gateway:139 (ESTABLISHED)
nc        76000 yanis    3u  IPv4 434788      0t0  TCP *:1234 (LISTEN)

```

### Filter on a specific process

```
lsof -P 76000
```

### Filter on a specific port

```
lsof -i 1234
```



