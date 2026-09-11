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


Now we enter the exploitation phase. We know that we have a php webshell and know that the scriptmanager will allow us to run commands with no password. What I decide to do is find a python script exploit that can set up a reverse shell so I can move around as the scriptmanager instead of www-data.





<img width="1751" height="1307" alt="Screenshot 2026-09-11 005329" src="https://github.com/user-attachments/assets/7f326000-d642-4941-b181-19af05776be7" />

After doing some quick research on google we find a python reverse shell script that looks promising. 



<img width="1710" height="1157" alt="Screenshot 2026-09-11 005629" src="https://github.com/user-attachments/assets/d469b1f5-fa96-439a-a2f2-707d3063d4f6" />



I edit the script in a texts editor to reflect my attacker IP and port and then set up a listener in another terminal.



<img width="1011" height="732" alt="Screenshot 2026-09-11 005853" src="https://github.com/user-attachments/assets/16c73718-960c-4d4b-a50e-b086c9879598" />



I then enter the command into the php webshell terminal and we are in! The Listener responds with a /bin/bash shell.


<img width="991" height="277" alt="Screenshot 2026-09-11 010248" src="https://github.com/user-attachments/assets/0aaabaab-6124-4b3b-8282-33f6a4d854a5" />




From here I now run the command sudo -l again to get the same response as before. I then run the sudo -u scriptmanager /bin/bash -i to to switch into the scriptmanager user and spawn a bash shell. This provides me with a full interactive shell and successfully indicates lateral movement. For good measure I run the whoami command and id command just to make sure I am infact scriptmanager. 




<img width="1197" height="932" alt="Screenshot 2026-09-11 010430" src="https://github.com/user-attachments/assets/aec9794a-a514-4d11-bf7b-bf1930db5571" />



Now that I am scriptmanager I want to see what accesses they have. I run the ls -l command to see the read, write, and execute privileges are available. Everything is root besides the scripts directory which only the scriptmanager has access too. I decide to see what I can find in that directory.




<img width="950" height="388" alt="Screenshot 2026-09-11 011021" src="https://github.com/user-attachments/assets/7be6d465-d666-45c4-99a2-7071c7c7242e" />



Moving into that directory I am able to find to files a test.py and a test.txt. I cat both files and see that the test.py has instructions of open the test.txt write testing 123 and close. Running cat on the test.txt shows the testing 123 that was found in the test.py. This tells me that if I write something to the test.py file the test.txt will run it. Since I can write to test.py I can create a shell that can give hopefully give me root access.






It then dawned on me that the same python script I used to gain a reverse shell the first time can be used again and written to the test.py file to gain root access. I take the same script from early (modifying it to reflect a new listening port) and run an echo command to write the file to the test.py file. 










### Flag
**user flag a3c7d37662cb75b58fa7ae1220efe4f0**

**root flag 781865fb16241899ae51437c82e7fc5b**





### Key Takeaway
Don't ship debug/dev tooling to reachable directories.
---
