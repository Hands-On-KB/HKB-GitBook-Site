---
description: >-
  https://en.wikipedia.org/wiki/Virtual_Router_Redundancy_Protocol
  https://en.wikipedia.org/wiki/Common_Address_Redundancy_Protocol
---

# Virtual Router Redundancy Protocol (VRRP)/Common Address Redundancy Protocol (CARP)

Protocols similar to HSRP created to open up network redundancy routing to others outside of Cisco's product ecosystem.

## VRRP

Virtual MAC Address: 00:00:5e:00:01:XX (XX being the VRRP group ID)

Multicast Group: 224.0.0.18

Protocol 112

Determines priority using an 8-bit "Priority" field with the largest value being the "network master." Provides no authentication or integrity checks.

## CARP

Uses the same MAC range as VRRP as well as using IP protocol 112, but functions more as redundant ARP instead of strictly a routing protocol like VRRP.

Passwords sent over the wire are protected by Hash-Based Message Authentication Code (HMAC) but not all CARP info is encrypted.

