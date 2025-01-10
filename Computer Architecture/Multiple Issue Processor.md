Techniques for reducing the effects of data control dependencies can be further exploited to obtain a CPI less than one: this requires issuing more than one instruction per clock cycle.
2 kind of processors able to do so:
- Superscalar Processors : statically or dynamically scheduling instructions
- Very Long Instruction Word (VLIW) Processors
Both groups the architecture obviously includes multiple functional units

## MULTIPLE-ISSUE STATIC SCHEDULING

Implementing a superscalar processor able to possibly issue several instructions according to the static order defined by the compiler is rather easy and inexpensive

Two instructions can be issued per clock cycle if:
- one is a load / store / branch / integer ALU operation
- the other is any FP operation
Two instructions (64 bits) are fetched and decoded at every clock cycle
The two instructions are aligned on a 64-bit boundary and constitute an issue packet

Old superscalar processors required a fixed structure for issue packets, current processors normally don't have this limitation

![[Pasted image 20241022090304.png]]

At each clock cycle 2 instructions must be read from the instruction memory
If two instructions belong to different cache blocks, several processors fetch one instructions, only.
In most of the cases, the issue packet may only contain one branch instruction

In order to obtain a real benefit, the floating point units should be either pipelined or multiple and independent
When the first instruction is a FP load, store, or move, there is a possible contention for a FP register port.
Possible solutions: forcing first instruction to be executed by itself or giving the FP register file an additional port.

Possible RAW Hazard: when the first instruction is a FP load, store or move, the second reads the result, a RAW hazard is possible.
In this case, the second instruction must then be delayed by one clock cycle.

In the MIPS pipeline, load has a latency of one clock cycle, which means that in the superscalar pipeline the result of a load instruction cannot be used on the same clock cycle but on the next one. Therefore, in the static MIPS superscalar version the load delay slot (as well as the branch delay slot) becomes equal to 3 instructions

Static multiple-issue scheduling is mainly adopted by processors for the embedded market.
## MULTIPLE-ISSUE DYNAMIC SCHEDULING

It can be obtained by adopting a scheme similar to the Tomasulo one. To make the implementation easier, instructions are never issued to the reservation stations out-of-order.

CDB Criticality: In some clock cycles, more than one instruction may be ready to write on the CDB, that can only service one instruction at a time. For this reason, duplicating the CDB can provide higher performance, at the cost of some area overhead