---
description: Core OS concepts that underpin everything else in this guide.
---

# Operating System Fundamentals

[Open the Operating System Fundamentals section →](https://handsonkb.gitbook.io/handsonkb-guide/operating-system-fundamentals)

## User Space vs. Kernel Space vs. Hardware

### User Space (Where Most of Us Work)

Where user actions within the OS, both via the Graphical User Interface (GUI) and command line/terminal, take place. Applications like text editors, video players, file system access, and web browsers are run in this space.

System calls (syscalls) and application programming interface (API) requests are used for entities in the user space to request resources and execute actions in the kernel space. This allows any security products to monitor for malicious actions done in user space trying to access the kernel space and blocking them based on that.

#### Things Running in User Space

Applications, File System Explorers, Settings and Configuration, Command Shells, Services, and the processes and threads those create.

Shared Object (.so) files for Linux and Dynamic Link Libraries (.dll) for Windows also live here. These are user-space code libraries that applications load at runtime, and they form the building blocks of most of what you do day to day. They are a common target for hijacking (see DLL hijacking and LD\_PRELOAD abuse), but they are not kernel code.

#### Data Used in User Space

Documents, spreadsheets, photos, source code, cached data (e.g., browser cookies, session tokens, saved passwords), secrets/keys for applications

### Kernel Space (Where the OS Works)

This is the core of the operating system where the lowest level functions and processes live and the highest level of privilege is granted. Actions executed in kernel space have full trust within the OS, and errors within the kernel cause a full system crash (a kernel panic on Linux, a bugcheck/BSOD on Windows).

Kernel code ships as loadable kernel modules: .ko files on Linux and .sys files on Windows. Malware made to explicitly target the OS kernel is known as a "rootkit."

#### Things Running in Kernel Space

CPU schedulers, memory managers, the file system and network stacks, device drivers (.sys for Windows and .ko for Linux), and interrupt handlers.

Processes and threads are created and scheduled by the kernel, but their own code runs in user space. A thread only enters kernel space for the duration of a syscall.

#### Data Used in Kernel Space

Process Tables, File Descriptors, Network Connection States, Interrupt Descriptor Tables (IDT)

### Hardware (The Physical Architecture)

Though the "Hardware Fundamentals" have a separate page, we do have to understand how the OS interacts with the hardware. This mainly comes down to firmware, which is the software directly interacting with the hardware devices. Firmware is the lowest level software running on your computer, and drivers are used to allow the OS kernel to interact with that firmware controlling those physical parts such as your storage drives, peripherals, and every other physical component of your computer (RAM, CPU, Wi-FI Antennae, Ethernet, etc.).

#### Things Operating in Hardware Space

Power Supply, CPU, RAM, UEFI/BIOS, Wired and Wireless Networking

#### Data Used in Hardware

Electrical Signals, CPU instruction sets (x86 and x86-64 are the instruction set architectures; x64 is Microsoft's name for x86-64), Memory Addresses
