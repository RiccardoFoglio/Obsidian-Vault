Devices: 
1. L1 : Repeater --> Hub, separate physical domains, same collision domain
2. L2: Bridge --> Switch, separate collision domains, same broadcast domain
3. L3: Router --> L3 switch, separate broadcast domains, not really specific for LANs, not covered in the current slides
## Repeater
Interconnection at the physical layer : receives and propagates a sequence of bits.
Used for: interconnecting networks having the same Medium Access Control (MAC) : same technology but different physical layer is possible
Recovering signal degradation (long cables) allowing larger distances
![[Pasted image 20241115135952.png|500]]

Multiport repeaters : Hubs
- hubs are multiport repeaters, with more than 2 ports
- Required for twisted pairs and fiber cabling (hub-and-spoke topology)
## Bridge

Pure software, 2 ports (mainly for economic reasons)
Interconnection at the data-link layer: ethernet to wifi, ethernet to fast ethernet, different MACs...
![[Pasted image 20241115140558.png|500]]
LAN extension (total diameter) : especially useful for FastEthernet and upper speed (200m)
Works by receiving and re-transmitting (later) a frame
1. store the frame (store and forward mode)
2. modify the frame
3. send it out
![[Pasted image 20241115140728.png|500]]

"Store and forward" allows smarter sending of data on output interfaces
Bridges decouples collision from broadcast domain. Collision domain is no longer a limitation.
![[Pasted image 20241115140923.png|500]]

Collision domain : area where a single instance of the access control algorithm operates
Broadcast domain : area where frames can be propagated without crossing a router

![[Pasted image 20241118140354.png|500]]

## Half Duplex and Full Duplex Mode

Half Duplex:
- Standard operating mode of network interfaces
- RX and TX cannot happen at the same time
- RX+TX activity is seen as collision

Full Duplex: 
can transfer and receive at the same time
Introduced with Fast Ethernet
Available whenever the other party can temporarily store the frame, instead of repeating immediately the received bits on the other ports
Simultaneous TX and RX possible
Advantages:
- Bandwidth : in theory double throughput, in practice limited advantage for clients and servers. May be interesting for bridges on the backbone (more symmetrical bandwidth)
- CSMA/CD : the real advantage, no longer needed since collisions are not possible. No requirement

Modern LANs are heavily based on full duplex
Hub-and-spoke topology : point-to-point connections between hosts and the bridge, no collision domain
Multiport bridges are called "switches" : same functions, different internal architecture

Modern LANs use Switched Ethernet
CSMA/CD no longer used
Wireless LANs are completely different : based on CSMA/CA (wifi) and hubs are used (wifi extenders)

Bridges used in Ethernet LANs are called **transparent bridges / switches**
other non-transparent bridges have been proposed in the past, no longer in use
Transparency:
- bridges (switches) should be plug-and-play and must not require any change in the configuration of end systems
- performance (throughput, max distances) may be different from the original network but functionalities are the same

Each port of a bridge/switch has a MAC level and therefore a MAC address
 That MAC address is never used when forwarding dara grames
 it's used when frames are generated / received by the switch itself

### Smart Forwarding Process
Plug and play switches
smarter forwarding rules apply for:
- Unicast : only the port toward the destination (Destination MAC-based forwarding)
- Multicast, broadcast : flooding (all ports except the port on which the frame has been received)

MAC forwarding table must be available locally : filtering database

A note about flooding: 
- frames are sent on all ports but may not be sent at the same time
- hubs flood data at the same time (bits forwarded immediately)

In order to operate successfully, a "smart" bridge/switch requires 3 features:
- **local forwarding table** #Filtering_Database
- **stations auto-learning** #Populating_a_Filtering_Database
- **loop detection** #Loop_Detection

The ultimate goal: bridge/switch should be able to do its job without any explicit configuration from network admin -- Plug and Play

### #Filtering_Database

 Table with the location of any MAC address found in the network
 - MAC address
 - Destination port
 - Ageing time (default expiration: 300sec)
- "filtering" database : the smart forwarding process was perceived as a way to filter out unwanted traffic from a link
![[Pasted image 20241118153423.png|500]]

Entry types:
- Dynamic : populated and updated by backward learning process. Between 2 - 64k entries
- Static : not updated by the learning process. Less than 1k entries

Old dynamic entries are purged out of the filtering database, stations don't exist anymore on the network (300sec aging default)

![[Pasted image 20241118153744.png|500]]

Forwarding Process and Transient

If MAC address not present in Filtering Database: switch behaves like a hub --> frame duplicated on all ports except the receiving one
This situation is called "Transient" : switches are plug-and-play and have an algorithm to learn the location of the hosts : Backward Learning

### #Populating_a_Filtering_Database

1. By hand
2. By means of a proper algorithm : Backward Learning

Idea : switch receives a frame whose source is host H1 from port P1, that host will be reachable thru port P1.
Topology is learned by inspecting received frames : analysis of MAC source address, the Destination MAC address is ignored by this algorithm

Works also in presence of multiple switches : remote switches learn the position anyway, even if end-system not connected locally.
Backward learning and frame forwarding taken together

![[Pasted image 20241118160915.png|500]]

If the end-system generates broadcaster frame immediately : no problem
If the end-system generates unicast traffic immediately : we may have forwarding errors
If the end-system does not generate traffic at all : forwarding troubles

Broadcast frame : reaches entire network, therefore all the switches update location of current station
Unicast frame : potentially reaches only a portion of the network, hence the rest may still have the old location of the station
In Real World : usually enough in order to cope with manual movements, possible problems in specific environments


Possible Attacks at the Filtering Database:
MAC Flooding Attack : generation of frames with random MAC sources. Filtering database gets full, switches will start flooding most of the frames.
Objective: 
- force switches to operate like hubs, so we can intercept traffic generated by other stations
- slows down the network
Some vendors give the opportunity to limit the number of MAC addresses learnt on each port

### #Loop_Detection

![[Pasted image 20241118162106.png|500]]

Multicast / broadcast frames : very commonly generate loops

Frame to a non-existing station : MAC address not present in filtering DB, Problem that may happen rarely
- IP sends an ARP before contacting an L2 station
- if station doesn't exist, ARP will never get a reply and destination MAC address is unknown
- no MAC frames will be sent to that station intentionally

Broadcast Storm : massive load due to broadcast / multicast traffic on a LAN. One of the most dangerous problems at data-link layer
No solutions, except disabling loops physically.
Network operators are almost impotent in such a case due to the lack of "time-to-live" field in L2 frames.
L3 networks can tollerate transient loops (TTL available in L3)

#### Spanning Tree Idea
to avoid troubles: avoid loops in the physical network
either create loop-free networks or define an algorithm that disables loops

Meshes detected and disabled : the network becomes a tree : unique path between any source and my destination

# Routers

L3 devices, not transparent with respect to MAC Address. Routers separate broadcast domains

![[Pasted image 20241118165736.png|500]]

It's better to keep the L2 as long as the network can operate as a single L2 entity
But single gigantic LAN vs multiple LANs?
- Performance : 
	- single LAN has too much broadcast traffic (not filtered by switches)
	- Flooded traffic
- Privacy and Security : don't want a station to leak some info out
- Management : smaller network, simple (and uniform) policies

Better to partition different users in different LANs

Different physical networks (full separation) : N networks = N links + N devices, waste of resources

Virtual LANs is the answer
# Virtual LANs
![[Pasted image 20241122132545.png]]
Single physical infrastructure : same devices, same cables, no switches in which only a few ports are used, no need to have multiple fibers in the backbone.
Different LANs:
- Different broadcast domains
	- Ethernet frames cannot be propagated on another VLAN
	- no broadcast between LANs
	- no MAC flooding attacks
	- no ARP spoofing
- created through a proper (logic) separation on switches
- intra-switch or inter-switch

VLAN Architecture:
![[Pasted image 20241122132756.png]]

![[Pasted image 20241122135319.png]]

L2 data cannot cross VLANs : an ethernet station cannot send a L2 frame to another station in a different VLAN. VLANs are different broadcast domains

![[Pasted image 20241122135421.png]]

A router is needed (device operating at layer 3) : lookup at layer 3 (IP destination address). The original L2 Header is thrown away and a new one is created with other MAC addresses (src/dst)

Broadcast cannot cross the VLAN boundaries: cannot use ARP to resolve the MAC address in another VLAN. Hosts in different VLANs must belong to different IP networks

Problem : how can we associate frames to VLANs?
VLANs on a single switch:
- Simple method: we can mark the ports on the switch --> the received frame is associated to the VLAN the port belongs to
- Other methods exist as well

VLAN on different switches
Problem: how to distinguish which VLAN a frame belongs to, as there is a single link between switches?
Same problem for devices that belong to different VLANs (servers, routers...)

Associate frames to VLANs : Tagging
Required only on links that transport traffic of different VLANs
An additional header is added to the MAC header
- Standardized by IEEE 802.1Q
- 4 additional bytes added to the frame
- Basically VLAN-ID plus a bunch of other info
![[Pasted image 20241122140243.png]]
![[Pasted image 20241122140256.png]]

Modification of existing MACs
- minor modifications
- new framing (for tagging) specified in 802.1Q
	- independent from the technology of the Medium Access Control
- maximum length of the frame has to be extended 4 bytes
	- Ethernet reaches 1522 bytes (from 1518)
	- minimum length unchanged (still 64 bytes)

## Port Types

### Access
Access ports receive and transmit Untagged frames
Default configuration (on hosts, switches. servers, routers etc...)
Usually used to connect end-stations to the network. Hosts don't need to change their frame format
![[Pasted image 20241122140713.png]]

Given the following network:
- All ports are configured in “access mode” 
- SW-1 is configured with the RED VLAN on all its ports
- SW-2 is configured with the GREEN VLAN on all its ports
Can host H1 communicate with host H4?
![[Pasted image 20241122140810.png]]
Yes, because values configured on access ports are not propagated outside the switch!
### Trunk
Trunk ports receive and transmit Tagged Frames
Must be configured explicitly. Often used in switch-to-switch connections and to connect servers/routers
![[Pasted image 20241122140912.png]]
## Assign Hosts to VLANs
Different methods to associate to the proper VLAN
- Port-based VLANs
- Transparent assignment (example: Associated to MAC addresses)
- Per-user assignment (802.1x)
- Cooperative assignment --> frames tagged directly by hosts
Note: a station can be associated also to multiple VLANs. Required in case of servers, routers. In this case, trunk ports are required on the device (frames tagged by the device -- cooperative assignment)

In theory: the VLANs and Spanning Tree are independent
- First, Spanning Tree is computed in order to disable loops
- Then, VLANs are used on the resulting topology -- unique forwarding tree for all the VLANs

Almost all vendors offer Per-VLAN Spanning Tree
- Most vendors can turn back to a unique STP via configuration
- Cisco cannot (per-VLAN STP is the only option)
## VLAN and Network Isolation

Network isolation is not complete, even with VLANs. Although frames cannot cross the border of a VLAN, links are shared, hence a problem on a link caused by traffic of one VLAN, may affect other VLANs
![[Pasted image 20241122141419.png]]

VLANs don't protect from broadcast storms
- broadcast traffic is sent on the entire network
- A trunk link may be saturated by a broadcast storm on a VLAN
- Other VLANs don't receive that broadcast but the trunk link is congested and it may be unable to transport the traffic of other VLANs

Per-VLAN QoS may be required : "Round Robin" service model based on VLAN ID, which guarantees a minimum amount of bandwidth to each VLAN

## VLANs and Network Switches

2 types of switches:
- VLAN-Aware : handle tagged and untagged frames
- VLAN-Unaware: do not accept tagged frames : may discard frames if too big, low-end devices

Availability on the market: almost all professional products can handle VLAN tagging, almost all domestic products don't.

VLANs are no longer a plug-and-play technology: STP is, with some limitations, This is one of the reasons VLANs are not supported on domestic switches, typical users are not skilled enough to configure them

Modern wired LANs are based on Switched Ethernet:
- Star topology with full-duplex links
- Collision domain no longer exists --> no need for CSMA/CD
- Fault tolerance given by redundancy + Spanning Tree Protocol

Wide adoption of VLANs:
- traffic isolation
- Broadcast domain size reduction
- routers required for both internal and external communications

L3 closer to the users, L3 switches