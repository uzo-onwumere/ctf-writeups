# HTB: Easy Machine — Devel

![Platform](https://img.shields.io/badge/Platform-HackTheBox-9fef00?style=flat-square&labelColor=0d1117)
![Tier](https://img.shields.io/badge/Easy-Machines-lightgrey?style=flat-square&labelColor=0d1117)
![Status](https://img.shields.io/badge/Status-Complete-00e5a0?style=flat-square&labelColor=0d1117)

---

## Overview

Working on the easy tier of machines after finishing the starting point ones.

| Machine | Service | Concept | Flag |
|:---|:---|:---|:---:|
| Devel | FTP Anonymous | Sudo misconfig -> writable cron script | ✅ |


---

## Bashed 

Date: 2026-09-13
Time in Box: 90 minutes

**Service:** FTP Anonymous (Port 21)
**Concept:** Manual windows kernel privesc — foothold gives a low-priv IIS account, systeminfo identifies an unpatched kernel, transfer-and-run the matching exploit to reach SYSTEM 

### Enumeration




### Exploitation






### Flag
**user flag obtained ✅**

**root flag obtained ✅**





### Key Takeaway
An unpatched Windows box lets you escalate from a low-privilege service account to SYSTEM by identifying the missing kernel patch from systeminfo and running the matching kernel exploit — the whole privesc is enumerate-the-patch-level, then transfer-and-run.
---
