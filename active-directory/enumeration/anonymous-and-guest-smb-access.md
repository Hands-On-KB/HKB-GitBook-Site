# Anonymous and Guest SMB Access

Null sessions and enabled guest accounts still turn up regularly. Where they exist they give share listings, and sometimes readable file content, with no credential at all.

## Why It Works

A null session authenticates with an empty username and password; a guest fallback accepts any username with an empty password. Legacy shares and appliances often leave one or both open.

## Prerequisites

* SMB (445/TCP) reachable on the target
* No credentials required

## Execution

**Anonymous & Guest access on SMB shares**

* `nxc smb <ip_range> -u '' -p ''` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* `nxc smb <ip_range> -u 'a' -p ''` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* `enum4linux-ng.py -a -u '' -p '' <ip>` [\[tool\]](https://github.com/cddmp/enum4linux-ng)
* `smbclient -U '%' -L //<ip>` [\[tool\]](https://linux.die.net/man/1/smbclient)

{% hint style="info" %}
Readable shares are a common source of the first credential — scripts, unattended install files, and configuration backups left on open shares.
{% endhint %}

## Related

* [LDAP Enumeration](file:///9685693/enumeration/ldap-enumeration.md) — the equivalent unauthenticated check against LDAP
* [Password Spraying](file:///9685693/credential-access/password-spraying.md) — what to do with usernames you find
