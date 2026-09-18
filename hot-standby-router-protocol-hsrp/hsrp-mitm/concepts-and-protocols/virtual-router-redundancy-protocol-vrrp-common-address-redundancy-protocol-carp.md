---
description: >-
  https://en.wikipedia.org/wiki/Virtual_Router_Redundancy_Protocol
  https://en.wikipedia.org/wiki/Common_Address_Redundancy_Protocol
---

# Virtual Router Redundancy Protocol (VRRP)/Common Address Redundancy Protocol (CARP)

First-hop redundancy protocols similar to HSRP, created to open up gateway redundancy to vendors outside of Cisco's product ecosystem. Like HSRP, they elect which device answers for a shared virtual IP; they do not distribute routes.

## VRRP

Virtual MAC Address: 00:00:5e:00:01:XX (XX being the VRRP group ID). VRRPv3 for IPv6 uses 00:00:5e:00:02:XX instead.

Multicast Group: 224.0.0.18

IP Protocol 112

Determines priority using an 8-bit "Priority" field with the largest value becoming the "master" router. Priority 255 is reserved for the address owner and the default is 100. The current standards (RFC 3768 for VRRPv2 and RFC 5798 for VRRPv3) provide no authentication or integrity checks — the authentication types in the original RFC 2338 were deprecated as ineffective.

## CARP

OpenBSD's royalty-free alternative, created in response to the patent situation around VRRP. It deliberately uses the same virtual MAC range as VRRP and the same IP protocol number 112, which is why the two cannot coexist cleanly on one segment.

Advertisements are authenticated with an HMAC-SHA1 over a shared passphrase, so the passphrase itself is not recoverable from the wire the way an HSRPv1 plaintext string is. The advertisements are not encrypted, though, so group membership and timing are still observable.
