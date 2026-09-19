# SCCM Recon

System Center Configuration Manager (now MECM) administers software across the estate, which makes its servers unusually high-value. Recon locates the site server, management points and distribution points, and the accounts that tie them together.

## Why It Works

SCCM registers its infrastructure in the directory and in DNS so clients can find it. That same discoverability lets an attacker map the deployment and identify the accounts worth targeting.

## Prerequisites

* Any valid domain credential
* LDAP and SMB reachable

## Execution

**recon**

* `sccmhunter.py find -u <user> -p <password> -d <domain> -dc-ip <dc_ip> -debug` [\[tool\]](https://github.com/garrettfoster13/sccmhunter)
  * `sccmhunter.py show -all` [\[tool\]](https://github.com/garrettfoster13/sccmhunter)
* `ldeep ldap -u <user> -p <password> -d <domain> -s ldap://<dc_ip> sccm` [\[tool\]](https://github.com/franc-pentest/ldeep)
* `nxc smb <sccm_server> -u <user> -p <password> -d <domain> --shares` [\[tool\]](https://github.com/Pennyw0rth/NetExec)

**Enumerate SCCM**

* `sccmhunter.py find -u <user> -p <password> -d <domain> -dc-ip <dc_ip> -debug` [\[tool\]](https://github.com/garrettfoster13/sccmhunter)
* `ldeep ldap -u <user> -p <password> -d <domain> -s ldap://<dc_ip> sccm` [\[tool\]](https://github.com/franc-pentest/ldeep)
  * **→ SCCM Exploitation**
* `SharpSCCM.exe local site-info` [\[tool\]](https://github.com/Mayyhem/SharpSCCM)

## Related

* [SCCM Credential Harvesting](file:///9685693/sccm/sccm-credential-harvesting.md) — what the recon points you toward
* [SCCM Relay & Takeover](file:///9685693/sccm/sccm-relay-and-takeover.md) — the escalation paths
