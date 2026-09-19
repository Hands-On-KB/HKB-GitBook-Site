# Timeroasting

Domain controllers answer MS-SNTP time requests with a signature computed from the queried computer account's NT hash. Because the request needs no authentication, anyone on the network can harvest crackable material for every computer account.

## Why It Works

The MS-SNTP extension authenticates the time response to the client using the machine account key, but the DC never verifies that the requester is the machine in question. The signature is returned to whoever asked.

## Prerequisites

* NTP (123/UDP) reachable on a DC
* No credentials required

## Execution

**TimeRoasting**

* `timeroast.py <dc_ip> -o <output_log>` [\[tool\]](https://github.com/SecuraBV/Timeroast)
  * **→ timeroast hash**

{% hint style="info" %}
Machine account passwords are 120 random characters by default, so these rarely crack — but accounts created by imaging scripts or left from decommissioned hosts sometimes do.
{% endhint %}

## Related

* [Hash Cracking Reference](file:///9685693/credential-access/hash-cracking-reference.md) — mode 31300 for the `$sntp-ms$` format
* [Locating Domain Controllers](file:///9685693/enumeration/locating-domain-controllers.md) — finding a DC to query
