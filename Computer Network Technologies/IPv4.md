# Terminology
IP = Internet Protocol
v4 = Version 4, previous ones not really important

**IP Address** = 32 bit identifier for host and router interfaces
	*Nominally Unique*
	decimal form : \[8b. 8b. 8b. 8b] --> x.y.z.w each 0-255

**Network ID** = High order bits of the IP
**Host ID** = Low order bits of the IP
length of each part is variable and depends on the needs of the network

**IP Network** = set of IP devices whose interfaces:
- have the same network part of the IP Address
- are connected to the same physical (Link-Layer) network

- Terminals = laptop, tablet etc...
- Router = layer 3 device, L3 protocol (IP)
- Switch = L2 device, can't read IP protocols
- Access point = L2 device, unless specific devices which are L2+L3

![[Pasted image 20240923212144.png|500]]

These are 2 subnetworks joined by a Router
# Special Addresses

- Value. all zeros --> Subnetwork ID (233.2.0) all devices in subnetwork
- all 1s --> Limited broadcast (local net), reach all destinations available locally (L2 packet, mapping)
- value. all 1s --> direct broadcast for the Network
- 127. something --> loopback (127.0.0.1), local host

![[Pasted image 20240923212637.png]]
# IP Addressing Classes

Class Purpose: make the size of Net ID evident.

- Class A: First Bit: 0
	Net ID: first **8 Bits** --> 128 networks
	Host ID: Remaining bits (24 bits) --> $2^{24}$ possible Host addresses
	
	Inefficient, under-utilized, too many host slots for the number of networks available

- Class B: First 2 Bits: 10
	Net ID: first **16 bits** --> 16k networks
	Host ID: remaining 16 bits
	
	under utilized

- Class C: First 3 Bits: 110
	Net ID: first **24 bits** --> 2M networks
	Host ID:  remaining 8 bits --> very small, 254 hosts (all 0s and 1s off limits)
	
	Inefficient

![[Pasted image 20240923212928.png]]

Routing depends on the Net ID, therefore the IP address is hierarchical

New Solution: CIDR
# CIDR
Classless InterDomain Routing

network portion of address is arbitrary length

Address format: **network ID** + **prefix length** or **netmask**
	prefix length: */x* where x is the number of bits in network portion of address
	netmask: all *1s* in the network part, all *0s* in the host part

![[Pasted image 20240925162349.png]]

IP Address (binary): 11001000 00010111 00010000 0000000
IP Address (decimal) : 200.23.16.0

Binary format: 11111111 11111111 11111110 00000000
Prefix Length Notation: 200.23.16.0/23
Netmask Notation: 255.255.254.0

## Valid Netmasks

possible values in the 4 byte composing the address
(in bracket: number of available host ID)
(broadcast : Host ID all 1s)
![[Pasted image 20240925163100.png]]
IP Address : 130.192.6.0   255.255.255.x 

- 254 valid addresses
	- net ID : 130.192.6.0/24
	- Broadcast : 130.192.6.255

- 126 valid addresses
	- Net ID : 130.192.6.0/25
	- Broadcast : 130.192.6.127

- 62 valid addresses
	- Net ID : 130.192.6.0/26
	- Broadcast : 130.192.6.63


252 Netmask : 30 bits Net ID, only 2 bits for Host ID, so only 2 addresses available (smallest you can have)

254 not valid, it would identify a network where you only have 1 bit for Host ID, so no actual space for the hosts to be connected

255 used to represent a single device

### Examples

130.192.0.0/16 -> first 16 bits for Net ID
	broadcast: 130.192.255.255
	IP example: 130.192.8.32

130.192.2.0/23 -> first 23 bits for Net ID, inside 3rd byte -> netmask: 255.255.254.0
	broadcast: 130.192.3.255 (0000 001-0 --> 000000**11** 11111111)
	IP example: 130.192.2.255

130.192.1.4/30 -> x x 00000001 000001-00
	broadcast: 130.192.1.7
	Possible IDs: 130.192.1.5 130.192.1.6

130.192.1.0/31 -> INVALID, 31 bits for Net ID -> 1 bit for Host ID not enough

### Valid / Invalid Net ID

Valid Net ID: in the Host ID it's not all 0s
- 130.192.1.4/30 -> (last byte) 0000 01-**00** 
- 130.192.1.16/30 -> 0001 00-00
- 130.192.1.16/29 -> 0001 0-000

Invalid Net ID: after the net ID there's at least a 1, odd number in last byte -> invalid net ID
- 130.192.1.1/30 -> 0000 00-01
- 130.192.1.4/29 -> 0000 0-100
- 130.192.1.24/28 -> 0001-1000

# IP Routing

Given a destination IP address to reach, an IP device (ROUTER) search its own routing table, looking for a Match
In case of multiple matches, it selects the most specific one (longest prefix matching)

![[Pasted image 20240925180253.png]]
## Example

packet, coming in thru Interface 3 (I3)

3. Packet: 200.23.19.45
- 1st row -> first 20 bits: match
	- 16: 0001 0000
	- 19: 0001 0011
- 2nd row -> first 23 bits: match (longer)
	- 18: 0001 0010
	- 19: 0001 0011
- 3rd row -> 23 31 different, for sure shorter than 23
	- 31
	- 23
-> 2nd row is the best match
# Exercises
## Exercise 1

Define netmask, longest prefix length and available addresses

| Number of Hosts | Netmask         | Prefix Length   | Available Addresses |
| --------------- | --------------- | --------------- | ------------------- |
| 2               | 255.255.255.252 | /30 (1111 1100) | 4 (2)               |
| 27              | 255.255.255.224 | /27 (1110 0000) | 32 (30)             |
| 5               |                 | /29 (1111 1000) |                     |
| 100             |                 |                 |                     |
| 10              |                 |                 |                     |
| 300             |                 |                 |                     |
| 1010            |                 |                 |                     |
| 55              |                 |                 |                     |
| 167             |                 |                 |                     |
| 1540            |                 |                 |                     |
## Exercise 2 Valid or Invalid?

check if the host ID is all 0s (V) or has 1s (I)

| IP / Prefix Length Pair | Valid or Invalid? |
| ----------------------- | ----------------- |
| 192.168.5.0 / 24        | V                 |
| 192.168.4.23 / 24       | I                 |
| 192.168.2.36 / 30       | V                 |
| 192.168.2.36 / 29       | I                 |
| 192.168.2.32 / 28       | V                 |
| 192.168.2.32 / 27       | V                 |
| 192.168.3.0 / 23        | I                 |
| 192.168.2.0 / 31        | I (31)            |
| 192.168.2.0 / 23        | V                 |
| 192.168.16.0 / 21       | V                 |
| 192.168.12.0 / 21       | I                 |
## Exercise 3

Find the configuration error in the network setup and explain why the network doesn't work properly.

![[Pasted image 20240925184232.png]]

DNS must be reachable.
DNS2:
	Net ID: 192.168.1.{0000}
	Host ID: 4 bits
But IP: 192.168.1.23/28 --> 23: 0001-0111
so it's impossible to reach because the 23 goes beyond the Host ID and to the Net ID
UNREACHABLE
## Exercise 5
![[Pasted image 20241003124753.png]]

R2 Routing Table:

| DESTINATION      | NEXT-HOP         | IF   |
| ---------------- | ---------------- | ---- |
| 0.0.0.0/0        | 130.192.3.6 (R3) | eth2 |
| 130.192.2.128/25 | ------------     | eth1 |
| 130.192.2.0/25   | 130.192.3.1 (R1) | eth0 |
R1 takes: \[0...63] and \[64...79]

Destination address : 130.192.2.140 --> match row 2 (and 1)
Destination address : 130.192.2.3 --> match row 3 (and 1)
# Methodology
1. **Location of IP Network** : where will the IP addresses be needed? what devices are being used?
2. **Amount of required addresses** : how many hosts? remember p2p link also needs addresses
3. **Amount of allocated addresses** : hosts + 2 (net ID and broadcast) + routers interface
4. **Address range validity** : select address range (EX: 10.0.0.0/24, private address)
5. **Netmask / Prefix Length** : 
	   LAN1 : 43 --> 255.255.255.192   /26
	   LAN2 : 103 -->255.255.255.128  /25
	   LAN3 : 4 --> 255.255.255.252     /30
6. **Address Range** : assignments in chunks of powers of 2
	   LAN3 : 10.0.0.0 - 10.0.0.3         -->  10.0.0.0/30
	   LAN1 : 10.0.0.64 - 10.0.0.127   -->  10.0.0.64/26
	   LAN2 :  10.0.0.128 - 10.0.0.255 --> 10.0.0.128/25
	   
7. **Host Addresses** : give addresses to each host from the ranges

### Private Address
private address are unique only in a local network

devices can have the same IP address around the world, but not in the same network

Network Address Translator (NAT) : when private IP device wants to reach the internet, it sends a packet to router, and NAT replaces private source with a public interface IP address, and it keeps track of who sent the request.