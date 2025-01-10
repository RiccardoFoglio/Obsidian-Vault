More wireless phone subscribers than fixed : 10-to-1
4G/5G cellular networks now embracing Internet Protocol stack, including SDN
2 important changes:
- wireless: communication over wireless link
- mobility: handling the mobile user who changes point of attachment to network

## Elements of a Wireless Network

- Wireless Hosts : laptop, smartphones etc...
- Base Stations : connected to wired network, relay - responsible for sending packets between wired network and wireless hosts in its area
- Wireless Link : typically used to connect mobiles to base station, also used as backbone link. Multiple access protocol coordinates link access. Various transmission rates and distances, frequency bands


- Infrastructure mode : base station connects mobiles into wired network. Handoff: mobile changes base station providing connection into wired network
![[Pasted image 20241023152019.png]]

- ad hoc mode : no base stations, nodes can only transmit to other nodes within link coverage, nodes organize themselves into a network: route among themselves
![[Pasted image 20241023152030.png]]

## Wireless Link Characteristics

Important differences from wired link:
- decreased signal strength: radio signal attenuates as it propagates through matter (path loss)
- interference from other sources: wireless network frequencies shared by many devices
- multipath propagation: radio signal reflects off objects ground, arriving at destination at different times

SNR = Signal-to-Noise Ratio : larger SNR the easier to extract signal from noise
BER = Bit Error Rate

Given physical layer : increase power --> increase SNR --> decrease BER
Given SNR : choose physical layer that meets BER requirement, giving highest throughput
![[Pasted image 20241023152940.png]]

Hidden Terminal Problem:
	B, A hear each other
	B, C hear each other
	A, C cannot hear each other --> unaware, they both send packet to B: interference

# Cellular Network

Network where a geographical area is covered by tessellation through adjacent or overlapping areas, called *Cells*
User terminals can move from one cell to another without communication disruption (handover)

In theory:
![[Pasted image 20241029172419.png]]
base station (antenna) at center of the cell, isotropic (360 degrees)
cells = regular hexagonal shape

in reality:
![[Pasted image 20241029172502.png]]
3 directional antennas with 120 degree opening, located at one extreme of each cell.
3 co-located antennas
## Cells
cells are not regular hexagon and don't have the same size.
Cell shape and size is determined by:
- power emitted at the antenna
- antenna height
- antenna gain
- morphology of geographical area
- propagation conditions
Macrocells vs Microcells: different size, depending on the specific needs

## Channel Access
Multiple user channels: wireless channel is a common resource that has to be shared among multiple users.
Possible techniques to share resources:
- FDMA (Frequency Division Multiple Access)
- TDMA (Time DMA)
- CDMA (Code DMA)
- SDMA (Space DMA)

Given the limited number of available resources, we need to ensure coverage and serve a high number of users.
Solution: reuse the same frequencies at different locations

We define as a cluster the set of G adjacent cells that use all the frequencies available to one
7-cell cluster:
![[Pasted image 20241029184356.png]]

Given a cluster size, the system capacity increases as the cell radius decreases

Given G:
- the smaller the R the larger the capacity
- the smaller the R, the larger the number of antennas needed to ensure the same total coverage
![[Pasted image 20241029185407.png]]

Given R: 
- the larger the G the smaller the number of channels er cell, the lower the system capacity
- the larger the G the larger the distance between co-channel cells, the less the interference, the better the quality
![[Pasted image 20241029192129.png]]

Splitting: split large cells into a set of small cells

Cell shaping: it's possible to use directional antennas to have cells with an ad-hoc size and shape. It's possible to create multi-layer cell coverage (umbrella coverage)
Microcells that follow the user as they move

Power control: needed to reduce interference and energy consumption

Frequency allocation
- Fixed Channel Allocation (FCA) : based on concept of cluster, frequencies are assigned in a static way, frequency plan is changed only rarely to improve performance and adapt to slow variations in user traffic
- Dynamic Channel Allocation (DCA) : resources assigned to cells by a central controller when needed, frequency plan changes over time to adapt to system status
- Hybrid Channel Allocation Scheme (HCS) : one portion is FCA the other is DCA

## Basic Procedures
### Registration

allows a mobile terminal to connect with the network and to identify and authenticate itself.
It's performed when a terminal is switched on and has to associate with the network, whenever a users wants to access a service (ex: start a call) or periodically
### Mobility Management

We need procedures to handle mobility:
- **roaming** : user position has to be traceable even if they move over the network area. the system has to store the user position in a database to locate users whenever needed. to store user positions the network area is divided into location areas (LAs)
- **location updating** : procedure through which the user position is updated. A control channel is periodically broadcasted in each cell of an LA. When the mobile terminal receives an LA different from the one previously stored. the user starts a location updating procedure in order to update the position in the database
- **paging** : procedure through which the system notifies a mobile terminal about an incoming call/data delivery. the system broadcasts a paging message within the LA where the user is
- **handover** : procedure that enables the transfer of an active connection from one cell to another, while the mobile terminal moves over the network area. Complex procedure that poses constraints on the network architecture, protocols and signaling

Handover classification:
- intra vs inter cell : indicates whether the handover is between frequencies within the same cell or different
- soft vs hard : whether during handover both radio channels are active (soft) or only one at the time
- MT vs BS initiated : whether first control message to start handover is sent by mobile terminal or BS
- Backward vs Forward : it indicates whether handover signaling occurs via origin BS (backward) or destination BS

## GSM 2nd Gen

Operators made huge investments in the 90s to create the GSM infrastructure. IT's still viable for voice communication.
Using GSM today for phone calls frees up frequencies that can be used for data in 4G and 5G

Services offered by GSM: Voice full rate (13kbit/s) or half rate (6.5kbit/s), SMS and supplementary services
### Mobile Station (MS)
User Equipment. Several types depending on the application:
- mobile phones, mobile devices, antennas on vehicles
Different transmitted power at antenna:
- 2W for mobile phones
- 8W for mobile devices
- 20W for car antennas
### Subscriber Identity Module (SIM)
MS = hardware only, to connect you need a SIM
SIM = smart card with processor and memory to be inserted in the SIM reader
SIM stores (encrypted) user information: phone no., accessible services, security parameters etc...
### Base Station Subsystem (BSS)
Base Station (BS)
- Base Transceiver Station (BTS)
	- physical interface in charge of transmission and reception
	- point of access for the MT
	- unlike other signal sources, BTW transmit signal only towards users that are active
	- Up to 32 FDM channels per BTW (half uplink, half downlink)
- Base Station Controller (BSC)
	- Resource control on the radio interface: BSCs and BTSs communicate over a wired link
	- A BSC controls a high number of BTSs : from 10s to 100s
	- typically BSCs are collocated with one MSC, instead of being located near the BTSs
	- BSC Main functions:
		- Voice transcoding 13kb/s <-> 64kb/s
		- Paging
		- Radio resource control: dynamic frequency assignment to BTSs
		- Signal quality measurement
		- Management of handover between BTWs controlled by the same BSC

### Network and Switching Subsystem (NSS)
Main functions:
- call handling
- service support
- mobility support
- authentication
![[Pasted image 20241031163330.png]]

- MCS : Mobile Switching Center -> mobility support, call routing, GMSC interface between GSM network and other networks
- HLR : Home Location Register -> database storing permanent user data and dynamic data to handle user mobility
- VLR : Visitor Location Register -> database storing info related o MTs currently in the area controlled by MSC
- AuC : Authentication Center -> authentication based on challenge & response control. Generations of encryption keys for over-the-air communication
- EIR : Equipment Identity Register -> Database of stolen devices

IMEI code of the device

### GSM Frequencies (Europe)
![[Pasted image 20241031164549.png]]

Channel access: FDMA / TDMA
Frequency spectrum divided into FDM channels, each 200kHz wide
Each FDM channel is divided into TDM frames
Each TDM frame is divided into 8 slots
$$Frequency + timeslot = physical\_channel$$
Transmission organized into "bursts". blocks of data that are transmitted on a physical channel
Transmission speed: 271kbit/s
Slot time : 0.577ms (1 slot carries 156.25 bits <-- 0.577 * 271kbps)
Each time slot caries 1 transmission burst
Slots are grouped into TDM frames, each of 8 slots
![[Pasted image 20241031165213.png]]

Circuit-switched network: each call @ full rate (13kb/s) is assigned 1 slot per frame in UL and 1 slot per frame in DL.
Each MT transmits on a carrier for one time slot, and remains silent during the other 7 slots
Frames on UL and DL are sync-ed on a time slot basis and shifted by 3 slots
Separating UL/DL transmissions in frequency and time allows the use of one transceiver only
![[Pasted image 20241031165852.png]]

Non-zero propagation times: bursts transmitted by MTs might arrive at the VTS when the slot is already finished, possible collisions
Solution : timing advance. transmission at the MT starts before the real timeslot beginning

![[Pasted image 20241031165934.png]]

Burst Structure
![[Pasted image 20241031171029.png]]

GSM Logical Channels:
- Physical channels: combination of timeslot and carrier, 8 physical channels per carrier
- Logical channels: carry useful information, specify "what" is transmitted. Mapped into physical channels according to proper criteria
- Two types of logical channels: control and traffic
#### Control channels
Network signaling: cell parameters, synchronization, receiver tuning
User signaling: call control, signal quality control

## LTE: 4th Gen

Speeds up to 300Mb/s DL - 50Mb/s UL with modulations up to 64 QAM. 10ms data latency
Up to 20MHz channels
![[Pasted image 20241031172851.png]]

- DL Downlink : data going from the network to the user device
- UL Uplink : data going from the user device to the network
- User plane : all the operations related to the transport of user data in DL and UL
- Control plane : all operations related to the setup, control and maintenance of communications between user and network.

- Radio Access Network (RAN) : it includes all devices involved with the direct interaction with user devices --> E-UTRAN
- Core Network (CN) : it includes all devices responsible for the transport of data to/from the Internet or towards other users --> EPC

![[Pasted image 20241104084732.png]]
### EPC
- clean-slate design
- packet-switched transport for traffic belonging to all QoS classes including conversational, streaming, real-time non real-time and background
- Radio resource management for end-to-end QoS, transport for higher layers, load sharing/balancing, policy management/enforcement across different radio access tech.
- Integration with existing 3GPP 2G and 3G networks

Functions:
- Network access control
- Packet routing
- Security
- Mobility management
- Radio Resource management
- |Network management
- IP networking functions

Components:
![[Pasted image 20241104085642.png]]
- Mobility Management Entity (MME)
	- resides in Control Plane, supports user equipment context, identity, authentication and authorization. Mainly perform Non Access Stratum procedures, consisting of two main groups:
		- functions related to bearer management
		- functions related to connection and mobility management
- Serving Gateway (S-GW)
	- resides in User Plane, receives and sends packets between the eNodeB and the core network. Perform packet routing and forwarding within EPC. Anchor point for intra LTE-mobility. Lawful intercept.
- Packet Data Network Gateway (P-GW)
	- Resides in User Plane, connects the EPC with external networks/Internet. Basically a router that performs UE IP assignment, per user packet filtering and NAT services. Anchor point for mobility with non-3GPP access network and visited networks. Lawful intercept
- Home Subscriber Server (HSS)
	- Database of user-related and subscriber-related info. Used for authentication (with MME) and authorization. Similar to HLR in GSM.

### LTE Bearers
![[Pasted image 20241104085946.png]]
bearer as a pipe (tunnel) that carries data from the UE to elements of the LTE architecture.
one default bearer is established to the P-GW whenever the UE is activated
the UE can establish other dedicated bearers to other networks based on QoS requirements
Example: streaming video over internet can be done over a dedicated bearer. Dedicated bearers can use a bandwidth guarantee (bitrate, or GBR guaranteed)

bearer is a concatenated tunnel consisting of 3 portions established in the following order:
- S5 bearer : connects the S-GW to the P-GW. (the P-GW to Internet is called S8) 
- S1 bearer : connects eNodeB to S-GW. Handover establishes a new S1 bearer for end-to-end connectivity
- Radio bearer : connects the UE to the eNodeB. This bearer follows the mobile user under the direction of the MME as the radio network performs handovers when the user moves from one cell to another

![[Pasted image 20241104090511.png]]

- E-UTRAN : mainly consists of the eNodeB. An interface X2 interconnects between eNodeB. 
  Functions: radio resource management (radio mobility control, radio bearer control, scheduling and dynamic allocation of radio resources at uplink and downlink), header compression, security, connectivity to EPC 

## LTE Data Plane / Control Plane

![[Pasted image 20241104090837.png]]

LTE Data Plane Protocol Stack: first hop

LTE Radio Access Network
- Downstream channel: FDM, TDM within frequency channel (OFDM - orthogonal frequency division multiplexing)
- upstream: FDM, TDM similar to OFDM. Each active mobile device is allocated two or more 0.5ms time slots over 12 frequencies. Scheduling algorithm not standardized - up to operator. 100's Mbps per device possible

LTE Link Layer protocols
- Packet Data Convergence: header compression, encryption
- Radio Link Control (RLC) Protocol: fragmentation / reassembly, reliable data transfer
- Medium Access: requesting, use of radio transmission slots

LTE Data Plane Protocol Stack: Physical Channels

Transmitted buts fit into the frame structure according to predefined subdivisions called Physical Channels
Each channel conveys specific information related to user data, TX/RX parameters, eNB identity, network control etc... as well as to the format of the channel itself (FDD or TDD)
physical channels are divided into Downlink and Uplink channels. Each U/D channel is further divided into Data and Control

LTE Physical Uplink Channels (control)
- Physical Random Access Channel (PRACH)
- Physical Uplink Control Channel (PUCCH)
LTE Physical Uplink Channels (data)
- Physical Uplink Shared Channel (PUSCH)

LTE Data Plane Protocol Stack: Packet Core

Tunneling: mobile datagram encapsulated using GPRS Tunneling Protocol (GTP), sent inside UDP datagram to S-GW.
S-GW re-tunnels datagrams to P-GW.
Supporting mobility: only tunneling endpoints change when mobile user moves

LTE Data Plane Protocol Stack: associating with a BS

![[Pasted image 20241104093831.png]]

LTE Mobiles: sleep modes
as in WiFi, Bluetooth: LTE mobile may be put radio to "sleep" to conserve battery
- light sleep : 100ms of inactivity --> wake up periodically to check for downstream transmissions
- deep sleep : after 5-10s of inactivity --> mobile may change cells while deep sleeping, need to re-establish association

## Mobility

![[Pasted image 20241108133008.png]]

IMSI : unique identity sim card ID

![[Pasted image 20241108134042.png]]
Mobile associates with visited mobility manager
Visited mobility manager registers mobile's location with home HSS
end result : 
- visited mobility manager knows about mobile
- home HSS knows location of mobile

![[Pasted image 20241108134213.png]]

2 - configuring LTE control-plane elements
- mobile communicates with local MME via BS control-plane channel
- MME uses mobile's IMSI info to contact mobile's home HSS
	- retrieve authentication, encryption, network service information
	- home HHS knows mobile now resident in visited network
- mobile select parameters for BS-mobile data-plane radio channel

3 - configuring data plane tunnels for mobile
- Solution 1 : Home-Routed Roaming
	- visited network provides radio and transport network resources
	- actual data session is anchored back at the home PGW
- S-GW to home P-GW tunnel:
	- UE connects to S-GW and MME in visited network, which routes traffic back to the home PGW using the S8 interface
- Home network manages user authentication, data billing and IP address allocation
	- user's data session and policies are consistent with their home subscription

- Solution 2 : Local Breakout
-  S-GW to Visited P-GW tunnel : the UE's data traffic exists locally within the visited network
- Visited network handles both the control and user planes, including IP address allocation and data traffic, through own PGW
	- useful for applications requiring low latency or for offloading data traffic to reduce network load on home operator
	- home network might lose some control over QoS policies or billing

Handover between Base Stations in same cellular network
1. current BS selects target BS, sends handover request message to target BS
2. target BS pre-allocates radio time slots, responds with HR ACK with info for mobile
3. source BS informs mobile of new BS : mobile can now send via new BS, handoverlooks complete to mobile
4. source BS stops sending datagrams to mobile, instead forwards to new BS
![[Pasted image 20241108135809.png]]
5. Target BS informs MME that it is new BS for mobile : MME instructs S-GW to change tunnel endpoint to be (new) target BS
6. target BS ACKs back to source BS: handover complete, source BS can release resources
7. mobile's datagram now flow through new tunnel from target BS to S-GW
![[Pasted image 20241108140432.png]]

# 5G

TIM SEMINAR































# Wireless LANs (WiFi)

![[Pasted image 20241108140526.png]]

all use CSMA/CA for multiple access, and have base-station and ad-hoc network verisions

802.11 LAN Architecture
- wireless host communicates with base station
	- base station = access point (AP)
- Basic Service Set (BSS) (aka "cell") in infrastructure mode contains:
	- wireless hosts
	- access point : base station
	- ad hoc mode : hosts only

Access Point works on specific frequency : channel (2.4GHz / 5GHz etc...)
interference is possible : channel can be same as the chosen by neighboring AP

Arriving host: must associate with an AP
- scans channels, listening for beacon frames containg AP's name (SSID) and MAC address
- selects AP to associate with
- then may perform authentication
- then typically run DHCP to get IP address in AP's subnet

Passive or Active Scanning

passive scanning : 
- beacon frames sent from APs
- association request frame sent : H1 to selected AP
- association response frame sent from selected AP to H1

active scanning : 
- Probe request frame broadcast from H1
- Probe response frames sent from APs
- Association Request frame sent : H1 to Selected AP
- Association Response frame sent from selected AP to H1

Multiple access
avoid collisions: 2+ nodes transmitting at the same time
- 802.11 : CSMA - sense before transmitting --> don't collide with detected ongoing transmission by another node
- 802.11 : no collision detection --> difficult to sense collisions, high transmitting signal, weak received signal due to fading, can't sense all collisions in any case (hidden terminal, fading). 

Goal : avoid collisions --> CSMA/CollisionAvoidance

![[Pasted image 20241108142349.png]]

idea : sender reserves channel use for data frames using small reservation packets
sender first transmits small request-to-send (RTS) packet to BS using CSMA
	RTSs may collide with each other
BS broadcast clear-to-send CTS in response to RTS
CTS heard by all nodes
	sender transmits data frame
	other stations defer transmission

![[Pasted image 20241115132133.png]]

![[Pasted image 20241115132152.png]]

![[Pasted image 20241115132440.png]]
From .11 frame for WIFI to .3 ethernet frame because there's a physical link between Access Point and Router


![[Pasted image 20241115132506.png]]

Advanced capabilities: 
- Rate Adaptation: base station, mobile dynamically change transmission rate (physical layer modulation technique) as mobile moves, SNR varies
	- SNR Decreases, BER increase as node moves away from base station
	- When BER becomes too high, switch to lower transmission rate but with lower BER

![[Pasted image 20241115133923.png]]

- Power Management
	- Node-to-AP: "I'm gonna sleep until next beacon frame"
		- AP knows not to transmit frames to this node
		- node wakes up before next beacon frame
	- beacon frame: contains list of mobiles with AP-to-mobile frames waiting to be sent
		- node will stay awake if AP-to-mobile frames to be sent, otherwise sleep again until next beacon frame