---
description: https://en.wikipedia.org/wiki/Open_Shortest_Path_First
---

# Open Shortest Path First (OSPF)

An interior gateway protocol that runs directly over IP as protocol 89. Routers send Hello packets **to** the multicast address 224.0.0.5 (AllSPFRouters), sourced from their own interface address, advertising their availability to other routers to form OSPF neighbor relationships. 224.0.0.6 (AllDRouters) is used for traffic directed at the designated routers.

These various routers share topology info via Link State Advertisements (LSA) to adjacent and upstream neighbors, allowing all routers to have copies of the full routing topology built into databases. OSPFv2 optionally protects this with a cryptographic authentication mode: a keyed MD5 digest computed over each packet along with a sequence number, rather than a challenge/response exchange.

Attackers can take advantage of this by establishing a relationship with the designated router (DR) and the backup designated router (BDR) to get the routing topology info. The full neighbor state machine runs Down → Init → 2-Way → ExStart → Exchange → Loading → Full; the attacker's device steps through it as follows:

1. ExStart: Forming the OSPF adjacency/neighbor relationship with primary and secondary routers, and electing which side is master for the exchange.
2. Exchange: Routers trade Database Description (DBD) packets, which are their own OSPF packet type and carry only LSA headers — a summary of what each side holds, not the LSAs themselves.
3. Loading: Each side requests the LSAs it is missing and receives the full link state information (which the attacker can now poison to set up their man-in-the-middle attack or sniff traffic).
4. Full: The routers and the attacker device are fully synchronized.

### Tools Capable of this Attack:

1. Loki
2. John the Ripper: Can be used to crack the MD5 key after capturing authenticated OSPF packets off the wire.
