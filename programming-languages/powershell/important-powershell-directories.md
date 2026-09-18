---
description: >-
  Where the most important PowerShell files reside on Windows machines by
  default.
---

# Important PowerShell Directories

* User PowerShell: `C:\Users\<username>\Documents\WindowsPowerShell\`
  * This is where the user's personal PowerShell scripts, custom modules, and profile configuration script (`Microsoft.PowerShell_profile.ps1`) are stored.
  * Can also be written as `$HOME\Documents\WindowsPowerShell\`, or resolved with `$env:USERNAME` if you need the account name itself.
  * PowerShell 7+ uses `Documents\PowerShell\` instead, so a host with both installed has two separate user profile directories.
* System PowerShell: `C:\Windows\System32\WindowsPowerShell\v1.0\`
  * This contains the core engine files, built-in modules, and system-wide profile templates. Changes here require Administrator rights and affect every user on the machine.
  * The `v1.0` in the path is legacy naming and does not indicate the installed version. This is the directory `$PSHOME` points at for Windows PowerShell 5.1.
* User Command History: `%USERPROFILE%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt`
  * This plain text file automatically logs all commands users type in the console. It allows them to press the `Up Arrow` to recall previous commands, even across different sessions.
  * The AppData folder is hidden, so you won't be able to navigate to it normally using File Explorer.

### Security Concerns

These files can either be modified to take advantage of the user's privileges for various purposes (persistence, lateral movement, privilege escalation, etc.) or scraped for sensitive data such as secrets, passwords, and keys.

## PowerShell AutoRuns

Profiles are plain .ps1 scripts that run automatically when a PowerShell session starts. There are four, defined by two axes: which users they apply to, and whether they apply to every host or only one. "Host" here means the program hosting the engine — the console, the ISE, VS Code.

* `$PSHOME\Profile.ps1`
  * All users, all hosts. The broadest of the four. _(Requires Admin rights to edit.)_
* `$PSHOME\Microsoft.PowerShell_profile.ps1`
  * All users, but only the console host. _(Requires Admin rights to edit.)_
* `$Home\Documents\WindowsPowerShell\Profile.ps1`
  * Current user only, but runs in the console, the ISE, and VS Code.
* `$Home\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`
  * Current user, console host only. This is the one most commonly abused, since it needs no elevation and covers ordinary interactive use.

The legacy ISE has its own current-host profiles, following the same pattern:

* `$PSHOME\Microsoft.PowerShellISE_profile.ps1` — all users, ISE only.
* `$Home\Documents\WindowsPowerShell\Microsoft.PowerShellISE_profile.ps1` — current user, ISE only.

`$PROFILE | Format-List -Force` will print all four paths for the session you are in, which is quicker than reconstructing them by hand. Note that the files often do not exist until something creates them — an absent profile is not the same as a clean one.

### Security Concerns

If any of these profile files are writeable to the attacker, they can modify them to include malicious commands that run every time PowerShell is loaded. The per-user console profile is the usual target because it requires no administrative rights; the `$PSHOME` profiles require Admin but hit every account on the box.
