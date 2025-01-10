
Given a destination IP address to reach, an IP device (ROUTER) search its own routing table, looking for a Match

In case of multiple matches, it selects the most specific one (longest prefix matching)

![[Pasted image 20240925180253.png]]

## Example

packet, coming in thru Interface 3 (I3)

1. Destination address: 199.31.2.7

(completa example 25/9)


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

