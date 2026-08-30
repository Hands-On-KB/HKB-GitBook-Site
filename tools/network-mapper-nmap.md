---
description: https://nmap.org/
icon: map
---

# Network Mapper (NMap)

* Full TCP port range scan to begin engaging the target while disabling ping discovery since you already know it's there. I also usually include the option to get progress feedback every 5 seconds because I'm impatient.
  * `sudo nmap -sS -p- -Pn <Target IP> --stats-every 5`
* Follow up scan on all open ports found with default scripts as well as a version scan, and output into a greppable file format.
  * `sudo nmap -sCV -p <Port Range> -Pn <Target IP> -oG <Filename> --stats-every 5`
* There are also a variety of other options in terms of scripts, ports, and scan types:
  * Scan Types
    * `-sT`: Full TCP Connection Attempt (SYN, SYN-ACK, ACK)
    * `-sU`: UDP Scan for services and protocols using UDP (extremely slow scans).
    * [`-sF, -sN, -sX`:](https://nmap.org/book/scan-methods-null-fin-xmas-scan.html) TCP FIN, NULL, and Xmas scans.
    * `--scan-flags <TCP Flag(s)>`: Manually select the specific TCP flags you want to include in your scanning packets (SYN, ACK, PSH, URG, FIN, RST, etc.).
    * [`--script <script name, category, directory>`](https://nmap.org/book/nse-usage.html#nse-categories): Uses the specified script(s) in your scan.
      * `--script-help <script name>:` Brings up a help page for your script (or multiple if you use a wildcard in your search like "smb-\*" to find all SMB scripts).
  * Other Options
    * `--top-ports=<number>`: Chooses the most common ports based on the number you input (ie. top 10 most common ports is --top-ports=10).

