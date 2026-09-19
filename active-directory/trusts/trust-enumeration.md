# Trust Enumeration

Before crossing a trust you have to map it: which domains trust which, in what direction, and whether the trust is transitive. The direction determines who can reach whom, and it is often the opposite of what intuition suggests.

## Why It Works

Trusts are directory objects describing an authentication relationship. They are readable, and their direction and attributes (transitivity, SID filtering, forest vs external) decide which cross-domain techniques are even possible.

## Prerequisites

* Any valid domain credential
* LDAP reachable on a DC

## Execution

**Enumeration**

* `nltest.exe /trusted_domains` [\[tool\]](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731935\(v=ws.11\))
* `([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()`
* `Get-DomainTrust -Domain <domain>` [\[tool\]](https://github.com/PowerShellMafia/PowerSploit)
* `Get-DomainTrustMapping` [\[tool\]](https://github.com/PowerShellMafia/PowerSploit)
* `ldeep ldap -u <user> -p <password> -d <domain> -s ldap://<dc_ip> trusts` [\[tool\]](https://github.com/franc-pentest/ldeep)
* `sharphound.exe -c trusts -d <domain>` [\[tool\]](https://github.com/SpecterOps/SharpHound)
  * `MATCH p=(:Domain)-[:TrustedBy]->(:Domain) RETURN p` [\[tool\]](https://github.com/SpecterOps/BloodHound)
* Get Domains SID
  * `Get-DomainSID -Domain <domain> Get-DomainSID -Domain <target_domain>` [\[tool\]](https://github.com/PowerShellMafia/PowerSploit)
  * `lookupsid.py -domain-sids <domain>/<user>:<password>'@<dc> 0 lookupsid.py -domain-sids <domain>/<user>:<password>'@<target_dc> 0` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/lookupsid.py)

## Related

* [Intra-Forest Escalation](file:///9685693/trusts/intra-forest-escalation.md) — abusing trusts inside a forest
* [Cross-Forest & External Trusts](file:///9685693/trusts/cross-forest-and-external.md) — crossing forest and external trusts
