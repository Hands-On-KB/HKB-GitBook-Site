# ADCS Enumeration

Certipy and Certify enumerate the certificate services configuration and label the findings by ESC number. This is the map for everything else in the group — run it first and let the tool tell you which escalations are present.

## Why It Works

Certificate templates, CA settings and PKI object ACLs all live in the configuration partition of the directory, readable by any authenticated user. The vulnerable states (ESC1–ESC16) are just specific combinations of those readable attributes.

## Prerequisites

* Any valid domain credential
* LDAP reachable on a DC

## Execution

**Enumeration**

* `certutil -v -dsTemplate` [\[tool\]](https://learn.microsoft.com/fr-fr/windows-server/administration/windows-commands/certutil)
* `certify.exe find [ /vulnerable]` [\[tool\]](https://github.com/GhostPack/Certify)
* `certipy find -u <user>@<domain> -p <password> -dc-ip <dc_ip>` [\[tool\]](https://github.com/ly4k/Certipy)
  * **→ Web enrollement**
  * **→ Vulnerable template**
* `ldeep ldap -u <user> -p <password> -d <domain> -s <dc_ip> templates` [\[tool\]](https://github.com/franc-pentest/ldeep)
  * **→ Vulnerable CA**
* Get PKI objects information
  * `certify.exe pkiobjects` [\[tool\]](https://github.com/GhostPack/Certify)
    * **→ Misconfigured ACL**
    * **→ Vulnerable PKI Object AC**
* Display CA information
  * `certutil -TCAInfo` [\[tool\]](https://learn.microsoft.com/fr-fr/windows-server/administration/windows-commands/certutil)
  * `certify.exe cas` [\[tool\]](https://github.com/GhostPack/Certify)

**Enumerate ADCS**

* `certify.exe find` [\[tool\]](https://github.com/GhostPack/Certify)
* `certipy find -u <user>@<domain> -p '<password>' -dc-ip <dc_ip>` [\[tool\]](https://github.com/ly4k/Certipy)
  * **→ ADCS Exploitation**

## Related

* [Misconfigured Templates (ESC1–ESC4)](file:///9685693/adcs/misconfigured-templates-esc1-esc4.md) — the most common finding
* [Web Enrollment Relay (ESC8)](file:///9685693/adcs/web-enrollment-relay-esc8.md) — the relay path
