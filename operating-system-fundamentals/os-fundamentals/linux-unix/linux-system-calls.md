---
description: >-
  The Linux/Unix method for user-space applications to interact with the kernel.
  Very similar to Windows API functions.
icon: phone-arrow-right
---

# Linux System Calls

[https://syscall.sh/](https://syscall.sh/)

System Calls (Sys Calls) in Linux are used for interacting with the hardware and operating system by users and applications. There are hundreds of system calls and how they're called varies based on CPU architecture, but some examples are:

| **Category**           | **Typical Syscalls**                          | **Purpose**                                                                              |
| ---------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Process Control        | `fork`, `clone`, `execve`, `exit`, `wait4`    | Process spawning, thread creation, binary execution, termination.                        |
| File & IO Management   | `openat`, `read`, `write`, `close`, `lseek`   | Interacting with disk files, pipes, sockets, and hardware devices treated as files.      |
| Memory Management      | `brk`, `mmap`, `munmap`, `mprotect`           | Requesting virtual memory regions, page protection, and file mapping.                    |
| Networking & IPC       | `socket`, `bind`, `connect`, `epoll_wait`     | Inter-process message passing, network stream/datagram management, and IO event polling. |
| System & Hardware Info | `sysinfo`, `clock_gettime`, `getpid`, `uname` | Querying kernel state, high-precision timers, and identity metadata.                     |

For learing more about system calls being made on a live Linux machine, you can use the strace tool for tracing system calls: [https://explainshell.com/explain/1/strace](https://explainshell.com/explain/1/strace)
