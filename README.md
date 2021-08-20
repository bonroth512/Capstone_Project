# Capstone_Project

The following write-up will articulate the Red Team's engagement on a vulnerable VM and Blue Team's analysis of the attack. 

***

![RedvsBlueDiagram](https://github.com/bonroth512/Capstone_Project/blob/main/Images/Red%20vs%20Blue%20Network%20Diagram.png)

***

### **Red Team**

Started by enumerating the network with NMAP.  

![nmap_results](https://github.com/bonroth512/Capstone_Project/blob/main/Images/nmap_results.PNG)

Ran a dirb command to determine any additional URL's on the website.

![dirb_returns](https://github.com/bonroth512/Capstone_Project/blob/main/Images/dirb_returns.PNG)

After identifying the target's IP and additional URL's, viewed the web server in the browser to determine more about the target.
Navigating through the directories revealed a note regarding a confidential folder.

![secret_folder](https://github.com/bonroth512/Capstone_Project/blob/main/Images/mention-of-secretfolder.PNG)

This folder was password protected and proceeded to perform a brute force attack against the web application.  Used the hydra command to run the attack.

![hydra](https://github.com/bonroth512/Capstone_Project/blob/main/Images/hydra_crack_better.PNG)

With access to Ashton credentials, was able to access the secret_folder.  

![webdav_connections](https://github.com/bonroth512/Capstone_Project/blob/main/Images/secret_folder_info.PNG)

Following the instructions, gained access to the webdav folder. Then to unhash the hash provided for a user's account and then to authenticate as Ryan. 

![cracked_hash](https://github.com/bonroth512/Capstone_Project/blob/main/Images/cracked_hash.PNG)
![webdav_authen.](https://github.com/bonroth512/Capstone_Project/blob/main/Images/webdav_authentication.PNG)

With access to the webdav folder, placed an executables within it. Msfvenom was used to create a payload to establish a reverse tcp shell.  This will be uploaded to the webdav folder to call out and evade network securitiy controls.    

![msfvenom](https://github.com/bonroth512/Capstone_Project/blob/main/Images/msfvenom_reverse-shell.PNG)

Placing the executable in the web server and clicking on it initates the exploit.  

![php_embed](https://github.com/bonroth512/Capstone_Project/blob/main/Images/php_embedded_in_webdav.PNG)

In Metasploit, configured the settings to run the exploit: php/meterpreter/reverse_tcp.  This exploit is set to listen for the target's broadcast on port 4444.  Once the php file is executed on the target's machine and metasploit runs the exploit, the meterpreter session will be connected and remote access is established.     

![meterpreter](https://github.com/bonroth512/Capstone_Project/blob/main/Images/meterpreter_session.PNG)

Gaining remote access capabilities then allows for the post-exploitation phase to be done on the target.   

***

### **Blue Team**

**Log Analysis and Attack Characterization**

Analysis: Identifying the Port Scan
- The attack occurred between 16:33-16:35 (the first 16 are shown).
- 11,026 packets were sent, all from 192.168.1.90 to 192.168.1.105.
- Tons of ports being hit in a inhuman amount of time indicuates that this is a port scan.  
![port_scan_splunk](https://github.com/bonroth512/Capstone_Project/blob/main/Images/Port_scan_splunk.PNG)

Analysis: Finding the Request for the Hidden Directory
-The request started at 19:48 and ended at 20:04 and there was a total of 16,857 requests.
-The url.path of /company_folders/secret_folder/ was listed.  
![secret_folder_splunk](https://github.com/bonroth512/Capstone_Project/blob/main/Images/secret_folder_splunk.PNG)

Analysis: Uncovering the Brute Force Attack
-64,482 requests were made during the attack.
![hydra_splunk](https://github.com/bonroth512Capstone_Project/blob/main/Images/hydra_splunk.PNG)

Analysis: Finding the WebDAV Connection
-100 requests were made to this directory.
-The URL.path is listed as /webdav/ indictating the attacker was successful with the brute force and was able to access the folder.  
![webdav_splunk](https://github.com/bonroth512/Capstone_Project/blob/main/Images/webdav_splunk.PNG)

***

### **Blue Team Mitigation Strategies**

Vulnerabilities to Mitigate:
- Blocking a Port Scan
- Unauthorized Users Accessing Confidential Directories / Shared Folders
- Preventing Brute Force Attack
- Preventing Reverse Shell Uploads
 
Blocking a Port Scan

An unwarranted port scan across your network signifies intent from external actors who are looking for vulnerabilities.  Steps can be taken to limit the amount of information gained from one of these active reconaissance scans.  Maintaining a baseline of which services and ports are running and open will help keep an defensive posture.

| Mitigation | Description |
|------------|-------------|
| Disable or Remove Feature or Program (MITRE ID:M1042) | Close unnecessary ports and services to prevent risk of discover and potential exploitation.|
| Network Intrusion Prevention (MITRE ID: M1031) | Use network IDS/IPS to detect and prevent remote service scans. |
| Network Segmentation (MITRE ID: M1030 / D3-NI) | Ensure proper network segmentation to protect critical servers and applications. |
| Whitelist IP addresses | Authorize select IP addresses to be able to perform scans. |
-Some mitigation recommendations are reflective of the MITRE mitigiation framework as well as the MITRE D3FEND Matrix.  

Unauthorized Users Accessing Confidential Directories / Shared Folders

Access to confidential directories and shared folders can lead to data breaches and malicious file injections.  It is imperative to allow only thoses who have authorized access and continue to monitor activity around critical data.  Analyzing network traffic based on a baseline from normal operational levels can provide the template to identify unusual behavior.  

| Mitigation | Description |
|------------|-------------|
| Whitelist allowed IP addresses | Only allow authorized users from select IP addresses. | 
| Account Use Policies (MITRE ID:M1036 / D3-AL) | Set account policies to prevent numerous unauthorized attempts. |
| Multi-Factor Authentication (MITRE ID: M1036 / D3-MFA) | Use MFA to authenticate authorize users. |
| Restrict File & Directory Permissions (MITRE ID: M10222) | Restrict read/write access. |
| Administrative Network Activity Analysis (D3-ANAA) | Detection of unauthorized use of admin protocols against a baseline. |
| Filter Network Traffic (D3-ITF) | Use network appliances to filter ingress / egress traffic. |

Prevention of Brute Force Attacks

To decrease the effectiveness of brute force attacks, an approach from the user's and the architecture of the network can be taken.

| Mitigation | Description |
|------------|-------------|
| Account Lockout Policies (D3-AL) | Considering limit the total number of attempts before an account is locked. |
| Strong Password Policy (D3-SPP) | Implementing strong (and complex) passwords for all users. |
| Rate Limiter | Placing a rate limiter, on both the username and IP, before the web server can limit the number of possible attempts per hour. | 
| Multi-Factor Authentication (MITRE ID: M1036 / D3-MFA) | Use MFA to authenticate authorize users. |

Prevention of Reverse Shell Uploads

| Mitigation | Description |
|------------|-------------|
| Whitelist Allowed Users (D3-EAL) | Only whitelist allowed authorized users. |
| Remote Terminal Session Detection (D3-RTSD) | Detection of session datatsets for signs of remote access. |
| Restrict executable from Temp Directories | Limited filetypes and the location of uploading files can reduce the attack surface. |
| Implementing behavioral-based Analysis to Anti-Virus Algorithms | Improving the antivirus to look beyond the signature scans alone. |
| Inbound Traffic Filter (D3-ITF) | Implement filtering policies to determine nature of suspicious network traffic. |

![defend_matrix](https://github.com/bonroth512/Capstone_Project/blob/main/Images/defend_matrix.PNG)





