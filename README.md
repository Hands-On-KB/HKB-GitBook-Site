---
description: >-
  The use of active and passive tooling to scope out a target endpoint, network,
  individual, or organization.
icon: binoculars
---

# Reconnaissance

<h2 align="center">Active Recon</h2>

The act of directly interacting with a target to gain information on them. This can be any of the following: Port Scanning, Browsing Target-Owned Web Pages, Web Directory Traversal, Brute Forcing API endpoints, etc.

### Tools

#### NMap ([https://nmap.org/book/man.html](https://nmap.org/book/man.html))

Network Mapper or "NMap" is a tool used to run a vast variety of TCP and UDP scans with configurable options for different ports, protocols, services, and scripts.

For example, here are the two scans I would start most HackTheBox machines with. A SYN scan of the full port range to see what's open and then a script and version scan against those ports to pull details about them.

```desktop
// sudo nmap -sS -p- 192.168.0.1 --stats-every 5
# SYN scan of the entire TCP port range against the targeted IP showing progress every 5 seconds.
// sudo nmap -sCV -p 21,80,445 192.168.0.1 --stats-every 5
# Default script and version scan against the same target for FTP,HTTP,and SMB.
// sudo nmap -sU --top-ports=100 192.168.0.1 --stats-every 5
# UDP scan to see if any of those ports are open. Limited to top 100 ports because UDP scans go very slowly.
```

#### MassScan ([https://github.com/robertdavidgraham/masscan](https://github.com/robertdavidgraham/masscan))

An extremely fast-paced scanner capable of scanning the entire Internet in roughly five minutes.

```
// sudo masscan 10.0.0.0/8 -p80 --banners --source-ip 192.168.1.200
# Used to pull web server banners from all active hosts on a target subnet while spoofing the source IP.
```

<h2 align="center">Passive Recon</h2>

Gathering information on a target or targets using techniques and services that don't interact with them in real-time, instead providing previously gathered information.

### Tools/Sites

#### WHOIS

A simple lookup of a website's registrar record to see things like who the owner is, the subnet they're on, their nameservers, and when the site was registered.

```
// whois shadowserver.org
```

#### theHarvester ([https://github.com/laramies/theharvester](https://github.com/laramies/theharvester))

A well-known and powerful tool for gathering open-source intelligence (OSINT) information from the public internet on targets. The Harvester is capable of scraping the internet for e-mails, subdomains, DNS records, employee name collection, hosts and IPs, and much more. It mainly works in a passive manner but has one or two active modules you can use as well.

```
// theharvester -d <domain> -b all
# Throwing all available modules at a target domain.
```

#### Crt.sh (https://crt.sh)

Certificate transparency records that can be used to look up subdomains and other details on organizations or websites you may be targeting.

#### Shodan.io (https://shodan.io)

An organization that broadly scans the internet for all publicly accessible devices. Shodan scans can be run focusing on specific countries, protocols, operating systems, device manufacturers, and more.

#### Censys.io (https://search.censys.io)

Similar to Shodan, Censys is another organization that performs widespread scanning of the public internet in order to identify publicly accessible devices and store information on them such as DNS records, software in use, certificate issuers, etc.

#### LeakIX ([https://leakix.net/](https://leakix.net/))

"A red-team search engine indexing misconfigurations and vulnerabilities online" according to their documentation page. Similar to Shodan and Censys but focuses specifically on storing and providing information on vulnerable targets only.

#### DNSDumpster ([https://dnsdumpster.com/](https://dnsdumpster.com/))

A website collecting DNS records for lookup in order to find hosts/IPs, software in use, and other details on targets based on their domain name entered into the site's search bar.

