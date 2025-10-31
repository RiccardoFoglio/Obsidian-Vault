Superscalar processor = CPI less than 1

- detecting and managing dependencies 
- dynamic scheduling
- branch prediction and speculation
This significantly increased the complexity of processors.

*Long Instruction Word* (LIW) and *Very Long Instruction Word* (VLIW) processors have long instructions encoding several operations, which are issued in parallel
The hardware includes as many functional units, as the operations in a single instruction
VLIW processors are relatively common for embedded applications (especially for signal processing and graphics).

- More **complex software** : up to the compiler to decide which instructions to pack together, exploiting parallelism, unrolling loops, scheduling code across basic blocks, etc

- **Simpler hardware** : doesn't perform any check on possible dependencies among instructions (this task is completely left to the compiler). This significantly simplifies the processor

Stalls = When an operation requires stalling (e.g., due to a cache miss) the whole instruction packet is stalled, in order to preserve the flow decided by the compiler.

Example: independent pack of instructions can be executed at the same time (parallelism)
![[Pasted image 20241211130528.png]]

Advantages: no hardware for scheduling required

Limitations: performance can be limited by
- inherent limitations of ILP in programs 
- difficulties in building the hardware
- limitations specific to a superscalar or VLIW processor.

It is difficult to find a sufficient number of independent instructions that can be executed in parallel
This is even more difficult due to pipelined functional units, having a latency greater than 1.
In general, to avoid any stall we need to find a number of independent operations roughly equal to

$\text{average pipeline depth} \times \text{number of functional units}$

With 5 functional units, and an average pipeline depth of 4 clock cycles, we need as many as 20 independent operations to avoid stalls

By increasing the number of functional units, there is also an increase in the bandwidth towards the register file and the memory. This means increasing the hardware complexity and possibly decreasing the performance.
Possible solutions:
- memory interleaving
- multiport memories
- multiple access per clock cycle memories

Code Size : it's much larger for VLIW processors mainly because
- loops are intensively unrolled to extract more parallelism
- the empty slots in instruction encoding.
Sometimes instructions are compressed in memory and expanded when loaded to the processor.

Memory access: access to memory is often a bottleneck, since the required bandwidth is higher, and stalls due to cache misses cause a stall on the whole processor

Binary Code Compatibility: Any change in the implementation of a VLIW processor requires recompiling the code. This is a major disadvantage with respect to superscalar processors, which can easily be made binary compatible with their previous versions
Object code translation or emulation will possibly solve this problem in the future


Processing instructions in parallel requires three major tasks: 
- checking dependencies between instructions to determine which instructions can be grouped together for parallel execution
- assigning instructions to the functional units in the hardware
- determining when instructions are initiated together.

These three tasks can each be assigned to the compiler or to the processor.

![[Pasted image 20241211134408.png]]

### Advantages
- Dependencies are determined by compiler and used to schedule according to function unit latencies.
- Function units are assigned by compiler and correspond to the position within the instruction packet.
- Reduces hardware complexity.
- Tasks such as decoding, data dependency detection, instruction issues etc. become simple.
- Ensures potentially higher Clock Rate.
- Ensures lower power consumption
### Disadvantages
- Higher complexity of the compiler
- Compatibility across implementations: Compiler optimization needs to consider technology dependent parameters such as latencies and load-use time of cache.
- Unscheduled events (e.g., cache miss) stall entire processor.
- Code density: In case of un-filled opcodes in a VLIW, memory space and instruction bandwidth are wasted (low slot utilization).
- Code expansion: Causes high power consumption


VLIW architecture is suitable for Digital Signal Processing applications
Processing of media data like compression/decompression of Image and speech data.