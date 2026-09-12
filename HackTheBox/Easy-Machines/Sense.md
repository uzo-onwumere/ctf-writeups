# HTB: Easy Machine — Sense

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Sense | Apache httpd | Sudo misconfig -> writable cron script | ✅ |


---

## Sense 

Date: 2026-09-10
Time in Box: 90 minutes

**Service:** Apache HTTP (Port 80)
**Concept:** The chain is a classic "left-behind dev tool → sudo misconfig → writable cron script" progression: 

### Enumeration

<img width="1875" height="1116" alt="Screenshot 2026-09-12 223058" src="https://github.com/user-attachments/assets/a58f0f5e-b08e-4de0-a377-9331e1eb9722" />


We first start of with our standard nmap scan and we find that both port 80 and port 443 are open.


<img width="1742" height="1303" alt="Screenshot 2026-09-12 224604" src="https://github.com/user-attachments/assets/36084647-fbe4-4623-bd3d-c75195b6d36b" />



Going to the website we see a PFSense login page that is requiring a username and password. Since we don't have that we need to start doing some web
enumeration.



<img width="1257" height="1045" alt="Screenshot 2026-09-12 230013" src="https://github.com/user-attachments/assets/2c5b64ea-af85-44cf-85a9-f468b74eb939" />


Running dirbuster and using a wordlist we find a lot of directories tied to the webpage. I started with the changelog.txt file which told me that two vulnerabilities
were patched and one will be patched at a later time. 



<img width="1271" height="1612" alt="Screenshot 2026-09-12 230216" src="https://github.com/user-attachments/assets/5608320c-64d0-4918-a553-372c8b44c38a" />



I then went through a few more directories that yielded no useful results until I reached the system-users.txt file.
This file provided me with a username and password for the login page. 


I tried the "company defaults" as the password but after a few failures I realized that
company defaults meant the default name of the company which is pfsense. I tried to enter that  and it still didn't work. I then tried lower case for
everything and was able to login. username and password ended up being rohit-pfsense.



<img width="1751" height="1308" alt="Screenshot 2026-09-12 231139" src="https://github.com/user-attachments/assets/0eaaff5c-83ac-4df7-bbff-57026da6f188" />




Now that we are in we can see a version for pfsense and information ranging from DNS servers, CPU usage and a whole host of other information. It is at this point
that I start doing research on the version of pfsense to try and find any vulnerabilities or CVE's that can exploit this version.




<img width="1745" height="1361" alt="Screenshot 2026-09-12 232107" src="https://github.com/user-attachments/assets/bab41564-73cd-40c4-b099-2eac04e65a71" />


After some searching we find that CVE-2016-10709 is a vulnerability that effects psfsense and can lead to RCE. They even give us the metasploit exploit as well.
Now we can start trying to exploit the machine.





### Exploitation



<img width="1517" height="1067" alt="Screenshot 2026-09-12 232432" src="https://github.com/user-attachments/assets/c37b456f-23a7-41a7-95be-a5030d706e7d" />



Now that we have the metasploit exploit we just have to input the username, password, Rhost, and Lhost and success! We establish a meterpreter shell
on the machine.





### Flag
**user flag a3c7d37662cb75b58fa7ae1220efe4f0**

**root flag 9781a0edf1c5ee3cd146448c8e11b725**





### Key Takeaway
Don't ship debug/dev tooling to reachable directories.
---
