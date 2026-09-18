# HTB: Easy Machine — Devel

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Devel | FTP Anonymous | Sudo misconfig -> writable cron script | ✅ |


---

## Devel 

Date: 2026-09-13
Time in Box: 90 minutes

**Service:** FTP Anonymous (Port 21)
**Concept:** Manual windows kernel privesc — foothold gives a low-priv IIS account, systeminfo identifies an unpatched kernel, transfer-and-run the matching exploit to reach SYSTEM 

### Enumeration

<img width="809" height="588" alt="nmap scan" src="https://github.com/user-attachments/assets/02da2885-e56e-498b-a1dd-f7381d2b134f" />

I ran a standard nmap command and found that two ports were open. Port 21 and port 80. I start by going to the site and realize there is nothing there I can interact with. I notice however that the site is running aspx so I start a msfvenom command to create a file that hosts a reverse shell exploit.

<img width="814" height="585" alt="msfvenom exploit" src="https://github.com/user-attachments/assets/854a8f11-4ac3-4489-8ef9-265b41f76678" />



<img width="805" height="585" alt="ftp put request" src="https://github.com/user-attachments/assets/c70354d3-aaaa-4510-9b5f-1d34e2e2e05b" />


With the exploit created I try the FTP port since Anonymous login is allowed. I am able to get into the FTP server and I upload the exploit using the put command.




<img width="843" height="748" alt="metasploit handler" src="https://github.com/user-attachments/assets/23408213-05f8-4f6b-ad2f-3e855f13e85b" />


With the exploit uploaded I start a msfconsole session and start a listerner on metasploit.


### Exploitation






### Flag
**user flag obtained ✅**

**root flag obtained ✅**





### Key Takeaway
An unpatched Windows box lets you escalate from a low-privilege service account to SYSTEM by identifying the missing kernel patch from systeminfo and running the matching kernel exploit — the whole privesc is enumerate-the-patch-level, then transfer-and-run.
---
