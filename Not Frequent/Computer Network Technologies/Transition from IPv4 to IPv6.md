
ideally: incremental, seamless, smooth

could be achieved via:
- dual stack approach --> IPv6 as new layer-3 protocol, generate/receive v6 or v4 as needed
  ![[Pasted image 20241014084709.png]]
- address mapping
- tunneling 
- translation mechanisms

Step 1 : isolated IPv6 networks
![[Pasted image 20241014084749.png]]
Step 2 : IPv6 Islands grow
![[Pasted image 20241014084810.png]]
Step 3 : Native IPv6 connectivity
![[Pasted image 20241014084827.png]]
Step 4 : IPv6 takes over
![[Pasted image 20241014084846.png]]

Protocols specified since 1996
IPv6 is already implemented on routers:
- even if less stable than IPv4
- possibly missing some functionalities
- also hardware implementations (L-3 switches)

IPv6 implemented in end systems:
- Windows (since 2000 version)
- Unix : Ubuntu, Debian, FreeBSD
- MAC (since MAC OS X)

Large IPv4 install base
As of Oct 2023: IPv6 adopted globally at 45% rate 
	US: 48%
	Italy: 15%
	France / Germany : 75%

Only reason : Address Space Depletion

Assumption : IPv4 and IPv6 will coexist for a while; it's also a problem to solve
## Dual Stack Approach

- Both IPv4 and IPv6 capabilities
	- all hosts and routers support IPv6
	- IPv4 support can be gradually removed once all hosts have IPv6
- Hosts communicate natively with both
- complete duplication of all protocol stack components
	- routing protocols
	- routing table
	- access list
![[Pasted image 20241014090048.png]]

Limitations : doesn't reduce the need for IPv4 addresses, each host still needs an IPv4 address to use IPv4. Applications have the responsibility whether to use IPv4 or IPv6.

Not all hosts will be dual stack. 
IPv6 hosts shall communicate with IPv6 hosts through an IPv4 network (same for IPv4 through IPv6 network)

IPv6 hosts shall communicate with IPv4 hosts: translation mechanism must be used, no targeting IPv4 hosts contacting IPv6 ones, difficult to map the large IPv6 address space on smaller IPv4 address space
## Tunneling Solutions

Dual stack and native IPv6 hosts exchange IPv6 packets through an IPv4 network
![[Pasted image 20241014090443.png]]

Tunnelling : encapsulation of IP (v6) packets into IP (v4) packets
Emulates a "direct" link among IPv6 devices

Dual Stack router capable of this encapsulation, routing table
![[Pasted image 20241014090633.png]]

End points : hosts and routers
Protocols :
	GRE (generic routing encapsulation)
	IPv6 in IPv4
Set up : manual and automatic
	IPv4 compatible addresses, 6over4, 6to4, Tunnel Broker, ISATAP, Teredo
#### 6to4
Each 6to4 router is assigned a unique IPv6 address that begins with the prefix 2002::/16 followed by its 32-bit IPv4 address.
Packets sent by an IPv6 host are encapsulated in IPv4 by 6to4 router and routed through IPv4 Internet to another 6to4 router
The 6to4 router decapsulates it to reveal the original IPv6 packet and route it to the destination

Not meant for IPv4 host to IPv6 host communication

Problems: 
- address conflicts and misconfigurations
- not all networks had globally routable IPv4 addresses available
- NAT and firewalls interfered with 6to4 deployment
- security vulnerabilities, particularly related to routing and address spoofing
## Scalable Carrier-grade Solutions

still need to support:
- IPv4 servers possibly communicating with IPv6 hosts
- IPv4 clients

Several options: 
- DS-Lite
- A+P (DS-Lite evolution)
- NAT64

#### NAT (Network Address Translation)
still widely used today, old solution
![[Pasted image 20241014091900.png]]

Private address not routable on global internet, so when outbound packet arrives to NAT, it replaces the source address with its own IP address

Problematic with inbound sessions (like servers, NAT + STUN/TURN may be ok for peer-to-peer sessions)
Bottleneck and single point failure

Several independent cascaded instances of NAT are now common (starting from virtual machines)
Difficult to do without due to scarce addresses

Same architecture with IPv6:
![[Pasted image 20241014093310.png]]

AFTR : Address Family Transition Router
allows IPv6 host to communicate with IPv4 hosts over an IPv6 carrier infrastructure (residential host and current providers) 
Features an IPv6 tunnel concentrator and possibly a large-scale NAT
In use in DS-Lite and A+P

#### DS-Lite (Dual-Stack Lite)
Dual-stack at the edge
IPv6-only Service Provider backbone
![[Pasted image 20241014093654.png]]
Reduces requirement for IPv4 addresses compared to dual-stack approach
	dual-stack requires public IPv4 address per host
Extended NAT enables customer assigned addressing : IPv6 address of CPE in NAT table

Limitations:
- NAT is not under control of customer (same problem as with CGN)
- Problematic with servers (static mapping and port forwarding cannot be configured)
#### A+P (Address plus Port)
NAT is under control of customer
Ranges of TCP/UDP ports are assigned to each customer (only ports used by NAT on outside)
![[Pasted image 20241014094025.png]]
No problem with overlapping private address spaces at customers'
Ports can be assigned automatically to CPE using PCP (Port Control Protocol) --> CPE can negotiate more ports any time
AFTR is just IPv4-in-IPv6 tunnel terminator (NAT44 no longer needed in the AFTR)
#### NAT64 + DNS64
![[Pasted image 20241014094506.png]]
Principles:
- IPv6 prefix dedicated to mapped IPv4 addresses, either well-known or network-specific
- DNS64 maps A record into AAAA using NAT64 prefix, then serves A and AAAA records to client
- NAT64 router advertises NAT64 prefix into IPv6 network to attract traffic toward IPv4 hosts
![[Pasted image 20241014094630.png]]

NAT64 Packet Forwarding : 
- translates IPv6 address and packet into IPv4
- Picks a free IPv4 address/port from its pool
- builds NAT session entry

Limitations:
- Only when the DNS is involved (hostnames used, doesn't work in case the user directly specifies an IPv4 address)
- No DNSSEC
	- In DNSSEC authoritative DNS signs record
	- but DNS64 modifies records