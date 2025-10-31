Multi-Protocol Label Switching
Enabling technology for the New Broadband (IP) Public Network, where Public means that networks can be sold.

MPLS allowed to control the quality of the network, which meant being able to sell high quality networks.

It wasn't designed to work for IP networks at first, now we only have IPv4 and IPv6 but back then there were more technologies that we don't use now.

How it was Before:
![[Pasted image 20241212122948.png]]

With MPLS:
![[Pasted image 20241212123013.png]]

Optical switching is the core, and now we use IP with MPLS 

![[Pasted image 20241212125552.png]]
Forwarding packets according to a label instead of the IP destination address
The label may be used as an index, instead of longest prefix matching. It's a faster lookup


MPLS introduces a connection-oriented paradigm in IP networks

MPLS is not meant to work host-to-host. It's only used in the backbone of the network, like the routers that forward packets (Label Switch Router LSR)
![[Pasted image 20241212151753.png|500]]

At the edge of the network, we have Label Edge Router Ingress/egress LSR
![[Pasted image 20241212151724.png|500]]

How the network works:
- packet arrives at ingress LSR and wants to go to a destination D
- gets routed to ingress LSR X
- looks up table and sends it to Y
- Y is a Label Switch Router (LSR), so it will attach a label to the packet in front
- the label will be used to navigate thru the network and get to the egress LSR Q
- the last label will be removed and sent to the correct router (which is not LSR)
- packet gets to D
![[Pasted image 20241212154354.png|500]]
Key elements:
- MPLS header contains the label
- Protocols for label distribution --> signaling
- enhanced routing protocols --> constraints in choosing paths

# MLPS Header

Shim Header : pushes apart layer 2 header and IP packet (split)
![[Pasted image 20241212155739.png|600]]
Routers only look at the top of the stack (left side). 
S (Bottom of stack) = to know if the label is done or no after popping

ATM and Frame Relay
both connection oriented : had a field in the header where the connection could be identified (which is a label)
That field can now be used to put the MPLS Label
made it fast to use and deploy

# LSP setup: label and path selection

how routers decide which label path to select ?

Label is switched at every hop because : easier to manage, need less labels overall, label must be unique only in the hop, not across the whole system

FEC : Forwarding Equivalence Class :
> Packets that are treated the same way by each LSR will follow the same path in the MPLS network and receive the same label

3 key actions taken by LSRs:
- Label binding
- Label mapping
- Label distribution

**Label Binding**  : an LSR determines the label that should be prepended to packets belonging to a given FEC.
	Downstream binding : (done by router on the receiving end of the stream)
		LSR on receiving end of a link
		Packets of the FEC shall be received with chosen label
		Upstream node to be notified
	Unsolicited
	On-Demand

**Label Mapping**  : Association between:
- Input Label --> chosen by considered LSR
- Output Label --> chosen by downstream LSR
- Next Hop --> based on routing

**Label Distribution**  : Notification of the label chosen for a given FEC
- Follows label binding 
- An LSR that has made a binding, distributes it to neighboring LSRs 
	Or at least to the upstream one

## Static Label Binding (and Mapping)
- Through management
- equivalent to PVC ATM
- non-scalable
- no interoperability among managing systems
- impossible to have LSPs through different networks
## Dynamic Label Binding
- Data / Traffic Driven
	- Triggered by data packets
- Control Driven
	- Triggered by control message
	- signaling
	- routing

Control Driven:
- Topology based : creation of LSPs is linked to discovery of routes towards destinations, Unsolicited
- Explicit creation of LSPs : explicit signaling, initiated by label edge routers, on demand

# Label Distribution Protocols
3 alternatives (incompatible)

- Routing protocol (BGP) --> only topology based
- Label Distribution Protocol (LDP) --> designed for the purpose
- Resource reservation Protocol (RSVP) --> designed for allocation in integrated service networks
## Routing Protocols 
Used to determine LSP routing
- Impact label mapping phase
- Indirectly determine packet routing
Existing protocols: 
- OSPF
- IS-IS
- BGP-4

In MPLS context, they are enhanced to carry information to constraint routing decisions (constraint data)
- Capacity of links
- Link utilization
- Dependencies among links
- Used for fault recovery

Enhanced routing protocols : constraint based routing is fundamental to support traffic engineering (TE)
- OSPF-TE
- IS-IS-TE
### Routing Modes

#### Hop-by-hop routing
Each LSR decides the next LSR of the LSP path 
Example: based on IP routing table, same principle and outcome as in traditional IP routing

- Pick label for upstream link 
	- label binding
- Map it to
	- next hop
		- address of connected interface of next LSR
	- label announced by next LSR
#### Explicit routing
- Single switch chooses the path of the whole LSP
- The choice is constraint based --> constraint based routing

Constraint Based Routing: distributed operation impossible
- no unique route selection criteria
- conflicting constraints
- hard to maintain constraint information synchronized
- changes more frequently than topology information
## LDP and RSVP

Should be modified
- CR-LDP
	- Constraint based routing LDP
- RSVP-TE
	- RSVP for Traffic Engineering
To be used with OSPF-TE and IS-IS-TE


New possibilities:
- Traffic engineering
- guaranteed quality of service (not yet supported)
- per-class traffic engineering (synergy with DiffServ)
- fast fault recovery (less than 50ms)
# Traffic Engineering

Traditional IP routing: traffic funneled towards the destination, closer paths will have a lot of traffic and will slow everything down.
Some links will be overloaded, and other underutilized

TE wants to distribute the traffic to optimize the utilization of the links

Routers:
- Control Plane : run routing protocols, routing database
- Data Plane : packets are processed and forwarded, based on routing table

If we chose path according to load: the routing tables would be constantly updated with the new lowest loaded paths, making an inconsistent and unstable path towards the destination

with MLPS:
![[Pasted image 20250104174739.png]]
content of forwarding table determined by the Signaling.
Control plane keeps the routing table in the control plane, then create a LSP, which writes an entry in the forwarding table
Even if routing table changes, the forwarding table doesn't change --> valuable for traffic engineering


Traffic Engineering without MPLS : 
- ATM has been used on IP networks
- Two control plans : routers are ATM-unaware
- Great number of adjacencies : limited scalability

Traffic Engineering with MPLS : 
MPLS is IP-aware
Only one control plan operating on physical topology
- simpler
- greater scalability
## Fast Fault Recovery

When a link breaks, LSPs keep trying to cross that link (forwarding table not updated)
Backup/protection LSP also created, when a failure happens, backup is used
Re-routing node : node which will change link

**Edge-to-Edge re-routing** : from entry point to destination, new path
![[Pasted image 20250104180241.png]]
expensive solution, a whole new path needed.

**Link re-routing** : protection LSP for each link, when link fails, the only changes are on the nodes at the edge of the failed link (before and after : push label, route to new node, route to after node, pop label and keep going)
![[Pasted image 20250104180233.png]]
Issue : not easy to check for quality of traffic, if constraint link fails everything flows thru (edge-to-edge would be a better option)

## Label Stack Hierarchy and Scalability

Stacking labels allows introduces hierarchy : Various hierarchical levels, as needed for the necessary scalability
Routing tables of transit routers do not have to be comprehensive
(Simpler and faster exact match of labels rather than longest prefix matching)

When entering a new domain, new label is pushed on the packet. This way the routers will be grouped together, and will divide on exit of the domain

![[Pasted image 20250104181258.png]]

## Penultimate Hop Popping (PHP)

The last but one node on the path of an LSP pops the label 
The LER routes the packet based on IP address (or next label in stack) 

Implicit PHP : label is removed
Explicit PHP : swap the label with 0 (using EXP bits in header)

## 6PE

6PE may offer a nice answer to any NSP whose core network runs MPLS
Idea:
- keep core unchanged
- add support for IPv6 networks at the edge
- Distribute IPv6 routing info in MPLS/BGP in the same way as currently do with VPNs

Possible options for IPv6 over MPLS Networks: 
- native IPv6 over MPLS 
- IPv6 over Circuit Transport over MPLS
- 6PE

6PE is a good choice for NSPs that have MPLS core network and supports MPLS VPN services
IPv6 services are requested by a limited number of customers
- if the number of IPv6 customers increases so that the NSP requires most of the access routers to become 6PE, the NSP should consider to upgrade the whole network
The ISP wants to avoid either to fully upgrade the core network or to deploy IPv6-over-IPv4 tunnels

Requirements
- ISP has to upgrade the Provider Edge (PE) routers to support IPv6 to MP-BGP (and are Dual Stack)
- Core routers (P) don't need any change in terms of configuration or software 


1)6PE routers advertise their IPv4 address (network) into the IGP routing protocol of the IPv4/MPLS core network
Each router in the MPLS domain will eventually assign a label corresponding to the route for each 6PE router
( the PEs will discover each other and create mesh of LSPs)
![[Pasted image 20250104190131.png]]

2)Customer Edge (CE) will let the PE routers know what can reach a specific network
PE-2 can't advertise it to P2 because it doesn't understand IPv6
![[Pasted image 20250104190122.png]]

3)PE-2 will advertise over MP-iBGP that the network X is reachable via BGP
A label is also bind for MPLS to PE-2
![[Pasted image 20250104203329.png]]

4)Finally, IGP protocol propagates the advertisement within the network of the other customer
![[Pasted image 20250104203706.png]]

Forwarding IPv6 Traffic (data plane)
green label: from 3)
pink label: from 1)
![[Pasted image 20250104203829.png]]
