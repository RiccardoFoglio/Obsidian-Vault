Pipelines exploit the parallelism existing among instructions which allows their execution in parallel
The highest the amount of ILP that can be found and exploited, the better the performance of the pipeline.

Two approaches to exploit ILP:
- Dynamic : depending on the hardware to locate parallelism
- Static : depending on the software (ex: compiler)
Can be partially combined

Dynamic Approach : dominates desktop and server markets but also included PMD (Personal Mobile Devices)

Static Approach : can mainly be found in products for embedded market. However both the intel IA-64 and Itanium also use this approach
### Basic blocks
first kind of ILP is the one among instructions belonging to the same basic block
A basic block is a sequence of instructions with:
- no branches in, except at the entry
- no branches out, except at the exit
### Rescheduling
Within a basic block, the compiler may reschedule instructions to optimize the code, if the target CPU architecture is known
## ILP in Basic Blocks

For typical MIPS programs the typical size of a basic block is between 4 and 7 instructions
since these instructions are likely to be dependent one from the other, the amount of parallelism existing within a basic block is normally rather small

To further increase the available parallelism, the parallelism among iterations of a loop is considered.

### Loop Unrolling
it's a technique that unrolls the loops, by explicitly replicating the loop body multiple times
![[Pasted image 20241014111506.png]]
If the iteration body corresponds to a basic block, after loop unrolling the block is wider.

In this way: 
- the relative overhead due to the control of iterations is reduced
- the loop body is made wider, this increasing the chance for the compiler to exploit rescheduling to eliminate stalls

Disadvantages: loop unrolling increases the size of the code
## SIMD
Single instruction stream, multiple data streams may be exploited in
- Vector processors : vector instruction operates on a set of data instead of scalar data
- Graphics Processing Units (GPUs) : different functional units perform similar tasks in parallel acting on multiple data
## Dependencies
If two instructions are independent, they can be executed in parallel without any stall
If they are dependent, they have to be executed in order (partially overlapped)
Therefore, exploiting the parallelism among instructions requires first identifying the dependencies existing among them
There are 3 kind of dependencies:
- data
- name
- control

Dependencies are properties of the program
Hazards are properties of the pipeline organization
Stalls depend on the program and the pipeline: a dependency can cause a hazard or not, and the hazard can cause a stall or not

Dependencies
- create the possibility for a hazard
- determine the order in which results must be calculated
- set an upper bound on the amount of parallelism that can be exploited

Detecting dependencies involving registers is easy
Detecting dependencies involving memory cells is much more difficult, because accesses to the same cell can look very different

If static techniques are used, the compiler must adopt a conservative approach, assuming that any load instruction refers to the same cell of a previous store
Dependencies involving memory cells can only be detected at run time, when the addresses are known

### Data Dependencies
Instruction *i* is data dependent on instruction *j* if either of the following conditions hold:
- instruction i produces a result that is used by instruction j
- instruction *j* is data dependent on instruction *k*, and instruction *k* is data dependent on instruction *i*
### Name Dependencies
Name dependency occurs when two instructions refer to the same register or memory location (name) but there is no flow of data associated to the name

There are two kinds of name dependencies between two instructions *i* and *j* :
- **anti dependence** : instruction *j* writes a register or memory location that instruction *i* reads, and instruction *i* is executed first
- **output dependence** : both instruction *i* and instruction *j* write the same register or memory location

Register Renaming : name dependencies do not prevent from reordering involved instructions, provided that we change the register used by one of the two instructions.
This operation can be performed
- Statically by compiler
- Dynamically by the processor
A similar method can be followed for name dependencies involving memory locations

Static Register renaming : some compilers perform register renaming to reduce the number of hazards. Note that detecting all name dependencies requires carefully analyzing the code, taking also into account the effects of branches.

Each time an operand involved in a dependency is accessed in a different order than the original one, there could be a hazard.
This means that the program output may become wrong
Data hazards can be classified in:
- RAW (Read after Write)
- WAW (Write after Write)
- WAR (Write after Read)
### Control Dependencies
A control dependency occurs when an instruction depends on a branch

An instruction that is control dependent on a branch cannot be moved before the branch (so that its execution is no more controlled by the branch)
An instruction that is not control dependent on a branch cannot be moved after the branch (so that its execution becomes dependent on the branch)

Preserving control dependencies is a sufficient condition for preserving the program correctness.
There are cases in which preserving control dependency is NOT a necessary condition
Critical properties for program correctness are:
- exception behavior
- data flow

Exception behavior: any change in the order of instruction execution must not change how exceptions are raised in the program

Data flow: actual flow of data among instructions that produce results and consume them. Data flow must be preserved
