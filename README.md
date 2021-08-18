# Capstone_Project

The following write-up will articulate the Red Team's engagement on a vulnerable VM and Blue Team's response to this attack. 

***

![RedvsBlueDiagram](https://github.com/bonroth512/Capstone_Project/blob/main/Images/Red%20vs%20Blue%20Network%20Diagram.png)

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

Unauthorized Users Accessing Confidential Directories

Access to confidential files and directories can lead to data breaches and possible loss of integrity.  It is imperative to allow only thoses who have authorized access and continue to monitor activity around critical data.  

| Mitigation | Description |
|------------|-------------|


