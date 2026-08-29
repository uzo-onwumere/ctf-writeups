# HTB: Easy Machine — Jerry

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Jerry | Apache Tomcat | Weak Credentials | ✅ |


---

## Jerry 

Date: 2026-08-28
Time in Box: 65 minutes

**Service:** SMB (Port 139/445)
**Concept:** Exploiting unpatched services with known CVEs 

### Enumeration


<img width="1657" height="467" alt="Screenshot 2026-08-28 204931" src="https://github.com/user-attachments/assets/662b8f48-2e9c-4cf7-9109-86343a621f7c" />






I ran a pretty simple NMAP scan and found that the only port open was 8080 and it was running an Apache Tomcat http server.



<img width="1746" height="1302" alt="Screenshot 2026-08-28 205118" src="https://github.com/user-attachments/assets/f7e62171-5b4e-4712-8f2f-f2653ad9a09e" />






Heading over to the website we find this including the version of Apache that is running which is 7.0.88. We can use this to find out what vulnerabilities exist for this version of the apache.


<img width="1742" height="620" alt="Screenshot 2026-08-28 205355" src="https://github.com/user-attachments/assets/71f62409-f0a6-4921-8b4e-68d48ea1d258" />






Clicking around on the page we find the manager/html file and at the top it gives us a username=tomcat and password=s3cret. We can now use this password and username combo to long into the
manager page.



<img width="1735" height="1305" alt="Screenshot 2026-08-28 205612" src="https://github.com/user-attachments/assets/e2036a37-d88b-4bff-bf7b-d4b7d5784ac6" />





After entering the credentials we are able to log in and see a bunch of different directories we can travel to. Along with a section to deploy a WAR file.



<img width="1651" height="1142" alt="Screenshot 2026-08-28 210614" src="https://github.com/user-attachments/assets/0dc83525-321a-4154-b21c-9d9222395003" />






for fun you can run a gobuster command with a wordlist to get those same directories we found earlier when we logged in. With that I think we have enough to start exploiting.




### Exploitation



<img width="2402" height="1552" alt="Screenshot 2026-08-28 210904" src="https://github.com/user-attachments/assets/d9fafbf7-0a3e-4f6e-9224-bcd823b0aa2e" />




Running msfconsole and searching apache tomcat shows us quite a few exploits that will work here. I am going by rank and see number 18 will give use an authenticated upload code execution. Since we are targeting the manager
page that is what I will go with. You can also use msfvenom to create a custom exploit and upload it via the war file upload we found but I am going to take the easier route and have metasploit do it all for me.



<img width="2392" height="1555" alt="Screenshot 2026-08-28 211249" src="https://github.com/user-attachments/assets/76f14df2-6a02-4bb0-8399-e27d25e0865f" />





We then set all the parameters needed to get metasploit to run properly (the RHOSTS, LHOSTS,PORTS, USERNAME, PASSWORD) and success! We see a Meterpreter line at the bottom meaning we now have access. From here we type the command
shell to get a shell session on the compromised host.

<img width="861" height="290" alt="Screenshot 2026-08-28 211537" src="https://github.com/user-attachments/assets/b4ad7097-ad06-43f8-8b70-5ed8a1c44825" />





Shell is successful and just for good measure I run a whoami command and see that I have the highest level privileges on the system with the nt authority\system. Now to find the flags






### Flag
**user flag 7004dbcef0f854e0fb401875f26ebd00**

**root flag 04a8b36e1545a455393d067e772fe90e**




<img width="2407" height="1558" alt="Screenshot 2026-08-28 211751" src="https://github.com/user-attachments/assets/414550b0-0bd5-4e74-b6d4-bb230c01f765" />




From the shell we just move through directories until we land on the desktop for the admin and find the flag directory. From here we see a file titled "2 for the price of 1.txt" we use the type command to read the contents and success!
Both flags are revealed




### Key Takeaway

Management interfaces with default creds are a full-compromise shortcut, not a foothold. The real lesson is the service context — 
Tomcat (and many Windows services) frequently run as SYSTEM, so RCE on the app equals RCE as the highest-privilege account.
---
