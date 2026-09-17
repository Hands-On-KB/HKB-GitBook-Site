# LaZagne

LaZagne is an open-source credential-recovery tool that dumps stored passwords from dozens of local applications (browsers, mail clients, Wi-Fi, password managers, databases, etc.) by reading the same local storage/registry locations and decryption routines those applications use themselves.

## Common Use Cases

* Post-exploitation password harvesting on Windows, Linux, and macOS
* Recovering plaintext credentials cached by browsers, WiFi profiles, git credential managers, etc.
* Identifying credential reuse across systems

## Core Syntax

```
# Run all modules
lazagne.exe all

# Target a specific category
lazagne.exe browsers
lazagne.exe wifi

# Output to a file
lazagne.exe all -oN -output C:\Temp\
```

{% hint style="warning" %}
LaZagne is widely signatured by AV/EDR because it directly targets known credential-storage locations (browser SQLite DBs, DPAPI blobs, etc.). Expect to need to compile from source or obfuscate it to avoid immediate detection on a monitored host.
{% endhint %}

## Related

* Mimikatz-style credential dumping techniques
* LinPEAS / WinPEAS — broader enumeration that often surfaces the same cached credentials
