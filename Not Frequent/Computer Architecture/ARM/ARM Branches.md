There are 4 instructions for unconditional branch:
![[Pasted image 20241104124403.png|600]]
BL and BLX save the return address in LR(r14) and hey are used to call subroutines

A standalone program, without OS, cannot compute beyond the end, otherwise the behavior is unpredictable. An infinite loop is added as last instruction:
```
stop B stop

	LDR r1, = stop
stop BX r1
```

Instructions are 16 or 32 bits long, so their address is always halfword aligned.
BX requires the least significant bit of register is 1, otherwise raises a usage fault exception.
BX jumps to the address created by changing the least significant bit of the register to 0.
LDR sets the least significant bit to 1 if the label precedes an instruction. bit is not changed if the label precedes a directive.
ADR, ADR.W and ADRL do not change last bit

Instruction addresses are halfword-aligned, so the immediate value specified in the B instruction is right shifted to save 1 bit.
B generates a Thumb instruction, with 11 bits reserved to the immediate value.
B.W generates a THumb-2 instruction, with 24 bits reserved to the immediate value
BX can jump to any 32-bit value = 4GB

B and BX change the value of PC
A jump can be implemented by changing the value of PC with MOV and LDR:
```
LDR <Rd>, =<label>
MOV PC, <Rd>

LDR PC, =<label>
```

MOV and LDR force the last bit of PC to 0
MOV instead of VX is discouraged: the assembler generates a warning

![[Pasted image 20241108161218.png]]

Compare a branch if Zero: jumps to label if $Rn = 0$
```
CBZ <Rn>, <label>
```

Compare a branch if Nonzero: jumps to label if $Rn \ne 0$
```
CBNZ <Rn>, <label>
```

Rn must be among r0-r7
Only forward branch is possible (4-130 byte)

```
CBZ r0, myLabel == CMP r0, #0, BEQ myLabel
CBNZ r0, myLabel == CMP r0, #0, BEN myLabel
```

almost equivalent instructions. Small differences:
- CMP sets the flags, while CBZ and CBNZ do not
- CBZ and CNBZ jump only forward, range is shorter
- CBZ and CBNZ cannot be used within an IT block

### While Loop
```
	 B test
loop ... ; do something
test CMP r0, #N
	 BNE loop


test CMP r0, #N
	 BEQ exit
	 ... ; do something
	 B test
exit
```

```
loop    CBZ r0, exit
		... ; do something
		B loop
exit    ...
```
### For Loop
```
	   MOV r0, #0
loop   CMP r0, #N
	   BHS exit
	   ... ; do something
	   ADD r0, r0, #1
	   B loop
exit
```

Optimization: 
```
	   MOV r0, #0
loop   ... ; do something
	   SUBS r0, r0, #1
	   BNE loop
exit
```

### Do-While Loop
```
loop    ... ; do something
test    CMO r0, #N
		BNE loop
```
## Conditional Execution

Solution to avoid skipping clock cycles when branches are taken (and next 2 instructions are already been fetched and decoded)

The IT (if-then) block avoids branch penalty because there is no change to program flow

```
		MOV r0, #N
		MOV r1, #M 
		CMP r0, r1 
		BLT neg 
		SUB r2, r0, r1 
		B exit 
neg     SUB r2, r1, r0 
exit    ... ; program continues
```
![[Pasted image 20241108164431.png]]

Becomes
```
		MOV r0, #N 
		MOV r1, #M 
		CMP r0, r1 
		ITE GE 
		SUBGE r2, r0, r1 ; GE if taken
		SUBLT r2, r1, r0 ; LT if not taken (opposite of GE)
		... ; program continues
```
No branch penalty!
![[Pasted image 20241108164419.png]]

IT Syntax
- first statement after IT must be the true case
- up to 4 instructions (true or false) allowed
- the number of instructions in true and false cases must match the number of T and E
- false condition is inverse of true condition
- branches to IT instructions are not allowed
- an IT instruction can be a branch only if it is the last one