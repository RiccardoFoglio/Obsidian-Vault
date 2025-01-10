## MOV
assigns a value to a register
the value can be:
- content of another register
- constant value
The value cannot be: 
- an address (neither code or data address)
- content of a memory cell

MOV a register into another one:
- `MOV r0, r1`
- `shift` is an optional shift applied to `Rn`
	- `ASR #n` :  arithmetic shift right
	- `LSL #n` : logical shift left
	- `LSR #n` : logical shift right
	- `ROR #n` : rotate right
	- `RRX` : rotate right 1 bit with extend
Equivalent shift instruction is preferred : `LSL r0,r1,#3` corresponds to `MOV r0,r1,LSL #3`

MOV a constant into a register
- `MOV <Rd>, #<constant>`
- constant can be:
	- a value obtained by shifting left an 8 bit value up to 24 positions
	- of form 0x00XY00XY
	- of form 0xXY00XY00
	- of form 0xXYXYXYXY
![[Pasted image 20241103154050.png]]

- `MVN` (move negative) moves the one's complement of the operand into a register
- `MOVW` moves a 16-vit value int ot he low halfword of a register

Extended use of `MOV`: assembler can replace `MOV` with `MVN` or `MOVW` if needed

- a 16-bit value: 0-65535 (MVW) 
- a value obtained by shifting left an 8-bit constant up to 24 positions
- a value obtained by shifting left an 8-bit constant up to 24 positions, and then applying a bitwise logical NOT operation (MVN)
- of the form 0x00XY00XY or 0xFFXYFFXY (MVN)
- of the form 0xXY00XY00 or 0xXYFFXYFF (MVN)
- of the form 0xXYXYXYXY.
## LDR
besides loading values from memory, LDR can be used to load constants into register:
```
LDR <Rd>, =<constant>
```

It's a pseudo-instruction:
- if constant is among the valid values of MOV, then the instruction is replaced with: 
  `MOV <Rd>, #<constant>`
- otherwise a bloc of constant, called literal pool, is created and instruction becomes:
  `LDR <Rd>, [PC, #<offset>]`

Offset : difference between address of literal pool and PC. the PC value is computed:
1. address of current instruction
2. plus 4
3. clearing the second bit for word alignment
The assembler puts literal pool in word-aligned addresses for faster access
Example:
![[Pasted image 20241121134930.png|500]]
## Addresses of literal pool

by default, the literal pool is placed at the END directive, after the last instruction
- LDR is concerted in a Thumb instruction: offset size is ranging \[0, 1020]
- LDR.W is converted in Thumb-2 instruction: offset size is \[-4095, 4095]
- LTORG if the offset is higher

```
	AREA |.text|, CODE, READONLY 
Reset_Handler PROC 
	EXPORT Reset_Handler [WEAK] 
	LDR r0, =0xC90147D2 
	;becomes LDR r0, [pc, #1020] 
stop B stop 
myEmptySpace SPACE 1020 
	ENDP 
	END ;literal pool is saved here
```
```
	AREA |.text|, CODE, READONLY 
Reset_Handler PROC 
	EXPORT Reset_Handler [WEAK] 
	LDR r0, =0xC90147D2 
	;becomes LDR r0, [pc, #1024] 
stop B stop 
myEmptySpace SPACE 1021 
	ENDP 
	END ;literal pool is saved here
```
```
	AREA |.text|, CODE, READONLY 
Reset_Handler PROC 
	EXPORT Reset_Handler [WEAK] 
	LDR r0, =0xC90147D2 
	;becomes LDR r0, [pc, #0] 
stop B stop 
	LTORG ;literal pool is saved here 
myEmptySpace SPACE 4095
	ENDP 
	END
```

## Loading Addresses into Registers

Two pseudo-instructions available:
- `LDR <Rd>, =<label>`
- `ADR <Rd>, <label>`

LDR creates a constant in a literal pool and uses PC relative load to get the data

ADR adds or subtracts an offset to/from PC. Doesn't increase the code size but cannot create all offsets. Loads addresses only in the same area.

```
Stack_Size EQU 0x00000200 
	AREA STACK, NOINIT, READWRITE 
Stack_Mem SPACE Stack_Size 
__initial_sp
	AREA |.text|, CODE, READONLY 
…
	LDR r12, =Stack_Mem 
…
	END ;literal pool is saved here
```

LDR can reference a label outside the current section 

- `ADR` is replaced with a Thumb instruction.
	The offset must be a multiple of 4 in \[0, 1020]

- If the target address is not word aligned, `ADR.W` must be used (replaced with a Thumb-2 instruction). 
	The offset must be in \[0, 4095]. 

- If offset is higher than 4095, `ADRL` must be used (replaced with 2 Thumb-2 instructions). 
	The offset can be up to 1 MB.
