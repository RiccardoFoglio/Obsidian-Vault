Microprocessor without Interlocked Pipeline Stages (MIPS)
MIPS is a family of RISC processors, successful in embedded applications

Simple load-store Instruction Set
Designed for Pipeline efficiency: fixed instruction length, low-power applications

![[Pasted image 20241002132719.png]]
PC = Program Counter
HI, LO = special registers used for divisions etc...


## Data Types

- Byte (8 bits)
- Half Word (16 bits)
- Word (32 bits)
- Double Word (64 bits)
- 32-bit single precision floating-point
- 64-bit double precision floating-point

## Addressing Modes

- Immediate: uses 16 bits Immediate field

```
DADDUI R1, R2, #32
	R1 <-- R2 + 32

DADDUI R1, R0, #32
	R1 <-- 32
``` 

- Displacement

```
LD R1, 30(R2)
R2 = XX
	R1 = MEM[R2 + 30]
```
![[Pasted image 20241002133121.png]]

```
LD R1, 0(R2)
R2 = YY
	R1 <-- MEM[R2]
```
![[Pasted image 20241002133218.png]]

```
LD R1, 64(R0)  --> absolute addressing
	R1 <-- MEM[64]
```
![[Pasted image 20241003110841.png]]

[[DADDI vs DADDUI]]
## Instruction Format
CPU instruction is a single 32-bit aligned word
	includes a 6-bit primary opcode

The CPU instruction formats are:
- Immediate
- Register
- Jump

### Immediate
![[Pasted image 20241003111147.png]]
### Register
![[Pasted image 20241003111207.png]]
Only registers in the instruction
### Jump
![[Pasted image 20241003111222.png]]
only instructions with offsets
26 bits, but memory is byte addressable --> up to 28 bits (add two 00 to complete the byte)
00 = 0     100 = 4    1000 = 8     1100 = 12

JR R1 --> R1 can have the address to jump to, so 31-32 bytes addressable
# Instruction Set
Grouped by function, each instruction is 32 bits long
## Load and Store
MIPS processors use a load/store architecture.
Main memory is accessed only through load and store instructions.

- LD load double word
	`LD R1, 28(R8) ==> R1 <- MEM[R8+28]`
- LB load byte
	`LB R1, 28(R8) ==> R1 <- ([MEM[R8+28]]_7)^56 ## MEM[R8+28]`
- LBU load byte unsigned
	`LBU R1, 28(R8) ==> R1 <- 0^56 ## MEM[R8+28]`

- L.S Load FP Single
	`L.S F4, 46(R5) ==> F4 <- MEM[R5+46] ## 0^32`
- L.D Load FP Double
	`L.D F4, 46(R5) ==> F4 <- MEM[R5+46]`

- SD Store Double
	`L.S R1, 28(R8) ==> MEM[R8+28] <- R1`
- SW Store Word
	`SW R1, 28(R8) ==> MEM[R8+28] <-_32 R1 LSB`
- SH  Store Half Word
	`SH R1, 28(R8) ==> MEM[R8+28] <-_16 R1 LSB`
- SB Store Byte
	`SB R1, 28(R8) ==> MEM[R8+28] <-_8 R1 LSB`
## ALU Operations

- Load a constant : add immediate with R0 as a source operand
	`DADDUI R1, R0, #25 ==> R1 <- 25`

- Register to Register : ADD Rx with R0 as source operand
	`DADD R1, R0, R2 ==> R1 <- R2`

- DADDU : double add unsigned (registers)
	`DADDU R1, R2, R3 ==> R1 <- R2 + R3`
- DADDUI : double add unsigned immediate (constants)
	`DADDUI R1, R2, #74 ==> R1 <- R2 + 74`
- LUI : Load Upper Immediate (16-31 bits can be loaded)
	`LUI R1, 0x47 ==> R1 <- 0^{63...32} ## 0x47 ## 0^{15...0}`
	`DADDUI R1, R1, 0x13 ==> R1 <- 0x470013`

- DSLL : Double Shift Left Logical : shift instruction to load first 32 bits (32-63)
	`DSLL R1, R2, #3 ==> R1 <- R2 << 3`
- SLT Set Less Than : 
	`SLT R1, R2, R3 ==> IF (R2<R3) R1 <- 1 ELSE R1 <- 0`

## Branch and Jump

- J : Unconditional Jump
	`J name ==> PC <- name`
- JAL : Jump and Link (procedure call)
	`JAL name ==> R31 <- PC+4; PC <- name`
- JALR : Jump and Link Register ()
	`JALR R4 ==> R31 <- PC+4; PC <- R4`

- JR : Jump Register
	`JR R3 ==> PC <- R3`
- BEQZ : Branch Equal Zero
	`BEQZ R4, name ==> IF (R4=0) then PC <- name`
- BNE : Branch Not Equal
	`BNE R3, R4, name ==> IF (R3 != R4) then PC <- name`

- MOVZ : Conditional Move if Zero
	`MOVZ R1, R2, R3 ==> IF (R3 == 0) then R1 <- R2`
- NOP : No Operation
	`SLL R0, R0, 0`
## Floating Point

The FPU instructions include almost the same instruction types
- Data Transfer Instructions
- Arithmetic Instructions
- Conditional Branch Instructions
- Miscellaneous Instructions

# Assembler Programs

![[Pasted image 20241007193006.png]]

## Data Section
![[Pasted image 20241007193020.png]]
## Code Section
![[Pasted image 20241007193040.png]]

