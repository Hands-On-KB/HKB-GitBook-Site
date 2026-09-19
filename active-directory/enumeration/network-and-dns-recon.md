# Network and DNS Recon

The first pass against an unknown Active Directory environment is ordinary network and DNS reconnaissance: sweep the range, fingerprint services, and see how much the DNS infrastructure will volunteer. Everything here runs unauthenticated.

## Why It Works

Domain-joined networks advertise themselves. Domain controllers expose a predictable service set (88, 389, 445, 464, 636, 3268), and misconfigured DNS servers will hand over a full zone to anyone who asks for a transfer.

## Prerequisites

* Layer 2/3 access to the target network
* No credentials required

## Execution

**Scan network**

* `nxc smb <ip_range>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* `nmap -sP -p <ip>` [\[tool\]](https://github.com/nmap/nmap)
* `nmap -Pn -sV --top-ports 50 --open <ip>` [\[tool\]](https://github.com/nmap/nmap)
* `nmap -Pn --script smb-vuln* -p139,445 <ip>` [\[tool\]](https://github.com/nmap/nmap)
  * **→ Vulnerable host**
* `nmap -Pn -sC -sV -oA <output> <ip>` [\[tool\]](https://github.com/nmap/nmap)
* `nmap -Pn -sC -sV -p- -oA <output> <ip>` [\[tool\]](https://github.com/nmap/nmap)
* `nmap -sU -sC -sV -oA <output> <ip>` [\[tool\]](https://github.com/nmap/nmap)

**Zone transfer**

* `dig axfr <domain_name> @<name_server>` [\[tool\]](https://linux.die.net/man/1/dig)

{% hint style="warning" %}
A full `-p-` sweep across a domain range is loud and will generate connection failures on every monitored host. Scope and pace the scan to match your rules of engagement.
{% endhint %}

## Related

* [Locating Domain Controllers](file:///9685693/enumeration/locating-domain-controllers.md) — narrowing the scan down to the DCs
* [LDAP Enumeration](file:///9685693/enumeration/ldap-enumeration.md) — the next step once a DC is identified
