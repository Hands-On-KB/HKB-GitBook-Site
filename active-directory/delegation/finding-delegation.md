# Finding Delegation

Delegation lets a service act on behalf of a user toward another service. Every form of it is an object attribute you can read, so the first step is simply listing which accounts are configured for which kind of delegation.

## Why It Works

Delegation is recorded in `userAccountControl` flags and the `msDS-AllowedToDelegateTo` / `msDS-AllowedToActOnBehalfOfOtherIdentity` attributes — all readable by any authenticated user.

## Prerequisites

* Any valid domain credential
* LDAP reachable on a DC

## Execution

**Find delegation**

* `findDelegation.py "<domain>"/"<user>":"<password>"` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/findDelegation.py)
* With BloodHound
  * Unconstrained
    * `MATCH (c:Computer {unconstraineddelegation:true}) RETURN c` [\[tool\]](https://github.com/SpecterOps/BloodHound)
    * `MATCH (c:User {unconstraineddelegation:true}) RETURN c` [\[tool\]](https://github.com/SpecterOps/BloodHound)
  * Constrained
    * `MATCH p=((c:Base)-[:AllowedToDelegate]->(t:Computer)) RETURN p` [\[tool\]](https://github.com/SpecterOps/BloodHound)
    * `MATCH p=shortestPath((u:User)-[*1..]->(c:Computer {name: "<MYTARGET.FQDN>"})) RETURN p` [\[tool\]](https://github.com/SpecterOps/BloodHound)

## Related

* [Unconstrained Delegation](file:///9685693/delegation/unconstrained-delegation.md) — the most dangerous result to look for
* [BloodHound & Automated Audit](file:///9685693/enumeration/bloodhound-and-automated-audit.md) — graphs delegation edges for you
