# Local Privilege Escalation

Landing a shell as a low-privileged user on a domain-joined host is often the middle of the chain, not the end. This page covers escaping AppLocker and UAC, finding local exploits, and the SeImpersonate-to-SYSTEM 'potato' family that service accounts are prone to.

## Why It Works

Service accounts commonly hold `SeImpersonatePrivilege`, which the potato techniques turn into SYSTEM. AppLocker and UAC are policy boundaries, not security ones, and both have well-known bypasses.

## Prerequisites

* Code execution as a low-privileged user on a domain host
* For potato techniques: an account holding SeImpersonatePrivilege

## Execution

**Bypass Applocker**

* Get-Applocker infos
  * `Get-ChildItem -Path HKLM:\SOFTWARE\Policies \Microsoft\Windows\SrpV2\Exe (dll/msi/...)` [\[tool\]](https://learn.microsoft.com/fr-fr/powershell/module/microsoft.powershell.management/get-childitem)
* files in writables paths
  * `C:\Windows\Temp`
  * `C:\Windows\Tasks`
* `installutil.exe /logfile= /LogToConsole=false /U C:\runme.exe` [\[tool\]](https://lolbas-project.github.io/lolbas/Binaries/Installutil/)
  * **→ Low access (without applocker)**
* `mshta.exe my.hta` [\[tool\]](https://lolbas-project.github.io/lolbas/Binaries/Mshta/)
* `MsBuild.exe pshell.xml` [\[tool\]](https://lolbas-project.github.io/lolbas/Binaries/Msbuild/)

**UAC bypass**

* `Fodhelper.exe`
* `wsreset.exe` [\[tool\]](https://lolbas-project.github.io/lolbas/Binaries/Wsreset/)
  * **→ Admin**
* `msdt.exe` [\[tool\]](https://lolbas-project.github.io/lolbas/Binaries/Msdt/)

**Auto Enum**

* `winPEASany_ofs.exe` [\[tool\]](https://github.com/peass-ng/PEASS-ng/blob/master/winPEAS/winPEASexe/README.md)
* `.\PrivescCheck.ps1; Invoke-PrivescCheck -Extended"`
  * **→ Admin**

**Search files**

* `findstr /si 'pass' *.txt *.xml *.docx *.ini` [\[tool\]](https://lolbas-project.github.io/lolbas/Binaries/Findstr/)
  * **→ User Account**

**Exploit**

* **→ SMBGhost CVE-2020-0796**
* **→ CVE-2021-36934 (HiveNightmare/SeriousSAM)**
  * `vssadmin list shadows` [\[tool\]](https://learn.microsoft.com/fr-fr/windows-server/administration/windows-commands/vssadmin)
    * **→ Admin**

**From Service account (SEImpersonate)**

* **→ RoguePatato**
* **→ GodPotato**
* **→ PrintSpoofer**
  * **→ Admin**
* RemotePotato0

{% hint style="info" %}
This overlaps the site's existing Privilege Escalation section — use that for OS-level enumeration detail, and this page for the domain-specific service-account paths.
{% endhint %}

## Related

* [Linpeas Winpeas](file:///privilege-escalation/linpeas-winpeas.md) — general local enumeration (existing section)
* [Credential Dumping](file:///9685693/host-access/credential-dumping.md) — what to collect once you are SYSTEM
