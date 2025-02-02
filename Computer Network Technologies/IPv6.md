Motivation: Larger Address Space

Additional Motivations:
- more efficient on LANs
- Multicast and Anycast
- Security
- Policy routing
- Plug and Play
- Traffic Differentiation
- Mobility
- Quality of Service support

It took a long time to define IPv6 and migrating to it
Many IPv4 problems:
- Port IPv4 solves IPv4 problems
- many workarounds found for IPv4

When IPv6 reached production stage, IPv4 solutions were acceptable and was too used to replace

IPv4 issues:
- Only part of the addresses are assigned to stations (class division) -> 3.5B instead of 4B
- hierarchical usage -> lots of unused addresses

IPv4 Solutions:
- Netmask
- Private Addresses (not enough)
- Network Address Translator (NAT) -> breaks end-to-end reachability, increases processing load at gateways, adds complexity
- ALG (Application Layer Gateway)

## Who Assigns IP Addresses?

**IANA** assigns blocks of /8 IP addresses to each **Regional Internet Registry** (**RIR**), one for each continent.

**RIR** split the blocks into smaller blocks to assign them to respective **National Internet Registries** (**NIRs**) and **Local Internet Registries** (**LIRs**)

Any individual IPv4 address can be in any of the 5 states:
- reserved for special use                                                           (RESERVED)
- part of IANA unallocated address pool                                   (FREE)
- part of the unassigned pool held by a RIR                              (NOT ASSIGNED BUT HELD BY RIR)
- assigned to and end user entity but unadvertised by BGP   (UNUSED)
- assigned and advertised in BGP                                               (USED)

## Routing Scalability Issues

When many addresses are not being used, RIR / IANA can cut a chunk of addresses and give it to someone else.
The problem falls on the routing tables, because part of the addresses now are used for other purposes, so all routing tables in the world have to add exceptions

Possible solutions:
- Aggregate multiple routers in one
- CIDR (Classless Inter-Domain Routing)
- Limited by non-rational assignment of IP Prefixes

Scalability of routing protocols, there is no solution at present. IPv6 wants to solve it.

## Birth of IPv6

IETF Boston Meeting (1992) "Call for proposals for IPng" (IP next gen)
Several proposals (among them a IPv5, awful). The best solution (SIPP with 128-bit address) was then called IPv6

128 bit address, using Hex numbers --> X:X:X:X:X:X:X:X (group of 2 bytes)
	Example: 1080:0000:0000:0007:0200:A00C:3423:A089

Shortcuts:
- Leading 0s can be omitted in each group
	- 1080:0:0:7:200:A00C:3423:A089
- All-0s Hextets can be substituted by "::" (only once)
	- 1080::7:200:A00C:3423:A089

## Routing and Addressing Principles

Same routing principle as IPv4

Address Structure:
![[Pasted image 20241003153420.png]]
usually: n=64 bits
	Site Address = 48 bits
	Subnet Address = 16 bits
Netmask (IPv4) is now called Prefix (IPv6) 
*n* is written as *Address/N*
No Address Classes

Subnetwork = Link
- On-link --> same prefix, direct communication
- Off-link --> different prefix, communication via router

Address Space:
![[Pasted image 20241003153852.png]]
## Multicast (1 to Many)

FFFF... -> FF00::/8 is equivalent to IPv4 224.0.0.0/4

- Well Known : (FF00::/12) predefined or reserved multicast addresses for assigned groups of devices (Routers)
- Transient : (FF10::/12) dynamically assigned by multicast apps
- Sollicitated-Node : (FF02:0:0:0:0:1:FF00::/104) similar to IP broadcast address in ARP

Inside address: SCOPE -> allows devices to define the range of the multicast packet (how broadly to propagate the packet)
![[Pasted image 20241003154508.png|700]]
## Unicast (1 to 1)

Aggregable global unicast addresses (GUA), same as public IPv4 addresses

Starting at: 001
Global Routing Prefix ($n$) --> assigned to site by provider
Subnet ID ($m$) --> link inside site
Interface ID ($128-n-m$)

### Link Local Addresses

1111 1110 1....

Used for addresses on a single link for auto-address configuration, neighbor discovery or when no routers are present
Packets sent to these addresses are not supposed to be forwarded across local links

### Unique Local Addresses

FC00::/7

private use and should not be routed to the global internet

8th bit is a "Local (L) Flag" dividing the range:
- 0 --> may be defined in the future, but not rn
- 1 --> address is locally assigned

### IPv4 Embedded Addresses
80 0s and 16 1s, then IPv4 address
![[Pasted image 20241004203218.png]]

### Loopback Addresses
Used by a node to send a IPv6 packet to itself for testing reasons
::1
Equivalent to IPv4 address: 127.0.0.

### Unspecified Addresses
:: --> means the host doesn't have a specified address yet

## Anycast

Address that can be assigned to more than one interface, multiple devices can have the same anycast address.
A packet sent to an anycast address is routed to the "nearest" interface having that address, according to the router's routing table
Initially designed for DNS but still in experimental stages (firstly introduced in 1993)

# Modified Protocols
Changes: 
- ICMP
- ARP --> Integrated inside ICMP
- IGMP --> Integrated inside ICMP

Upgraded but not changed:
- DNS
- RIP and OSPF
- BGP and IDRP
- TCP and UDP (mandatory to check payload)
- Socket Interface

IPv4 Header Recall
![[Pasted image 20241004204204.png]]

New IPv6 Header
![[Pasted image 20241004212323.png]]

## Extension headers concept

Next Header similar to IPv4 "protocol" field. It specifies the protocol carried in the data portion of the IPv6 packet
Additional "next header" fields are added in header extensions to allow chaining
Protocols coded using the same values as IPv4 (plus some)
- 0: Hop by Hop option
- 6: TCP
- 17: UDP
- 43: Routing extension
- 44: Fragment header
- 50: encapsulating Security Payload
- 51: Authentication
- 60: Destination option
- 59: no next header

![[Pasted image 20241004213019.png]]

This way combines the old IPv4 IHL (header length) and Protocol Field --> tells us what protocol is carried in payload

### Hop-by-Hop Extension Header
Used to carry optional information that should be examined by every router along the path of the packet.
Each option contains a set of Options Type, Options Length, and Options Value fields 

### More Extension Headers

- Routing Extension Header --> source routing
- Fragment Extension Header --> 
	- used when the source needs to divide the packet into fragments, each with its own main IPv6 header and a fragment extension header
	- The recipient of the packet reassembles the fragments
	- Recall: unlike in IPv4, an IPv6 router does not fragment a packet unless it's the source of the packet
- Authentication and Encapsulating Security Payload Extension Headers --> 
	- used by IPsec, suite of protocols for securing delivery of packets in IP networks
	- the Authentication Header (AH) is used to guarantee the authenticity and integrity of a packet
	- Encapsulating Security Payload (ESP) provides authentication, integrity and encryption

# Interfacing with Lower Level

## Encapsulation

IPv6 packets are encapsulated in layer-2 frames (for Ethernet, Type: 86DD)
They are treated as a new protocol
	Enables dual stack (IPv4, IPv6) approach
	Keeps running IPv4 as-is

## Address Mapping

How is the destination MAC address set when an IPv6 packet is encapsulated?

IP Unicast address
- procedural discovery
- neighbor discovery

IP Multicast Address
- Algorithmic mapping

### IPv6 Multicast Transmission

based on Ethernet Multicast, unlike Ethernet Broadcast, an Ethernet Multicast can be filtered by the NIC
IPv6 multicast address mapped to MAC address
- 33-33-xx-xx-xx-xx is the reserved Ethernet MAC address when carrying an IPv6 multicast packet (RFC 7042)
- The lower 32 bits of the MAC address (xx-xx-xx-xx) are copied from the lower 32 bits of the IPv6 multicast address

![[Pasted image 20241004214148.png]]

### Neighbor Discovery and Address Resolution

It substitutes ARP
Based on multicast: leverages the solicited-node Multicast Address
Given the way multicast solicited address are constructed, most likely only one station gets involved

Before: ARP packet received by everyone via broadcast (L2), not ideal

Now: ICMP + Multicast Addressing, using the Solicited-node Multicast Address

takes 24 LSBs of Global Unicast Address of the Host and mapping into the 24 LSB of the Solicited-Node Multicast Address
It's possible to have multiple hosts subscribed to the same multicast if they have the last 24 bits in common

![[Pasted image 20241004214701.png]]
#### Address Resolution

**ICMP Neighbor Solicitation**: requester sends frame to Solicited Node Multicast Address of target IPv6 address

(L2) Packet will be received only by the Host that subscribed to the multicast, so only the host creating it's own solicitated mode multicast address.

Avoids broadcasting and saves resources

# ICMPv6

Used for:
- Diagnostics
- Neighbor Discovery
- Multicast group management
- Issue notification

Includes functions that in IPv4 were delegated to ARP (Address Resolution Protocol) and IGMP (Internet Group Membership Protocol)

Message Format: encapsulated in IPv6 packets, Next header = 58 ; At most 576 bytes

![[Pasted image 20241010152701.png]]

Type Fields:
![[Pasted image 20241010152713.png]]
## Error Messages
- **Destination Unreachable** (type = 1) --> generated by router or firewall, code field provides reason
- **Packet too big** (type = 2) --> IPv6 routers no longer fragment packets
- **Time exceeded** (type = 3) --> if router receives packet with Hop Limit = 0
- **Parameter Problem** (type = 4) --> generated when devices finds a problem with a field in the main IPv6 header or extension header
## Informational Messages
### Echo
- Echo request (type = 128)
- Echo reply (type = 129)
- used by `ping`
![[Pasted image 20241010153144.png]]
### Neighbor Solicitation
Type = 135
Target Address = full address trying to contact
Options = Ethernet address (MAC) of sender, so destination can build reply
![[Pasted image 20241010153218.png]]
### Neighbor Advertisement
Type = 136
3 flags:
- Router : specific case
- Solicited : 1 if neighbor adv sent as reply to a solicited message
- Override : 1 if this packet should override the obsolete address in the cache

If you change IP address, it sends out this neighbor advertisement with solicited = 0 and override = 1 to update the cache of the new address.

Target Address = IP Address of host we are replying to
Options = Ethernet address of sender of message
![[Pasted image 20241010153238.png]]
We write the Ethernet addresses even on layer 3, because we lose information of Layer 2 (encapsulation) and we store these addresses in cache for future use. This way we save Layer 2 info also if we deal with Layer 3
### Multicast Group Management
- within a link, rely on data link layer multicasting service. 
	- Mapping of IPv6 multicast addresses onto MAC multicast addresses.
- Among links, packets routed by routers.
	- ICMPv6 to know on-link members (hosts interests in receiving packets)
	- Multicast routing protocols to know where there are off-link members

#### Group Management Messages
- Multicast Listener Query (Type = 130) sent by router
	- General
	- Multicast group (address) specific
- Multicast Listener Report (Type = 131) sent by Listener to all-router multicast address
- Multicast Listener Done (Type = 132)  sent by Listener to all-router multicast address

![[Pasted image 20241010155245.png]]
# Device Configuration in IPv6

Information needed:
- Address prefix
- Interface identifier
- Default gateway
- DNS server
- Hostname
- Domain name
- MTU (Maximum Transmission Unit)

Options:
- Manual config
- Stateful config --> all info obtained through DHCP
- Stateless config --> autogenerated, address prefix obtained from router
- Hybrid config --> info other than address obtained through DHCP
## Interface Identifier
- Manually configured
- Obtained through DHCPv6
- Automatically generated : from EUI-64 MAC address, Privacy Aware
### EUI-48 to EUI-64 Mapping
EUI = Extended Unique Identifier
![[Pasted image 20241010160028.png]]
- Split MAC address
- Add 0xFF and 0xFE in the middle (16 bits)
- Flip the 7th bit --> if manually configured, probably a lot of 0s in beginning, so flipping makes it harder to be duplicated
### Privacy Concerns

Traceability : the least significant 64 bits of IPv6 address of an interface never change when MAC address is used
RFC4941 --> "Privacy Extensions for Stateless Address Autoconfiguration in IPv6"

![[Pasted image 20241010160901.png]]

- Interface ID from MAC address + random 64 bits
- MD5 hashing
- new random (hashed) 128 bits
- Take first 64 bits, perform bitwise AND with mask (all 1s and 7th bit = 0)
	--> New Interface ID (can be taken and used for next Interface ID)
## Address Prefix

- Manually config
- Obtain from DHCPv6
- Automatically generated --> link local
- Obtained from a router
### Router / Prefix Discovery

- ICMP Router Advertisement message --> sent by routers
- Solicited --> answering to Router Solicitation by host
- Unsolicited : periodic
#### Router Solicitation
ICMPv6 message
![[Pasted image 20241010161657.png]]
Sent to all-routers multicast address (FF02::2)
#### Router Advertisement
![[Pasted image 20241010161742.png]]
M (Managed Address Configuration) --> 1 = address available through DHCP
O (Other Configuration) --> DNS Server, etc...
Options : will store the DHCP Address / other addresses (O=1)
Router Lifetime, Reachable Time, Retrans Timer : emergency settings, usually undefined or 0
#### Options Format
General format, length in multiple of 8 bytes
![[Pasted image 20241010162159.png]]
##### Prefix Information Option
![[Pasted image 20241010162239.png]]
L - prefix is on-link (send your own link prefix)
A - prefix can be used for autonomous configuration (can send other links in subnet reachable by router)
##### MTU Option
ensures all hosts on-link use the same MTU value
![[Pasted image 20241010162246.png]]

##### Link Layer Address Option
Diagnostic messages
![[Pasted image 20241010162255.png]]
##### ICMP Redirect
Sent by a router to advise a host about a best first-hop
The first -hop is always on-link, irrespective of prefix.
Changing the route means also changing the routing table, so message must contain info for that
	Target address = new address to use instead of old one
	Destination address = in order to reach the destination

![[Pasted image 20241010163139.png]]
##### Redirect Header Option
Information about the packet being redirected
![[Pasted image 20241010163316.png]]

## Duplicate Address Detection (DAD)

- Probe uniqueness of an IPv6 address
- Neighbor solicitation with address being probed as target
	- sent to corresponding IPv6 Solicited Node Multicast Address to the address just created, so if someone is already using my address, it will reply to this one and as source address send null ( :: )
	- Corresponding MAC multicast address
- wait for a response for at least 1 sec
	- if no answer is received, the address is valid
	- if there is an answer, stop

## Stateless Config
### Basic Step
- generate link local address
- probe the uniqueness (DAD)
- subscribe to the corresponding IPv6 Solicited Node Multicast Address
	- Configure reception of corresponding multicast MAC
	- Send ICMP Multicast Listener Report
- On-Link communication enabled
### With Router
- Possibly send Router Solicitation
- Listen to Router Advertisements
- Create address from advertised prefix
- Probe for its uniqueness (DAD)
- Subscribe to the corresponding IPv6 Solicited Node Multicast Address
	- Configure reception of corresponding multicast MAC
	- Send ICMP Multicast Listener Report

97