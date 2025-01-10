
# Introduction and Taxonomy

Routing = determination of optimal path from source to destination for packet transmission in a computer network. Based on routing algorithms and protocols. 
Control plane operation
## Forwarding 
transmission of packets from one network device (router) to the next hop in their route
- no real-time decision-making, just routing table look-up
- next-hop choice predetermined by a routing algorithm and written in the routing table
Data plane operation

Forwarding Procedures
- Forwarding by Network Address (most common)
- Label Swapping (MPLS)
- Source Routing (rarely used)

Forwarding phases
- Next-hop/output port selection (using routing tables)
- Switching: transfer to output port
- Transmission

Routing Algorithm Classification
- Non-adaptive algorithms (static)
- Adaptive algorithms (dynamic)
## Non-Adaptive Algorithms
- Fixed Directory Routing --> admin has full control, error prone, doesn't adapt to topology changes
	- static routing
	- manual configuration
- Selective flooding and similar
	- Random : randomly select an output port
	- Flooding : forward on any output port (except the one on which the PDU was received)
	- Deflection or "hot potato" : if correct port available, route on it, otherwise select an available port
## Adaptive Algorithms
- Centralized Routing
- Isolated Routing
- Distributed Routing : Distance Vector, Link State
### Centralized Routing
- One node: Routing Control Center RCC, collects info from all other notes
- Computes the path
- Distributes to all nodes

Pros : optimizes performance, Simplifies troubleshooting
Cons : single point failure, bottleneck, not suitable for highly dynamic networks
### Isolated Routing
Each node decides independently
No exchange of information 
E.g., Backward Learning --> IEE 802.1D switches
### Distributed Routing
- Routers co-operate in exchanging connectivity information
- each router decides independently, but **coherently**
- combines advantages of isolated and centralized routing
- 2 big families:
	- Distributed Routing with Partial Information : Distance Vector Algorithms
	- Distributed Routing with Global Information : Link State Algorithms
#### Distance Vector algorithms

- Every node (Router) periodically exchanges with adjacent nodes a Distance Vector, containing
	- Reachable destinations
	- Current distance to each destination
		- E.g., number of hops on the path

- Each node maintains two data structures:
	- Distance Table (DT): lists costs to any known destination through each neighbor (next hop)
	- Routing Table (RT): lists costs and next hops of selected routes to each destination

- A Distance Vector contains the same information as the RT, minus the next hop

- Nodes compare the received vector with the current DT and RT and modify them
	- Adding new destinations
	- Modifying routing if new paths are shorter
	- Modifying costs if needed n Bellman–Ford algorithm

Structure of Distance Table in every node: 
- One row for each possible destination
- One column for each adjacent node

Example: Element of node X’s DT, for destination Y through adjacent node Z
Cost of reaching Y from X, using Z as next hop
![[Pasted image 20241127130448.png]]
![[Pasted image 20241127130719.png]]

Issues:
- black hole : DoS attack by malicious node advertising impossibly short, non existent routs to attract traffic, which is then discarded
- count to infinity : misalignment of Routing Tables due to slow convergence time of the Distance Vector algorithm and inconsistent information at different nodes
- bouncing effect : (temporary) Loop caused by inconsistent Routing Tables following a link failure
Common Reason: lack of information on the global topology

(Partial) Solutions:
- **Split Horizon** : “*If C reaches destination A through B, it is useless for B trying to reach A through C*".
  Prevents loops between two nodes. Speeds up convergence. "personalized" DVs to neighbors. In actual implementations, route has to expire
- **Path Hold Down** : "*If link L fails, all destinations reachable through link L are considered unreachable for a certain period of time*"
  High convergence time for the examined node. The router that noted the fault may not participate to any loop at least until the timeout of Hold Down timer

When routes have increasing costs, routing loops may happen
- Cost-increasing routes in DVs are not used
- May be combined with Path Hold Down
- CON: Might block routes with legitimate cost increase

- **Split Horizon with Poisonous** : An invalid route is advertised at infinite distance/cost instead of just omitting it. It can substitute or complement Path Hold Down. It's more aggressive.
  Only advantage: faster convergence.
  No need to wait for route expiration
##### Bottom Line
- PROS:
	- Simple to implement and deploy
	- Very little configuration
- CONS:
	- Exponential worst-case complexity and convergence time $O(n^2)$
	- routers do not know the network topology
	- B cannot distinguish between:![[Pasted image 20241202191020.png]]
	- Convergence limited by slower links and routers set pace
	- Complex tuning and troubleshooting
	- Large routing traffic (and storage) : not suitable for large complex networks
##### Path Vector
Nodes exchange dynamically-updated path information.
Easy to detect loops in the vector
Increased overhead
![[Pasted image 20241203113934.png]]
#### Link State algorithms
Every node sends cost info (status) of each link in broadcast to all other nodes. Implemented with multicast among routers
Every node builds the network topology, knowing all link costs
Every node computes minimum cost path (paths) toward every other node. Info stored in routing table
Dijkstra algorithm is used to compute the optimal path: iterative algorithm, after k iterations the min cost path towards k destinations are obtained. Only works for positive costs
![[Pasted image 20241203114247.png]]

Low Complexity: $L\cdot \log(N)$ where L = n. of links, N = n. of nodes
Shortest path first : the next nearest to the root is identified. Its next hop is inserted into the routing table

PROs
- Rapid Convergence
- Limited Routing Traffic and Storage
- It rarely generates loops
- Simple to understand and troubleshoot

CONs
- high implementation complexity
- protocols with complex configuration

In principle: link state generation when there is a topology change
Actual protocols : LS are generated periodically (increased reliability)

# Internet Routing Architecture

Protocols that allow routers to exchange information on the network and determine the best route to each destination. Uses routing algorithms
Define metric and their encoding in packets
Specific timing
Problem: what is the operational domain of the protocol?
- A subnet?
- An operator's network?
- A country?
- The Internet at large?

Scale: with billions of destinations:
- can't store all destinations in routing tables
- routing table exchange would swamp links
- our routing description thus far:
	- idealized, all routes identical, network flat --> not true in practice

Administrative autonomy:
- internet = network of networks
- each network admin may want to control routing in its own network

Autonomous System:
- A set of subnets grouped based on topology and organizational criteria
- subnets of large internet service provider
- administration:
	- autonomous internal routing choices
	- negotiated external routing choices
- Scalability: not all info propagated everywhere
- Identification: AS number (2 bytes, assigned by IANA)

Internet Approach to scalable routing : aggregate routers into "autonomous systems" (AS) (a.k.a "domains")

- Intra-AS routing : routing among hosts, routers in same AS (network), all routers in AS must run same intra-domain protocol.
- Inter-AS routing : routing among AS'es, gateways perform inter-domain routing (as well as intra-domain routing)

Routers in different AS can run different intra-domain routing protocol.
Gateway router: at "edge" of its own AS, has links to routers in other AS'es

## Intra-AS Routing protocols

forwarding table configured by both intra-and inter-AS routing algorithm 
- intra-AS routing determine entries for destinations within AS 
- inter-AS & intra-AS determine entries for external destinations

suppose router in AS1 receives datagram destined outside of AS1: 
router should forward packet to gateway router, but which one?
AS1 must: 
1. learn which destinations are reachable through AS2, which through AS3 
2. propagate this reachability info to all routers in AS1 job of inter-AS routing!

also known as interior gateway protocols (IGP) 
most common intra-AS routing protocols: 
- RIP: Routing Information Protocol (obsolete) 
- OSPF: Open Shortest Path First (IS-IS protocol essentially same as OSPF) 
- IGRP: Interior Gateway Routing Protocol (Cisco proprietary for decades, until 2016)

OSPF (Open Shortest Path First)
- “open”: publicly available 
- uses link-state algorithm 
	- link state packet dissemination 
	- topology map at each node 
	- route computation using Dijkstra’s algorithm 
- router floods OSPF link-state advertisements to all other routers in entire AS 
	- carried in OSPF messages directly over IP (rather than TCP or UDP)
	- link state: for each attached link 
- IS-IS routing protocol: nearly identical to OSPF

Advanced Features:
- security: all OSPF messages authenticated (to prevent malicious intrusion)
- multiple same-cost paths allowed (only one path in RIP) 
- for each link, multiple cost metrics for different TOS (e.g., satellite link cost set low for best effort ToS; high for real-time ToS) 
- integrated uni- and multi-cast support: 
	- Multicast OSPF (MOSPF) uses same topology database as OSPF 
- hierarchical OSPF in large domains

Hierarchical OSPF
![[Pasted image 20241203132420.png]]

- two-level hierarchy: local area, backbone. 
	- link-state advertisements only in area 
	- each nodes has detailed area topology; only know direction (shortest path) to nets in other areas.
- area border routers: “summarize” distances to nets in own area, advertise to other Area Border routers.
- backbone routers: run OSPF routing limited to backbone. 
- boundary routers: connect to other AS’es

## Inter-AS Routing

Differences between Intra- and Inter-AS Routing:
policy: 
inter-AS: admin wants control over how its traffic routed, who routes through its net. 
intra-AS: single admin, so no policy decisions needed 
scale: hierarchical routing saves table size, reduced update traffic 
performance: 
- intra-AS: can focus on performance 
- inter-AS: policy may dominate over performance

BGP:
- BGP (Border Gateway Protocol): the de facto inter-domain routing protocol: “glue that holds the Internet together”
- BGP provides each AS a means to:
	- obtain subnet reachability information from neighboring ASes (eBGP)
	- propagate reachability information to all AS-internal routers (iBGP)
	- determine “good” routes to other networks based on reachability information and policy 
- Not necessarily shorter path 
	- Choice based on policies 
	- Reflect agreements among ASs 
- allows subnet to advertise its existence to rest of Internet: “I am here” 
- destinations can be aggregated: 195.1.2.O/24 and 195.1.3.O/24 can be announced as 195.1.2.O/23

BGP session: two BGP routers (“peers”) exchange BGP messages over semi-permanent TCP connection:
- advertising paths to different destination network prefixes (BGP is a “path vector” protocol)

when AS3 gateway router 3a advertises path AS3,X to AS2 gateway router 2c:
- AS3 promises to AS2 it will forward datagrams towards X

advertised prefix includes BGP attributes 
`prefix + attributes = “route”`
two important attributes: 
- AS-PATH: list of ASes through which prefix advertisement has passed 
- NEXT-HOP: indicates specific internal-AS router to next-hop AS

Policy-based routing:
- gateway receiving route advertisement uses import policy to accept/decline path (e.g., never route through AS Y).
- AS policy also determines whether to advertise path to other other neighboring ASes

BGP messages exchanged between peers over TCP connection
BGP messages: 
- OPEN: opens TCP connection to remote BGP peer and authenticates sending BGP peer
- UPDATE: advertises new path (or withdraws old) 
- KEEPALIVE: keeps connection alive in absence of UPDATES; also ACKs OPEN request 
- NOTIFICATION: reports errors in previous msg; also used to close connection

BGP Path Advertisement
AS2 router 2c receives path advertisement AS3,X (via eBGP) from AS3 router 3a 
Based on AS2 policy, AS2 router 2c accepts path AS3,X, propagates (via iBGP) to all AS2 routers
Based on AS2 policy, AS2 router 2a advertises (via eBGP) path AS2, AS3, X to AS1 router 1c
![[Pasted image 20241203134600.png]]

gateway router may learn about multiple paths to destination: 
- AS1 gateway router 1c learns path AS2,AS3,X from 2a 
- AS1 gateway router 1c learns path AS3,X from 3a 
- Based on policy, AS1 gateway router 1c chooses path AS3,X, and advertises path within AS1 via iBGP

router may learn about more than one route to destination AS, selects route based on: 
1. local preference value attribute: policy decision 
2. shortest AS-PATH 
3. closest NEXT-HOP router: hot potato routing 
4. additional criteria

Hot Potato Routing : choose local gateway that has least intra-domain cost (e.g., 2d chooses 2a, even though more AS hops to X): don’t worry about inter-domain cost!

BGP Connection Types
- **Transit** : an ISP can provide reachability to the entire Internet for another endpoint (e.g., enterprise, content or application provider, residential broadband provider, etc.). 
	- The endpoint entity pays the ISP to carry traffic to and from the Internet. 
- **Peering** : The networks interconnect to exchange only traffic that originates or terminates within their networks (including the networks of their customers, in the case of carriers and Tier 1 networks) 
	- Public peering through an internet exchange point (IXP) -> Peering with multiple networks 
	- Private peering: networks interconnect to exchange only traffic that originates or terminates within their networks

Private Peering:
- A,B,C are provider networks 
- X,W,Y are customer (of provider networks) 
- X is dual-homed: attached to two provider networks 
- Policy to enforce: X does not want to route from B to C via X -> Private peering
- .. so X will not advertise to B a route to C
![[Pasted image 20241203140150.png]]

Let us now look at a case of Peering Agreements among ISPs 
- A advertises path Aw to B and to C 
- B chooses not to advertise BAw to C: 
- B gets no “revenue” for routing CBAw, since none of C, A, w are B’s customers 
- C does not learn about CBAw path 
- C will route CAw (not using B) to get to w 

Rule of thumb: traffic flowing across an ISP’s backbone network must have either a source or a destination (or both) in a network that is a customer of that ISP;