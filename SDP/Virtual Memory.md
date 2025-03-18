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










## Copy-on-Write 
## Page Replacement 
## Allocation of Frames 
## Thrashing 
## Memory-Mapped Files 
## Allocating Kernel Memory 
## Other Considerations 
## Operating-System Examples