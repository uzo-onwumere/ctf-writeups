# HTB: Easy Machine — Shocker

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Shocker | Apache httpd | Sudo misconfig -> writable cron script | ✅ |


---

## Bashed 

Date: 2026-09-12
Time in Box:  minutes

**Service:** Apache HTTP (Port 80)
**Concept:** The chain is a classic "left-behind dev tool → sudo misconfig → writable cron script" progression: 

### Enumeration

<img width="1482" height="1143" alt="Screenshot 2026-09-13 000349" src="https://github.com/user-attachments/assets/d80359ff-fd1e-4343-976b-88150e5b0472" />

We start with our nmap scan that shows us two open ports. One being port 80 and the  other being port 2222. When going to the website we get an image that says don't bug me 
and not much else to work with so I start the web enumeration with dirbuster to see what directories I can find and work with.


<img width="1426" height="1045" alt="Screenshot 2026-09-13 000942" src="https://github.com/user-attachments/assets/4f083f67-ebb1-45f4-a866-ec34ff330034" />



Using dirbuster shows me that there is a cgi-bin/user.sh directory that I can access. 



<img width="1742" height="1182" alt="Screenshot 2026-09-13 001038" src="https://github.com/user-attachments/assets/873e92a8-51cc-47dd-92c7-a983b4342ee2" />



Going to the cgi-bin/user.sh shows me an uptime test script that doesn't really give me too much to work on. So from here I decide to do some research and see if 
I can find anything on cgi-bin/user.sh





After doing research I find that there is a vulnerability called shell shocker that effects the cgi-bin/user.sh  that I enumerated earlier. (Makes sense since the room is called
shocker). I find online that metasploit has an exploit for this vulnerability and with that it's time to enter the exploitation phase.





### Exploitation


<img width="1875" height="1148" alt="Screenshot 2026-09-13 003007" src="https://github.com/user-attachments/assets/af755523-601b-48dc-80a3-8440ea697d96" />

After inputing the various fields I run the exploit and gain a meterpreter shell. From there I navigate to the user.txt file and cat the flag.



<img width="1780" height="377" alt="Screenshot 2026-09-13 003259" src="https://github.com/user-attachments/assets/3a53fa37-09c0-4ab3-80b1-5d16a5f10709" />



I  drop into a shell and run the command sudo -l and find that the user shelly is able to run the command /usr/bin/perl as root with no password.


This lets me know that if I can find a way to run a shell in the /usr/bin/perl directory that it will lead me to gain root access. I start by going to the internet
to find a script for a reverse perl shell.


<img width="972" height="320" alt="Screenshot 2026-09-13 004201" src="https://github.com/user-attachments/assets/9f990e87-58f6-4026-ae5e-763110019912" />



After finding a perl reverse shell script I enter that into the shell and set up a listener and success! We obtain a root shell on the listener. From there I move
into the root directory and cat the root flag.



### Flag
**user flag obtained ✅**

**root flag obtained ✅**





### Key Takeaway
Don't ship debug/dev tooling to reachable directories.
---
