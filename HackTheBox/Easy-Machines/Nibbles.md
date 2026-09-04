

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


<img width="1747" height="690" alt="Screenshot 2026-09-04 091146" src="https://github.com/user-attachments/assets/e0462595-01ca-4939-b484-4913d8dd2120" />



Starting with the content page we find three subdirectories to look through for information. 


<img width="1742" height="948" alt="Screenshot 2026-09-04 091245" src="https://github.com/user-attachments/assets/fa45a41e-5cf1-41c6-b8e9-d1d7290fe722" />


<img width="1747" height="923" alt="Screenshot 2026-09-04 091307" src="https://github.com/user-attachments/assets/dd40d527-5c62-4568-b744-4b8dc9a37661" />


Here we see a users.xml and in that file we find a username of admin. (Weak credential). This is something we should make a note of for later use. 





<img width="1750" height="1302" alt="Screenshot 2026-09-04 091635" src="https://github.com/user-attachments/assets/2da19508-0f7d-4994-87ed-147a6c982b74" />




Going through the rest of the files we do not find a password that pairs with the username but in the config file we see a lot of references to nibbles (the name of the room) Worth a shot as a potential password. Nothing else is standing out at the moment with the other subdirectories so lets head to the admin.php directory and see if we can get into it.




<img width="1752" height="1272" alt="Screenshot 2026-09-04 091809" src="https://github.com/user-attachments/assets/72174cce-e269-4cd8-9a76-05befd8b0dcd" />


Here we get to the login page and we attempt to enter the username - admin password - nibbles 





<img width="1750" height="1233" alt="Screenshot 2026-09-04 091914" src="https://github.com/user-attachments/assets/5ce3287f-64b9-49e6-a870-6d536b60d402" />

Success! We have guessed the password correctly and have access to the admin page. From here we can move around and see if there are any other vulnerabilities we may be able to use for our Exploitation phase.


<img width="1750" height="1005" alt="Screenshot 2026-09-04 092106" src="https://github.com/user-attachments/assets/64230da4-d5b7-49b8-ba54-dd5f472adcd4" />


Going through various tabs on the left hand side of the admin page we get to plugins and the my image file where we can upload files leading us to believe that file injection is another vulnerability we can exploit on this website. With all this found we can now start some exploitation attempts. 



### Exploitation


<img width="1005" height="732" alt="Screenshot 2026-09-04 092527" src="https://github.com/user-attachments/assets/fe47bcac-a083-481d-8006-f1d5cf66d702" />

We fire up msfconsole and search for nibbleblog and come up with a file upload exploit. This means out file injection hypothesis from the upload section on the website was correct. 


<img width="1222" height="1055" alt="Screenshot 2026-09-04 092702" src="https://github.com/user-attachments/assets/4193e4a5-ab54-4974-9b49-37da566dd01e" />


After utilizing the module we need to enter all of the information we have gathered along with setting our attacker host and targetURI.




<img width="1227" height="1057" alt="Screenshot 2026-09-04 092915" src="https://github.com/user-attachments/assets/fdfdda35-1528-493b-9133-410f6daec806" />


Now with everything added we can run the exploit.




<img width="1230" height="461" alt="Screenshot 2026-09-04 093049" src="https://github.com/user-attachments/assets/1f7e86ff-b930-4916-8d91-bca06f34dc99" />

Success! We now how a working meterpreter shell (disregard the pwd and whoami the shell was taking a long time to form so I was checking to see if it was working).



<img width="1057" height="247" alt="Screenshot 2026-09-04 093345" src="https://github.com/user-attachments/assets/ea23f7e7-9154-4b57-bd9b-816d42ea518e" />

After dropping into a shell we navigate to the usr directory and from there into the nibbler directory and locate the user.txt file. we cat the file and get the first user flag. 



<img width="1227" height="283" alt="Screenshot 2026-09-04 093554" src="https://github.com/user-attachments/assets/0484b7e8-bfc9-455a-9385-5636dabac7c6" />


In the same nibbler folder we find a personal.zip file and use the command unzip to gain access to the  contents which leads to the directory /personal/personalstuff/monitor.sh. We will move into that directory next.



<img width="1227" height="320" alt="Screenshot 2026-09-04 093807" src="https://github.com/user-attachments/assets/3d1c1c0e-c16d-4049-bf11-e55021765866" />

We run the sudo-l command and see that the monitor.sh file has root privileges and has NOPASSWD (no password). That means we can access a file that has root privileges without a password. Meaning if we can access it we can possibly gain root access over the machine through this file. 

<img width="927" height="151" alt="Screenshot 2026-09-04 094116" src="https://github.com/user-attachments/assets/6b1ef19a-1d4b-44db-97bd-2b2f302a6098" />

Running the ls -l command we can see we have read, write, and execute privileges on the monitor.sh file. What this means is that we can write to the file and write in a reverse shell to the file and have it execute and capture that response to a lister we set up on our machine. 







### Flag
**user flag 375376a3ac09793dddd235144b616852**

**root flag 04a8b36e1545a455393d067e772fe90e**





### Key Takeaway
Writeable files executed with elevated privileges means you instantly have a method for root access.
---
