# HTB: Easy Machine — Devel

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Devel | FTP Anonymous | FTP Misconfiguration | ✅ |


---

## Devel 

Date: 2026-09-13
Time in Box: 90 minutes

**Service:** FTP Anonymous (Port 21)
**Concept:** Anonymous FTP misconfiguration can lead to web shell execution via file uploads.

### Enumeration

<img width="809" height="588" alt="nmap scan" src="https://github.com/user-attachments/assets/02da2885-e56e-498b-a1dd-f7381d2b134f" />

I ran a standard nmap command and found that two ports were open. Port 21 and port 80. I start by going to the site and realize there is nothing there I can interact with. 

<img width="814" height="585" alt="msfvenom exploit" src="https://github.com/user-attachments/assets/854a8f11-4ac3-4489-8ef9-265b41f76678" />

I notice however that the site is running aspx so I start a msfvenom command to create a file that hosts a reverse shell exploit.


<img width="805" height="585" alt="ftp put request" src="https://github.com/user-attachments/assets/c70354d3-aaaa-4510-9b5f-1d34e2e2e05b" />


With the exploit created I try the FTP port since Anonymous login is allowed. I am able to get into the FTP server and I upload the exploit using the put command.



<img width="843" height="748" alt="metasploit handler" src="https://github.com/user-attachments/assets/23408213-05f8-4f6b-ad2f-3e855f13e85b" />



<img width="841" height="742" alt="meterpreter shell" src="https://github.com/user-attachments/assets/923b909d-3837-4827-8dd2-9b16a35f9e09" />


With the exploit uploaded I start a msfconsole session and start a listerner on metasploit. I go back to the site and put in a /exploit.aspx in the URL and run it. Success! We get reverse shell on the machine.




<img width="834" height="195" alt="sysinfo results" src="https://github.com/user-attachments/assets/f1014dcb-3714-4d30-8f4e-ded1f11603fa" />


Running a sysinfo command we get the exact OS version and architecture running on the machine. From here we can manually search for an exploit or vulnerability targeting this OS version. I decide to use do something different since I am already on metasploit with a meterpreter shell.



<img width="841" height="410" alt="search suggester" src="https://github.com/user-attachments/assets/acc85d83-38d8-4f2c-8223-fa5d02367836" />


I use the search suggester command in metasploit and enter my session. From here it will run and tell me what exploits this machine I have a shell on is vulnerable to.





### Exploitation



<img width="1553" height="766" alt="exploit results" src="https://github.com/user-attachments/assets/64f10f28-7069-4d93-a62d-60a04ec7a1e2" />

We get a pretty decent sized list of exploits that could work on the machine. I pick the schlamperei one and start the exploit and we get the shell.




<img width="937" height="242" alt="root shell" src="https://github.com/user-attachments/assets/03e41871-f2b7-47e4-9196-0e58a6ee9bc0" />


I drop into the shell and run a whoami command and I get nt authority\system aka root.



From here I navigate to the user desktop to snag the user flag.


<img width="611" height="327" alt="user flag" src="https://github.com/user-attachments/assets/1befd0ae-b0cb-41ce-8d74-48a2f5d27bc5" />




From there I use the same process to snag the root flag

<img width="754" height="399" alt="root flag" src="https://github.com/user-attachments/assets/6f970de0-1158-4584-96db-7b745b30543a" />




### Flag
**user flag obtained ✅**

**root flag obtained ✅**










### Key Takeaway
Enforce Proper Service Isolation: Never map an anonymous upload directory directly into a web server's execution path. If users must upload files, they should be stored completely outside the web root, and file execution permissions must be explicitly disabled.
---
