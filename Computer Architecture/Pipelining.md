
Pipelining is an implementation technique whereby multiple instructions are overlapped in execution

In a pipeline, different units (stages) are completing different parts of different instructions in parallel

![[Pasted image 20241007194535.png|500]]

Throughput = number of instructions which exit the pipeline in the time unit
All the pipeline stages are synchronized (proceed to executing a new task all together), the time for executing one step is called *machine cycle* and normally corresponds to one clock cycle
The length of the machine cycle is determined by the slowest stage
CPI = Clock Cycles per Instruction

In an ideal pipeline, all stages are perfectly balanced (require same Time)

 The throughput of an ideal pipeline is
$$
Throughput_{pipelined} = Throughput_{unpipelined} * n
$$
(n is the number of pipeline stages)

Execution of each instruction may be composed of at most 5 clock cycles:
- Instruction fetch cycle (IF)
- Instruction decode / register fetch cycle (ID)
- Execution / Effective address cycle (EX)
- Memory Access / branch completion cycle (MEM)
- Write-back cycle (WB)

### Instruction Fetch (IF)
```
IR <- MEM[PC]
NPC <- PC + 4
```
### Instruction Decode (ID)
```
A <- Regs[IR6...10]
B <- Regs[IR11..15]
Imm <- (([IR]_16)^16##IR16..31)
```

![[Pasted image 20241007195629.png|500]]

IR16..31 : Fixed-field decoding -> allows for decoding to b e performed while registers are read
\## :  sign extension
### Execution (EXE)

- Memory Reference : *ALUOutput <- A + Imm*
- Register-Register ALU Instruction : *ALUOutput <- A op B*
- Register-Immediate ALU Instruction : *ALUOutput <- A op Imm*
- Branch : *ALUOutput <- NPC + Imm; Cond <- (A op 0)*
### Memory Access (MEM)

- Memory Reference : *LMD <- MEM\[ALUOutput] or MEM\[ALUOutput] <- B*
- Branch : *IF (cond) PC <- ALUOutput else PC <- NPC*
### Write-Back (WB)

- Register-Register ALU Instruction : *Regs\[IR16..20] <- ALUOutput*
- Register-Immediate ALU Instruction : *Regs\[IR11..15] <- ALUOutput*
- Load instruction : *Regs\[IR11..15] <- LMD*

![[Pasted image 20241007223148.png]]

All instructions require 5 clock cycles, a simple control unit is required to produce the signals required by the datapath.

A new instruction is started at each clock cycle
Different resources work on different instructions at the same time
At every clock cycle, each resource can be used for one purpose only
Pipeline registers must be added between stages

Pipelining increases the processor throughput without making single instructions faster
single instruction processing is made slower due to the pipeline control overheads.
The depth of a pipeline is limited by:
- the need for balanced stages
- pipelining overhead (pipeline register delay and clock skew)

Example:
- clock cycle: 1ns
- ALU operations: 4 cycles
- Memory operations: 5 cycles
- frequency: 40% ALU, 20% branches, 40% memory
Average Instruction exec. time: 
$$
Clock\ cycle\ \times average\ CPI = 1ns \times ((0.4+.0.2)\times 4 + 0.4 \times 5) = 1ns \times 4.4 = 4.4ns
$$
Pipelining: slows down clock of slowest stage by 20% --> average instruction exec time = 1.2ns
$$speedup = 4.4ns / 1.2ns = 3.7 times$$
## Pipeline Hazards

Hazards are situations that prevent an instruction from executing during its designated clock cycle
- **Structural Hazards** : coming from resource conflicts
- **Data Hazards** : instruction depends on the result of a previous instruction
- **Control Hazards** : depend on branches and other instructions that change the PC
### Stalls
one way of dealing with hazards is to force the pipeline to stall, so to block instruction for one or more clock cycles
When an instruction is stalled:
- the instructions following the stalled instructions are also stalled
- the instructions preceding the stalled instruction continue
A stall causes introduction of a *bubble* in the pipeline
![[Pasted image 20241007225901.png]]
### Structural Hazards
pipeline unit is not able to execute all the operations scheduled for a given cycle
- given unit not able to complete its task in one clock cycle
- pipeline has only one read port and one write port to the register file, but there are cycles where two register reads are requested
- pipeline refers to a single-port memory and there are some cycles in which different instructions would like to access to the memory together
#### Removing Structural Hazards
requires adding new hardware or improving the existing one
The designer has to trade-off between performance and cost, basing on the frequency of occurrence of structural hazards
### Data Hazards
Overlapping the execution of instructions as it's done by pipelining, changes the order of read/write access to operands.
This can result in wrong results and indeterministic behavior.

```
DADDUI R1, R2, R3
DADDUI R4, R1, R2
```
reading and writing on the same register while being used.
#### Interrupt effects
if an interrupt occurs during the execution of a critical piece of code (from the POV of data hazards) correctness may be lost or not, depending on the cases
This may cause an indeterministic behavior.
#### Overcoming Data Hazards
- Implement a forwarding technique
- stalling the instructions requiring the data until they are available
##### Forwarding
Special hardware in datapath detects when a previous ALU operation should write the register corresponding to the source of the current ALU operation
In this case, the hardware selects the ALU result as the ALU input rather than the value from the register file.
The hardware must be able:
- to forward a data from any of the prev. started instructions
- not to forward anything, if the following instruction is stalled or an interrupt occurred

To avoid stalling, forwarding should be made possible between any pipeline register to any input of any functional unit

Data Hazard: whenever there is a dependence between instructions that are close enough to overlap.
Not all potential data hazards can be solved through data forwarding.

Each clock cycle:
- all tests for detecting a possible data hazard concerning an instruction are performed when this is in the ID stage
- if a data hazard is detected, two actions can be alternatively taken:
	- appropriate forwarding activated
	- instruction stalled before entering the stage where operands are not available
![[Pasted image 20241007231734.png]]

Introducing a stall in the EX stage can be done:
- forcing all 0s in the control portion of the ID/EX pipeline register (NOP instruction)
- forcing the IF/ID pipeline register to maintain the current value
### Control Hazards

Due to branches (conditional and unconditional) which may change the PC after the following instruction has been fetched already.
In the case of conditional branches, the decision on whether the PC should be modified or not (branch taken / untaken) can be taken even later.
In the considered implementation the PC is written with the target address at the end of the ID stage.

Solution : stalling the pipeline as soon as a branch instruction is detected (ID) by:
- Decide earlier whether the branch has to be taken or not
- compute earlier the new PC value

Several other techniques to manage branches in a pipeline:
- freezing the pipeline
- predict untaken
- predict taken
- delayed branch
#### Freezing the Pipeline
pipeline is stalled as soon as a branch instruction is detected, until the decision about the branch is known. Simplest to implement.
#### Predict Untaken
Assume the branch is not taken. Avoid any change in the pipeline status until the branch decision has been taken. Undo all performed operations if the branch turns out to be taken
![[Pasted image 20241008084856.png]]
the IDLE result can be obtained by turning the already fetched instruction into a NOP
The cost for the branch instruction is different depending on whether the branch is taken or not.
#### Predict Taken
If the target address is known before the branch outcome, it may be possible to assume the branch is taken.

COMPILER : if the hardware supports both predict taken or untaken scheme, a smart compiler can improve the probability of guessing the prediction by assuming backward branches will be taken and forward branches will not.
This technique can help with prediction accuracy of loops, which are usually backward-pointing branches and are taken more often than not.
#### Delayed Branch
This technique is based on filling the slot after the branch instruction (named *branch-delay slot*) with instructions which have to be executed no matter the branch outcome.
It's up to the compiler to fill each branch-delay slot with the right instructions.
The processor does nothing special when a branch instruction is decoded

It's effectiveness depends on the compiler ability in finding the right instructions to put in the delay slots. Using this technique, only 30% of branches do produce a penalty.

With the advent of deeply pipelined processors, the delays slots are becoming longer and the advantages of delayed-branches smaller.
Therefore, several current RISC architectures don't support delayed branches anymore.
## Exceptions

Exceptions are events that modify the normal execution order of the program
Exceptional events (exceptions, interrupts or faults) are more complex to handle in pipelined architectures because several instructions are being executed at a time.

Possible causes: 
- I/O device request
- Operating system call by a user program
- Tracing instruction execution
- Breakpoint (programmer-requested interrupt)
- Integer arithmetic overflow or underflow
- FP arithmetic anomaly
- Page Fault
- Misaligned memory access
- Memory-protection violation
- Undefined instruction
- Hardware malfunction
- Power failure

Classification:
- **Synchronous vs Asynchronous** --> sync if it can be triggered always at the same position in the code. Async are generated by external devices
- **User Requested vs Coerced** --> user req. are similar to procedures. coerced are out of control of the user program
- **User Maskable vs Non-Maskable** --> user can normally force the hardware not to answer to exception requests
- **Within vs Between instructions** --> if between instructions, exceptions are created by the instruction itself 
- **Resume vs Terminate** --> after activating, the program can either terminate or resume

Restartable machines: are able to handle an exception, save the state, and restart without affecting the execution of the program. Nearly all processors nowadays are restartable machines.

When an exception occurs, the pipeline must execute the following steps:
- force a trap instruction into the pipeline on the next IF stage
- until the trap is taken, turn off all writes for the instruction that raised the exception (faulty instruction) and for all the following instructions in the pipeline
- when the exception-handling procedure receives control, it immediately saves the PC of the faulty instruction

After an exception has been handled, special instructions return the machine from the exception by reloading the PC ad restarting the instruction stream.
### Precise Exceptions
a processor has precise exceptions if the pipeline can be stopped so that:
- instructions before the exception trigger are completed AND
- instructions following the exception trigger can be restarted from scratch
Restarting after an exception may be really hard if exceptions are not handled precisely.
Precise exception handling is a must for most architectures, at least for integer instructions.

Some processors implement precise exceptions in debug mode only. This mode is about 10 times slower than usual normal mode.

Possible sources of exceptions:

| Pipeline Stage | Cause of Exception                                                                         |
| -------------- | ------------------------------------------------------------------------------------------ |
| IF             | Page Fault on instruction fetch<br>Misaligned Memory Access<br>Memory-Protection Violation |
| ID             | Undefined or Illegal Opcode                                                                |
| EX             | Arithmetic Exception                                                                       |
| MEM            | Page Fault on data fetch<br>Misaligned Memory Access<br>Memory-Protection Violation        |
| WB             | None                                                                                       |

Example:
![[Pasted image 20241008101557.png|400]]
A data page fault exception can occur in the MEM stage of the LD instruction, and an arithmetic exception in the EX stage of the DADD instruction.
The first exception is processed and, if its cause is removed, the second exception is handled.

There are cases in which two exceptions can occur in the opposite order of the instructions they relate to.
An exception caused by an IF page fault in the 2nd instruction occurs before an exception caused by a MEM page fault in the 1st instruction.

Possible solution: 
- status flag is associated to each instruction in the pipeline
- if an instruction causes an exception, the status flag is set
- if the status flag is set, the instruction can't perform any write operation
- when an instruction reaches the last stage, and its status flag is set, an exception is triggered
### Instruction Set Complications

When an instruction is guaranteed to complete, it's called *committed*
Some machines have instructions that change the state before they are committed
If one of these is aborted because of an exception, it leaves the machine state altered --> very difficult to implement precise exceptions.
Possible solution is based on allowing to roll-back all the state changes made by an instruction before it is committed.

Instructions implicitly updating condition codes create complications:
- they can cause data hazards
- they require to be saved and restored in the case of an exception
- they make more difficult the task of the compiler for filling possible delay slot between instruction writing the condition codes and the branch one

Complex instructions are difficult to implement in a pipeline. forcing them to have the same length can hardly be achieved.
Sometimes these problems are solved by pipelining the microinstructions implementing each instruction.