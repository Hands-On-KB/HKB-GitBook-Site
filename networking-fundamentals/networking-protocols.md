---
description: The basic protocols that make local and global networking functional.
icon: globe-pointer
---

# Networking Protocols

<h2 align="center">Networking Protocols</h2>

### Transmission Control Protocol (TCP)/Internet Protocol (IP)

The foundation of communication protocols that powers the global Internet and virtually all modern computer networking. It establishes how data is packaged, addressed, routed, transmitted, and reassembled from one endpoint to another regardless of underlying physical hardware.

#### TCP Handshake

This is the way that most endpoints establish a TCP connection, usually in a client-server interaction.

* SYN (Synchronization): Request sent by the client to "synchronize" or connect.
* SYN-ACK (Synchronization Acknowledgement): Response sent by the server confirming desire to connect.
* ACK (Acknowledgement): Client receives approval and connects.

That's how the vast majority of TCP connection attempts go. Those are TCP flags (SYN, SYN-ACK, ACK) but they aren't the only ones. Some others you might see are:

* RST (Reset): Resetting the state of the connection attempt between the client and server. Usually happens when traffic's being rejected.
* FIN (Finish): Ends a properly established TCP connection.
* PSH (Push): Tells the server to speed up data transfer with no wait for full packets to be made. Important for things like online chat.
* URG (Urgent): Tells the receiver that the data being transferred needs to be processed first.

### Domain Name System (DNS)

### Network Address Translation (NAT)

### Address Resolution Protocol (ARP
