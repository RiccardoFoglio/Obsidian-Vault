Processor modes:
- Thread mode: on reset or after completing the exception routine
- handler mode: when an exception occurs (handler is always privileged)

Two access levels:
- user level: limited access to resources
- privileged level: access to all resources

![[Pasted image 20241113145320.png|500]]

CONTROL\[0] changes the access level : 0 = privileged, 1 = user
CONTROL\[1] selects the current stack : 0 = Main Stack Pointer MSP, 1 = Process Stack Pointer PSP
![[Pasted image 20241113145816.png]]

CONTROL\[0] is writable only in privileged level.
Switching to user level can occur in any privileged code (Thread or Handler mode)
Switching to privileged level can occur only inside an exception handler (Handler mode)

Move to User Level
```
MRS r8, CONTROL 
ORR r8, r8, #1 
MSR CONTROL, r8
```

Move to Privileged Level
```
MRS r8, CONTROL 
BIC r8, r8, #1
MSR CONTROL, r8
```

Program Status Register
It can be accessed as a combination of
- Application Program Status Register (APSR)
- Execution Program Status Register (EPSR)
- Interrupt Program Status Register (IPSR)
When an exception occurs, IPSR stores the exception number

### Nested Vectored Interrupt Controller
It is involved in the management of internal exceptions and up to 240 external interrupts.
It contains control registers 
- interrupt processing 
- SYSTICK timer 
- debugging 

Registers are accessible as memory-mapped devices, starting from 0xE000E000. 
Most of registers are accessible only in privileged mode

![[Pasted image 20241114161546.png]]

### Task 1 : stack registers

r0-r3, r12, LR, PC and PSR are saved in the currently used stack (MSP or PSP) 
Then, MSP is always used during exception 
PC and PSR are stacked first, so instruction fetch and PSR update can be started early
![[Pasted image 20241114161640.png]]
![[Pasted image 20241114161804.png]]

### Task 2  : fetch interrupt vector table

NVIC puts the exception code on the data bus
The CPU uses the code as an index to access a vector named Interrupt Vector Table (IVT). 
Each entry of the IVT manages an exception: it contains either one ARM instruction or an address in memory. 
In the v7-M architecture, there are addresses of the exception service procedures.

### Task 3: register updates

The following registers are updated when entering the exception handler:
- SP (either MSP or PSP): new value after stacking
- PSR (IPSR only): exception number
- PC: address of the exception handler
- LR: exception return information EXC_RETURN
- some NVIC registers: pending status of exception is cleared, active bit of exception is set.

EXC_RETURN
- Bit 0: processor state. 0 -> ARM, 1 -> Thumb
- Bit 1: reserved, it must be 0
- Bit 2: return stack. 0 -> MSP, 1 -> PSP
- Bit 3: return mode. 0 -> handler, 1 -> thread
- Bits 4-31: each bit must be 1
- Allowed values of EXC_RETURN are:
	- `0xFFFFFFF1` : return to handler mode
	- `0xFFFFFFF9` : return to thread mode with MSP
	- `0xFFFFFFFD` : return to thread mode with PSP

### Task 5 and 6: exception exits
1. The interrupt return sequence is triggered 
	- `BX LR`
	- `PUSH {…, LR}` and `POP {…, PC}`
	- `STM xx, {…, LR}` and `LDM xx, {…, PC}`
2. Unstacking: registers pushed to the stack are restored in the same order
3. NVIC register update
	- the active bit of the exception is cleared
	- if the input of an external interrupt is still asserted, the pending bit is set again, so handler is reentered

### Task 4: execute exception handler
Possible operations performed by the handler: 
- reading a fault status register (FSR) to know the cause of usage, bus, memory management fault
- reading a fault address register (FAR) to know the memory location of a precise fault (bus or memory)
- reading the stacked program counter to retrieve the offending instruction
- Value of bits can be checked through a mask. 
```
LDR r0, =FSRaddress 
LDR{size} r1, [r0] 
TEQ r1, #mask
```

![[Pasted image 20241116162403.png]]
### Non Maskable Interrupt (NMI)
The use of NMI depends on the SoC design.
NMI can be connected to: watchdog timer or voltage-monitoring block, which warns the CPU when the voltage drops below a certain level.
### Hard Fault
Bit 30 set : A bus fault, memory management fault, or usage fault occurred, but the corresponding handler is disabled or cannot be started because another exception with same or higher priority is running, or because exception mask is set.


Attempting to switch to ARM state causes a usage fault.
If usage fault is not enabled, a hard fault is generated. 
```
ADRL r0, label ; label is even, so r0 LSB is 0
BX r0          ; checks if LSB = 0, usage fault
```

![[Pasted image 20241114173628.png]]

Enabling Faults:
```
LDR r2, =SystemHandlerControl 
LDR r1, [r2] 
ORR r1, #0x40000 ; enable usage fault 
ORR r1, #0x20000 ; enable bus fault 
ORR r1, #0x10000 ; enable memory mngm. fault 

;ORR r1, #0x70000 enable all STR r1, [r2]
```

### Usage Fault Status Register
![[Pasted image 20241116152713.png]]

- Switch to ARM state
```
ADRL R0, label
BX R0
```
- Coprocessor Instruction
```
LDC p1, c0, [r1]
```
- Undefined Instruction
```
DCD 0xe7f0def0
```
### Bus Fault Status Register
![[Pasted image 20241116152941.png]]

### MemManage Status Register
![[Pasted image 20241118104156.png]]

### SYSTICK Exception
24-bit timer used to generate interrupts
- internal timer: core free-running clock
- external timer: external reference clock (STCLK)
![[Pasted image 20241118104356.png]]
SYSTICK Reload Value Register stores the value to reload when timer reaches 0.
SYSTICK Current Value Register stores the current value of the timer. Writing any number clears its content
![[Pasted image 20241118104719.png]]
![[Pasted image 20241118104744.png]]

1. Stop timer counter to prevent interrupt triggered accidentally. 
	- write 0 to the Control and Status register 
2. Set the desired interval between interrupts 
	- write a new value to the Reload Value register. 
3. Reset the SYSTICK timer counter 
	- write any value to the Current Value register 
4. Start the SYSTICK timer
	- write proper value to Control and Status register. 
	- e.g.: core clock, interrupt enabled, timer enabled

```
LDR r0, =SYScontrolAndStatusReg 
MOV r1, #0 
STR r1, [r0] ; step 1 
LDR r0, =SYSreloadValueReg 
LDR r1, =1023 ; example 
STR r1, [r0] ; step 2 
LDR r0, =SYScurrentValueReg 
STR r1, [r0] ; step 3 
LDR r0, =SYScontrolAndStatusReg 
MOV r1, #7 
STR r1, [r0] ; step 4
```