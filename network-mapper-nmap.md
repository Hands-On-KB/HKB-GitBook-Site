---
description: https://nmap.org/
icon: map
---

# Network Mapper (NMap)

* Full TCP port range scan to begin engaging the target while disabling ping discovery since you already know it's there. I also usually include the option to get progress feedback every 5 seconds because I'm impatient.
  * `sudo nmap -sS -p- -Pn <Target IP> --stats-every 5`

