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






### Exploitation






### Flag
**user flag 375376a3ac09793dddd235144b616852**

**root flag 781865fb16241899ae51437c82e7fc5b**





### Key Takeaway
Don't ship debug/dev tooling to reachable directories.
---
