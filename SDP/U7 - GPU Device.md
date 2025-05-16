
# Parallel and Distributed Computing

Programming today a synonym of parallel programming
CPU has 6-8-12 cores, multithreading is growing on other devices
Fast network adapters are usual even in PCs motherboards

concurrency and parallelism are often intended with the same meaning, but don't identify the same thing

- Concurrency = manipulates many things at the same time.
	- multiple tasks are performed in overlapping time intervals without specific order
	- task execution seems contemporary but it's not
- Parallelism = Performs many things at the same time
	- multiple tasks are executed in the same time intervals in multi-processor or multi-core systems
	- obtained by transforming a sequential execution flow into a parallel one

efficiency depends on hardware structures and architectures, software parallelization, capability of compilers to generate efficient code

$$
P = C \cdot Vˆ{2}\cdot F
$$
C = capacity of transistors
V = supply voltage
F = clock frequency

at first: Moore's law --> number of transistors doubles every year, then 2 years, then 18 months...
NVIDIA founder --> Bill Dally Law: energy-efficient parallel computing architectures (GPUs) are the next step to enhance performance without proportional rise in power consumption

different levels of parallelism:
- Bit-level : word length determines the efficiency of an instruction
- Instruction-level : multi-stage pipelines for the execution of an instruction flow
- Task-level : different computations executed in parallel (sorting and matrix product)

classification of parallel architectures:
- Single Instruction Single Data (SISD)
![[Screenshot 2025-05-17 at 1.11.26 AM.png]]
Sequential Von Neumann architecture, single processor executing single stream of instructions

- Single Instruction Multiple Data (SIMD)
![[Screenshot 2025-05-17 at 1.12.56 AM.png]]
single instruction executed concurrently on multiple data streams by multiple processing units

- Multiple Instruction Single Data (MISD)
![[Screenshot 2025-05-17 at 1.14.12 AM.png]]
Multiple instructions operate on a single data flow. not used and not commercially implemented

- Multiple Instruction Multiple Data
![[Screenshot 2025-05-17 at 1.15.07 AM.png]]
Multiple autonomous processors execute different instruction streams independently, operating on different data streams
Most common architecture for parallel computing today

Efficiency: evaluated in time and memory, but there are different metrics

- User Time : total time that the CPU uses to perform a given task a the user level (doesn't take into account the time used in management, IO operations, routine execution at kernel level ecc...)
- CPU Time  : total time dedicated by the CPU to the execution of a task (counts everything)
- Wall-clock time : actual time required to complete a given task --> task finish time - task start time
  (single thread 60 min, 10 threads 10 min each --> 60min CPU time vs 100 min CPU time)

doubling the number of processing elements should halve the wall-clock time
result obtained rarely if a task cannot be parallelized, or only for a small number of processors/cores 







# GPU and Parallelism