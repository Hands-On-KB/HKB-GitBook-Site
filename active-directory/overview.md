# Overview

Active Directory is the identity backbone of most enterprise networks, and its attack surface spans every phase of an engagement. This section is organized as a progression: you begin on the left with no access and move rightward as you gain credentials, escalate privilege, take administrative control of hosts, and finally dominate the domain and cross its trust boundaries.

It is transcribed and expanded from the Orange Cyberdefense _AD mindmap v2025.03_, reorganised to match this site's phase-based structure and page template. See **The Mindmap** for the original diagram and full credits.

## How this maps to the rest of the site

Three areas overlap sibling sections on purpose, and cross-link rather than duplicate:

* **Host Access → Local Privilege Escalation** is the domain-specific companion to the existing **Privilege Escalation** section — use that section for OS-level enumeration detail.
* **Coercion & Relay** covers domain-authentication MitM; the existing **Man-in-the-Middle Attacks** group covers a broader scope, such as L2 network MitM.
* **Domain Dominance → Directory Persistence** is the AD-specific counterpart to the existing **Persistence** section.

## Notation

Placeholders in angle brackets — `<domain>`, `<dc_ip>`, `<user>`, `<password>` — are values you supply. A ⚠️ marks a technique the original authors flagged as unsafe or disruptive against production.

{% hint style="warning" %}
**Scope.** Everything here is for authorized assessment and defensive engineering only. Run these techniques exclusively against systems you have explicit, written permission to test.
{% endhint %}
