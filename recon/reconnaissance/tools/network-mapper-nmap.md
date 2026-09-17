---
description: https://nmap.org/
hidden: true
icon: map
layout:
  width: wide
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
  anchors:
    visible: true
tags:
  - scanning
  - nmap
  - recon
---

# Network Mapper (NMap)

## My Usual Go-To Scans

* Subnet host discovery scan to find targets for follow-on actions. Skips port scanning (for now) and outputs the results of the scan in all formats.
  * `sudo nmap <Target CIDR Range> -sn -oA <Output Filename>`
* Full TCP port range scan to begin engaging the target while disabling ping discovery since you already know it's there. I also usually include the option to get progress feedback every 5 seconds because I'm impatient (though you can press spacebar during the scan to get the status also).
  * `sudo nmap -sS -p- -Pn <Target IP> --stats-every 5`
* Follow up scan on all open ports found with default scripts as well as a version scan, and output into a greppable file format.
  * `sudo nmap -sCV -p <Port Range> -Pn <Target IP> -oG <Filename> --stats-every 5`
* There are also a variety of other options in terms of scripts, ports, and scan types:

## Scan Types

* `-sT`: Full TCP Connection Attempt (SYN, SYN-ACK, ACK)
* `-sU`: UDP Scan for services and protocols using UDP (extremely slow scans).
* [`-sF, -sN, -sX`:](https://nmap.org/book/scan-methods-null-fin-xmas-scan.html) TCP FIN, NULL, and Xmas scans.
* `--scan-flags <TCP Flag(s)>`: Manually select the specific TCP flags you want to include in your scanning packets (SYN, ACK, PSH, URG, FIN, RST, etc.).
* [`--script <script name, category, directory>`](https://nmap.org/book/nse-usage.html#nse-categories): Uses the specified script(s) in your scan.
  * `--script-help <script name>:` Brings up a help page for your script (or multiple if you use a wildcard in your search like "smb-\*" to find all SMB scripts).
* `-A`: Aggressive Scan. Performs service detection, OS detection, traceroute and uses defaults scripts to scan the target. Highly visible though due to speed and......aggression.

## Other Options

* `--top-ports=<number>`: Chooses the most common ports based on the number you input (ie. top 10 most common ports is --top-ports=10).
* `--packet-trace`: Show all packets sent and received during the scan.
* `-PE`: Perform the ping discovery scan specifically using ICMP Echo Requests.
* `--reason`: Displays the reason why NMap produced a the result you got.
* `--disable-arp-ping`: Disable ARP requests sent for host discovery on the local network.
* `-n`: Disable DNS resolution.
* `-F`: Scan the top 100 ports.
* `xsltproc <Output Filename>.xml -o <Filename>.html`: Create an HTML report of the scan results you output to XML format with the `-oX` flag.
* `-v` or `-vv`: Increase verbosity, or really increase verbosity, to get more details in your scan output.
* `--min-RTT-timeout`: Limits the amount of time the scanner waits before moving on to the next port. By default NMap usually waits for 100ms. The `--initial-RTT-timeout` and `max-RTT-timeout` flags serve similar purposes.
* `-T <0-5>`: Sets the speed of the scan on a scale of 0 to 5. Going slower increases stealth but takes longer to get results while going faster is noisy but gets you results sooner. The speeds are Paranoid, Sneaky, Polite, Normal, Aggressive, and Insane.
* `--min-rate <number>`: How many packets should be sent simultaneously to the target's ports.
* `--max-retries <number>`: Allows you to control how many times NMap attempts to contact a port. The default is 10, so setting it lower can speed up scans.
* `-D RND:<Number>`: Includes a randomized number of decoy IPs that your scan is run from in order to confuse defenders analyzing network traffic.

## Script Scans

* The NMap Scripting Engine (NSE) has hundreds of scripts already written for it broken down into 14 categories:

| `auth`      | `Determination of authentication credentials.`                                                                                            |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `broadcast` | `Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans.` |
| `brute`     | `Executes scripts that try to log in to the respective service by brute-forcing with credentials.`                                        |
| `default`   | `Default scripts executed by using the -sC option.`                                                                                       |
| `discovery` | `Evaluation of accessible services.`                                                                                                      |
| `dos`       | `These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services.`              |
| `exploit`   | `This category of scripts tries to exploit known vulnerabilities for the scanned port.`                                                   |
| `external`  | `Scripts that use external services for further processing.`                                                                              |
| `fuzzer`    | `This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time.`     |
| `intrusive` | `Intrusive scripts that could negatively affect the target system.`                                                                       |
| `malware`   | `Checks if some malware infects the target system.`                                                                                       |
| `safe`      | `Defensive scripts that do not perform intrusive and destructive access.`                                                                 |
| `version`   | `Extension for service detection.`                                                                                                        |
| `vuln`      | `Identification of specific vulnerabilities.`                                                                                             |

Scripts can be called within a scan command by using the `--script` flag. That can be followed by the name of a category, or categories, and/or specific script(s).

## Port Statuses

| `open`             | `This indicates that the connection to the scanned port has been established. These connections can be`` `**`TCP connections`**`,`` `**`UDP datagrams`**` ``as well as`` `**`SCTP associations`**`.`    |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `closed`           | `When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an RST flag. This scanning method can also be used to determine if our target is alive or not.` |
| `filtered`         | `Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.`                |
| `unfiltered`       | `This state of a port only occurs during the`` `**`TCP-ACK`**` ``scan and means that the port is accessible, but it cannot be determined whether it is open or closed.`                                 |
| `open\|filtered`   | `If we do not get a response for a specific port, Nmap will set it to that state. This indicates that a firewall or packet filter may protect the port.`                                                |
| `closed\|filtered` | `This state only occurs in the`` `**`IP ID idle`**` ``scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.`                                 |

## Useful NMap Documentation

{% embed url="https://nmap.org/book/man-performance.html" %}

{% embed url="https://nmap.org/book/man-nse.html" %}

{% embed url="https://nmap.org/book/man-bypass-firewalls-ids.html" %}
