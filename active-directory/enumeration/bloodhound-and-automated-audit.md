# BloodHound and Automated Audit

Automated tooling turns the raw directory dump into attack paths. BloodHound models the domain as a graph and answers 'what is the shortest path from here to Domain Admin'; PingCastle and adPEAS score the domain against known misconfiguration classes.

## Why It Works

The directory's danger is rarely one object — it is the chain of rights between thousands of objects. Graph analysis finds chains no human would spot by reading ACLs one at a time.

## Prerequisites

* A completed collection (see Authenticated Domain Enumeration)
* Any valid domain credential

## Execution

**Scan Auto**

* from BH result
  * `AD-miner -c -cf Report -u <neo4j_username> -p <neo4j_password>` [\[tool\]](https://github.com/AD-Security/AD_Miner)
* `PingCastle.exe --healthcheck --server <domain>` [\[tool\]](https://www.pingcastle.com/)
* `Import-Module .\adPEAS.ps1; Invoke-adPEAS -Domain '<domain>' -Server '<dc_fqdn>'`

## Related

* [Authenticated Domain Enumeration](file:///9685693/enumeration/authenticated-domain-enumeration.md) — how to gather the data these tools consume
* [Rights on Users & Groups](file:///9685693/acl-abuse/rights-on-users-and-groups.md) — acting on the edges BloodHound reports
