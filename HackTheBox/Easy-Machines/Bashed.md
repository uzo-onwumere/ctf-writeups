# HTB: Easy Machine — Bashed

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Bashed | Apache httpd | Sudo misconfig -> writable cron script | ✅ |


---

## Bashed 

Date: 2026-09-10
Time in Box: 90 minutes

**Service:** Apache HTTP (Port 80)
**Concept:** The chain is a classic "left-behind dev tool → sudo misconfig → writable cron script" progression: 

### Enumeration


<img width="1001" height="735" alt="Screenshot 2026-09-11 002107" src="https://github.com/user-attachments/assets/be29a916-1ee6-49fc-8110-618f4bb11212" />

We start by running an nmap scan to see what ports and services are running. We get back a result that shows only one port (port 80) and one service (apache httpd) are running. So we instantly know this is going to be dealing with a web server vulnerability/exploit.



<img width="1747" height="1297" alt="Screenshot 2026-09-11 002644" src="https://github.com/user-attachments/assets/d339f5f7-d567-4267-b8b4-cd2b07aaf491" />

Going to the website we see an explanation for phpbash but none of the links on the left side are working. Viewing the source page doesn't reveal anything juicy so it's time to enumerate some more and see if we can find some directories to uncover.



<img width="1440" height="697" alt="Screenshot 2026-09-11 003957" src="https://github.com/user-attachments/assets/446fe5c3-e8af-4ca8-b389-bb9a7b6c71d8" />




Running the gobuster tool and using the medium wordlist we enumerate quite a few directories. I started with the php directory but that didn't yield any results. 


<img width="1725" height="817" alt="Screenshot 2026-09-11 004037" src="https://github.com/user-attachments/assets/0d1de197-ab8c-4ea3-a287-6c8c5b02a91c" />



However the dev directory drops us right into two php web shells, the phpbash.php and the phpbash.min.php


<img width="1731" height="1301" alt="Screenshot 2026-09-11 004306" src="https://github.com/user-attachments/assets/f5459d9d-c419-4016-91f5-33cddff1ae23" />



Clicking on either drops us into a php webshell terminal that we can use to move around directories and try and locate a flag.


<img width="1737" height="1258" alt="Screenshot 2026-09-11 004554" src="https://github.com/user-attachments/assets/9e2f0379-287b-4f94-b1f9-a939443f7a24" />



Success! we locate the first flag in the home/arrexel directory

<img width="1737" height="1292" alt="Screenshot 2026-09-11 004704" src="https://github.com/user-attachments/assets/98190302-de7d-48ce-b1cb-3405eb3e7c20" />

After finding the flag I then run the sudo -l command which allows me to see what commands I can run with no password and it looks like the scriptmanager is able to do so. We need to find a way to laterally move to the scriptmanager to see what access they have.



### Exploitation






### Flag
**user flag a3c7d37662cb75b58fa7ae1220efe4f0**

**root flag 781865fb16241899ae51437c82e7fc5b**





### Key Takeaway
Don't ship debug/dev tooling to reachable directories.
---
