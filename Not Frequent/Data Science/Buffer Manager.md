![[Pasted image 20241114110701.png]]

Buffer Manager manages page transfer from disk to main memory and vice versa.
It's in charge of managing the DBMS buffer
Efficient buffer management is a key issue DBMS performance

Buffer:
- large main memory block
- pre-allocated to the DBMS
- shared among executing transactions

Buffer organization:
- memory is organized in pages
- the size of a page depends on the size of the operating system I/O block

Memory management strategies:
- data locality : data referenced recently is likely to be referenced again
- Empirical law: 20-80 : 20% of data is read/written by 80% of transactions

The buffer manager keeps additional "snapshot" information on the current content of the buffer
for each buffer page:
- physical location of the page on disk : File identifier, Block number
- state variables : 
	- Count of the number of transactions using the page
	- Dirty bit which is set if the page has been modified

It provides the following primitives to access methods to load pages from disk and vice versa:
- fix
- unfix
- force
- set dirty
- flush
Requires shared access permission from the concurrency control manager
### Fix Primitive
Used by transactions to require access to a disk page: the page is loaded into the buffer. A pointer to a page into the buffer is returned to the requesting transaction
At the end of the Fix primitive, the requested page is in the buffer, is valid and the Count state variable of the page is incremented by 1
The Fix primitive requires an I/O operation only if the requested page is not yet in the buffer

It looks for the requested page among those already in the buffer
if found: it returns ro the requesting transaction the address of the page in the buffer. It happens often because of data locality
if not found: page is searched into the buffer where the new page can be loaded. first among free pages, next among pages with count = 0 (victim pages, may still be locked).
If the selected page has Dirty = 1, it is synchronously written on disk
The new page is loaded in the buffer and its address is returned to the requesting transaction
### Unfix Primitive
It tells the buffer manager that the transaction is no longer using the page 
The state variable Count of the page is decremented by 1
### Dirty Primitive
It tells the buffer manager that the page has been modified by the running transaction 
The Dirty state variable of the page is set to 1
### Force Primitive
It requires a synchronous transfer of the page to disk 
- The requesting transaction is suspended until the Force primitive is executed 
- It always entails a disk write
### Flush Primitive
It transfer pages to disk, independently of transaction requests
It is internal to the Buffer Manager 
It runs when the CPU is not fully loaded In CPU idle time 
It downloads pages which are not valid (state variable Count=0) or are not accessed since a longer time

## Writing Strategies

- Steal : The Buffer Manager is **allowed** to select a locked page with Count=0 as victim. The page belongs to an active transaction
- No Steal : The Buffer Manager is **not allowed** to select pages belonging to active transactions as victims

The steal policy writes on disk dirty pages belonging to uncommitted transactions
In case of failure these changes must be undone: same operations as in transaction rollback

- Force : All active pages of a transaction are synchronously written on disk by the Buffer Manager during the commit operation
- No Force : Pages are written on disk asynchronously by the Buffer Manager by means of the Flush primitive

Pages belonging to a committed transaction may be written on disk after commit 
In case of failure these changes must be redone

Typical usage is steal/no force, because of its efficiency 
	No force provides better I/O performance 
	Steal may be mandatory for queries accessing a very large number of pages

## File System and Buffer Manager

The Buffer Manager exploits services provided by the file system: 
- Creation/deletion of a file
- Open/close of a file
- Read : provides a direct access to a block in a file, it requires file identifier, block number and buffer page where to load data in memory
- Sequential Read : provides sequential access to a fixed number of blocks in a file. It requires file identifier, starting block, count of the number of blocks to be read, starting buffer page where to load data in memory
- Write and Sequential Write : same as reading
- Directory management functions

