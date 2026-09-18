---
description: >-
  Where the most important PowerShell files reside on Windows machines by
  default.
---

# Important PowerShell Directories

* User PowerShell: `C:\Users\$USER\Documents\WindowsPowerShell\`
  * This is where the user's personal PowerShell scripts, custom modules, and profile configuration script (`Microsoft.PowerShell_profile.ps1`) are stored.
  * Can also be written as `$HOME\Documents\WindowsPowerShell\`
* System PowerShell: `C:\Windows\System32\WindowsPowerShell\v1.0\`
  * This contains the core engine files, built-in modules, and system-wide profile templates. Changes here require Administrator rights and affect every user on the machine.
* User Command History: `%USERPROFILE%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`
  * This plain text file automatically logs all commands users type in the console. It allows them to press the `Up Arrow` to recall previous commands, even across different sessions.
  * The AppData folder is hidden, so you won't be able to navigate to it normally using File Explorer.

### Security Concerns

These files can either be modified to take advantage of the user's privileges for various purposes (persistence, lateral movement, privilege escalation, etc.) or scraped for sensitive data such as secrets, passwords, and keys.

## PowerShell AutoRuns

This mainly consists of PowerShell profiles for users and/or hosts either in the console or ISE. Anything within these files will be executed upon starting up a PowerShell session.

* $Home\Documents\WindowsPowerShell\Profile.ps1
  * Affects only the user, but runs in the Console, ISE, and VS Code.
* $PsHome\MicrosoftPowerShell\_profile.ps1
  * Affects only the current user.
* $PsHome\Profile.ps1
  * Applied globally to every single user and every console/editor on the system. _(Requires Admin rights to edit)._
* $Home\Documents\WindowsPowerShell\Microsoft\PowerShellISE\_profile.ps1
  * Affects only you when using the legacy Integrated Scripting Environment (ISE).
* $PsHome\Microsoft.PowerShellISE\_profile.ps1
  * Affects all users, but only when they run the legacy ISE editor.

### Security Concerns

If any of these profile files are writeable to the attacker, they can modify them to include malicious commands that run every time PowerShell is loaded.

