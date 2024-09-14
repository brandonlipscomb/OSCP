# Prerequisites
Ensure nmap is installed on your system
## Installation of Nmap
**Ubuntu/Debain**
```bash
sudo apt install nmap
```
**CentOS/Fedora
```bash
sudo yum install nmap
```
# Installation 
1. Clone the repository or download the script file
2. Make the script executable:
```bash
chmod +x nmap_auto
```
# Usage 
Run the script with sudo priviledges
```bash
sudo ./nmap_auto [-A] <IP>
```
- \-A: Runs the Initial scan with a high packet rate

# Explaination
1. **Initial Scan**: The script performs an initial nmap scan on all 65,535 ports using a TCP SYN scan
Aggressive Mode (With -A)
```bash
nmap -p- --min-rate=5000 -oN nmap/initial "$IP" -r -v
```
- \-p-: Scan all ports
- \--min-rate=5000: Sets the minimum packet transmission rate to 5000 packets/second.
- \-r: Scans ports sequentially
- \v: Enables Verbose Mode
- \-oN: Saves the output to a specified file
Default Mode (Without -A)
```bash
sudo nmap -sS -p- -oN nmap/initial "$IP" -r -v -T 4
```
- \-sS: TCP SYN "Half-Open" Scan
- \-p-: Scan all ports
- \-oN: Saves the output to a specified file
- \-r: Scans ports sequentially
- \v: Enables Verbose Mode
- \T 4: Sets the timing to level 4 (aggressive)

3. **Parse Open Ports**: The script parses the inital scan results to find open ports
```bash
OPEN_PORTS=$(grep '^[0-9]\+/tcp[ ]\+open' nmap/initial | cut -d '/' -f 1 | tr '\n' ',' | sed 's/,$//')
```
- grep '^[0-9]\+/tcp[ ]\+open' nmap/initial
  - ^: Anchors the match to the beginning of the line
  - [0-9]\+: Matches one or more digits
  - /tcp: Matches the string "/tcp"
  - [ ]\+: Matches one or more spaces
  - open: Matches the string "open"
- cut -d '/' -f 1
  - \-d '/': Sets the delimiter to '/' and breaks each line into fields
  - -f 1: Extracts the first field from each line
- tr '\n' ',': Replaces all newlines (\n) with commas (,)
- sed 's/,$//': Deletes the instance of a comma followed by the end of line
5. **Detailed Scan**: If open ports are found, a more detailed nmap scan is conducted on those ports, using service detection (-sC), version detection (-sV), and OS detection (-O)
```bash
sudo nmap -sC -sV -p$OPEN_PORTS -oN nmap/detailed "$IP" -r -v -T 4
```
- \-sC: Runs default nmap scripts
- \-sV: Detects service versions
- \-oN: Saves the output to a specified file
- \-r: Scans ports sequentially
- \v: Enables Verbose Mode
- \T 4: Sets the timing to level 4 (aggressive)
