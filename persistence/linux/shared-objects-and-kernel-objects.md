---
description: Abusing Linux dynamically compiled libraries of code and how they're loaded.
---

# Shared Objects & Kernel Objects

## Shared Objects (.so)

These are loaded by the dynamic linker (`ld.so`) at process start, not by the kernel itself.

`LD_PRELOAD`

* Environment variable that forces the loader to load a specific .so file ahead of any other shared object, so its symbols win over the real ones.

`LD_LIBRARY_PATH`

* Environment variable that determines what directories the loader searches for shared object files.

`/etc/ld.so.preload`

* Global configuration file containing paths of shared objects to be loaded into every dynamically linked process on the host. Attackers can append the path to their .so file to this configuration. Unlike the environment variables above, this is system-wide and survives a new login session, but writing to it requires root.

{% hint style="warning" %}
`LD_PRELOAD` and `LD_LIBRARY_PATH` are ignored when the loader is in secure-execution mode, which includes any setuid or setgid binary. This is the case that matters most for privilege escalation, so do not expect either variable to carry across a setuid boundary on its own. `/etc/ld.so.preload` is still honoured there, which is part of why it is attractive to an attacker who already has root and wants to keep it.
{% endhint %}

## Kernel Objects (.ko)

`/lib/modules/$(uname -r)/`

* This is where attackers place their Kernel Object files in order to survive reboots.
* To get the module loaded automatically they add its name to `/etc/modules` or a file under `/etc/modules-load.d/`, which systemd-modules-load reads at boot.
* `depmod` is run to regenerate `modules.dep` and the map files so the module's dependencies resolve and `modprobe` can find it by name. It builds the dependency map — it does not itself bind a module to hardware.
* Autoloading on hardware detection is a separate mechanism: the kernel emits a modalias string for a device and udev matches it against the aliases in `modules.alias`, which a module declares through `MODULE_DEVICE_TABLE` at build time.
