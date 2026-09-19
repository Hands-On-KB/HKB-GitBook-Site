# Remote Execution

With a working password and the rights to use it, this is the toolkit for running commands on another host: PsExec-style services, WMI, WinRM, DCOM, and interactive RDP. They differ mainly in what they leave behind and how monitored each is.

## Why It Works

These are the same remote-administration protocols administrators use daily. That is their strength for an attacker — the traffic is expected — and the reason the choice between them is a detectability decision as much as a technical one.

## Prerequisites

* A valid credential with local admin (or the specific right) on the target
* The relevant service reachable: 445, 5985, 135, 3389

## Execution

**Clear text Password**

* Interactive-shell - psexec
  * `psexec.py <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/psexec.py)
  * `psexec.exe -AcceptEULA \\<ip>` [\[tool\]](https://learn.microsoft.com/fr-fr/sysinternals/downloads/psexec)
    * **→ Authority/System**
  * `psexecsvc.py <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/sensepost/susinternals)
* Pseudo-shell (file write and read)
  * `atexec.py <domain>/<user>:<password>@<ip> "command"` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/atexec.py)
  * `smbexec.py <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/smbexec.py)
  * `wmiexec.py <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/wmiexec.py)
  * `dcomexec.py <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/dcomexec.py)
  * `nxc smb <ip_range> -u <user> -p <password> -d <domain> -x <cmd>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* WinRM
  * `evil-winrm -i <ip> -u <user> -p <password>` [\[tool\]](https://github.com/Hackplayers/evil-winrm)
  * `Enter-PSSession -ComputerName <computer> -Credential <domain>\<user>` [\[tool\]](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enter-pssession)
    * **→ Low access**
      * **→ Admin**
  * `nxc winrm <ip_range> -u <user> -p <password> -d <domain> -x <cmd>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
    * **→ Admin**
* RDP
  * `xfreerdp /u:<user> /d:<domain> /p:<password> /v:<ip>` [\[tool\]](https://linux.die.net/man/1/xfreerdp)
    * **→ Low access**
    * **→ Admin**
* SMB
  * `smbclient.py <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/smbclient.py)
  * `smbclient-ng.py -d <domain> -u <user> -p <password> --host <ip>` [\[tool\]](https://github.com/p0dalirius/smbclient-ng)
    * **→ Search files**
* MSSQL
  * `nxc mssql <ip_range> -u <user> -p <password>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * `mssqlclient.py -windows-auth <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/mssqlclient.py)
    * **→ MSSQL**

## Related

* [Pass-the-Hash & OverPass-the-Hash](file:///9685693/lateral-movement/pass-the-hash.md) — the same execution without the password
* [MSSQL Lateral Movement](file:///9685693/lateral-movement/mssql-lateral-movement.md) — movement through database links
