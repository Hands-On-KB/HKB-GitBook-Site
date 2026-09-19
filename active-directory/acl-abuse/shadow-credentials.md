# Shadow Credentials

Write access to a target's `msDS-KeyCredentialLink` lets you add your own key credential to the account. You can then authenticate as that account using PKINIT and recover its NT hash — no password reset, no template needed.

## Why It Works

Key-trust authentication lets an account log in with a key pair instead of a password. The public key lives in a directory attribute, so anyone who can write that attribute can enrol a credential they control.

## Prerequisites

* Write access to the target's `msDS-KeyCredentialLink` attribute
* A domain with ADCS or key-trust configured (Windows 2016+ functional level)

## Execution

**can change msDS-KeyCredentialLInk (Generic Write) + ADCS**

* Shadow Credentials
  * `certipy shadow auto '-u <user>@<domain>' -p <password> -account '<target_account>'` [\[tool\]](https://github.com/ly4k/Certipy)
  * `pywhisker.py -d "FQDN_DOMAIN" -u "user1" -p "CERTIFICATE_PASSWORD" --target "TARGET_SAMNAME" --action "list"` [\[tool\]](https://github.com/ShutdownRepo/pywhisker)
    * **→ PassTheCertificate**

## Related

* [Rights on Computers & OUs](file:///9685693/acl-abuse/rights-on-computers-and-ous.md) — GenericWrite on a computer is the usual source
* [Certificate Mapping Abuse (ESC9–ESC10)](file:///9685693/adcs/certificate-mapping-abuse-esc9-esc10.md) — the certificate mapping it relies on
