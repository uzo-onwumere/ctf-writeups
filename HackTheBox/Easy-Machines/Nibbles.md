

# HTB: Easy Machine — Nibbles

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Nibbles | Apache httpd | Misconfiguration and Privesc | ✅ |


---

## Nibbles 

Date: 2026-09-04
Time in Box: 90 minutes

**Service:** Apache HTTP (Port 80)
**Concept:** Two-stage chain based on misconfiguration and writeable files executed with elevated privileges. 

### Enumeration


<img width="1256" height="697" alt="Screenshot 2026-09-04 084706" src="https://github.com/user-attachments/assets/d7c90b5a-0880-42db-90d1-ea67684f2f78" />


Starting with Nmap we get a view the services active on the machine and the ports that are open. Here we see an apache webserver running on port 80 so our next step is to go to the website and see what we can find.


<img width="1742" height="612" alt="Screenshot 2026-09-04 084902" src="https://github.com/user-attachments/assets/f6fe25f1-01f9-406e-8e86-f8f21fd30fda" />


Going to the webpage we see a simple "Hello World" message. 





<img width="1745" height="788" alt="Screenshot 2026-09-04 084929" src="https://github.com/user-attachments/assets/92efa96d-42e8-4d98-bb54-2b5475be81a7" />


Viewing the source page however shows us a little more information that is interesting.



<img width="1737" height="1113" alt="Screenshot 2026-09-04 085432" src="https://github.com/user-attachments/assets/6cfdf901-7da1-4989-bfc8-47a94264e87e" />

With the notes found in the source page we get a look at the actual webpage that has a little bit more information we can work with. In the bottom right corner we see that the page is powered by nibbleblog and has three categories that have clickable links. These links don't lead to anything however. What we can do now though is further enumerate the site now that we have the /nibbleblog directory. 


<img width="1262" height="1027" alt="Screenshot 2026-09-04 085957" src="https://github.com/user-attachments/assets/513b05e6-c2c2-48db-aa3d-4d1b5a65809e" />


using the gobuster command with a wordlist we are able to enumerate quite a few more directories. The most interesting one being the admin.php directory and the admin directory. We will go through them and see what we can find.



<img width="1746" height="780" alt="Screenshot 2026-09-04 090255" src="https://github.com/user-attachments/assets/c3895f48-b3fb-458b-98da-204418c5fce6" />

starting with the content page we find quite a few subdirectories to look through for information.



### Exploitation




### Flag
**user flag 7004dbcef0f854e0fb401875f26ebd00**

**root flag 04a8b36e1545a455393d067e772fe90e**





### Key Takeaway
Writeable files executed with elevated privileges means you instantly have a method for root access.
---
