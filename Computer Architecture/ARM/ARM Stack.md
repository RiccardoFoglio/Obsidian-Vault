A stack is a LIFO queue
Data is pushed and popped from the top of the stack.
The stack pointer contains the address of the top of the stack.
![[Pasted image 20241111140302.png]]

Stack pointer updating:
- descending stack : address of the stack pointer decreases after a push
- ascending stack : the address of the stack pointer increases after a push

Entry pointed by the stack pointer:
- empty stack : stack pointer points to the entry where new data will be pushed
- full stack : stack pointer points to the last pushed entry

After 3 PUSH
![[Pasted image 20241111140618.png]]

After 1 POP
![[Pasted image 20241111140749.png]]
## LDM / STM

`LDM{XX} / STM{XX} <Rn>{!}, <regList>`
`Rn` = base register
`XX` specifies the addressing mode
- ! => Rn is set to the updated value
- without ! => Rn is set to the initial value
`regList` is a list of registers

Consecutive registers are indicated by separating the initial and final registers with a dash
Non consecutive registers are separated with a comma
SP cannot appear in the list
PC can appear only with LDM, and only if LR is missing in the list

Order of registers doesn't matter. Registers are automatically sorted in increasing order:
- lowest register is stored into / loaded from the lowest memory address
- the highest register is stored into / loaded from the highest memory address

Addressing Modes:
- IA : increment after
	1. memory is accessed at the address specified in the base register
	2. base register is incremented by 1 word (4 bytes)
	3. if there are other registers in the list, go to 1.
- DB : decrement before
	1. base register is decremented by 1 word (4 bytes)
	2. memory is accessed at the address specified in the base register
	3. if there are other registers in the list, go to 1.

Supported types of stack : stack-oriented suffixes can be used instead of increment/decrement and before/after

Push and Pop instructions facilitate the use of a full descending stack
- `PUSH <regList>` is the same as `STMDB SP!, <regList>`
- `POP <regList>` is the same as `LDMIA SP!, <regList>`

![[Pasted image 20241111143337.png]]