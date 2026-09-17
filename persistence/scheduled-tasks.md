# Scheduled Tasks

The Windows Task Scheduler lets any sufficiently privileged user configure a program to run automatically on a schedule or trigger (logon, boot, idle, specific time). Creating a scheduled task is a common and flexible Windows persistence technique.

## Why It Works

Task Scheduler is a legitimate, heavily used Windows subsystem, so a new task can easily blend in among the dozens already present from the OS and installed software. Tasks can be triggered by time, logon, or system events, and can run with SYSTEM privileges if created by an admin.

## Prerequisites

* Local admin (for SYSTEM-level tasks or tasks visible machine-wide) or standard user rights (for tasks under the current user's context)
* A payload path or command line to execute

## Execution

```
# Create a task that runs at logon
schtasks /create /tn "WindowsUpdateCheck" /tr "C:\Users\victim\update.exe" /sc onlogon

# Create a task that runs every 30 minutes as SYSTEM (requires admin)
schtasks /create /tn "SystemSync" /tr "C:\Windows\Temp\svc.exe" /sc minute /mo 30 /ru SYSTEM

# List / query tasks
schtasks /query /fo LIST /v
```

{% hint style="warning" %}
Detection & Mitigation: Task creation is logged as Security Event ID 4698 (a scheduled task was created) when auditing is enabled — enable it. Sysmon and EDR products also commonly flag new tasks pointing to binaries in user-writable or temp directories. Regularly review `schtasks /query` output for unfamiliar entries.
{% endhint %}

## Related

* Registry Run Keys — simpler but more heavily monitored alternative
* WMI Event Subscription — less commonly audited persistence mechanism
* DLL Hijacking — can be combined with a scheduled task that triggers the vulnerable loader
