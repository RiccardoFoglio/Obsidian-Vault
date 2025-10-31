Hardware-based speculation is a technique for reducing the effects of control dependences in a processor implementing dynamic scheduling.
If a processor supports branch prediction with dynamic scheduling, it fetches and issues instructions as if the branch prediction was always correct.
If a processor supports hardware-based speculation, it also executes them

It combines 3 ideas:
- Dynamic Branch prediction
- Dynamic scheduling
- Speculation
In such a way, the processor implements data flow execution: operations execute as soon as operands are available

Basic Tomasulo's Architecture is extended. Two different steps in instruction execution:
- computation of results and their bypassing to other instructions
- Update of register file and memory, which is only performed when the instruction is no longer speculative (instruction commit); in this way in-order commitment is implemented
## ReOrder Buffer (ROB)

It's the data structure containing instruction results while the instruction didn't commit yet.
It provides additional virtual registers and integrates the store buffer existing in the Tomasulo's architecture.

Data may be read from the ROB if the producing instruction didn't commit yet, otherwise from register file.

4 Fields:
- Instruction Type: branch, store or register 
- Destination: register number, memory address
- Value: value when the instruction has completed but hasn't committed yet
- Ready: whether the instruction completed its execution

![[Pasted image 20241021112259.png]]

## Instruction Execution Steps

### Issue (or Dispatch)
instruction extracted from queue if there is an empty reservation station AND an empty slot in ROB. If it's not the case, stall.
Operands for the instruction are sent to the reservation station, if they are in the register file or in the reorder buffer.
The number of the reorder buffer entry for the instruction is sent to the reservation station to tag the instruction (and its results when they will be written on the CBD) 
### Execute
Instruction is executed as soon as all required operands are available. Avoids RAW hazards.
Operands are possibly taken from CDB as soon as another instruction produces them.
The length of this step varies depending on the instruction type
### Write Result
As soon as it's available, the result is put on the CDB and sent to the ROB
Any reservation station waiting for the result reads it.
The reservation station entry is marked as available.
### Commit
ROB is ordered according to instructions original order. As soon as instructions reached the head of the buffer 
	if it's a miss predicted branch the buffer is flushed, and the execution is restarted with the correct successor of the instruction
	otherwise the result is written in the register or in memory
	in both cases the reorder buffer entry is marked as free
The ROB is implemented as a circular buffer
### WAW and WAR Hazards
cannot rise since dynamic renaming is implemented and memory updating occurs in-order.
Prevented by:
- enforcing the program order while computing the effective address of a load wrt all earlier store instructions
- avoiding a load to initiate its second step if any active ROB entry occupied by a store has a Destination field matching the A field of the load
### Store Instructions
They write to memory when they commit, only. Therefore their input operand is required when they commit, rather than in the Write Result stage. This means that the ROB should have a further field, specifying where the input operand for each store instruction should come from.
### Exception Handling
Exceptions are not executed as soon as they are raised, but stored in reorder buffer.
When the instruction is committed, the possible exception is executed and the following instructions flushed from the buffer.
If the instruction is flushed from the buffer, the exception is ignored.
Fully *precise exception* handling is thus supported
### Speculating expensive events
When a time-expensive event occurs speculatively, some processors wait for its execution until the event is no more speculative.
On the other side, low-cost events are normally executed speculatively.