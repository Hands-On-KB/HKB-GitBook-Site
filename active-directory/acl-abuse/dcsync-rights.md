# DCsync Rights

An account holding the replication extended rights (`DS-Replication-Get-Changes` and `-All`) can ask a domain controller to replicate secrets to it — including the krbtgt hash. This is DCSync, and it does not require code execution on the DC.

## Why It Works

Replication is how domain controllers stay in sync, so the protocol to request account secrets already exists. Whether you can use it is purely an ACL question: the two extended rights on the domain object are all that gate it.

## Prerequisites

* An account granted the replication extended rights on the domain
* Network access to a DC

## Execution

**Dcsync**

* Administrators, Domain Admins, or Enterprise Admins as well as Domain Controller computer accounts
  * **→ Domain Admin**
* `mimikatz lsadump::dcsync /domain:<target_domain> /user:<target_domain>\administrator` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
  * **→ Lateral move**
* `secretsdump.py '<domain>'/'<user>':'<password>'@'<domain_controller>'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
  * **→ Crack hash**

## Related

* [NTDS Extraction & DCSync](file:///9685693/domain-dominance/ntds-and-dcsync.md) — DCSync as a domain-dominance action
* [Rights on Users & Groups](file:///9685693/acl-abuse/rights-on-users-and-groups.md) — ACL edges that can grant these rights
