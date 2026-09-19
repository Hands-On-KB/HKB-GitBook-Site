# User Enumeration

A validated username list is the input to password spraying and AS-REP roasting. It can be built from an SMB session, by walking RIDs, or — with no credential at all — from the way Kerberos distinguishes an unknown principal from a known one.

## Why It Works

The KDC returns a different error for a non-existent user than for a valid user with a wrong password. That difference makes the username space enumerable without a single authentication attempt.

## Prerequisites

* Kerberos (88/TCP) or SMB (445/TCP) reachable
* A candidate username wordlist for the Kerberos method

## Execution

**Enumerate Users**

* `nxc smb <dc_ip> --users` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
* `nxc smb <dc_ip> --rid-brute 10000 # bruteforcing RID` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * **→ Username**
* `net rpc group members 'Domain Users' -W '<domain> -l <ip> -U '%'`

**Bruteforce users**

* `kerbrute userenum -d <domain> <userlist>` [\[tool\]](https://github.com/ropnop/kerbrute)
* `nmap -p 88 --script=krb5-enum-users --script-args="krb5-enum-users.realm= '<domain>',userdb=<user_list_file>" <dc_ip>` [\[tool\]](https://github.com/nmap/nmap)
  * **→ Username**

{% hint style="info" %}
Kerberos username enumeration does not generate failed logon events in the same way a password attempt does, which makes it the quieter of the two approaches.
{% endhint %}

## Related

* [Password Spraying](file:///9685693/credential-access/password-spraying.md) — the usual next step
* [AS-REP Roasting](file:///9685693/credential-access/as-rep-roasting.md) — targets users without pre-auth
