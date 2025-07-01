## I/O Hardware

Variety of I/O devices: storage, transmission, human-interface
common conceps: signals from I/O devices interface with computer
- Port : connection point for device
- Bus : daisy chain or shared direct access
	- PCI bus common in PCs and servers, PCI Express (PCIe)
	- expansion bus connects relatively slow devices
	- Serial-Attached SCSI (SAS) common disk interface
- Controller (host adapter) : electronics that operate port, bus, device
	- sometimes integrated
	- sometimes separate circuit board (host adapter)
	- contains processor, microcode, private memory, bus controller etc... 
![[Screenshot 2025-06-25 at 4.16.52 PM.png|500]]

Fibre channel (FC) is complex controller, usually separate circuit board (host-bus adapter HBA) plugging into bus
I/O instructions control devices
Devices usually have registers where device driver places commands, addresses, and data to write or read data from registers after command execution
Devices have addresses used by I/O instructions and memory-mapped I/O

Polling: for each byte of I/O
1. ready busy bit from status register until 0
2. Host sets read or write bit and if write copies data into data-out register
3. host sets command-ready bit
4. controller sets busy bit, executes transfer
5. controller clears busy bit, error bit, command-ready bit when transfer done

Step 1 is busy-wait cycle to wait for I/O from device. It's reasonable if the device is fast, but inefficient if device slow. 

Polling can happen in 3 instruction cycles
- read status
- logical-and to extract status bit
- branch if not zero

CPU interrupt-request line triggered by I/O device, checked by processor after each instruction

Interrupt handler receives interrupts: maskable to ignore or delay some interrupts

Interrupt vector to dispatch interrupt to correct handler: context switch at start and end, based on priority, some nonmaskable, interrupt chaining if more than one device at same interrupt number.

![[Screenshot 2025-07-01 at 1.40.05 PM.png|400]]
Interrupt mechanism is also used for exceptions to terminate process, crash system due to hardware error.
Page fault executes when memory access error
System call executes via trap to trigger kernel to execute request
Multi-CPU systems can process interrupts concurrently if OS designed to handle it
Used for time-sensitive processing, frequent, must be fast

Latency: stressing interrupt management because even single-user systems manage hundreds of interrupts per second. macOS generates 23k interrupts over 10sec

Direct Memory Access : used to avoid programed I/O for large data movement. Requires DMA controller. Bypasses CPU to transfer data directly between I/O devices and memory.
OS writes DMA command block into memory
## Application I/O Interface

I/O System Calls encapsulate device behaviors in generic classes
Device-Driver layer hides differences among I/O controllers from kernel. New devices talking already-implemented protocols need no extra work
Each OS has its own I/O subsystem structures and device driver frameworks

Devices vary in:
- Character-stream or block
- sequential or random-access
- synchronous or asynchronous
- sharable or dedicated
- speed of operation
- read-write, read only, write only

![[Screenshot 2025-07-01 at 2.04.52 PM.png|400]]
![[Screenshot 2025-07-01 at 2.05.07 PM.png|400]]

Subtleties of devices handed by device drivers
Broadly I/O devices can be grouped by the OS into:
- block I/O
- Character I/O (stream)
- Memory-mapped file access
- Network sockets

direct manipulation of I/O device specific characteristics, usually an escape / back door

Block devices include disk drivers: commands include read write seek, 
raw I/O, direct I/O, or file-system access 
memory-mapped file access possible --> file mapped to virtual memory and clusters brought
Character devices include keyboards, mice, serial ports

Network Devices: varying enough from bloc and character to have their own interface.
Linux, Unix, Windows include socket interfaces --> separate network protocol from network operation, includes `select()` functionality
Approaches vary widely (pipes, FIFOs, streams, queues, mailboxes)

Clock and Timers: Provide current time, elapsed time, timer
Normal resolution: 1/60 second
Some systems provide higher-resolution timers
Programmable interval time used for timings, periodic interrupts

Nonblocking and Asynchronous I/O
- Blocking = process suspended until I/O completed --> easy to use and understand, insufficient for some needs
- Nonblocking = I/O call returns as much as available --> user interface, data copy (buffered I/O), implemented via multi-threading, returns quickly with count of bytes read or written
- Asynchronous = process runs while I/O executes --> difficult to use

![[Screenshot 2025-07-01 at 3.15.32 PM.png|400]]

Vectored I/O allows one system call to perform multiple I/O operations
scatter-gather method better than multiple individual I/O calls
- decreases context switching and system call overhead
- some versions provide atomicity
	- avoid worry about multiple threads changing data as reads / writes occurring

## Kernel I/O Subsystem

Scheduling : some I/O request ordering via per-device queue, some OSs try fairness, some implement QoS

Buffering : store data in memory while transferring between devices to cope with device speed mismatch, with device transfer size mismatch and to maintain copy semantics

Double buffering = 2 copies of data

Caching : faster device holding copy of data, always just a copy, key to performance, sometimes combined with buffering

Spooling : hold output for a device, if can serve only one request at a time (ex: printer)

Device reservation: provides exclusive access to a device, system calls for allocation and de-allocation, watch out for deadlock

Error handling: OS can recover from disk read, device unavailable, transient write failures, with a retry on read or write, and in more complex systems they track error frequencies and stop using devices with an increasing frequency of errors

I/O Protection: user process may accidentally attempt to disrupt normal operation via illegal I/O instructions
- All I/O instructions defined to be privileged
- I/O must be performed via system calls
	- memory-mapped and I/O port memory locations must be protected too

![[Screenshot 2025-07-01 at 3.22.05 PM.png|400]]

Kernel keeps state info for I/O components, including Open File Tables, Network Connections and character device state
Many complex data structures to track buffers, memory allocation = dirty blocks

Power Management : not strictly domain of I/O but it's related
Computers use electricity and generate heat that must be dissipated (cooling)
OS can help manage and improve use
- Cloud computing environments move virtual machines between servers
Mobile computing has power management as first class of OS aspect

Android Implements:
- Component-level power management : understands relationship between components and turns off unused components
- Wake locks : prevent sleep of device when lock is held
- Power collapse : device into very deep sleep, marginal power use
## Transforming I/O Requests to Hardware Operations

- Determine device holding file
- Translate name to device representation
- Physically read data from disk into buffer
- Make data available to requesting process
- return control to process

![[Screenshot 2025-07-01 at 4.27.18 PM.png|400]]

## STREAMS

STREAM = full-duplex communication channel between a user-level process and a device in Unix System V and beyond

STREAM consists of:
- STREAM head interfaces with the user process
- driver end interfaces with the device
- zero or more STREAM modules between them

Each module contains a read queue and write queue
Message passing is used to communicate between queues
	Flow control option to indicate available or busy
Asynchronous internally, synchronous where user process communicates with stream head
## Performance

I/O is a major factor for system performance:
- demands CPU to execute device driver, kernel I/O code
- Context switches due to interrupts
- data copying
- network traffic especially stressful

To improve performance:
- reduce number of context switches
- reduce data copying
- reduce interrupts by using large transfers, smart controllers, polling
- use DMA
- Use smarter hardware devices
- Balance CPU, memory, bus and I/O performance for highest throughput
- Move user-mode processes / daemons to kernel threads






