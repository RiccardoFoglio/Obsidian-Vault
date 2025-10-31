Controller Area Network - CAN bus
is a vehicle bus standard that allows microcontrollers and devices to communicate without a host computer
message-based protocol designed originally for multiplex electrical wiring
data frame is transmitted serially
devices have priority and can transmit simultaneously
frames are received by all devices including the transmitting device

CAN devices that uses 11-bit identifiers : CAN 2.0A
CAN devices that uses 29-bit identifiers : CAN 2.0B

CAN bus is empowered with software alone. Cheaper than implemented hardwired using traditional automotive electronics

CAN BUS working principles are genius but challenging to understand:

- multi-master serial bus : everyone can be a master or slave
- 2 channels to transmit : more robust
- need transceiver : convert a single logic value into a couple of electronics signals

All nodes are sending something:
- talking nodes are sending either dominant or regressing values
- listening nodes are only sending regressing values

Master-Slave mode:
- single or no transmission :
	- when any device is transmitting a dominant (logical value 0) high-speed CAN signaling drives the CANH wire towards 3.5V and the CANL wire towards 1.5V
	- if no device is transmitting a dominant, the terminating resistors passively return the two wires to the recessive (logic value 1) state with a nominal differential voltage of 0V
	- receivers consider any differential voltage less than 0.5V to be recessive
	- the dominant differential voltage is a nominal 2V

Bit Timing

All notes on the CAN network must operate at the same nominal bit rate, sync necessary

Synchronization starts with a hard sync on the first recessive to dominant transition after a period of bus idle (start bit)
- resync occurs on every recessive to dominant transition (receive a 0)
- the CAN controller expects transition to occur at a multiple of the nominal bit time
- if the transition doesn't occur at the exact time the controller expects it, the controller adjusts the nominal bit time accordingly

time divided in quanta. Bit is divided in a set number of quanta.

CAN Frames:
- Data frame : frame containing node data for transmission
- Remote frame : a frame requesting the transmission of a specific identifier
- Error frame : frame transmitted by any node detecting an error
- Overload frame : inject a delay

Frame content composition
![[Pasted image 20241218153041.png]]
![[Pasted image 20241218155346.png]]

Acknowledge
1. Transmitter sends recessive (1)
2. ANY receiver can assert a dominant (0)
3. Receivers and receiver read bus value and validate the transmission if it is dominant (0)
4. If different (ACK = 1), retransmission is necessary
5. If data is NOT received correctly (i.e., wrong CRC due to strong noise on the line), the receiver sends ACK = 1

BUS arbitration – ID Bits
CAN data transmission uses a lossless bitwise arbitration method of contention resolution
This arbitration method requires all nodes on the CAN network to be synchronized to sample every bit on the CAN network simultaneously.

BUS arbitration – Multi-master 
BUS arbitration is necessary when more than one CAN node needs to send asynchronous messages to the other nodes
If the communication is not «dense», then it is less probable that they contend the BUS

USAGE RULES ARE NEEDED – see the scenario below:
1. Any node has its ID
2. Any node can act as a master
3. If a node currently uses the BUS as a master
	A. ALL connected CAN nodes know it (including the sender) because they got the «start of frame» bit 
	B. NONE is allowed to interfere with the CAN BUS till the frame is fully sent, till the «end of frame» bits
4. Along the transmission of the previous frame
	A. Messages may accumulate in the CAN controllers’ queues 
	B. They may come from different nodes
5. Who is «taking» the BUS among accumulated requests when it free up?

any node that transmits a logical 1, when another node transmits a logical 0, loses the arbitration and drops out.
A node that loses arbitration re-queues its message for later transmission and the CAN frame bit-stream continues without error until only one node is left transmitting


It's possible for a destination node to request the data from the source by sending a remote frame.
The RTR-bit must be set to 1, recessive, to use the remote frame

