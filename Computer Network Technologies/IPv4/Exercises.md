# Exercise 1

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
# Exercise 2 Valid or Invalid?

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

# Exercise 3

Find the configuration error in the network setup and explain why the network doesn't work properly.

![[Pasted image 20240925184232.png]]

DNS must be reachable.

DNS2:
	Net ID: 192.168.1.{0000}
	Host ID: 4 bits

But IP: 192.168.1.23/28 --> 23: 0001-0111

so it's impossible to reach because the 23 goes beyond the Host ID and to the Net ID

UNREACHABLE


# Exercise 5
![[Pasted image 20241003124753.png]]

R2 Routing Table:

| DESTINATION      | NEXT-HOP         | IF   |
| ---------------- | ---------------- | ---- |
| 0.0.0.0/0        | 130.192.3.6 (R3) | eth2 |
| 130.192.2.128/25 | ------------     | eth1 |
| 130.192.2.0/25   | 130.192.3.1 (R1) | eth0 |
R1 takes: [0...63] and [64...79]

Destination address : 130.192.2.140 --> match row 2 (and 1)
Destination address : 130.192.2.3 --> match row 3 (and 1)

