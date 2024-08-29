# Network Enumeration
## Nmap
### Initial Scan
```bash
$ sudo nmap -p- -sS -T4 <IP> -v -r
```
### Detailed Scan
```bash
$ nmap -p<PORTS> -sV -sC -T4 <IP> -v -r 
```
#### Ex
```bash
$ nmap -p22,80,110,139,143,445 -sV -sC -T4 10.10.223.234 -v -r 
```

