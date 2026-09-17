---
description: >-
  YARA rules can search for text strings, hexadecimal byte sequences, regular
  expressions, and file-format properties within files to find potentially
  malicious artifacts.
---

# YARA Rules

YARA is a pattern-matching tool built for malware researchers and threat hunters to identify and classify files. Rules combine text strings, hex byte sequences, regular expressions, and file-format conditions into a single signature that can be run against files, memory, or live processes to flag matches.

## Common Use Cases

* Writing custom detection signatures for a specific malware family or campaign
* Scanning a disk, memory dump, or live endpoint for known-bad artifacts
* Hunting for variants of malware by matching on unique strings/byte patterns rather than a hash (which changes on every recompile)
* Classifying/triaging large batches of unknown samples in a malware analysis pipeline

## Core Syntax

```
rule Suspicious_PowerShell_Encoded
{
    meta:
        description = "Detects base64-encoded PowerShell commands"
        author = "your_name"
        date = "2026-01-01"

    strings:
        $enc1 = "-enc" nocase
        $enc2 = "-EncodedCommand" nocase
        $b64  = /[A-Za-z0-9+\/]{50,}={0,2}/

    condition:
        any of ($enc1, $enc2) and $b64
}
```

Running YARA:

```
# Scan a single file
yara rule.yar suspicious_file.exe

# Recursively scan a directory
yara -r rule.yar /path/to/scan/

# Scan a running process by PID
yara rule.yar 1234
```

{% hint style="info" %}
Rules can be combined into a single ruleset file and are widely reused across the industry — many vendors and researchers (e.g. Florian Roth's `signature-base`) publish open rule sets that are a good starting point before writing your own.
{% endhint %}

{% hint style="warning" %}
Overly broad string/byte patterns cause false positives at scale; overly narrow patterns (e.g. a single hardcoded string from one sample) miss variants entirely. Good rules balance specificity with resilience to minor code changes, and should be tested against a benign corpus before deploying broadly.
{% endhint %}

## Related

* [YARA documentation](https://yara.readthedocs.io/en/latest/index.html)
* Sigma rules — similar concept applied to log/EDR telemetry instead of files
