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

# Other
## Sudo 
Check Sudo Priviledges of Current User
```bash
sudo -l
```
