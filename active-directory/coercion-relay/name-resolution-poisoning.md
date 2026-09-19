# Name Resolution Poisoning

When DNS resolution fails, Windows falls back to broadcast protocols and trusts whoever answers first. Responding to those broadcasts puts you in the authentication path for any client that mistypes a hostname or looks for a share that no longer exists.

## Why It Works

LLMNR, NBT-NS and mDNS have no authentication — the first reply wins. IPv6 compounds it: Windows prefers IPv6 over IPv4 and most networks run no DHCPv6 server, so a rogue one is uncontested.

## Prerequisites

* Layer 2 access to the client network segment
* No credentials required

## Execution

**Poisoning**

* LLMNR / NBTNS / MDNS
  * `responder -l <interface>` [\[tool\]](https://github.com/lgandx/Responder)
* ⚠️ DHCPv6 (IPv6 prefered to IPv4)
  * `mitm6 -d <domain>` [\[tool\]](https://github.com/dirkjanm/mitm6)
    * **→ poisoning SMB**
  * `bettercap` [\[tool\]](https://www.bettercap.org/)
    * **→ poisoning LDAP**
    * **→ poisoning HTTP**
* ⚠️ ARP Poisoning
  * `bettercap` [\[tool\]](https://www.bettercap.org/)
  * `asreqroast`
    * `Pcredz -i <interface> -v` [\[tool\]](https://github.com/lgandx/PCredz)
      * **→ Hash found ASREQ**

{% hint style="warning" %}
ARP poisoning and DHCPv6 takeover both redirect live production traffic and can break connectivity for the hosts affected. The source diagram marks both with ⚠️ for that reason.
{% endhint %}

## Related

* [Capturing Authentication](file:///9685693/coercion-relay/capturing-authentication.md) — what to do with the authentication you attract
* [NTLM Relay](file:///9685693/coercion-relay/ntlm-relay.md) — forwarding it rather than cracking it
