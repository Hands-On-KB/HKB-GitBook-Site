# Locating Domain Controllers

Almost every later technique needs a domain controller address. There are three reliable ways to get one without credentials: read the DHCP-supplied DNS server, query the `_ldap._tcp.dc._msdcs` SRV record, or sweep for Kerberos on 88.

## Why It Works

Domain members must be able to find a DC to function, so the location of every DC is published in DNS by design. Any host on the network can resolve those records.

## Prerequisites

* Network access to the domain DNS server
* No credentials required

## Execution

**Find DC IP**

* `nmcli dev show <interface>` [\[tool\]](https://linux.die.net/man/1/nmcli)
* `nslookup -type=SRV _ldap._tcp.dc._msdcs.<domain>` [\[tool\]](https://linux.die.net/man/1/nslookup)
* `nmap -p 88 --open <ip_range>` [\[tool\]](https://github.com/nmap/nmap)

## Related

* [Network & DNS Recon](file:///9685693/enumeration/network-and-dns-recon.md) — broader sweep of the same network
* [LDAP Enumeration](file:///9685693/enumeration/ldap-enumeration.md) — querying the DC once you have its address
