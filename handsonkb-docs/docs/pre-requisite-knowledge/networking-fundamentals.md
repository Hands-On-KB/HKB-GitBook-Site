---
description: >-
  Core networking concepts referenced throughout the offensive and defensive
  sections.
---

# Networking Fundamentals

[Open the Networking Fundamentals section →](https://handsonkb.gitbook.io/handsonkb-guide/networking-fundamentals)

## Ethernet

Ethernet is the dominant wired local area network (LAN) technology, and it is still in heavy use today alongside Wi-Fi rather than a relic that preceded it. A twisted-pair cable runs from a host's network interface to a switch, and the switch uplinks toward the router or modem that reaches the wider network.

Cabling is graded by category. CAT-3 carried early 10BASE-T, CAT-4 was a short-lived grade used mainly for 16 Mbps Token Ring and saw almost no Ethernet deployment, and CAT-5/5e/6/6a are what actually carry modern Gigabit and multi-gigabit Ethernet.

## TCP and UDP

[Transmission Control Protocol (TCP)](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) and [User Datagram Protocol (UDP)](https://en.wikipedia.org/wiki/User_Datagram_Protocol) are the core of the Internet as we know it.

## ICMP

[Internet Control Message Protocol (ICMP)](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol) is what's used when you think of checking if a computer is up and reachable by "pinging" it.

## HTTP & HTTPS

HTTP is the application protocol behind what we think of as "being online" when visiting websites and using mobile apps. It is not deprecated — HTTPS _is_ HTTP, carried inside a TLS-encrypted connection. What has been deprecated in practice is sending HTTP over an unencrypted channel, which browsers now warn on and most sites refuse outright.
