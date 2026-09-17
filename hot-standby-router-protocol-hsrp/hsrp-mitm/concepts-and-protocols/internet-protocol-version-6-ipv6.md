---
description: https://en.wikipedia.org/wiki/IPv6
---

# Internet Protocol Version 6 (IPv6)

[https://github.com/vanhauser-thc/thc-ipv6](https://github.com/vanhauser-thc/thc-ipv6)

## Neighbor Impersonation MitM

Neighbor Discovery (ND) replaces ARP for IPv6, and Neighbor Impersonation works similarly to ARP Spoofing. Instead of ARP requests and responses, IPv6 has Neighbor Solicitations (NS) and Neighbor Advertisements (NA).

1. Victim sends Neighbor Solicitation (NS) message on the multicast address trying to map an IPv6 address to a MAC address.
2. Legitimate device on the network sees the Neighbor Solicitation from the victim and sends a Neighbor Advertisement in response.
3. Attacker sends a Neighbor Advertisement (NA) message in response to the victim with the Override flag set impersonating the legitimate device on the network that the victim was looking for. This takes the place of the legitimate response in the victim's neighbor mapping for the network.

## Router Advertisement MitM

Nodes on an IPv6 network use Router Solicitation (RS) messages to determine where local routers are, and the routers send Router Advertisement (RA) messages in response with configuration information for the nodes. Similar to HSRP or VRRP MitM attacks, the attacker is able to claim to be a router with a higher preference to insert themselves between the victim nodes and the legitimate router.

1. Nodes send ICMPv6 RS messages looking for routers on the local network using the anycast class of IPv6 addresses.
2. Routers receiving those messages will send an ICMPv6 RA message to the multicast address class that reaches all nodes.
3. The attacker sends their own RA message to that same multicast address reaching all nodes, and claims their device has the highest preference in order to take over all IPv6 traffic from the victim and forward it to the real router while sniffing.

## Linux IPv6 Traffic Forwarding

In order to forward traffic to the real router so as to not raise an alarm on the network from dropped traffic, attackers would need to do the following:

1. `sysctl -w net.ipv6.conf.all.forwarding=1`
2. `ip route add default via <Real IPv6 Gateway/Router> dev <NIC>`
   1. Or use the [Linux RADVD](https://radvd.litech.org/) software to configure IPv6 routing in the /etc/radvd.conf file.
