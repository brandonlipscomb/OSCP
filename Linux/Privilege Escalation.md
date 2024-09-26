# Important Files
## Passwords
If foothold was through a webserver:
- Check all .php files in /var/www/html/.../<login> for hard coded passwords
```bash
cat * | grep -i passw*
```
- Check all .php files (Manually)
Check if /etc/shadow is readable
```bash
cat /etc/shadow
```
## Users
Display Users:
```bash
cat /etc/passwd
```
Display ID of Current User:
```bash
id
```

# Sudo 
Check Sudo Priviledges of Current User
```bash
sudo -l
```

# SUID (Set Owner User ID)
## Finding Files 
Owned by Root w/ the SUID Bit Set:
```bash
find / -perm -4000 -user root -type f 2>/dev/null
```
```bash
find / -perm -u=s -type f 2>/dev/null
```
## Check Files for Relative Paths 
If there is a binary with a relative path:
1) vi /tmp/<BIN>
```file
/bin/sh
```
2) Execute Binary
3) Clean Up
