---
description: https://en.wikipedia.org/wiki/Hot_Standby_Router_Protocol
---

# Hot Standby Router Protocol (HSRP)

A Cisco proprietary first-hop redundancy protocol (FHRP). It does not exchange routes the way a routing protocol does — it builds redundancy for a "fault-tolerant default gateway" by having multiple routers with different priority levels that are maintained based on "HELLO" messages sent in plaintext over the local network.

This can be abused by attackers on the local network if they're able to insert themselves as the new active router, or if they're able to read the plaintext authentication string sent over the wire by HSRP in its multicast hello messages (the default is the string `cisco`).

1. Attacker sends a hello message to 224.0.0.2 on UDP port 1985 with a higher priority than the current active router.
2. Attacker changes their MAC address to 00:00:0c:07:ac:XX (with XX being the HSRP group ID) and uses the default gateway IP.
3. Attacker then becomes the central point for all traffic on the network, occasionally sending HSRP Hello (Active) messages to maintain their active status while forwarding intercepted traffic to the intended destinations.

{% hint style="info" %}
The above is HSRPv1. HSRPv2 uses the multicast address 224.0.0.102 and the virtual MAC range 00:00:0C:9F:Fx:xx, so check which version is in use before setting up the attack.

A higher priority only wins the election if preemption is enabled on the group. Where it is not, attackers typically claim priority 255 and wait for, or force, a re-election.
{% endhint %}

### Tools Capable of This Attack:

1. Yersinia (Press "g" to open "Protocol Mode" > select HSRP > find virtual IP to attack and press "x" to bring up the attack panel > select attack option)
   1. Has an HSRP mode that can be used for sending raw HSRP packets, becoming the active router, and becoming the active router while forwarding traffic intercepted.
2. Loki
