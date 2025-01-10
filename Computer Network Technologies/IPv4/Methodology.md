1. **Location of IP Network** : where will the IP addresses be needed? what devices are being used?
2. **Amount of required addresses** : how many hosts? remember p2p link also needs addresses
3. **Amount of allocated addresses** : hosts + 2 (net ID and broadcast) + routers interface
4. **Address range validity** : select address range (EX: 10.0.0.0/24, [[private address]])
5. **Netmask / Prefix Length** : 
	   LAN1 : 43 --> 255.255.255.192   /26
	   LAN2 : 103 -->255.255.255.128  /25
	   LAN3 : 4 --> 255.255.255.252     /30
6. **Address Range** : assignments in chunks of powers of 2
	   LAN3 : 10.0.0.0 - 10.0.0.3         -->  10.0.0.0/30
	   LAN1 : 10.0.0.64 - 10.0.0.127   -->  10.0.0.64/26
	   LAN2 :  10.0.0.128 - 10.0.0.255 --> 10.0.0.128/25
	   
7. **Host Addresses** : give addresses to each host from the ranges