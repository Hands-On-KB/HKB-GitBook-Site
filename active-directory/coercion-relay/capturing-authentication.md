# Capturing Authentication

Once traffic is pointed at you, a listener collects the authentication attempt. Responder is the standard choice; running it in analyse-only mode first lets you see what would be captured without answering anything.

## Why It Works

A Windows client authenticating to an SMB or HTTP server sends a NetNTLM response computed from the user's NT hash. Capturing the challenge and response gives crackable material even though the hash itself is never transmitted.

## Prerequisites

* A position in the authentication path — see Name Resolution Poisoning or Authentication Coercion
* No credentials required

## Execution

**Listen**

* `responder -l <interface> #use --lm to force downgrade` [\[tool\]](https://github.com/lgandx/Responder)
  * **→ Hash NTLMv1 or NTLMv2**
  * **→ Username**
* `smbclient.py` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/smbclient.py)
  * **→ Credentials (ldap/http)**

{% hint style="info" %}
Start with `responder -A` (analyse mode). It shows you which protocols are in use and which hosts are talking without injecting a single reply.
{% endhint %}

## Related

* [Name Resolution Poisoning](file:///9685693/coercion-relay/name-resolution-poisoning.md) — how to get into the path
* [Hash Cracking Reference](file:///9685693/credential-access/hash-cracking-reference.md) — mode 5600 for NetNTLMv2
