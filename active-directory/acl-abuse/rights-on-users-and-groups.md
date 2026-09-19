# Rights on Users and Groups

The everyday ACL edges. GenericAll over a user lets you reset its password or add shadow credentials; over a group, add yourself as a member; WriteDACL and WriteOwner let you grant yourself the rights you do not yet have.

## Why It Works

Active Directory permissions are granular and additive, and decades of delegation leave most domains with a dense web of edges no one has audited. Each edge is a small, legitimate-looking grant; chained together they reach Domain Admin.

## Prerequisites

* An ACL edge over the target user or group (found via BloodHound)
* Any valid domain credential

## Execution

**On User**

* GenericAll / GenericWrite
  * Change password
    * `net user <user> <password> /domain`
      * **→ User with clear text pass**
  * add SPN (target kerberoasting)
    * `targetedKerberoast.py -d <domain> -u <user> -p <pass>` [\[tool\]](https://github.com/ShutdownRepo/targetedKerberoast)
      * **→ Hash found (TGS)**
  * add key credentials
    * **→ shadow credentials**
  * login script
    * **→ Access**
* ForceChangePassword
  * `net user <user> <password> /domain`
    * **→ User with clear text pass**

**On Group**

* GenericAll/GenericWrite/Self/Add Extended Rights
  * Add member to the group
* Write Owner
  * Grant Ownership
* WriteDACL + WriteOwner
  * Grant rights
    * Give yourself generic all

## Related

* [BloodHound & Automated Audit](file:///9685693/enumeration/bloodhound-and-automated-audit.md) — finding the edges
* [Shadow Credentials](file:///9685693/acl-abuse/shadow-credentials.md) — one way to abuse GenericWrite on a user
