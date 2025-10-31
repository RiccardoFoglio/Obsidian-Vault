
Floating point units perform more complex operations than integer ones
To force them to perform their job in one clock cycle, the designer should either:
- use a slow clock
- make units very complex
Popular alternative: FPU takes more than one clock.
The EX stage is composed of different functional units which may require as many clock cycles as the instruction requires.

![[Pasted image 20241008104713.png]]

**Latency** = number of cycles that should last between an instruction that produces a result and an instruction that uses the same result (length of instruction-1)
**Initiation Interval** = it's the number of cycles that must elapse between issuing two operations of the same type to the same unit
![[Pasted image 20241008104853.png]]
## Hazards
Due to different structure of the EX stage, hazards may become more frequent.
### Structural Hazards
- unpipelined divide unit, when several instructions could need it at the same time
- instructions have varying running times, when the number of register writes required in a cycle is larger than 1
![[Pasted image 20241008105747.png|550]]
Solution:
- add write ports (too expensive)
- force a structural hazard: instructions stalled at ID stage or stalled before entering the MEM or WB stage
### Data Hazards
longer latency of operations -> stalls for data hazards may stall the pipeline for longer periods
![[Pasted image 20241008110317.png|550]]
RAW (Read After Write) hazard

Instructions no longer reach WB in order, therefore new kinds of data hazards are now possible
![[Pasted image 20241008110421.png|550]]
WAW (Write After Write) hazard

Solution: before issuing an instruction to the EX stage, check whether it's going to write on the same register of an instruction still in the EX stage. In case, stall the new instruction.
## Imprecise Exceptions

Guaranteeing precise exceptions is more complex with multiple cycle architectures.
```
DIV.D F0, F2, F4
ADD.D F10, F10, F8
SUB.D F12, F12, F14
```
Instructions `ADD.D` and `SUB.D` complete before `DIV.D` (out-of-order completion). 
IF an exception arises during `SUB.D`, an imprecise exception occurs

Solutions:
- Accepting imprecise exceptions
- Providing a fast but imprecise operating mode and a slow precise one
- Buffering the results of each instructions until all the previously issued instructions have been completed
- Forcing the FP units to early determine whether a instruction can cause an exception and issuing further instructions only when the previous ones are guaranteed not to cause an exception.

## MIPS R4000 Pipeline

MIPS R4000 : 64-bit microprocessor introduced in 1991
Deeper pipeline (8 stages) to account for slower cache access and higher clock frequency

Long pipelines sometime take the name of superpipelines
![[Pasted image 20241014102654.png]]
- Instruction Fetch (2 parts)
- Instruction Decode, Register Fetch : Hazard checking
- Execution (ALU, branch target computation etc..)
- Data Fetch (3 stages)
- Write-Back for load and register-register operations

Instruction and Memory access pipelined: new instruction can start on every clock cycle
More forwarding required, increased load delay slot (2 cycles), increased branch delay slot (3 cycles)

Load Delay Slot
![[Pasted image 20241014103000.png]]
Data available at the end of DS, activating the forwarding logic, it's passed to the ADDD instruction

Branch Delay Slot
![[Pasted image 20241014103108.png]]
Condition evaluation is performed during EX

### FP Pipeline

(last slides # 4)