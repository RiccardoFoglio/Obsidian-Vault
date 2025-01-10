A subroutine must contain
- a label before the first instruction
- at the end a branch to the address stored in LR
```
MyAdd    ADD r0, r0, r1
         BX LR
```

Optionally, the begin and end can be indicated with a couple of directives (PROC, ENDP)

A subroutine is called with BL or BLX
![[Pasted image 20241113141241.png]]
`BL <label>` and `BLX <Rn>`:
- write the address of the next instruction to LR
- write the value of `label` or `Rn` to `PC`

Nested Calls to subroutines:
![[Pasted image 20241113142106.png]]
When sub1 calls sub2, LR is overwritten
sub1 is not able to return to main.
Besides changing LR when called, sub2 may also change the value of registers used in sub1.
Every subroutine should save LR and the other used registers as first instruction:
`PUSH {regList, LR}`
At the end, the subroutine restores PC and the initial value of the used registers:
`POP {regList, PC}`

Passing Parameters: 3 approaches
- in register
- by reference --> register with an address in memory
- on the stack

In Registers:
```
		MOV r0, #0x34 
		MOV r1, #0xA3 
		BL sub1 
		; r2 contains the result 
... 
stop    B stop 
...


sub1    PROC 
		PUSH {LR} 
		CMP r0, r1 
		ITE HS 
		SUBHS r2, r0, r1 
		SUBLO r2, r1, r0 
		POP {PC} 
		ENDP
```

By Reference:
```
		MOV r0, #0x34 
		MOV r1, #0xA3 
		LDR r3, =mySpace 
		STMIA r3, {r0, r1} 
		;r3 holds address of parameters BL sub2 
		;r3 holds address of result 
		LDR r2, [r3] 
		;r2 holds result 
... 
stop    B stop 
...

sub2    PROC 
		PUSH {r2, r4, r5, LR} 
		LDMIA r3, {r4, r5} 
		CMP r4, r5 
		ITE HS SUBHS r2, r4, r5 
		SUBLO r2, r5, r4 
		STR r2, [r3] 
		POP {r2, r4, r5, PC} 
		ENDP
```

On the Stack:
```
		MOV r0, #0x34 
		MOV r1, #0xA3 
		PUSH {r0, r1, r2} 
		BL sub3 POP {r0, r1, r2} 
		; r2 contains the result 
... 
stop    B stop 
...

sub3    PROC 
		PUSH {r6, r4, r5, LR} 
		LDR r4, [sp, #16] 
		LDR r5, [sp, #20] 
		CMP r4, r5 
		ITE HS 
		SUBHS r6, r4, r5 
		SUBLO r6, r5, r4 
		STR r6, [sp, #24] 
		POP {r6, r4, r5, PC} 
		ENDP
```

### ARM Architecture Procedure Call Standard (AAPCS)
It regulates the interaction between a calling program and the called subroutine:
- obligations on the caller: proper program state
- obligations on the called subroutine: which parts of the program state must be preserved
- rights of the called subroutine: which parts of the program state can be changed

Obligations on the caller:
- The first 4 parameters are passed in r0-r3
- Further parameters are passed in the stack
- After returning from the called subroutine, parameters must be removed from the stack (the stack pointer must have the same value as before the call)
- The subroutine must preserve the contents of the registers r4-r8, r10, r11 and SP.
- The subroutine can use r0-r3 and r12 as scratch registers.

The return value is passed in r0-r3:
- 32-bit sized type -> r0
- 64-bit sized type -> r0-r1
- 128-bit sized type -> r0-r3