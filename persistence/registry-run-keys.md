# Registry Run Keys

Registry Run Keys are one of the oldest and most common Windows persistence techniques: adding a value under a `Run` key causes Windows to automatically execute the referenced program every time the target user logs in (or on every boot, for the machine-wide keys).

## Why It Works

Windows checks a well-known, fixed set of registry locations on every logon/boot and executes whatever is listed there — no additional configuration needed. Any account with write access to `HKCU` (always available to the logged-in user) or admin access to `HKLM` can add an entry.

## Prerequisites

* Write access to the registry (HKCU requires no special privileges; HKLM requires admin)
* A payload path (executable, script, or living-off-the-land binary command line)

## Execution

```
# Per-user (no admin required), runs at that user's logon
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v Updater /t REG_SZ /d "C:\Users\victim\update.exe"

# Machine-wide (requires admin), runs for any user at boot
reg add "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v Updater /t REG_SZ /d "C:\Windows\update.exe"

# View existing entries
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
```

{% hint style="warning" %}
Detection & Mitigation: These keys are extremely well-monitored — Sysmon Event ID 13 (Registry value set) on `...\Run` and `...\RunOnce` paths, and most EDR products alert on writes here by default. Autoruns (Sysinternals) is the standard tool for auditing all auto-start locations, not just Run keys.
{% endhint %}

## Related

* Scheduled Tasks — less commonly monitored alternative
* WMI Event Subscription — a stealthier, less commonly checked persistence mechanism
