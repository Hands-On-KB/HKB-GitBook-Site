# Hash Cracking reference

A quick lookup from hash format to cracking mode, keyed by the leading characters of the hash string. Every format here is produced by a technique elsewhere in this section.

## Why It Works

Getting the mode wrong wastes GPU time on a hash that will never match. The prefix of the hash string identifies the type unambiguously.

## Prerequisites

* A captured or extracted hash
* A wordlist — `rockyou.txt` is the usual starting point

## Execution

**LM (299bd128c1101fd6)**

* `john --format=lm hash.txt --wordlist=<rockyou.txt>` [\[tool\]](https://github.com/openwall/john)
* `hashcat -m 3000 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)

**NT (b4b9b02e6f09a9bd760...)**

* `john --format=nt hash.txt --wordlist=<rockyou.txt>` [\[tool\]](https://github.com/openwall/john)
* `hashcat -m 1000 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)

**NTLMv1 (user::85D5BC...)**

* `john --format=netntlm hash.txt --wordlist=<rockyou.txt>` [\[tool\]](https://github.com/openwall/john)
* `hashcat -m 1000 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)
* crack.sh

**NTLMv2 (user::N46iSNek...)**

* `john --format=netntlmv2 hash.txt --wordlist=<rockyou.txt>` [\[tool\]](https://github.com/openwall/john)
* `hashcat -m 5600 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)

**Kerberos 5 TGS ($krb5tgs$23$...)**

* `john --format=krb5tgs hash.txt --wordlist=<rockyou.txt>` [\[tool\]](https://github.com/openwall/john)
* `hashcat -m 13100 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)

**Kerberos 5 TGS AES128 ($krb5tgs$17...)**

* `hashcat -m 19600 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)

**Kerberos ASREP ($krb5asrep$23...)**

* `hashcat -m 18200 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)

**MSCache 2 (very slow) ($DCC2$10240...)**

* `hashcat -m 2100 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)

**Timeroast hash ($sntp-ms$...)**

* `hashcat -m 31300 -a 3 hash.txt -w 3 ?l?l?l?l?l?l?l` [\[tool\]](https://hashcat.net/hashcat/)

**pxe hash ($sccm$aes128$...)**

* `hashcat -m 19850 -a 0 hash.txt <rockyou.txt>` [\[tool\]](https://hashcat.net/hashcat/)

{% hint style="info" %}
MSCache 2 (`$DCC2$`) is deliberately slow by design — treat it as a last resort rather than a primary cracking target.
{% endhint %}

## Related

* [Kerberoasting](file:///9685693/credential-access/kerberoasting.md) — produces `$krb5tgs$` hashes
* [AS-REP Roasting](file:///9685693/credential-access/as-rep-roasting.md) — produces `$krb5asrep$` hashes
