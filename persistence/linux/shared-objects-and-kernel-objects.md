---
description: Abusing Linux dynamically compiled libraries of code and how they're loaded.
---

# Shared Objects & Kernel Objects

## Shared Objects (.so)

LD\_PRELOAD

* Environment variable that forces the OS to load a specific .so file before any other code library/shared objects.

LD\_LIBRARY\_PATH

* Environment variable that determines what directories the loader looks in for shared object files.

etc/ld.so.preload

* Global configuration that contains the paths of shared objects to be loaded. Attackers can append the path to their .so file to this configuration.

## Kernel Objects (.ko)

/lib/modules/$(uname -r)/

* This is where attackers add their Kernel Object files in order to survive reboots.
* They also add the module name to `/etc/modules`, `/etc/modules-load.d/`, or use `depmod` to map it to specific hardware triggers.
