---
description: >-
  Ways to get past Microsoft's User Access Control (UAC) and Administrator
  Protection features.
---

# UAC Bypass & Admin Protection

| **Feature**       | **Traditional UAC**                                       | **Administrator Protection (Windows 11)**                                                          |
| ----------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Execution Context | Runs as the logged-in user with elevated token            | Runs under an isolated virtual/shadow admin account named the System Managed Admin Account (SMAA). |
| Token Lifespan    | Tied to user session / logon                              | Ephemeral; destroyed once the elevated process terminates                                          |
| Auto-Elevation    | Allowed for select signed Windows binaries                | Disabled; requires explicit interactive authorization                                              |
| Bypass Resistance | Susceptible to medium-to-high integrity bypass techniques | Hardened via account/profile isolation and JIT tokens                                              |

### Automated Multi-UAC Bypass

{% embed url="https://github.com/x0xr00t/Automated-Multi-UAC-Bypass" %}

### **UAC-BOF-Bonanza**

{% embed url="https://github.com/icyguider/UAC-BOF-Bonanza" %}

{% embed url="https://projectzero.google/2026/26/windows-administrator-protection.html" %}
