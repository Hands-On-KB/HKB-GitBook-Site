# LDAP Enumeration

LDAP is the directory's query interface. An anonymous bind is often restricted to the RootDSE, but that alone confirms the domain name, functional level, and naming contexts.

## Why It Works

Every domain-joined system reads the directory over LDAP, so the service must be reachable. What an anonymous bind can read varies by domain configuration and is sometimes far more than intended.

## Prerequisites

* LDAP (389/TCP) or LDAPS (636/TCP) reachable on a DC
* No credentials required for RootDSE; a valid account for most other queries

## Execution

**Enumerate LDAP**

* `nmap -n -sV --script 'ldap*' and not brute -p 389 <dc_ip>` [\[tool\]](https://github.com/nmap/nmap)
  * **→ Username**
* `ldapsearch -x -H <dc_ip> -s base` [\[tool\]](https://linux.die.net/man/1/ldapsearch)

## Related

* [User Enumeration](file:///9685693/enumeration/user-enumeration.md) — pulling the user list specifically
* [Authenticated Domain Enumeration](file:///9685693/enumeration/authenticated-domain-enumeration.md) — the full picture once you hold a credential
