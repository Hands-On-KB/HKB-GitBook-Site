# Linux Smart Enumeration (LSE)

LSE is a Linux privilege-escalation enumeration script that checks a system for common misconfigurations and escalation vectors, then color-codes findings by how likely they are to lead to privilege escalation.

## Common Use Cases

* Fast first-pass enumeration of a freshly obtained Linux shell
* Identifying SUID binaries, sudo misconfigurations, writable cron jobs, kernel exploits, and exposed credentials
* Prioritizing what to investigate first via its traffic-light output (red = high priority)

## Core Syntax

```
# Download and run directly
curl -L https://github.com/diego-treitos/linux-smart-enumeration/raw/master/lse.sh -o lse.sh
chmod 700 lse.sh
./lse.sh -l1     # level 1: quick, low-noise checks
./lse.sh -l2     # level 2: more thorough, more output
```

{% hint style="info" %}
Level flags (`-l0`, `-l1`, `-l2`) trade completeness for noise/runtime. Start at `-l1` on most engagements; only go to `-l2` if the first pass doesn't surface a path.
{% endhint %}

{% hint style="warning" %}
Pulling the script live via `curl`-then-execute touches disk/process logs and may trip EDR file-write or network-egress detections. Consider transferring it via an already-established channel instead of live-fetching on a monitored host.
{% endhint %}

## Related

* LinPEAS — similar goal, broader/more verbose Linux enumeration
* WinPEAS — Windows equivalent
