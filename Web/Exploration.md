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
### Forgot Password
See if you can validate email addresses.
> [!Caution]
> Running a high risk factor could negatively impact the server
## Searchsploit
Search for CMS exploits with similar version numbers 
> [!NOTE]
> Compare creation_date/vendor_contact to copyright (creation after copyright)



