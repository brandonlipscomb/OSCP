# Web Enumeration
## Nikto
Scan Web Server for Known Vulnerabilities
```bash
nikto -host http://DOMAIN/
```
## Gobuster
### Directory Scan
#### Initial Scan
```bash
gobuster dir --url http://IP/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -t 50
```
#### Detailed Scan
```bash
gobuster dir --url http://IP/ --wordlist=/usr/share/wordlists/dirbuster/directory-list-2.3-medium-reversed.txt -t 50
```
### File Scan
```bash
gobuster dir --url http://IP/ --wordlist=/usr/share/wordlists/dirbuster/directory-list-2.3-medium-reversed.txt -x cgi,py,pl,php,sh,txt,html -t 50
```
### Sub-Domain Scan (VHOST)
```bash
gobuster vhost -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://DOMAIN --append-domain
```
>[!NOTE]
>Any subdomains that are found will need to be added to /etc/hosts before they can be reached
