FLAGS
The uppermost nibble of the program status register contains flags.
Flags are set and cleared by means of:
- comparison instructions
- ALU instructions if they have the suffix S
- direct write to the program counter

Negative Flag N: corresponds to the first bit of the result
Overflow Flag V: if either the carry bit goes into the MSB or carry bit comes out of the MSB
Carry Flag C: 
	after sum, C=1 if size is 33bits
	after subtraction, C is inverted
	after move or logical instruction, C is the result of an inline barrel shifter operation
Zero Flag Z: set if the result is zero

Comparison Instructions:
they compare the value or test some bits:
`compare/test <Rd>, <operand2>`
They set the flags without updating `Rd`. Operand2 can be:
- a register with optional shift
- a constant obtained by shifting left an 8-bit value
- a constant of the form 0x00XY00XY
- a constant of the form 0xXY00XY00
- a constant of the form 0xXYXYXYXY

- CMP (compare) subtracts `operand2` from `Rd` and updates the flags
- CMN (compare negative) adds `operand2` from `Rd` and updates the flag
- TST (test) computes the logical AND between `operand2` and `Rd`, then updates flags except V
- TEQ (test equivalence) computes the logical EOR between `operand2` and `Rd`, then updates flags except V
- MRS copies a special register into a register
- MSR copies a general purpose register into a special register

Special Registers can be APSR, EPSR, IPSR, and PSR

ALU can perform: arithmetic operations, logic operations and shift and rotate operations.
The flags are affected only if the suffix S is appended to the instruction

Operand1 is a register, Operand2 can be:
- shifted register, shifted 8-bit constant, 0x00XY00XY, 0xXY00XY00, 0xXYXYXYXY, 12-bit constant (`ADD` and `SUB` only)
### Addition
`ADD <Rd>, <Rn>, <op2>` --> Rd = Rn + op2
### Subtraction
`SUB <Rd>, <Rn>, <op2>` --> Rd = Rn - op2
Reverse subtraction: `RSB <Rd>, <Rn>, <op2>` --> Rd = op2 - Rn
### Multiplication
`MUL <Rd>, <Rn>, <Rm>` --> Rd = Rn * Rm
No distinction between signed and unsigned multiplication with 32-bit result
With 64 bit:
- unsigned : `UMULL <RLo>, <RHi>, <Rn>, <Rm>`
- signed : `SMULL <RLo>, <RHi>, <Rn>, <Rm>`
Multiplication with accumulation: `MLA/MLS <Rd>, <Rn>, <Rm>, <Ra>` --> Rd = Rn\*Rm+/-Ra
Thanks to the inline barrel shifter, some multiplications can be performed without using the multiplier array:
- multiplication by $2^n$ : `LSL r1, r0, #3   ;r1=r0*8`
### Division
- unsigned division : `UDIV <Rd>, <Rn>, <Rm>`
- signed division : `SDIV <Rd>, <Rn>, <Rm>`
--> Rd = Rn / Rm
If the Rn is not exactly divisible by Rm, the result is rounded towards zero
### Logic Instructions
![[Pasted image 20241104123027.png|500]]

![[Pasted image 20241104123718.png|500]]
![[Pasted image 20241104123731.png|500]]