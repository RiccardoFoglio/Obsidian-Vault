Packets routed from source to multiple destinations. It's key for group communication (videoconferencing, video broadcasting etc...)

Multicast Address identifies a group of hosts

![[Pasted image 20241003144557.png]]

Multicast Addresses in Class D: 1110 start --> 224.0.0.0 - 239.255.255.255

Address identifies a host group: packet is delivered to all hosts in the group, anywhere in the network.


Hosts join and leave dynamically

IGMP : (Internet Group Management Protocol) is a protocol incapsulated in IPv4 datagrams, that handles membership, used by hosts and routers.
- JOIN
- LEAVE

**Recipients** establish which hosts receive a packet (if they joined the multicast)
- in unicast it's the source to deal with it
- controlling traffic reach is difficult, depends on how many hosts subscribed

## Within an IEEE 802 Network

Router delegates the delivery to Layer 2 (MAC)

packet to router -> router sends to switch -> only some hosts connected should receive the packet (the subscribed hosts)

IP Multicast address is mapped to a MAC multicast address (48-bit)
- 01-00-5E-0
- 23 least significant bits of IP Address

![[Pasted image 20241003145629.png]]

Switch will recognize the multicast address and share to the right hosts

Interface card configured to receive that MAC Multicast
Recipient-initiated group joins through IGMP
Switch support through IGMP Snooping:
- multicast delivery only on ports connected to a group member
- requires cross-layer approach

## Beyond a Single Network

Routers discover host groups on each LAN using IGMP
Routers announce host groups to others: multicast routing protocols
Routers build a distribution tree for each host group: to all LANs with at least a member

## Deployment Status

- Not widely supported
- Not fit to common traffic control / engineering practice
- Mostly limited to controlled environment