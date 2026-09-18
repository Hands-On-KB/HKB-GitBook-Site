---
description: >-
  The order in which the Windows operating systems establish priority of
  commands run from user space.
---

# Windows Command Run Hierarchy

## Default Order of Precedence

* Doskey Aliases (Prior to Windows 10's PSConsoleHostReadline)
  * These were command aliases run in Windows' older DOS terminal and later used in the cmd.exe console.
* Aliases
  * Essentially like nicknames for commands. A good example is if you want to use "Get-ChildItem" you can also just type "gci."
* Functions
  * Functions you write directly in your PowerShell session will take precedence over predefined ones, like if you write a function named "Invoke-WebRequest" that actually connects to a C2 server it would run instead of the legitimate version of that cmdlet.
* Cmdlets
  * These are the built in .NET functions in PowerShell (Start-Process, Get-Content, Select-Object, etc.). If you try to load your own versions over them, PowerShell will check the object-type before choosing which version to run.
* Executables
  * Windows attempts each $PATHEXT for each directory in your $PATH variable.

## Security Concerns

This can be abused by attackers to run commands with a higher execution priority in order to beat out legitimate script or command execution on the Windows workstation or server.
