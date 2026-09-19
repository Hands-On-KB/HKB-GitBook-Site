# Entra Connect Credentials

Hybrid domains run a synchronisation server holding an `MSOL_` account. That account is granted directory replication rights so it can sync the whole directory to the cloud — which makes it equivalent to DCSync rights in practice.

## Why It Works

Synchronisation has to read every attribute of every object, including password hashes. The account that does it is a standard domain account with extraordinary rights, and its credential is stored recoverably on the sync server so the service can restart unattended.

## Prerequisites

* Any valid domain credential to locate the MSOL account
* Administrative access to the Entra Connect server to recover the credential

## Execution

**Intra ID Connect**

* Find MSOL
  * `nxc ldap <dc_ip> -u '<user>' -p '<password>' -M get-desc-users |grep -i MSOL` [\[tool\]](https://github.com/Pennyw0rth/NetExec)

## Related

* [DCSync Rights](file:///9685693/acl-abuse/dcsync-rights.md) — what the MSOL account can do
* [DPAPI Secrets](file:///9685693/host-access/dpapi-secrets.md) — how the stored credential is protected
