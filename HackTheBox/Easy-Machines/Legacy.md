# HTB: Easy Machine — Legacy

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

## Legacy 

Date: 2026-08-11
Time in Box: 40 minutes

**Service:** SMB (Port 139/445)
**Concept:** Exploiting unpatched services with known CVEs 

### Enumeration


<img width="1636" height="993" alt="nmap enumeration" src="https://github.com/user-attachments/assets/b96f6288-5f18-4e99-afde-eb07cf3fb191" />

I used a standard nmap command and flags to enumerate the box and found that the machine had open ports for 139/445. Further breakdown of the nmap returns shows that
the machine is running windows XP.

<img width="1812" height="1446" alt="script vlun smb" src="https://github.com/user-attachments/assets/b738d59e-d282-4f18-a18f-5c0f4cf42a32" />

I then conducted another nmap scan but this time using the script=smb-vuln* flags to find out that this machine is vulnerable to CVE-2008-4250 and CVE-2017-0143.
It also showed me that this vulnerability leads to RCE (remote code execution) and even gave me the ms17-010 which is the infamous EternalBlue.

Armed with this information I was now able to fire up metasploit for the exploitation phase.



### Exploitation

<img width="2384" height="1060" alt="metasploit smb" src="https://github.com/user-attachments/assets/3170bf39-5004-4688-b35b-c12201ee4db9" />

I run a search for CVE-2008-4250 which was found during the enumeration phase. After looking at the results I ran the command "use 0" to use the first
exploit on the list.

<img width="2373" height="1648" alt="show options metasploit" src="https://github.com/user-attachments/assets/ea2666b4-36fb-456c-b5d2-9e8f8be8c95a" />

From here I run "show options" and populate the RHOSTS, and LHOST with the target IP address and my attacker IP address respectively and run the exploit.

<img width="2365" height="353" alt="exploit" src="https://github.com/user-attachments/assets/e9002dd6-9975-454a-a413-cc935084fc42" />

We have success! A meterpreter session is opened and a C:\WINDOWS\system32 prompt is available. Now we can search for the user and root flags.



### Flag
**user flag e69af0e4f443de7e36876fda4ec7644f**

**root flag 993442d258b0e0ec917cae9e695d5713**

<img width="1550" height="1020" alt="user  flag" src="https://github.com/user-attachments/assets/f3fc6f5e-0132-4fb4-b74a-273731d481a7" />

After searching the directories I find the user flag on the user John's desktop.


<img width="1745" height="1858" alt="root flag" src="https://github.com/user-attachments/assets/5bd3c291-d752-493e-8a3a-b6d3527d5374" />

After a little more searching in the directories the root flag was found in the Admin directory on the desktop.



### Key Takeaway
Unpatched services with public exploits are the fastest path to compromising a machine.
---
