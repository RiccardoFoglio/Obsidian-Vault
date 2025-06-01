
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

Amdhal's law: $$speedup = \frac{1}{S+\frac{1-S}{n}}$$
S = percentage of elapsed time for the execution of the sequential part of the program (non parallelizable)
n = number of processors or cores

![[Screenshot 2025-06-01 at 6.44.56 PM.png]]
![[Screenshot 2025-06-01 at 6.45.11 PM.png]]

TO the Ahmad law it should be adde the overhead related to the management of the parallelism:
$$speedup = \frac{1}{S+\frac{1-S}{n}+H(n)}$$

given an infinite number of processors:
$$speedup = \lim_{n \rightarrow \infty} \frac{1}{S+\frac{1-S}{n}} = \frac{1}{S}$$

Small program segments intrinsically sequential limit the total speed-up that can be obtained
![[Screenshot 2025-06-01 at 6.48.30 PM.png]]

Corollary to Amdhal law: decreasing the serializable part and increasing the parallelizable part is more important than increasing the number of processors

The use of parallelism has been subjected to criticism for many years
- limits don't depend only on availability of CPU cycles but also on other factors
- very difficult to compute S and (1-S)

Limits:
- scaling may fail due to lock, barriers for synchronizing, and memory collisions
- Multi-core systems should have multiple caches for lowering memory latency and increasing efficiency
- Some algorithms have better parallel formulations or with a smaller number of execution steps
- Amdhal assumes that the dimension of the problems remains constant, while in general the dimension increases with the increase of the available resources, and what remains constant is the execution time

Gustafson's Law: instead of assuming a fixed problem size, it assumes that the problem size scales up with the number of processors available, typically aiming to keep the total execution time constant
Answers the question: Given more processors, how much larger a problem can I solve in the same amount of time?

Core Idea: as you add more processors, you can tackle significantly larger problems. While the serial portion still takes the same absolute amount of time, its fraction of the total execution time on the larger parallel system decreases because the parallel portion of the workload grows

The Gustafson law expects a linear speed-up:
$$
speedup = S + (1-S)\cdot n = n(n-1) \cdot S
$$

# GPU and Parallelism

GPU = Graphics Processing Unit
Originally designed as a graphics processor: single-chip processor for mathematically-intensive tasks, transforms of vertices and polygons, lighting polygon, clipping, texture mapping, polygon rendering...

Started using GPUs to accelerate a range of scientific applications
- GPGPU = General Purpose GPU
- GPGPU spread in many applications that processed large data sets and could use data-parallel programming models

NVIDIA launches CUDA in 2006 : API that allows to code algorithms for execution on Geforce GPUs using C
Now present in embedded systems, personal computers, game consoles, mobile phones, work-stations


Latency = number of cycles we need to wait for before another dependent operation can start
specifies how long you wait for a specific action to happen or for a piece of data to arrive
Lower latency is generally better, meaning faster response time

Throughput = rate that measures how much data can be processed or moved successfully within a given timeframe
It's about how much work gets done in a set amount of time
Higher throughput is generally better, meaning more data processed

CPUs are designed to execute various tasks, including sequential instructions and complex conditional logic as quickly as possible
Their target is to minimize the time to complete each instruction, minimizing the latency 
- large caches, sophisticated branch prediction, out-of-order execution, high clock speeds --> aimed to reduce latency for individual threads of execution

Threads can run concurrently as long as there is available hardware to use
Each core can run a thread independently from other cores
When there are more software threads on the fly than available hardware cores, the OS makes context switching
- running thread is frozen, and new thread takes the hardware resurces
- context switch involves some overhead

GPUs are designed for tasks that involve performing similar operations on large amounts of data in parallel, like rendering graphics or training AI models
Their goal is to maximize the total amount of work done per unit of time, maximizing the Throughput
	thousands of simpler cores, high memory bandwidth, massive threading capabilities --> GPU process vast datasets concurrently, achieving extremely high throughput for parallelizable workloads

In GPU: thousands of threads can be run in parallel efficiently, threads very lightweight.
- thousands of available registers
- each thread running on GPU maintains its status in its registers
- NO PENALTY in case of context switch

CPU architectures: minimize latency within each thread
GPU architectures: hide latency with computation from other threads

![[Screenshot 2025-06-01 at 8.47.56 PM.png]]


Typical GPU architecture
- Main Global Memory : (16-40GB) very high bandwidth (800-1200 GB/s)
- Streaming Processors (SM) : Grouping independent cores and control units, each SM unit has: many cores, lots of registers, instruction scheduler dispatchers, shared memory with very fast access to data

![[Screenshot 2025-06-01 at 8.51.11 PM.png]]
![[Screenshot 2025-06-01 at 8.51.54 PM.png]]
![[Screenshot 2025-06-01 at 8.52.44 PM.png]]


NVIDIA GPUs are classified based on their compute capability:
- Floating-Point Operations --> support for specific precision levels is consistent within a compute capability
- Advanced Features --> tensor cores are designed for deep learning applications
- Ray Tracing Cores --> real-time ray tracing capabilities introduced in compute capabilities for Turin architecture
- Software Compatibility --> GPUs with the same compute capability are compatible with specific CUDA versions and features
- API Support --> certain APIs and libraries may require a minimum compute capability to function properly
- Performance Metrics --> max threads per block is typically consistent within a compute capability version. Max blocks per multiprocessor consistent for GPUs of the same compute capability

Programming languages deliver maximum flexibility:
![[Screenshot 2025-06-01 at 9.04.34 PM.png]]

CUDA is NVIDIA's program development environment
- based on C/C++ with some extensions
- Fortran support is also available
- lots of sample codes and good documentation
- Fairly short learning curve

AMD has a CUDA lookalike : HIP

Installing CUDA on a system involves 2 components:
- Driver : now-level software that controls the graphics card
- Toolkit (12.5) 
	- the nvcc CUDA complier
	- Nsight plugin for Eclipse or Visual Studio
	- Profiling and debugging tools
	- lots of libraries
