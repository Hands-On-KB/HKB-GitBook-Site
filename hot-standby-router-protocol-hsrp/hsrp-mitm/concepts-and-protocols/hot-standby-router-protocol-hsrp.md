---
description: https://en.wikipedia.org/wiki/Hot_Standby_Router_Protocol
---

# Hot Standby Router Protocol (HSRP)

A Cisco proprietary routing protocol that builds in redundancies for a "fault-tolerant default gateway" by having multiple routers with different priority levels that are maintained based on "HELLO" messages sent in plaintext over the local network.

This can be abused by attackers on the local network if they're able to insert themselves as the new primary router, or if they're able to read the plaintext credentials sent over the wire by HSRP in its multi-cast hello messages (by default).

1. Attacker sends hello message to 224.0.0.2 with a higher priority than the current primary router.
2. Attacker changes their MAC address to 00:00:0c:07:ac:XX (with XX being the HSRP group ID) and uses the default gateway IP.
3. Attacker then becomes the central point for all traffic on the network, occasionally sending HSRP Hello (Active) messages to maintain their primary status while forwarding intercepted traffic to the intended destinations.

### Tools Capable of This Attack:

1. Yersinia (Press "g" to open "Protocol Mode" > select HSRP > find virtual IP to attack and press "x" to bring up the attack panel > select attack option)
   1. Has an HSRP mode that can be used for sending raw HSRP packets, becoming the primary router, and becoming the primary router while forwarding traffic intercepted.
2. Loki
