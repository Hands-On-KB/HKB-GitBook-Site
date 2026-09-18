---
description: The order in which PowerShell resolves a command name typed from user space.
---

# Windows Command Run Hierarchy

## Default Order of Precedence

When you type a bare command name, PowerShell resolves it in this order and stops at the first match:

* Aliases
  * Essentially like nicknames for commands. A good example is if you want to use "Get-ChildItem" you can also just type "gci."
* Functions
  * Functions you write directly in your PowerShell session will take precedence over predefined ones, like if you write a function named "Invoke-WebRequest" that actually connects to a C2 server it would run instead of the legitimate version of that cmdlet.
* Cmdlets
  * These are compiled commands, written as .NET classes and shipped in modules (Start-Process, Get-Content, Select-Object, etc.).
* Executables and Scripts
  * Windows attempts each extension in `$env:PATHEXT` for each directory in your `$env:PATH` variable.

There is no type checking involved in this. A function that shares a name with a cmdlet wins simply because functions sit higher in the list, and the cmdlet becomes unreachable unless you qualify it with its module name (`Microsoft.PowerShell.Utility\Invoke-WebRequest`) or call it with `&` and a full path. Where two imported modules export the same command, the most recently imported one wins.

{% hint style="info" %}
Doskey macros are a cmd.exe feature and do not take part in this resolution order at all. A doskey macro affects what cmd.exe does with a typed line; it has no effect inside a PowerShell session.
{% endhint %}

## Security Concerns

This can be abused by attackers to shadow a legitimate command with their own, so that a script or an operator running a routine command executes attacker code instead. Because aliases and functions sit above cmdlets and executables, anything that can write to a loaded profile or module can hijack a command name without touching the file on disk that the command normally resolves to.
