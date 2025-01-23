
Technique aimed at rearranging in hardware the instruction execution order to reduce the pipeline stalls while maintaining data flow and exception behavior.

Advantages:
- identifies some dependencies that are unknown at compile time
- simplifies the compiler job
- allows the processor to tollerate unpredictable delays
- allows to run the same code on different pipelined processors

Out of order execution : can introduce WAR, WAW hazards and imprecise exceptions

Most adopted hardware scheme for scheduling: Tomasulo's algorithm

# Tomasulo Approach

Robert Tomasulo, architect of the IBM 360/91 floating point unit
Key ideas: track the operands availability, introduce register renaming
## Reservation Stations
- buffer the operands of instructions waiting to issue, operands are stored in the station as soon as they are available
- Implement issue logic
- univocally identify an instruction in the pipeline: pending instructions designate the reservation station that will provide them with an input operand

![[Pasted image 20241017153605.png]]
- Operation to be performed (Op)
- Values of the source operands (V)
- Reservation stations that will produce a source operand if not available (Q)
- Immediate field first and effective address (A)
- status of reservation station (Busy)
## Register Renaming
Each time an instruction is issued, the register specifiers for the pending operands are renamed to the names of the reservation stations in charge of computing them
This implements a register renaming strategy
WAW and WAR hazards are eliminated

Reservation Station will identify the product of the instruction, not the register on which the value will be stored. 
EX: DADD writes on F0, DSUB writes on F0, MUL uses F0 (from DADD) --> in reservation station it will be loaded that one of the Q values comes from the DADD, not from F0

The reservation stations related to each functional unit control when an instruction can begin execution at that unit.

Common Data Bus (CDB) : results are passed directly to other functional units, rather than being stored in registers. All results from functional units and from memory are sent to CDB which:
- goes everywhere (except to the load buffer)
- allows all units waiting for an operand to load it simultaneously when it's available

![[Pasted image 20241017151502.png]]
(Decode Unit is able to understand if there are going to be stalls, given the code)
(after Instruction Unit, Issue Unit: decodes and dispatches instructions to different units using reservation stations)

## Instruction Execution Steps
- Issue
- Execute
- Write results
### Issue
Get an instruction from Instruction Queue (FIFO strategy)
- If no reservation station is available : structural hazard, stall until one is available
- empty reservation station : send instruction
### Execution
Normal instructions:
When an operand appears on the CDB it's read by the reservation unit
As soon as all operands for an instruction are available in the reservation unit, the instruction is executed.
This way RAW hazards are avoided

Load and Store Instructions:
These instructions compute address in memory and accesses memory.
Reservation station needs to store operand that allows to compute the address. Then it can access memory and write on CDB
Uses special field in reservation station : A, to store the address
- load --> executed
- store --> wait for second operand which has to be stored, then execute

Branches:
To preserve exception behavior, no instruction is allowed to initiate execution until all branches that precede the instruction in program order have completed (BIG LIMITATION)
Speculation may allow to improve this mechanism
### Write Results
When results are available, immediately written on CDB, and from there in registers and functional units.

### Pros and Cons
Advantages:
- hazard detection logic distributed
- Stalls for WAW and WAR hazards eliminated
Disadvantages:
- high complexity hardware (including an associative buffer for each reservation station)
- CDB can be bottleneck

Loop Handling : Loop unrolling is not required by the Tomasulo architecture. Loop instructions are naturally performed in parallel
### Load and Store order
Load and store instructions can be executed in any order, provided that they don't refer to the same address. In this case by exchanging their order
- if the load is before the store, WAR hazard
- if the load is after the store, RAW hazard
To avoid hazards coming from reordering load and store instructions, the processor must: compute the memory addresses in order, perform checks described in the next transparencies for every load or store instruction, possibly block the load or store instruction before accessing to memory.

Each time a load is ready to be issued, the store buffer is checked for store instructions acting on the same address. In this case the load is not sent to load buffer until the store completes.

Each time a store is ready to be issued: The store and load buffers are checked for store instructions acting on the same address. In this case the store is not sent to the store buffer until all the previous load and store instructions complete 

