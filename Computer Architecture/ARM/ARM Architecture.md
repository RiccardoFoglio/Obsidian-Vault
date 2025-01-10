The following data types are supported: 
- byte: 8 bits 
- halfword: 16 bits 
- word: 32 bits 

Registers are 32-bit wide. 
Instructions are 16 or 32 bits long.

![[Pasted image 20241028112615.png]]

## Program Status Register
32 bit register
3 categories:
- **Application Program Status Register** (5 bits: 26-31)
	- N = negative result from ALU
	- Z = Zero result from ALU
	- C = ALU operation carried out
	- V = ALU operation overflowed
	- Q = "sticky" flag for saturation arithmetic
- **Execution Program Status Register** (EPSR)
	- IT = IF-THEN instruction status bits
	- ICI = Interrupt-Continuable Instruction bits
	- T = Thumb bit, always 1: cleaning this bit will cause hard fault exception
- **Interrupt Program Status Register** (IPSR) : contains an exception number used in exception handling
## Processor Mode
2 operating modes:
- thread mode: on reset or after completing the exception routine
- handler mode: when an exception occurs (always privileged access level)

2 access levels: 
- user level: limited access to resources
- privileged level: access to all resources
### Control Register
2 bits only:
- CONTROL\[0]  changes the access level
- CONTROL\[1] selects which stack is being used (Main or Process Stack Pointer)
CONTROL is writable only in privileged level

![[Pasted image 20241028115454.png|700]]

### Vector Table
Each entry manages an exception, interrupt or other atypical event such as reset.
The vector table contains either ARM instructions or addresses in memory.
In the v7-M architecture, there are addresses

![[Pasted image 20241028115744.png|500]]

Features of ARM instruction Sets
- most instructions execute in a single cycle
- instructions can be conditionally executed
- a load/store architecture:
	- data processing instructions act only on registers
	- two or three operand formats
	- combined ALU and shifter
	- Memory access instructions with auto-indexing

3 kind of instructions
- ARM instructions : 32 bits
- Thumb instructions: 16 bits
- Thumb-2 instructions: 16/32 bits
Example:
````
ADD r0, r0, r2   #32 bits
ADD r0, r2       #16 bits
````
