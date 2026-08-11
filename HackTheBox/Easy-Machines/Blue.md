# HTB: Easy Machine — Blue

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Legacy | SMB (Server Message Block) | unpatched services with known CVEs | ✅ |


---

## Blue 

Date: 2026-08-11
Time in Box: 25 minutes

**Service:** SMB (Port 139/445)
**Concept:** Exploiting unpatched services with known CVEs 

### Enumeration


<img width="1318" height="966" alt="nmap enumeration 1" src="https://github.com/user-attachments/assets/5d24997a-2881-4f00-9061-ab548bdc3927" />


I used a standard nmap command and flags to enumerate the box and found that the machine had open ports for 139/445. Further breakdown of the nmap returns shows that
the machine is running windows 7.

<img width="1246" height="846" alt="nmap enumeration 2" src="https://github.com/user-attachments/assets/b58c0e2f-8c37-41a3-9c8e-7f3fd3a33421" />


I then conducted another nmap scan but this time using the script=smb-vuln* flags to find out that this machine is vulnerable to CVE-2008-4250 and CVE-2017-0143.
It also showed me that this vulnerability leads to RCE (remote code execution) and even gave me the ms17-010 which is the infamous EternalBlue.


<img width="681" height="247" alt="smb shares" src="https://github.com/user-attachments/assets/76295435-ecb6-46df-858e-eebb3fd5cc90" />

I also ran the smbclient command because one of the questions asked how many SMB shares there were. Didn't end up doing anything with it but good syntax practice.


Armed with this information I was now able to fire up metasploit for the exploitation phase.



### Exploitation

<img width="2530" height="1213" alt="msfconsole" src="https://github.com/user-attachments/assets/77a3044e-4db8-44e3-a515-c4d44ec1e0a6" />


I ran a search for ms17-010 which was found during the enumeration phase. After looking at the results I ran the command "use 0" to use the first
exploit on the list.


<img width="2513" height="1052" alt="msfconsole 1" src="https://github.com/user-attachments/assets/34885053-c9b2-4f34-be00-1890b92507b5" />




<img width="2517" height="1150" alt="msfconsole 2" src="https://github.com/user-attachments/assets/9f21bef4-22e3-4f50-838f-4f5f6a3c9320" />



From here I run "show options" and populate the RHOSTS, and LHOST with the target IP address and my attacker IP address respectively and run the exploit.




<img width="2522" height="1003" alt="shell" src="https://github.com/user-attachments/assets/00658ac1-4cb7-4b20-924e-231ad177844a" />


We have success! A meterpreter session is opened and a C:\WINDOWS\system32 prompt is available. Now we can search for the user and root flags.



### Flag
**user flag e00f3b24a379139c65594d4500c5d028**

**root flag b888f15a2d3e60c104a1f49b3f719e76**


<img width="2522" height="1590" alt="user flag 1" src="https://github.com/user-attachments/assets/4b235b6c-5304-40aa-97e4-4847b2e12e8a" />




<img width="2522" height="250" alt="user flag 2" src="https://github.com/user-attachments/assets/35343781-bf1c-4484-8d1c-3c7f1c3dcfa6" />



After searching the directories I find the user flag on the user Haris's desktop.



<img width="2522" height="1495" alt="root flag" src="https://github.com/user-attachments/assets/995883ff-ad01-4844-bbbb-bc5077e392e2" />



<img width="2522" height="245" alt="root flag 2" src="https://github.com/user-attachments/assets/bdeeb649-1c3f-4173-b17a-6e0da9eee0b9" />



After a little more searching in the directories the root flag was found in the Admin directory on the desktop.



### Key Takeaway
Modern operating systems don't automatically mean patched operating systems.
---
