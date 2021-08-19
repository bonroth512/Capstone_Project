# Capstone_Project

The following write-up will articulate the Red Team's engagement on a vulnerable VM and Blue Team's response to this attack. 

***

![RedvsBlueDiagram](https://github.com/bonroth512/Capstone_Project/blob/main/Images/Red%20vs%20Blue%20Network%20Diagram.png)

***

**Red Team**

2.Scanning

![nmap_results](https://github.com/bonroth512/Capstone_Project/blob/main/Images/nmap_results.PNG)

![dirb_returns](https://github.com/bonroth512/Capstone_Project/blob/main/Images/dirb_returns.PNG)

![secret_folder](https://github.com/bonroth512/Capstone_Project/blob/main/Images/mention-of-secretfolder.PNG)

3.Exploitation
4.Post Exploitation
5.Reporting



***

**Blue Team Mitigation Strategies**

Vulnerabilities to Mitigate:
- Blocking a Port Scan
- Unauthorized Users Accessing Confidential Directories
- Preventing Brute Force Attack
- Preventing Reverse Shell Uploads
 
Blocking a Port Scan

An unwarranted port scan across your network signifies intent from external actors that are looking for vulnerabilities.  Steps can be taken to limit the amount of information gained from one of these active reconaissance scans.  Maintaining a baseline of which services and ports are running and open will help keep an defensive posture.

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





