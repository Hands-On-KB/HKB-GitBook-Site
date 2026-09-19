# Rights on computers and OUs

Write access over a computer object leads to RBCD or shadow credentials; write access over an OU, especially WriteDACL, lets you push an inherited ACL down onto every object the OU contains.

## Why It Works

An OU is a container, and ACLs inherit downward. Rights over the container therefore become rights over its contents — a single WriteDACL on an OU can compromise every user and computer beneath it.

## Prerequisites

* An ACL edge over the computer object or OU
* Any valid domain credential

## Execution

**On Computer**

* GenericAll / GenericWrite
  * msDs-AllowedToActOnBehalf
    * **→ RBCD**
  * add Key Credentials
    * **→ shadow credentials**

**On OU**

* Write Dacl
  * ACE Inheritance
    * Grant rights
* GenericAll / GenericWrite / Manage Group Policy Links
  * `OUned.py --config config.ini` [\[tool\]](https://github.com/synacktiv/OUned)

## Related

* [Resource-Based Constrained Delegation](file:///9685693/delegation/resource-based-constrained-delegation.md) — the usual computer-object abuse
* [Shadow Credentials](file:///9685693/acl-abuse/shadow-credentials.md) — the other computer-object path
