
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

New Solution: [[CIDR]]
