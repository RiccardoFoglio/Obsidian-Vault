## Background

Code needs to be in memory to execute, but entire program rarely used, so entire code not needed at the same time
Execute partially-loaded programs
- program no longer constrained by limits of physical memory
- each program takes less memory while running --> more programs can run at the same time
- less I/O needed to load or swap programs into memory --> each user program runs faster

Virtual memory = separation of user logical memory from physical memory
	Logical address space can be much larger than physical address space
	Allow address spaces to be shared by several processes
	Allows for more efficient process creation
	More programs running concurrently
	Less I/O needed to load or swap processes

Virtual Address Space -- Logical view o how process is stored in memory
	Usually start at address 0, contiguous addresses until end of space
	Physical memory organized in page frames
	MMU must map logical to physical

Virtual memory can be implemented via:
- Demand paging
- Demand segmentation
![[Screenshot 2025-03-18 at 1.39.29 PM.png|400]]

Virtual-address Space:
- Usually design logical address space for stack to start at max logical address and grow "down" while heap grows "up"
- Enables sparse address spaces with holes left for growth, dynamically linked libraries
- System libraries shared via mapping into virtual address space
- Shared memory by mapping pages read-write into virtual address space
- Pages can be shared during `fork()` spending process creation

![[Screenshot 2025-03-18 at 1.48.56 PM.png|300]]
## Demand Paging 

Could bring entire process into memory at load time
Or bring page into memory only when it's needed
- Less I/O needed
- Less Memory needed
- Faster response
- More users
Page is needed ==> Reference to it
- Invalid reference = Abort
- not-in-memory = bring to memory
Lazy swapper --> never swaps a page into memory unless page will be needed
	Swapper that deals with pages is a pager

![[Screenshot 2025-03-18 at 1.53.21 PM.png|300]]

With swapping, pager guesses which pages will be used before swapping out again
Instead, pager brings in only those pages into memory
How to determine that set of pages? --> needed new MMU functionality to implement demand paging
If pages needed are already memory resident --> no difference from non demand paging
If page needed and not memory resident --> Need to detect and load the page into memory from storage
	Without changing program behavior
	Without programmer needing to change code

### Valid - Invalid Bit

- V = valid --> Memory resident, In memory
- I = Invalid --> not-in-memory
During MMU address translation, if valid-invalid bit in page table entry is i --> page fault

To handle Page Fault:
1. if there is a reference to a page, first reference to that page will trap to OS: Page Fault
2. OS looks at another table to decide:
	- Invalid reference --> Abort
	- Just not in memory
3. Find free frame
4. Swap page into frame via scheduled disk operation
5. Reset tables to indicate page now in memory. Set validation bit = V
6. Restart the instruction that caused the page fault
![[Screenshot 2025-03-18 at 2.18.19 PM.png|400]]

### Aspects of Demand Paging
- Extreme case : stat process with no pages in memory. 
	- OS sets instruction pointer to first instruction of process, non-memory-resident = Page Fault
	- And for every other process pages on first access
	- Pure demand paging
- Given instruction could access multiple pages --> multiple page faults
	- Consider fetch and decode of instruction which adds 2 numbers from memory and stores result back to memory
	- Pain decreased because of locality of reference
- Hardware support needed for demand paging
	- Page table with valid/invalid bit
	- Secondary memory (Swap device with swap space)
	- Instruction Restart

### Instruction Restart

### Free-Frame List

### Stages in Demand Paging

### Performance of Demand Paging

- Service the interrupt : careful coding means several hundred instructions needed
- read the page : lots of time
- restart the process : more overhead added

Page Fault rate: $0 \le p \le 1$
Effective Access Time (EAT) = (1-p) * memory access + p (page fault overhead + swap page out + swap page in )

![[Screenshot 2025-03-19 at 11.41.28 AM.png|400]]

Optimizations:
- Swap space I/O faster than file system I/O even if on the same device
- Copy entire process image to swap space at process load time
- Demand page in from program binary on disk, but discard rather than paging out when freeing frame
	- still need t o write to swap space:
		- pages not associated with a file = anonymous memory
		- pages modified in memory but not yet written back to the file system
- Mobile systems typically don't support swapping, instead demand page from file system and reclaim read-only pages (such as code)
## Copy-on-Write 

COW allows both parent and child processes to initially share the same pages in memory
allows for more efficient process creation as only modified pages are copied
In general, free pages are allocated from a pool of zero-fill-on-demand pages
	Pool should always have free frames for fast demand page execution
		Don't want to have to free a frame as well as other processing on page fault
`vfork()` variation of `fork()` system call has parent suspend and child using copy-on-write address space of parent
	designed to have child call `exec()`
	Very efficient

![[Screenshot 2025-03-19 at 11.55.01 AM.png|500]]
![[Screenshot 2025-03-19 at 11.55.27 AM.png|500]]

If there is no Free Frame:
used up by process pages
in demand from kernel, I/O Buffers etc...
How much to allocate to each?
- Page Replacement = Find some page in memory but not really use it, page it out
	- algorithm: terminate? swap out? replace the page?
	- performance: want an algorithm which will result in minimum number of page faults
Same page may be brought into memory several times
## Page Replacement 

Prevent over-allocation of memory by modifying page-fault service routine to include page replacement
Use modify (dirty) bit to reduce overhead of page transfer -- only modified pages are written to disk
Page replacement completes separation between logical memory and physical memory -- large virtual memory can be provided on a smaller physical memory
![[Screenshot 2025-03-19 at 12.31.48 PM.png|400]]

Basic Page Replacement:
1. find the location of desired page on disk
2. Find a free frame:
	- if there is a free frame, use it
	- if there isn't, use a page replacement algorithm to select a victim frame
	- write victim frame to disk if dirty
3. Bring the desired Page into the new frame, update the page and frame tables
4. continue the process by restarting the instruction that caused the trap
![[Screenshot 2025-03-19 at 12.34.37 PM.png|300]]

Frame-allocation algorithms determine how many frames to give each process and which frames to replace
Page-replacement algorithms want lowest page-fault rate on both first access and re-access
Evaluate algorithm by running it on a particular string of memory references and computing the number of page faults on that string
- string is just page numbers, not full addresses
- repeated access to the same page doesn't cause page fault
- results depend on number of frames available

![[Screenshot 2025-03-19 at 12.38.42 PM.png|500]]

Page Replacement Strategies:
![[Screenshot 2025-03-19 at 12.39.11 PM.png|500]]
- `A` = page replacement algorithm under evaluation
- `w` = a given reference string
- `p(w)` = probability of reference string w
- `len(w)` = length of reference string w
- `m` = number of available page frames
- `F(A,m,w)` = number of faults generated with the given reference string w using algorithm A on system with m page frames

![[Screenshot 2025-03-19 at 12.43.13 PM.png|400]]

### FIFO Algorithm
Reference string: 7,0,1,2,0,3,0,4,2,3,0,3,0,3,2,1,2,0,1,7,0,1
3 frames
![[Screenshot 2025-03-19 at 12.45.42 PM.png|400]]
15 page faults

adding more frames can cause more page faults --> Belady's Anomaly
How to track ages of pages? --> use FIFO queue
![[Screenshot 2025-03-19 at 12.50.12 PM.png|300]]

Optimal solution: replace page that will not be used for the longest period of time
can't read the future tho
### Least Recently Used (LRU) Algorithm
Use past knowledge rather than the future
Replace page that has not been used in the most amount of time
Associate time of last use with each page
![[Screenshot 2025-03-19 at 1.17.20 PM.png|400]]
12 faults is better than FIFO but worse than Optimal solution (9)
Generally good and frequently used

![[Screenshot 2025-03-19 at 1.26.10 PM.png|500]]

Counter Implementation: every page entry has a number, every time page is referenced through this entry, copy the clock into the counter
When a page needs to be changed, look at the counters to find smallest value

Stack implementation: keep a stack of page numbers in a double link form
Page referenced --> move it to the top, requires 6 pointers to be changed

LRU and OPT are cases of stack algorithms that don't have belady's anomaly

![[Screenshot 2025-03-19 at 1.35.18 PM.png|400]]

### LRU Approx. Algorithms

- Reference bit : 
	- with each page associate a bit, initially 0
	- when age is referenced, bit = 1
	- Replace any with reference bit = 0 (if one exists) 
- Second-chance algorithm :
	- FIFO + hardware provided reference bit
	- Clock replacement
	- if page to be replaced has
		- reference bit = 0 --> replace it
		- reference bit = 1 --> set it to 0, leave page in memory, replace next page, subject to same rules

![[Screenshot 2025-03-19 at 1.41.16 PM.png|400]]

Enhanced Second-Chance Algorithm:
- Improve algorithm by using reference bit and modify bit in concert
- Take ordered pair (reference, modify)
	- (0, 0) --> Neither recently used not modified -- best page to replace 
	- (0, 1) --> not recently used but modified -- not quite as good, must write out before replacement
	- (1, 0) --> recently used but clean -- probably will b used again soon
	- (1, 1) --> recently used and modified -- probably will be used again soon and must be written out before replacement
When page replacement is called, use the clock scheme but use the four classes to replace the page with lowest non-empty class

### Counting Algorithms
Keep a counter of the number of references that have been made to each page (not common)
- Least Frequently Used (LFU) Algorithm --> replaces page with smallest count
- Most Frequently Used (MFU) Algorithm --> based on the argument that the page with the smallest count was probably just brought in and has yet to be used

### Page-Buffering Algorithms
Keep a pool of free frame, always
- when a free frame is needed, not found at fault time
- read page into free frame and select victim to evict and add to free pool
- when convenient, evict victim

Keep a list of modified pages
- when backing store otherwise idle, write pages there and set to non-dirty

Possibly keep free frame contents intact and note what is in them
- if referenced again before reused, no need to load contents again from disk
- generally useful to reduce penalty if wrong victim frame selected


All these algorithms have OS guessing about future page access
Some apps have better knowledge (like databases)
Memory intensive apps can cause double buffering
	OS keeps copy of page in memory as I/O Buffer
	Application keeps page in memory for its own work
OS can give direct access to the disk, getting out of the way of the applications
	Raw disk mode
Bypasses buffering, locking etc...

## Allocation of Frames 

Each process needs minimum number of frames
Maximum is the total frames in the system
2 major allocation schemes:
- Fixed
- Priority
(many variations)

### Fixed Allocation
Equal allocation: 100 frames 5 processes --> each gets 20 frames
Proportional allocation: allocate according to size of process

Global Replacement : process selects a replacement frame from the set of all frames, one process can take a frame from another
	But the process execution time can vary greatly
	But greater throughput so more common
	
Local Replacement : each process selects from only its own set of allocated frames
	More consistent per-process performance
	Possibly underutilized memory

Reclaiming Pages:
- strategy to implement global page-replacement policy
- all memory requests are satisfied from the free-frame list, rather than waiting for the list to drop to zero before we begin selecting pages for replacement

![[Screenshot 2025-03-19 at 2.05.02 PM.png|400]]

### Non-Uniform Memory Access (NUMA)
So far all memory accessed equally
Many systems are NUMA -- speed of access to memory varies
	Consider system boards containing CPUs and memory, interconnected over a system bus

NUMA multiprocessing architecture
![[Screenshot 2025-03-19 at 2.06.31 PM.png|300]]

Optimal performance comes from allocating memory "close to" the CPU on which the thread is scheduled.
- modify scheduler to schedule the thread on the same system board when possible
- solved by solaris by creating **Igroups**
	- structure to track CPU / Memory low latency groups
	- used my schedule and pager
	- when possible schedule all threads of a process and allocate all memory for that process within the Igroup
## Thrashing 

If a process doesn't have enough pages, the page-fault rate is very high
- page fault to get page
- replace existing frame
- quickly need the replaced frame back

--> low CPU utilization, OS thinking that it needs to increase the degree of multiprogramming, another process added to the system

Thrashing : a process is busy swapping pages in and out
![[Screenshot 2025-03-19 at 2.13.38 PM.png|400]]

Why does demand paging work? Locality Model --> Process migrates from one locality to another. Localities might overlap
Why does thrashing occur? --> size of locality > total memory size
Limit effects by using local or priority page replacement

## Working Set Model

$\Delta$ = working set window = fixed number of page references
WSS = total number of pages referenced in the most recent $\Delta$
- if too small it will not encompass entire locality
- if too large ti will encompass several localities
- if infinite it will encompass the entire program

$D = \sum WSS_i$ = total demand frames
if D > m --> Thrashing. Policy: suspend or swap out one of the processes

Page Fault Frequency (PFF) : establish an acceptable rate and use local replacement policy
- too low = process loses frame
- too high = process gains frame
![[Screenshot 2025-04-02 at 11.41.13 AM.png|400]]

Page Fault Frequency Algorithm:
	Activated at every page fault, not at page reference. 
	Based on time interval τ from previous page fault.
	if τ < c, i.e. page fault frequency greater than desired, add a new frame to the Resident Set of the page faulting process.
	if τ ≥ c, i.e. page fault frequency OK, remove from Resident Set of page faulting process all pages with reference bit at 0; 
	then clear (set 0) reference bit of all other pages in Resident Set.
![[Screenshot 2025-04-02 at 11.43.46 AM.png|500]]

Working set and page-fault rate are in direct relationship
Working set changes over time
peaks and valleys over time
![[Screenshot 2025-04-02 at 11.44.52 AM.png|400]]

## Allocating Kernel Memory 

Different from user memory
Allocated from free-memory pool
- kernel requests memory for structure of varying sizes
- some kernel memory needs to be contiguous (ex: for device I/O)

Buddy System: allocates memory from fixed-size segment consisting of physically-contiguous pages
Memory allocated using power-of-2 allocator
- satisfies requests in units sized as power of 2
- request rounded up to next highest power of 2
- when smaller allocation needed than is available, current chunk split into two buddies of next-lower power of 2
	- (continue until appropriate size chunk available)

Example: 256KB available, Kernel requests 21KB
- split into $A_L$ and $A_R$ of 128KB each
- divide further into $B_L$ and $B_R$ of 64KB
- $C_L$ and $C_R$ of 32 will satisfy the request

Slab Allocator: alternate strategy
- slab = one or more physically contiguous pages
- Cache = one or more slabs
- single cache for each unique kernel data structure
	- each cache filled with objects -- instantiations of the data structure

cache created, filled with objects marked as `free`
when structures stored, objects marked as `used`
if slab is full of used objects, next object allocated from empty slab
	if no empty slabs, new slab allocated
Benefits : no fragmentation, fast money request satisfaction

![[Screenshot 2025-04-02 at 3.41.34 PM.png|500]]

In Linux: slab can be in 3 possible states
1. full - all used
2. empty - all free
3. partial - mix of free and used

Upon request, slab allocator
- uses free struct in partial slab
- if none, takes one from empty slab
- if no empty slab, create new empty

## Other Considerations 

### Prepaging

Reduce the large number of page faults that occur at process startup
Prepage all or some of the pages a process will need, before they are referenced
But if prepaged pages are unused I/O and memory was wasted
Assume s pages are prepaged and a of the pages is used
	cost of $s * a$ saves page faults greater or lesser than the cost of prepaging
	if a near zero --> prepaging loses
### Page Size

Some OS designers have a choice, especially if running on custom CPUs
page size selection must take into account:
- fragmentation
- page table size
- resolution
- I/O Overhead
- Number of page faults
- Locality
- TLB size and effectiveness

### TLB Reach

TLB Reach = TLB size x Page size (amount of memory accessible from the TLB)
Ideally, working set of each process is stored in the TLB
	Otherwise here is a high degree of page faults
Increase the page size
	This may lead to an increase in fragmentation as not all applications require a large page size
Provide multiple page sizes
	This allows applications that require larger page sizes the opportunity to use them without an increase in fragmentation

### Program Structure

Given `int[128,128] data;`
Each row is stored in one page

Program 1:
```c++
for (j = 0; j < 128; j++)
	for (i = 0; i < 128; i++)
		data[i,j] = 0;
```
128x128 = 16384 page faults

Program 2:
```c++
for (i = 0; i < 128; j++)
	for (j = 0; j < 128; j++)
		data[i,j] = 0;
```
128 page faults
### I/O Interlock

I/O Interlock – Pages must sometimes be locked into memory 

Consider I/O - Pages that are used for copying a file from a device must be locked from being selected for eviction by a page replacement algorithm 

Pinning of pages to lock into memory

![[Screenshot 2025-04-02 at 5.24.26 PM.png|300]]
## Operating-System Examples

### Windows

Uses demand paging with clustering, clustering brings in pages surrounding the faulting page
Processes are assigned working set minimum and working set maximum
when the amount of free memory in the system falls below a threshold, automatic working set trimming is performed to restore the amount of free memory
Working set trimming removes pages from processes that have pages in excess of their working set minimum
### Solaris

Maintains a list of free pages to assign faulting processes
`Lotsfree` – threshold parameter (amount of free memory) to begin paging 
`Desfree` – threshold parameter to increasing paging 
`Minfree` – threshold parameter to being swapping 
Paging is performed by pageout process
`Pageout` scans pages using modified clock algorithm
`Scanrate` is the rate at which pages are scanned. This ranges from `slowscan` to `fastscan`
`Pageout` is called more frequently depending upon the amount of free memory available 
Priority paging gives priority to process code pages

