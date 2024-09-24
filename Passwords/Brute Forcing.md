# Hydra
A very fast network logon cracker which support many different services
## Try One Password Against Multiple Accounts
SSH:
```bash
hydra -L usernames.txt -p '<PASSWORD>' <IP> ssh
```
# Crackmapexc (CME)
## Try One Password Against Multiple Accounts
SSH:
```bash
cme SSH <IP> -u <USERS.txt> -p '<PASSWORD>'
```
>[!NOTE]
> Brute-forcing SMB/FTP will be much faster and less noisy then brute-forcing SSH
