# Overview of Mass Storage Structure

Bulk of secondary storage for modern computers is hard disk drives (HDDs) and nonvolatile memory (NVM) devices 
HDDs spin platters of magnetically-coated material under moving readwrite heads 

- Drives rotate at 60 to 250 times per second 
- Transfer rate is rate at which data flow between drive and computer
- Positioning time (random-access time) is time to move disk arm to desired cylinder (seek time) and time for desired sector to rotate under the disk head (rotational latency) 
- Head crash results from disk head making contact with the disk surface -- That’s bad 

Disks can be removable

![[Screenshot 2025-04-02 at 5.38.20 PM.png|500]]

### Hard Disk Drives (HDD)

platters range from .85" to 14", commonly now 3.5", 2.5" and 1.8"
Range from 30GB to 3TB per drive
Performance:
- Transfer Rate : theoretical 6GB/sec
- Effective Transfer Rate : 1GB/sec
- Seek time from 3ms to 12ms -- 9ms is common for the desktop drives
- Average seek time measured or calculated based on 1/3 of tracks
- Latency based on spindle speed
$$
1 / (RPM / 60) = 60 / RPM
$$
Average latency = 1/2 latency

Access Latency = Average Access Time = Average Seek Time + Average Latency
- faster disks : 3ms + 2ms = 5ms
- slow disks : 9ms + 5.56ms = 14.56ms

Average I/O time = average access time + (amount to transfer / transfer rate) + controller overhead

### Nonvolatile Memory Devices

Solid-State Disks (SSD)
other forms include: USB Drives, DRAM Disk replacements, surface-mounted on motherboards and main storage in devices like smartphones

- more reliable than HDDs
- More expensive per MB
- Shorter lifespan
- less capacity
- much faster
- busses can be too slow --> connect directly to PCI 
- no moving parts so no seek time or rotational latency

Nonvolatile memory devices : present challenges
- read and written in "page" increments but can't overwrite in place
	- must be erased first --> erases happen in larger block increments
	- can only be erased a limited number of times before worn out
	- life span measured in drive writes per day (DWPD)
		- 1TB NAND drive with rating 5DWPD --> 5TB per day within warrantee period

NAND Flash Controller Algorithms
with no overwrite, pages end up mixing valid and invalid data.
to track which logical blocks are valid, controller maintains Flash Translation Layer (FTL) Table
it also implements garbage collection to free invalid page space
allocates overprovisioning to provide working space for GC
each cell has lifespan so wear leveling needed to write equally to all cells

![[Screenshot 2025-05-06 at 1.20.19 PM.png|500]]

### Volatile Memory

DRAM frequently used as mass-storage device : not technically secondary storage because volatile, but can have file systems, be used like very fast secondary storage

RAM Drives (many names, including RAM disks) present as raw block devices, commonly file system formatted

Computers have buffering, caching via RAM, so why RAM drives?
- Caches / buffers allocated / managed by programmer, operating system, hardware
- RAM drives under user control
- found in all major OSes

Used as high speed temporary storage
	Programs could share bulk date quickly, by reading/writing to RAM Drive

Disk attachements:
- I/O busses
	- ATA : Advanced Technology Attachment
	- SATA : Serial ATA 
	- eSATA
	- SAS : Serial attached SCSI
	- USB : Universal Serial Bus
	- FC : Fibre Channel
Most common: SATA
NVM much faster than HDD, new interface is called NVM Express NVMe connecting directly to PCI bus
Data transfers on a bus carried out by special electronic processors called controllers (or Host-Bus Adapters, HBAs)
- Host controller on the computer end of the bus, device controller on device end
- computer places command on host controller, using memory-mapped I/O ports

Disk Structure : large 1-dimensional array of logical blocks where logical block is the smallest unit of transfer
Address Mapping : 1-dimensional array of logical blocks is mapped into the sectors of the disk sequentially
- Sector 0 is first sector of the first track of the outermost cylinder
- Mapping proceeds in order through that track, then the rest of the tracks in that cylinder, and then through the rest of the cylinders from outermost to innermost
- Logical to physical address should be easy, except for bad sectors

HDD Scheduling: OS responsible for using hardware efficiently, for disk drives it means having fast access time and disk bandwidth
minimize seek time
seek time similar to seek distance
disk bandwidth is the total number of bytes transferred, divided by the total time between the first request for service and the completion of the last transfer

Many sources of disk I/O request:
- OS
- System processes
- User processes

I/O request includes input or output mode, disk address, memory address, number of sectors to transfer
OS maintains queue of requests, per disk or device
Idle disk can immediately work on I/O request, busy disk means work must queue
in the past, OS responsible for queue management, disk drive head scheduling
	now built into storage devices, controllers
	provide LBAs, will handle sorting of requests

Drive controllers have small buffers and can manage a queue if I/O requests (of varying "depth")
Several algorithms exist to schedule the servicing of disk I/O requests
The analysis is true for one or many platters

We illustrate scheduling algorithms with a request queue 0-199
	98, 183, 37, 122, 14, 124, 65, 67
	Head pointer: 53

FCFS
640 head movements
![[Screenshot 2025-05-06 at 1.48.14 PM.png|400]]

SCAN : disk arm starts at one end of the disk and moves toward the other end, servicing requests until it gets to the other end of the disk, where the head movement is reversed and servicing continues
SCAN Algorithm also called elevator algorithm
Illustration shows total head movement of 208 cylinders
![[Screenshot 2025-05-06 at 1.50.28 PM.png|400]]

C-SCAN : Provides more uniform wait time than SCAN
head moves from one end to the disk to the other, servicing requests as it goes
![[Screenshot 2025-05-06 at 1.51.26 PM.png|400]]

Selecting a Disk-Scheduling Algorithm
- SSTF is common and has natural appeal
- SCAN and C-SCAN perform better for systems that place heavy load on disk --> less starvation

To avoid starvation --> linux implements deadline scheduler
- maintains separate read and write queues, gives read priority
- implements 4 queues : 2R and 2W
	- 1R and 1W queue sorted in LBA order, essentially implementing C-SCAN
	- 1R and 1W queue sorted in FCFS order
	- all I/O requests sent in batch sorted in that queue's order
	- after each batch, check if any requests in FCFS older than configured age
		- if so, LBA queue containing that request is selected for next batch of I/O

in RHEL 7 : also NOOP and completely fair queueing scheduler (CFQ) also available, defaults vary by storage device

NVM Scheduling : no disk heads or rotational latency but still room for optimization
in RHEL 7 **NOOP** (no scheduling) is used but adjacent LBA requests are combined
- NVM best at random I/O, HDD at sequential
- Throughput can be similar
- **Input/Output operations per second** (**IOPS**) much higher with NVM (hundreds of thousands vs hundreds)
- But **write amplification** (one write, causing garbage collection and many read/writes) can decrease the performance advantage

##  Error Detection and Correction

Fundamental aspect of many parts of computing (memory, networking, storage)

**Error detection** determines if there a problem has occurred (for example a bit flipping)
- If detected, can halt the operation
- Detection frequently done via parity bit

Parity one form of **checksum** – uses modular arithmetic to compute, store, compare values of fixed-length words
- Another error-detection method common in networking is **cyclic redundancy check** (**CRC**) which uses hash function to detect multiple-bit errors

**Error-correction code** (**ECC**) not only detects, but can correct some errors
- Soft errors correctable, hard errors detected but not corrected

## Storage Device Management

**Low-level formatting**, or **physical formatting** — Dividing a disk into sectors that the disk controller can read and write
- Each sector can hold header information, plus data, plus error correction code (**ECC**)
- Usually 512 bytes of data but can be selectable

To use a disk to hold files, the operating system still needs to record its own data structures on the disk
- **Partition** the disk into one or more groups of cylinders, each treated as a logical disk
- **Logical formatting** or “making a file system”
- To increase efficiency most file systems group blocks into **clusters**
	- Disk I/O done in blocks
	- File I/O done in clusters

**Root partition** contains the OS, other partitions can hold other OSes, other file systems, or be raw
- **Mounted** at boot time
- Other partitions can mount automatically or manually

At mount time, file system consistency checked
- Is all metadata correct?
	- If not, fix it, try again
	- If yes, add to mount table, allow access

Boot block can point to boot volume or boot loader set of blocks that contain enough code to know how to load the kernel from the file system
- Or a boot management program for multi-os booting

Raw disk access for apps that want to do their own block management, keep OS out of the way (databases for example)

Boot block initializes system
- The bootstrap is stored in ROM, firmware
- **Bootstrap loader** program stored in boot blocks of boot partition

Methods such as **sector sparing** used to handle bad blocks
![[Screenshot 2025-05-06 at 2.08.22 PM.png|400]]

Swap-Space Management

Used for moving entire processes (swapping), or pages (paging), from DRAM to secondary storage when DRAM not large enough for all processes

Operating system provides **swap space management**
- Secondary storage slower than DRAM, so important to optimize performance
- Usually multiple swap spaces possible – decreasing I/O load on any given device
- Best to have dedicated devices
- Can be in raw partition or a file within a file system (for convenience of adding)
- Data structures for swapping on Linux systems:
- ![[Screenshot 2025-05-06 at 2.10.02 PM.png|300]]


Storage Attachments
- host-attached --> through local I/O ports, using one of several technologies
- network attached
- cloud

Network-Attached Storage (NAS) : storage made available over a network rather than over a local connection. Remotely attaching to file systems
NFS and CIFS are common protocols
Implemented via remote procedure calls (RPCs) between host and storage over typically TCP or UDP on IP network
iSCSI protocol uses IP network to carry the SCSI protocol

User sees disk as a local disk but attached remotely
![[Screenshot 2025-05-06 at 2.12.43 PM.png|400]]

Cloud Storage
Similar to NAS, provides access to storage across a network
- unlike NAS: accessed over internet or WAN to remote data center
NAS presented as just another file system, while could storage is API based with programs using them to provide access

Dropbox, Amazon S3, Microsoft OneDrive, Google Drive, Apple iCloud



RAID Structure: Redundant Array of Inexpensive Disks
increases mean time to failure
mean time to repair : exposure time when another failure could caused data loss
mean time to data loss : based on above factors
If mirrored disks fail independently, consider disk with 100,000 mean time to failure and 10 hour mean repair:
$${100,000}ˆ{2} / (2*10) = 500 * 10ˆ6 \ hours = 57,000 \ years$$
Frequently combined with **NVRAM** to improve write performance
Several improvements in disk-use techniques involve the use of multiple disks working cooperatively

Disk **striping** uses a group of disks as one storage unit

RAID is arranged into six different levels

RAID schemes improve performance and improve the reliability of the storage system by storing redundant data
- **Mirroring** or **shadowing** (**RAID 1**) keeps duplicate of each disk
- Striped mirrors (**RAID 1+0**) or mirrored stripes (**RAID 0+1**) provides high performance and high reliability
- **Block interleaved parity** (**RAID 4, 5, 6**) uses much less redundancy

RAID within a storage array can still fail if the array fails, so automatic **replication** of the data between arrays is common

Frequently, a small number of **hot-spare** disks are left unallocated, automatically replacing a failed disk and having data rebuilt onto them
![[Screenshot 2025-05-06 at 2.22.46 PM.png|300]]

![[Screenshot 2025-05-06 at 2.23.14 PM.png|400]]

Snapshot feature: view of file system before a set of change take place
Replication is automatic duplication of writes between separate sites
	for redundancy and disaster recovery
	can be synchronous or asynchronous
Hot spare disk is unused, automatically used by RAID production if a disk fails to replace the failed disk and rebuild the RAID set if possible --> decreases mean time to repair

Extensions: RAID alone doesn't prevent or detect data corruption, just disk failures

