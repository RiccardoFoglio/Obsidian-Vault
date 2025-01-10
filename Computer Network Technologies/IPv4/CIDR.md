Classless InterDomain Routing

network portion of address is arbitrary length

Address format: **network ID** + **prefix length** or **netmask**
	prefix length: */x* where x is the number of bits in network portion od address
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


### Real Example

(recupera after break, 25/9)

