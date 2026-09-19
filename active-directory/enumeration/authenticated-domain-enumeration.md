# Authenticated Domain Enumeration

Any valid domain account, however unprivileged, can read most of the directory. This is the single highest-value step in a domain assessment: it produces the user and computer inventory, the group graph, delegation settings, ACLs, and share listings that every later technique depends on.

## Why It Works

Active Directory is designed to be readable by authenticated users so that domain members can resolve names, find services, and apply policy. Read access is not a misconfiguration — it is the default, and it exposes far more than most administrators expect.

## Prerequisites

* Any valid domain credential: cleartext password, NT hash, or Kerberos ticket
* LDAP and SMB reachable on a DC

## Execution

**Classic Enumeration (users, shares, ACL, delegation, ...)**

* Find all users
  * `GetADUsers.py -all -dc-ip <dc_ip> <domain>/<username>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/GetADUsers.py)
    * **→ Username**
  * `nxc smb <dc_ip> -u '<user>' -p '<password>' --users` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* Enumerate SMB share
  * `nxc smb <ip_range> -u '<user>' -p '<password>' -M spider_plus` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * `nxc smb <ip_range> -u '<user>' -p '<password>' --shares [--get-file \\<filename> <filename>]` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
    * **→ Scroll shares**
  * `manspider <ip_range> -c passw -e <file extensions> -d <domain> -u <user> -p <password>` [\[tool\]](https://github.com/blacklanternsecurity/MANSPIDER)
* Bloodhound Legacy
  * `bloodhound-python -d <domain> -u <user> -p <password> -gc <dc> -c all` [\[tool\]](https://github.com/dirkjanm/BloodHound.py)
  * `rusthound -d <domain_to_enum> -u '<user>@<domain>' -p '<password>' -o <outfile.zip> -z` [\[tool\]](https://github.com/NH-RED-TEAM/RustHound)
    * **→ ACL**
    * **→ Delegation**
  * `import-module sharphound.ps1;invoke-bloodhound -collectionmethod all -domain <domain>`
    * **→ Username**
  * `sharphound.exe -c all -d <domain>` [\[tool\]](https://github.com/SpecterOps/SharpHound)
* Bloodhound CE
  * `bloodhound-python -d <domain> -u <user> -p <password> -gc <dc> -c all` [\[tool\]](https://github.com/dirkjanm/BloodHound.py)
  * `rusthound-ce -d <domain_to_enum> -u '<user>@<domain>' -p '<password>' -o <outfile.zip> -z --ldap-filter=(objectGuid=*)` [\[tool\]](https://github.com/g0h4n/RustHound-CE)
    * **→ ACL**
    * **→ Delegation**
  * `sharphound.exe -c all -d <domain>` [\[tool\]](https://github.com/SpecterOps/SharpHound)
  * `SOAPHound.exe -c c:\temp\cache.txt --bhdump -o c:\temp\bloodhound-output --autosplit --threshold 900` [\[tool\]](https://github.com/FalconForceTeam/SOAPHound)
    * **→ Username**
* Enumerate Ldap
  * `ldeep ldap -u <users> -p '<password>' -d <domain> -s ldap://<dc_ip> all <backup_folder>` [\[tool\]](https://github.com/franc-pentest/ldeep)
    * **→ ACL**
  * `ldapdomaindump.py -u <user> -p <password> -o <dump_folder> ldap://<dc_ip>:389` [\[tool\]](https://github.com/dirkjanm/ldapdomaindump)
    * **→ Delegation**
  * `ldapsearch-ad.py -l <dc_ip> -d <domain> -u <user> -p '<password>' -o <output.log> -t all` [\[tool\]](https://github.com/yaap7/ldapsearch-ad)
    * **→ Username**
* Enumerate DNS
  * `adidnsdump -u <domain>\\<user> -p "<password>" --print-zones <dc_ip>` [\[tool\]](https://github.com/dirkjanm/adidnsdump)
    * **→ New targets (low hanging fruit)**

{% hint style="info" %}
Collect once, analyse offline. A single BloodHound collection gives you the material for the ACL, delegation, and lateral movement sections without re-querying the DC.
{% endhint %}

## Related

* [BloodHound & Automated Audit](file:///9685693/enumeration/bloodhound-and-automated-audit.md) — automated analysis of the same data
* [DCSync Rights](file:///9685693/acl-abuse/dcsync-rights.md) — one of the key findings to look for
