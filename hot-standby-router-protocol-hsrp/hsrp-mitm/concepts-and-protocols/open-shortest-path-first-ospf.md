---
description: https://en.wikipedia.org/wiki/Open_Shortest_Path_First
---

# Open Shortest Path First (OSPF)

An interior gateway protocol that sends multicast packets on the local network from 224.0.0.5 advertising its availability to other routers to form OSPF neighbor relationships.&#x20;

These various routers share topology info via Link State Advertisements (LSA) to adjacent and upstream neighbors, allowing all routers to have copies of the full routing topology built into databases. This sometimes includes an MD5 challenge authentication/response setup.

Attackers can take advantage of this by establishing a relationship with the designated router (DR) and the backup designated router (BDR) to get the routing topology info. the attacker will step through the OSPF state tree in the following steps:

1. ExStart: Forming the OSPF adjacency/neighbor relationship with primary and secondary routers.
2. Exchange: Routers share database descriptor (DBD) information in LSA packets.
3. Loading: OSPF routers share link state information (which the attacker can now poison to set up their man-in-the-middle attack or sniff traffic).
4. Full: The routers and the attacker device are fully synchronized.

### Tools Capable of this Attack:

1. Loki:&#x20;
2. John the Ripper: Can be used to crack the MD5 hash after getting it from sniffing traffic.
