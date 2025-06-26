
## File-System Structure

File structure: logical storage unit, collection of related info.
File system resides on secondary storage (disks)
- provided user interface to storage, mapping logical to physical
- provides efficient and convenient access to disk by allowing data to be stored, located and retrieved easily
Disk provides in-place rewrite and random access
File Control Block (FCB) = storage structure consisting of information about a file
Device driver controls the physical device
File system organized into layers
![[Screenshot 2025-06-24 at 9.57.16 AM.png|200]]

- Device Drivers: manage I/O devices at the I/O control layer
- Basic file systemL translate command to device driver and manages memory buffers and cache (allocation, freeing, replacement)
	- Buffers hold data in transit
	- Caches hold frequently used data
- File organization module: understands files, logical address, and physical blocks
- Logical File System: manages metadata information
	- translates file name into file number, file handle, location by maintaining file control blocks (inodes in UNIX)
Layering useful for reducing complexity and redundancy, but adds overhead and can decrease performance.
Logical layers can be implemented by any coding method according to OS designer

Many file systems, sometimes many within the OS
## File-System Operations

System calls at the API level, how to implement the functions? --> on disk memory structures

Boot control block contains info needed by systems to boot OS from that volume
	Needed if volume contains OS, usually first block of volume

Volume control block (superblock, master file table) contains volume details:
- total no. of blocks, no. of free blocks. block size, free block pointers or array

Directory structure organizes the files: names, inode numbers, master file table

File Control Block (FCB): contains many details about the file:
![[Screenshot 2025-06-24 at 10.16.56 AM.png|400]]

Mount Table: storing file system mounts, mount points, file system types
System-wide open-file table: contains a copy of the FCB of each file and other info
Per-process open-file table: contains pointers to appropriate entries in system-wide open-file table as well as other info

opening a file (a) and reading a file (b)
![[Screenshot 2025-06-24 at 10.24.52 AM.png|500]]
## Directory Implementation

Linear list of file names with pointer to the data blocks: simple to program, time-consuming to execute (linear search time, could keep ordered alphabetically via linked list or use a B+ tree)

Hash Table: linear list with hash data structure
	decreases directory search time
	collisions: situations where two file names hash to the same location
	only good if entries are fixed size, or use chained-overflow method
## Allocation Methods

Allocation method refers to how disk blocks are allocated for files
Contiguous allocationL each file occupies set of contiguous blocks
- best performance in most cases
- simple: only starting location (block #) and length (number of blocks) are required
- Problems: find space for file, knowing file size, external fragmentation, need for compaction off-line (downtime) or on-line

Contiguous Allocation
![[Screenshot 2025-06-24 at 10.59.48 AM.png|500]]

Extent-Based Systems: many newer file systems use modified contiguous allocation scheme. Extent-based systems allocate disk blocks in extents
Extent = contiguous block of disks: they are allocated for file allocation, file consists of one or more extents
### Linked Allocation 
each file ends at nil pointer
no external fragmentation
each block contains pointer to next block
no compaction, external fragmentation
free space management system called when new block needed
improve efficiency by clustering blocks into groups but increases internal fragmentation
reliability can be a problem
locating a block can take many I/Os and disk seeks

FAT (File Allocation Table) variation:
- beginning of volume has table, indexed by block number
- much like a linked list, but faster on disk and cacheable
- new block allocation simple

Each file is a linked list of blocks scattered anywhere on the disk
Block to be accessed is the Q-th block on the linked chain of blocks representing the file. 
Displacement into block: R+1

![[Screenshot 2025-06-24 at 11.06.29 AM.png|300]]
![[Screenshot 2025-06-24 at 11.06.51 AM.png|300]]

### Indexed Allocation
Each file has its own index block of pointers to its data blocks
![[Screenshot 2025-06-24 at 11.20.11 AM.png|300]]
![[Screenshot 2025-06-24 at 11.20.32 AM.png|500]]

need an index table
random access
dynamic access without external fragmentation, but have overhead of index block
mapping from logical to physical in a file of maximum size of 256K bytes and block size of 512 bytes
Need 1 block of index table 

![[Screenshot 2025-06-24 at 11.22.34 AM.png|300]]

Mapping from logical to physical in a file of unbounded length
Linked scheme -- link blocks of index table (no limit in size)

![[Screenshot 2025-06-24 at 11.28.33 AM.png|500]]

Performance:
- best method depends on file access type
	- contiguous great for sequential and random
	- linked good for sequential, not random
- Declare access type at creation --> select either contiguous or linked
- Indexed more complex
	- single block access could require 2 index block reads then data block read
	- clustering can help improve throughput, reduce CPU overhead
- for NVM no disk head, so different algorithms and optimizations needed
- adding instructions to the execution path to save one disk I/O is reasonable
## Free-Space Management

file system maintains free-space list to track available blocks/clusters
bit vector or bit map (n blocks)
![[Screenshot 2025-06-24 at 11.31.44 AM.png|400]]

![[Screenshot 2025-06-24 at 11.32.06 AM.png|400]]

Linked free space list on disk: linked list cannot get contiguous space easily, no waste of space, no need to traverse the entire list

![[Screenshot 2025-06-24 at 11.33.50 AM.png|400]]

- Grouping: modify linked list to store address of n-1 free blocks in first free block, plus a pointer to next block that contains free-block-pointers
- Counting: because space is frequently contiguously used and freed, with contiguous-allocation --> allocation, extents or clustering
	- keep address of first free block and counting of following free blocks
	- free space list then has entries containing addresses and counts

- Space Maps: used in ZFS, consider metadata I/O on very large file systems. Divides device space into metaslab units and manages metaslabs (given volume can contain hundreds of metaslabs)
- each metaslab has associated space map --> uses counting algorithm
  records to log file rather than file system --> log of all block activity in time order in counting format
  metaslab activity --> load space map into memory in balanced-tree structure, indexed by offset

TRIMing Unused Blocks : HDDs overwrite in place so need only free list.
block not related specially when freed
storage devices not allowing overwrite suffer badly with same algorithm
## Efficiency and Performance

Efficiency dependent on:
- disk allocation and directory algorithms
- types of data kept in file's directory entry
- pre-allocation or as-needed allocation of metadata structures
- fixed-size or varying-size data structures

Performance: 
- keeping data and metadata close together
- Buffer cache - separate section of main memory for frequently used blocks
- synchronous writes sometimes requested by apps or needed by OS 
  --> no buffering/caching = writes must hit disk before acknowledgement.
  --> asynchronous writes more common, buffer-able, faster
- free-behind and read-ahead -- techniques to optimize sequential access
- reads frequently slower than writes

Page Cache = caches pages rather than disk blocks using virtual memory techniques and addresses
Memory mapped I/O uses page cache
Routine I/O through the file system uses the buffer (disk) cache

![[Screenshot 2025-06-25 at 4.01.44 PM.png|400]]

Unified Buffer Cache = uses the same page cache to cache both memory-mapped pages and ordinary file system I/O to avoid double caching
## Recovery

Consistency checking: compares data in directory structure with data blocks on disk, and tries to fix inconsistencies (can be slow and sometimes fails)
Use system programs to back up data from disk to another storage device 
Recover lost file or disk by restoring data from backup

Log structured (or journaling) file systems record each metadata update to the file system as a transaction
all transactions are written to a log
- transaction considered committed once it's written to the log (sequentially)
- sometimes to a separate device or section of disk
- file system may not yet be updated

transactions in the log are asynchronously written to the file system structures, when the file system structures are modified, the transaction is removed from the log
if the file system crashes, all remaining transactions in the log must still be performed
Faster recovery from crash, removes chance of inconsistency of metadata

