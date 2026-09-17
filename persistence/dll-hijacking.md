# DLL Hijacking

DLL hijacking (a.k.a. DLL search order hijacking) abuses the order in which Windows searches for a DLL a program needs. If an attacker can place a malicious DLL somewhere earlier in that search path than the legitimate one, the target application loads and executes the attacker's code instead — often with the same privileges as the hijacked process.

## Why It Works

Many applications load DLLs by name only (not full path), and Windows' default search order checks the application's own directory before some system directories. If a writable folder in that search order is checked before the real DLL's location, or the app is missing an expected DLL entirely, an attacker-supplied DLL with the right name and exports gets loaded instead.

## Prerequisites

* Write access to a directory in the target process's DLL search order (its own folder, or a folder listed in `PATH` ahead of `System32`)
* A vulnerable/misconfigured application or service that loads a DLL insecurely
* Knowledge of the expected DLL name and exported function signatures, so the malicious DLL doesn't crash the host process

## Execution

```
# 1. Identify DLLs an application loads and from where (Process Monitor / Procmon)
#    Filter on: Process Name = target.exe, Result = NAME NOT FOUND, Path ends with .dll

# 2. Confirm you have write access to a directory earlier in the search order
icacls "C:\Program Files\VulnApp"

# 3. Build a malicious DLL exporting the same function names as the real one,
#    with a payload in DllMain, then drop it in that writable directory

# 4. Trigger the target application/service to load it
```

{% hint style="warning" %}
Detection & Mitigation: Enable Safe DLL Search Mode (default on modern Windows) and use fully-qualified paths or `LoadLibraryEx` with `LOAD_LIBRARY_SEARCH_SYSTEM32` in your own applications. Defenders should monitor for DLL loads from unusual paths (user-writable directories, temp folders) via Sysmon Event ID 7 (Image Loaded) and alert on unsigned DLLs loaded by signed system binaries.
{% endhint %}

## Related

* Scheduled Tasks — another common persistence mechanism to repeatedly trigger a hijacked DLL
* Registry Run Keys — alternative persistence via auto-run
