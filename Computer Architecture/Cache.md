Small but high-speed memories that are interposed between the processor and main memory.
![[Pasted image 20241022094028.png]]

The presence of a cache can improve the performance of a system due to the locality of references observed in most programs. 

Locality takes two forms: 
- Temporal locality: if at time $t$ the program accesses to a given memory cell, it is highly probable that the program accesses again the same cell by the time $t_0 + \Delta t$
- Spatial locality: if at time $t_0$ the program accesses a memory cell with address $X$, it is highly likely that by the time $t_0 + \Delta t$ the program will also access the memory cell with address $X ± e$.

If the entire block is loaded in the cache at time $t_0$ (first access to a memory block), it is likely that for a certain time $\Delta t$ the program will find in the cache all of the words it needs.

Performance:
- H = cache hit ration (in the order of 0.9)
- C = cache access time
- M = memory access time when data is not in cache
$$
T_{avg} = h\cdot C + (1-h)\cdot M
$$
![[Pasted image 20241022100551.png]]

Cache is organized in lines. A line contains a Memory Block that includes some memory words. Each line is associated with a Tag Field, which indicates the memory block present in the line at that time.
The cache also contains the logic for:
- intercepting the addresses produced by the processor
- checking inside the cache the possible presence of the block that the processor wants to access
- possibly loading the block

The cache is located between the processor and the main memory. Each time the processor performs an access to memory the cache intercepts the address and checks if the block to which the word belongs is in the cache, checking the value of the gats:
- YES: extracts word from block and allows CPU to access it without any access to main memory
- NO : loads into cache the block that the word is part of

In case of Hit, cache reduces the memory access times by factor that is dependent on the ration between cache and primary memory access time,
In case of Miss: 
- access memory and load entire missing block, then provides requested word. Access time higher than cache-free system
- access memory and immediately provide word. Higher cost in terms of hardware, but misses have more limited impact on performance

Position: cache is normally located between the CPU and the bus, rather than between the main memory and the bus
![[Pasted image 20241022104638.png]]
Benefits: bus load is reduced, solution is compatible with a multiprocessor architecture

In some cases there are separate caches for data and instructions, in other cases there is only one for both.
The cache for instructions is typically easier to handle than data, as the instructions cannot be changed (no write operations)
## Harvard Architecture
two caches, the architecture of the system falls into the Harvard architecture scheme, characterized by the existence of 2 separate data and code memories. The Harvard architecture contrasts with the von Neumann architecture.

Choosing the optimal size of cache is very important for the system cost and performance.
As size increases, cost increases, system performance improves, cache itself becomes slower.
Frequent size range: from few KB to few MBs

Mapping mechanism defines in which line of the cache a certain memory block is written when it is uploaded in the cache. You must ensure (at an acceptable cost) that the cache can quickly verify if it contains the data corresponding to a certain address.
![[Pasted image 20241022105435.png]]
### Direct Mapping
Each memory block *i* is statically associated to a line *k* in the cache using the expression: 
$$
k = i\cdot mod(N)
$$
where N = number of lines in cache. K can be easily computed by taking the LSB in the value *i*

Pros: mechanism can be easily implemented in hardware
Cons: if program frequently accesses 2 blocks corresponding to the same cache line, a miss occurs at each memory access
### Set Associative Mapping
the cache lines are subdivided into S sets, each consisting of W (ways) lines
$$ k = i\cdot mod(s)$$
A set associative cache with W lines in each set is called a W-ways cache. 
Common values for W are 2 and 4 (so can deal up to 4 collisions).

### Fully Associative Mapping

Each block of the main memory can be stored in any cache block. 
Advantages : maximum flexibility in choosing the cache block to use 
Disadvantages : complexity of search hardware (usually adopting associative memory).

![[Pasted image 20241211113325.png]]

## Replacing Algorithm

It defines which cache line should be used to store a memory block, amongst those associated with the block (in the case of associative or set associative mapping).
Direct Mapping doesn't deal with collisions, so these algorithms are for set and fully associative.

It is chosen from: 
- LRU (Least Recently Used): the most used 
- FIFO (First-In First-Out): the cheapest 
- LFU (Least Frequently Used): theoretically, the most effective
- random: simple and effective.

## Main Memory Update
When a write operation is performed on a data in the cache, we also need to update the main memory. 
Two solutions can be adopted: 
- write-back 
- write-through

Write-Back: for each cache block, a flag (called dirty bit) is introduced, which remembers whether or not the block has been changed since it was loaded into the cache.
When a block is evicted from the cache and the dirty bit is set, the block is copied from the cache to the main memory. 
Disadvantages:
- the replacement is slower because it sometimes requirescopying in the main memory the replaced block 
- in multiprocessor systems there may be incoherence between the caches of different processors
- it may not be possible to restore memory data after possible system failures.

Write-through: each time the CPU performs a write operation, it writes on both the cache data and the main memory data. The resulting loss of efficiency is limited by the fact that writing operations are usually much less numerous than reading ones.

Cache coherence : this is a major problem in multiprocessor systems with shared memory, in which each processor has its own cache. Similar problems may occur if the system uses a DMA controller.

![[Pasted image 20241211113700.png]]

Validity Bit: to achieve cache coherence, a validity bit is introduced for every cache line.
If disabled =any access to the block must produce a miss.
At power-on, the validity bits of all cache lines are disabled.

## Cache Levels
Each time the processor performs a memory access 
- It checks first whether the word is in L1 
- If so, it can access the word in L1
- If not, it checks whether the word is in L2
	- If so, it will access to the word in L2 and eventually update L1
	- If not, it checks whether the word is in L3
		- If yes, L3 is accessed and L2 is updated 
		- If not, it accesses to the main memory and eventually updates L3.

### Example of Cache Size
Let us consider a cache with the following characteristics: 
- 64KByte size (data only)
- direct mapping
- 4 bytes blocks
- 1 block per page entry
- 32 bits addresses. 

You are asked to determine the structure of the cache(number of lines, size of the tag field)

1. Since each block is composed of 4 bytes, I don't need to take 2 bits (ghost bits). 
   The memory is composed of $2^{30}$ blocks
2. The number of lines in the cache is 64KByte/4=16K=$2^{14}$
3. The tag field identifies the block currently stored in each line.
4. Hence, the tag field should be composed of 30 bit.
5. since in the generic cache line only blocks whose index has the 14 least significant bits equal to the line index, the tag field is composed of 16 bits, only

14 bits to choose the line (INDEX), 16 bits to check the correct data (TAG)

Hence, the total size of the cache is given by: $$2^{14} \times (32+16) = 768Kbit = 96KByte $$![[Pasted image 20241211115424.png]]