Instruction Set Architecture (ISA) is how the computer is seen by the programmer or compiler
Many alternatives for ISA designer, evaluated in terms of: 
- processor performance
- processor complexity
- compiler complexity
- code size
- power consumption

## Taxonomy
CPU are often classified according to the type of internal storage:
- Stack
- Accumulator
- Registers
	- register-memory (CISC)
	- register-register (load-store) (RISC)
	- memory-memory (no real cases) 

![[Pasted image 20241002115510.png]]

Code Example: code sequence for C = A + B
![[Pasted image 20241002115606.png]]

GPR Machines: all processors are General-Purpose Register machines
- registers are faster than memory
- registers are easier for a compiler to use

CPU can be classified according to:
- typical number of operands per ALU instruction (2 or 3)
- typical number of memory operands per ALU instruction (0 - 3)
![[Pasted image 20241002120626.png]]
## Memory Addressing
recap:
- Von Neumann : one RAM for code and data, one single interconnection (2R, 1W)
- Harvard : code and data separate, so 2 interconnection (1R 1W each) -> better performance (faster) but more costly
### Endian
- Little Endian : bytes with the lower address at the LS position
![[Pasted image 20241002121217.png]]
- Big Endian : byte with lower address at MS position
![[Pasted image 20241002121248.png]]
### Aligned vs Misaligned
- Aligned : allows only aligned accesses to memory is a limitation (0-3,4-7 but no 2-5 for example)
- Misaligned : more freedom, but more hardware and performance overhead
### Addressing modes
in GPR Machines an addressing mode specifies a constant, a register or a memory location (thru its effective address)
#### Register Mode
![[Pasted image 20241002121545.png]]
#### Immediate Mode
![[Pasted image 20241002121558.png]]
#### Displacement Mode
![[Pasted image 20241002121655.png]]
#### Register Deferred (or Indirect) Mode
![[Pasted image 20241002121714.png]]
#### Indexed Mode
![[Pasted image 20241002121726.png]]
#### Direct (or Absolute) Mode
![[Pasted image 20241002121750.png]]
(obsolete, not used anymore)
#### Memory Indirect Mode
![[Pasted image 20241002121819.png]]
R3 points to another address to be used.
#### (post) Autoincrement Mode
![[Pasted image 20241002121854.png]]
useful in loop cycles
#### (pre) Autodecrement Mode
![[Pasted image 20241002122004.png]]
useful in loop cycles
#### Scaled Mode
![[Pasted image 20241002122019.png]]

By carefully choosing the addressing modes, one can obtain some important consequences:
- Reduce the number of instructions
- Increase the CPU architecture complexity
- Increase the average Cycles Per Instruction (CPI)

RISC -> limited amount

Most Used:
![[Pasted image 20241002122738.png]]

### Summary
Displace, Immediate and Register Indirect represent from 75-99% of addressing modes
the address size for displacement mode should be 12-16 bits (75-99% of displacements)
the size of the immediate field should be at least 8-16 bits (50-80% of cases)

## Operations in Instruction Set
![[Pasted image 20241002123955.png]]

Not all instructions are executed with the same frequency!
When designing and implementing an instruction set, the most commonly executed instructions should be made faster.
![[Pasted image 20241002124136.png]]

### Control Flow Instructions
4 categories:
- conditional branches
- jumps
- procedure calls
- procedure returns
Conditional branches are by far the most frequently executed control flow instructions
### Destination Address
It's normally specified as a displacement with respect to the current value of the Program Counter. 
`base + offset`

This way: 
- we save bits, since the target instruction is often close to the source one, 
- the code is position-independent.
### Register Indirect Jumps and Procedure Calls
They allow:
- to write code including jumps whose target is not known at compile time
- to implement case/switch statements
- to support dynamically shared libraries (libraries loaded only when called)
- to support virtual functions (calling different functions depending on data type)
### Evaluating Branch Conditions
![[Pasted image 20241002125115.png]]
(MIPS : R31 register saves the return address in that register)

Information to be saved:
- Return Address
- Accessed registers:
	- caller saving
	- callee saving
## Type and Size of Operands
Most frequently supported data types:
- char (1 byte)
- half word (2 bytes)
- word (4 bytes)
- double word (8 bytes)
- single-precision floating-point (4 bytes)
- double-precision floating-point (8 bytes).
![[Pasted image 20241002130636.png]]
## Instruction Encoding
Depends on:
- which instructions compose the Instruction Set
- which addressing modes are supported

When a high number of addressing modes is supported, an **address specifier** field is used to specify the addressing mode and the registers which are possibly involved.

When the number of addressing modes is low, they can be encoded together with the opcode.

![[Pasted image 20241002130904.png]]

A: **Variable** -> supports any number of operands, instructions have a variable length, lower performance and minimum code size

B: **Fixed** -> fixed number of operands, address specifier included in opcode, fixed instruction length, max performance, larger code size

C: **Hybrid** -> multiple formats (specified by opcode), allow trading-off between code size and performance

### Conflicting Issues
designer should address several conflicting issues:
- code size
- size of Instruction Set, the number of addressing modes, and the number of registers
- complexity of the fetch and decoding hardware

### Hardware-Compiler Interaction
Assembly-level programs are now produced by compilers only
CPU designer and compiler writer must interact and cooperate

### Register Allocation
Choosing which variable have to be put in which register and when, is one of the crucial optimization phases in a compiler.
This problem is based on graph coloring and can be better solved if the number of registers is high (>16)

### Variables Access
Optimizing variable access time by allocating variables to registers is only possible for those stored in the stack or for global variables in memory.

It's impossible for variables belonging to the heap, due to the *aliasing problem* (access to a variable done thru pointers)

### Architect can Help the Compiler Writer
Make the frequent case fast and rare case correct
- Regularity
- Provide primitives, not solutions
- Simplify trade-offs among alternatives
- Provide instructions that bind the quantities known at compile time as constants
- At least 16 registers
- Orthogonality
- Simplicity