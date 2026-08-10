# HTB: Easy Machine — Lame

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Lame | Vulnerable Services | Exploitation | ✅ |


---

## Lame 

Date: 2026-08-10
Time in Box: 35 minutes

**Service:** Samba (Port 445)
**Concept:** Exploiting Vulnerable Services

### Enumeration
<img width="1227" height="1054" alt="Screenshot_20260810_174317" src="https://github.com/user-attachments/assets/25c7c82f-664a-4368-8253-731bcfc147ad" />

Enumeration here shows that Port 21, 22, 139, and 445 are open. Also the host script results are showing
that Samba (smb for linux) is the service running on port 445.

### Exploitation

We have quite a few options to explore for exploitation. The easiest would be looking into port 21
since it is running ftp specifically version vsftpd 2.3.4.

<img width="1227" height="615" alt="Screenshot_20260810_180322" src="https://github.com/user-attachments/assets/a6d5966e-c55c-48f9-aa0a-1946552d4cf2" />

So we see we have successfully utilized anonymous login to access the ftp server. Let's see what else we can find by moving on to Samba.



<img width="1424" height="991" alt="Screenshot_20260810_180749" src="https://github.com/user-attachments/assets/f8cc84c6-048a-431f-b6cb-f2f9d5483e6f" />



Utilizing Smbmap we see that the tmp file has a status permission of READ, WRITE with the comment of "oh noes!" We can further enumerate this with the smbclient command.

Here is the result when using the smbclient command.

 smbclient -L \\\\10.129.81.41\\\tmp
Protocol negotiation to server 10.129.81.41 (for a protocol between SMB2_02 and SMB3) failed: NT_STATUS_CONNECTION_DISCONNECTED

What this indicates is that the server we are using smbclient is using a very old version of SMB (by design for this machcine). What we can go on to now is the specific
version of Samba that is running on this machine. We can find if there are exploits for it by using searchsploit.


<img width="1424" height="377" alt="Screenshot_20260810_181603" src="https://github.com/user-attachments/assets/642efc8a-2cc8-4446-a7fe-25eb629a8e1a" />

And we get a few results. The most interesting being the 'Username' map script' command execution (Metasploit). What we can do now is search this map script in google and when we do that we see there is a vulnerability (CVE-2007-2447) on ports 139, and 445 attached that gives you a root shell. Armed with this information we can now open up msfconsole and see if we can create
a shell to gain root access to the machine. Once we get into msfconsole we can use the search command along with the version of samba we need to find an exploit.

<img width="2374" height="630" alt="Screenshot_20260810_181906" src="https://github.com/user-attachments/assets/0fcf6311-6a3c-4f5f-ad4f-dfd377f803a5" />

Now after using the exploit number we can use the command show options to see what needs to be changed to create a shell.

<img width="2374" height="1625" alt="Screenshot_20260810_182026" src="https://github.com/user-attachments/assets/59c41c91-0640-4fc2-8021-afc03b10670c" />


From here we need to change the RHOSTS to the target Machines IP address and change the LHOST to our machines IP address. We can leave the default LPORT alone.

<img width="2374" height="1596" alt="Screenshot_20260810_182259" src="https://github.com/user-attachments/assets/6c1f7cf2-7ecc-416b-8e4f-21106d2a50fb" />

Everything now looks to be in order so  we can now run the command exploit to see if we can gain access.

<img width="2374" height="530" alt="Screenshot_20260810_182455" src="https://github.com/user-attachments/assets/ceeb8567-be80-4828-8fa5-afc8fba58364" />

As you can see we have successfully connected to the target machine with a root shell. Now we can proceed to finding the flags for user and for root.




### Flag
**user flag 7b26805abd5aae443765cde3a99ab5be**
**root flag 7677e8e0580504b4c2a0c627b325a83c**

<img width="2374" height="743" alt="Screenshot_20260810_182722" src="https://github.com/user-attachments/assets/794ef371-8f48-47ba-a675-8845cbb3c8e4" />

by moving into the user directory and into the user we are able to find the user.txt, cat the file and locate the flag. Next we move onto the root user flag.



<img width="1446" height="1580" alt="Screenshot_20260810_183506" src="https://github.com/user-attachments/assets/2d6d29ac-dc6b-422d-80fe-6ba1e4f7a064" />

and here we are, by moving into the root directory we find the root.txt and with the cat command we capture the root flag and own the machine.


### Key Takeaway
The key takeaway for this machine is enumeration of outdated services can lead to gaining access to machines. Keeping up with vulnerability patching is the way to go!

---
