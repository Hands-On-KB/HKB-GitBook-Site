---
description: >-
  The bare minimum that needs to be understood in regards to computer networking
  in order to functionally perform a role in cybersecurity.
icon: circle-wifi
---

# Networking Fundamentals

| **Network Type** | **Primary Scope**                                              | **Core Transport / Infrastructure**          | **Key Focus**                                                      |
| ---------------- | -------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------------------------------ |
| PAN              | \~1 to 10 meters (Up to 100ft for certain Bluetooth standards) | Bluetooth, BLE, UWB, NFC                     | Connecting personal accessories and wearables around one user      |
| LAN              | Up to \~1 km                                                   | Cat6a/Cat8 copper, internal Wi-Fi            | High-speed, low-latency device sharing within a building or office |
| MAN              | \~5 to 50 km                                                   | Optical rings (DWDM), Metro Ethernet         | Linking city-wide municipal, campus, or corporate branches         |
| WAN              | Multi-city to global                                           | Leased circuits, MPLS, SD-WAN, subsea fiber  | Connecting geographically distant networks across regions          |
| Global Internet  | Worldwide                                                      | Tier 1/2/3 ISPs, BGP, IXPs, DNS root servers | Public, decentralized inter-network of all autonomous systems      |
| Cloud Network    | Logical / Global                                               | Overlay SDN, VXLAN, VPCs, transit gateways   | Scalable, software-defined infrastructure for virtual workloads    |

## Personal Area Networks (PAN)

A computer network organized around an individual person, typically covering an operating range of less than 10 meters (approximately 33 feet). It interconnects personal consumer devices—such as smartphones, laptops, smartwatches, wireless headphones, and tablets—to allow data exchange and peripheral control without relying on a wide-area infrastructure.

#### Core Characteristics

* Geographical Range: Confined to the personal workspace or immediate vicinity of an individual (usually within arm’s reach up to 10 meters).
* Low Power Consumption: Designed to conserve battery life on wearable and handheld devices, prioritizing power efficiency over high throughput.
* Low-to-Moderate Data Rates: Varies from small telemetry packets (kilobits per second for heart rate monitors) to a few megabits per second (streaming audio or transferring photos).
* Ad-Hoc Topology: Often established dynamically on demand without a fixed router, forming point-to-point links or star topologies centered on a primary controller (e.g., a phone acting as the host).

### Near Field Communication (NFC)

A short-range wireless connectivity standard that enables two electronic devices to establish communication by bringing them within approximately 4 centimeters (1.5 inches) of each other. Built as an extension of Radio Frequency Identification (RFID), it combines the interface of a smart card and a reader into a single device to support contactless transactions, digital identification, and instant peer-to-peer data exchange.

This is the technology used when you "tap to pay" with your credit card at retail businesses.

#### Core Characteristics

* Ultra-Short Range: Operates within a physical boundary of less than 4 cm. This physical proximity constraint serves as an inherent security barrier against eavesdropping.
* Operating Frequency: Operates on the globally available, unlicensed ISM radio frequency of 13.56 MHz.
* Data Transmission Rates: Standardized speeds are modest—106 kbps, 212 kbps, or 424 kbps—prioritizing low power and rapid session setup over high-bandwidth payload delivery.
* Near-Instant Setup: Connects in under 0.1 seconds automatically on proximity, eliminating manual device discovery, PIN entry, or pairing sequences.
* Inductive Coupling: Relies on electromagnetic induction between two loop antennas. An active device generates a radio frequency field that can power a passive target via magnetic induction.

### Bluetooth

An open, short-range wireless communication standard designed to exchange data between fixed and mobile devices over personal distances. Managed by the Bluetooth Special Interest Group (SIG), it operates in the unlicensed 2.4 GHz ISM radio band and serves as the foundational protocol for Wireless Personal Area Networks (WPANs).

#### Core Characteristics

* Operating Frequency & Spread Spectrum: Operates across the globally available 2.4 GHz ISM band (2.402 GHz to 2.480 GHz). It avoids radio interference with Wi-Fi and microwaves using Adaptive Frequency Hopping (AFH), cycling through frequencies up to 1,600 hops per second.
* Network Topology (Piconet & Scatternet): Devices form an ad-hoc micro-network called a piconet, where one primary device coordinates with up to seven active secondary devices. Multiple overlapping piconets can interlink to form a scatternet.
* Operating Range: Typically optimized for 10 to 30 meters in standard consumer hardware, but capable of extending beyond 100 meters under line-of-sight conditions using Low Energy (LE) coded PHYs.
* Data Transfer Rates: Varies from 125 kbps to 2 Mbps in Bluetooth Low Energy up to 2 to 3 Mbps (Basic Rate/Enhanced Data Rate, BR/EDR) in Classic implementations.

### AirDrop

Apple’s proprietary ad-hoc wireless file-sharing service, enabling devices across the iOS, iPadOS, and macOS ecosystems to transfer files, photos, videos, contacts, and URLs directly to one another without an external router or internet connection. It operates as a hybrid transport layer, coupling Bluetooth Low Energy (BLE) for device discovery and session negotiation with direct peer-to-peer Wi-Fi for high-bandwidth data payloads.

#### Core Characteristics

* Hybrid Radio Architecture: Uses low-power BLE for passive discovery and advertises availability, then spins up an on-demand, encrypted point-to-point Wi-Fi connection (using Apple Wireless Direct Link, or AWDL) solely for file transmission.
* Operating Range: Typically effective within 9 to 10 meters (30 feet), bounded by Bluetooth discovery range and direct line-of-sight Wi-Fi strength.
* High Throughput: Because payloads move across peer-to-peer Wi-Fi rather than Bluetooth, transfer rates routinely hit 200 to 500+ Mbps, allowing multi-gigabyte 4K video files to transfer in seconds.
* Zero Infrastructure Dependency: No shared local Wi-Fi router, cellular connection, or active internet access is required. Devices negotiate a private, direct link on a shared Wi-Fi channel on the fly.
* Out-of-Range Continuity: If both sender and receiver are signed into iCloud, transfers that move out of local wireless range can automatically continue uploading and downloading via the internet over cellular or external Wi-Fi.

## Local Area Networks (LAN)

A computer network that interconnects computing devices within a bounded, distinct geographic area—such as a single home, office suite, school building, or data center floor. Unlike wide-area networks that cross public rights-of-way, a LAN is typically privately owned, maintained, and controlled by a single individual or organization.

#### Core Architectural Characteristics

* Localized Scope: Confined to a single physical location or tightly clustered group of buildings, usually within a radius of a few meters up to roughly one kilometer.
* High Data Rates & Throughput: Delivers significantly higher speeds than typical internet uplink connections—ranging from standard 1 Gbps (Gigabit Ethernet) and modern Wi-Fi 6/7 wireless speeds up to 10 Gbps, 25 Gbps, 40 Gbps, or 100 Gbps+ in enterprise switches and server fabrics.
* Low Latency & Error Rates: Because transmission lines are short, shielded, and private, internal network round-trip times (RTT) are minimal (often sub-millisecond to under 2 ms), with negligible packet loss compared to internet routes.
* Single Administrative Control: The addressing schemes (private RFC 1918 IP spaces), security policies, routing tables, and physical access switches are managed internally by a local network administrator or router rather than an ISP.
* Resource Sharing: Enables seamless local peripheral sharing (network-attached storage/NAS, shared printers), inter-server database synchronization, local compute clustering, and internal VoIP routing.

### Subnets

A logical subdivision of an IP network. Within a Local Area Network, subnetting divides a broad IP address space into smaller, distinct network segments, defining routing boundaries and isolating broadcast traffic.

#### Reserved Addresses: The "-2" Rule

In any standard IPv4 subnet, two host addresses are reserved by protocol definition and cannot be assigned to individual endpoints:

1. Network ID (All Host Bits = 0): Identifies the subnet itself (e.g., `192.168.1.0` in a `/24`).
2. Directed Broadcast Address (All Host Bits = 1): Reaches all hosts on that specific subnet simultaneously (e.g., `192.168.1.255` in a `/24`).

### Gateways

A network node that serves as the entry and exit access point between two distinct networks operating with different IP subnets, routing policies, or protocol suites. In the context of a Local Area Network, the default gateway is the router or Layer 3 boundary device that local hosts send traffic to whenever an IP destination lies outside their own local subnet (people on their home network accessing the Internet).

## Metropolitan Area Networks (MAN)

A computer network that spans a geographic area larger than a single building or campus (LAN), but smaller than a state, country, or continent (WAN). Typically covering an entire city, large municipality, or metropolitan region, a MAN interconnects multiple local networks to provide a shared high-speed backbone for public utilities, universities, municipal agencies, or multi-branch enterprises.

#### Core Characteristics

* Geographic Coverage: Ranges from 5 kilometers up to roughly 50 kilometers (approximately 3 to 30 miles), bounded by the borders of a single metropolitan area or contiguous urban district.
* High-Throughput Backbone: Provides data rates significantly higher than traditional long-haul WANs, typically offering 1 Gbps, 10 Gbps, 40 Gbps, or 100 Gbps over dense municipal fiber runs.
* Low Latency & High Reliability: Because physical path lengths are strictly regional (rarely exceeding 50 km), propagation latency is low—routinely between 1 ms and 10 ms—with low jitter compared to intercontinental WAN hops.
* Dual Ownership Models: A MAN can be privately constructed and owned by a large enterprise consortium, school district, or municipal government; alternatively, it can be leased from regional telecom operators as a managed Layer 2/3 transport service.

## Cloud Networks

A software-defined, virtualized networking architecture provisioned and managed on top of a cloud service provider’s physical data center infrastructure (such as AWS, Microsoft Azure, or Google Cloud). Instead of configuring physical switches, routers, and patch panels, network engineers define topologies, subnets, routing tables, and security perimeters purely through software APIs, web consoles, or Infrastructure-as-Code (IaC) templates.

#### Core Characteristics

* Software-Defined Networking (SDN): The physical network hardware (the data plane) is decoupled from the software controllers (the control plane). Users interact exclusively with virtual abstractions, allowing complex multi-tier topologies to spin up or reconfigure in seconds.
* Elastic Scalability & Dynamic Provisioning: Network resources—such as virtual interfaces, bandwidth allocations, and managed gateways—scale on demand to accommodate traffic surges without physical hardware bottlenecks.
* Multi-Tenant Physical Isolation: Public cloud providers run millions of customer workloads across shared physical hypervisors. Cloud networks enforce strict cryptographic and encapsulation boundaries (using protocols like VXLAN and Geneve) to ensure tenants cannot intercept or inspect each other's traffic.
* Global Backbone Integration: Cloud providers maintain private, high-capacity international fiber backbones. Traffic moving between two virtual networks in different continents stays on the provider's dedicated, optimized fiber network rather than traversing the public Internet.

## Wide Area Networks

A telecommunications network that spans a large geographical territory—connecting multiple Local Area Networks (LANs) and Metropolitan Area Networks (MANs) across cities, regions, countries, or entire continents. The global Internet is the largest and most prominent example of a public WAN.

#### Core Characteristics

* Broad Geographic Footprint: Covers distances ranging from dozens of kilometers to thousands of kilometers, crossing public rights-of-way, municipal lines, and international borders.
* Third-Party Infrastructure & Leased Media: Unlike a privately owned LAN, organizations rarely lay their own long-haul physical cabling for a WAN. Instead, they lease capacity or buy transport services from telecommunications service providers, network carriers, and cable operators.
* Higher Latency & Variable Propagation Delays: Transmission delay is governed by physical distance, fiber propagation limits, transit through intermediary carrier hops, and satellite/submarine links. Round-trip times (RTT) range from 10 ms up to hundreds of milliseconds.
* Dynamic, Multi-Path Routing: Because WAN paths are long and span complex topologies, routing engines dynamically reroute traffic around severed undersea cables, backhoe cuts, or congested carrier peering exchanges.

### The Internet

A decentralized, worldwide network of interconnected computer networks operating on standardized communication protocols (primarily the Internet Protocol Suite, or TCP/IP). It represents the culmination of all smaller network models—linking billions of private, public, academic, corporate, and government LANs, MANs, and WANs into a single collaborative fabric.

#### Core Architectural Principles

* Decentralized & Distributed Design: No single entity, government, or central processor controls the Internet. It is built as a mesh where failure in one geographic node, carrier, or link simply triggers dynamic path recalculation across alternate routes.
* The "End-to-End" Principle: Network intelligence and application state reside entirely at the endpoints (the client and server machines). The intermediate network core (routers and switches) remains application-agnostic, functioning solely as a high-speed "dumb pipe" dedicated to forwarding datagrams.
* Packet Switching: Rather than dedicating a physical transmission circuit between two parties (like traditional landline telephone networks), data is split into discrete IP packets. Each packet travels independently across available paths and is reassembled at the destination.

#### The Three-Tier Carrier Hierarchy

The global routing ecosystem relies on an economic and operational hierarchy of Internet Service Providers (ISPs):

* Tier 1 Backbone Carriers: The backbone operators (such as Lumen, AT\&T, Telia, NTT, and Tata). They operate transcontinental and transoceanic optical fiber trunks. Crucially, Tier 1 providers peer settlement-free with each other—meaning they swap massive volumes of traffic without paying one another transit fees.
* Tier 2 Providers: Regional and national networks that peer with some peers for free but must pay Tier 1 carriers for transit access to reach the rest of the global routing table.
* Tier 3 ISPs (Last Mile): Local retail consumer and enterprise providers. They sell direct connectivity to homes, businesses, and mobile cell towers, purchasing upstream transit from Tier 2 or Tier 1 networks.
