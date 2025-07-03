CUDA expose GPU parallelism for general-purpose computing, retain performance

CUDA C/C++ : small set of extensions to enable heterogeneous programming, straightforward APIs to manage devices, memory etc...

Host: CPU and its memory
Device: GPU and its memory

Serial code : running on CPU
Parallel code : running on GPU
CPU prepares the GPU to execute

![[Screenshot 2025-06-19 at 12.24.30 PM.png]]

Processing Flow:
- copy memory input data from HOST to DEVICE
- Load GPU program and execute, caching data on chip for performance
- Copy memory output from DEVICE to HOST

Data movement is often the bottleneck of many GPU porting activities or applications
- many unexperienced GPU devs don't keep data transfer problem in mind
- bus transfer can be slow wrt GPU throughput capacity
- data transfer can take more time than GPU computation if the problem is too easy

function which runs on GPU is called **kernel**
- launched on a GPU, thousands of threads will execute its code
- programmers choose the number of threads to launch
- each thread may work on a different data element independently from all other threads

```c++
// .cu file

#include <iostream>
#include <cuda_runtime.h>

__global__ void hello_kernel() {
	printf("Hello world from a GPU !\n");
}

int main() {
	dim3 gridDim(1, 1, 1);
	dim3 blockDim(1, 1, 1);
	hello_kernel<<<gridDim, blockDim>>>();
	cudaDeviceSynchronize();
	return 0;
}
```

`__global__` indicates a function that runs on the device and is called from the host code.
`nvcc` compiler separates source code into host and device components

```
dim3 gridDim(1,1,1);
dim3 blockDim(1,1,1);
helloc_kernel<<<gridDim, blockDim>>>();
```

triple angle brackets mark a call from host code to device code
grids = 3D shape blocks
blocks = 3D shape units of threads

`dim3 gridDim(4,2,1)`
![[Screenshot 2025-06-19 at 1.44.59 PM.png]]

each block: `dim blockDim(4,3,1)`
![[Screenshot 2025-06-19 at 1.45.20 PM.png]]

8 blocks of 12 threads each --> 96 total threads

when kernel starts, it's executed by a number of threads, each of which knows about
- variables passed as arguments
- global constants in device memory
- shared memory and private variables
- some special variables:
	- gridDim - size of grid of blocks
	- blockDim - size of each block
	- blockIdx - index of block
	- threadIdx - index of thread

each thread can use these variables to identify itself, given its identity it should know the data fields it has to manage

Thread Hierarchy: 
a kernel is executed as a grid of thread blocks, all threads share data memory space
in a block, threads that can cooperate with each other by
- synchronizing their execution
- efficiently sharing data thru a low latency shared memory
Two threads from two different blocks cannot cooperate

blocks and threads have IDs, it simplifies memory addressing when processing multi-dimensional data. IDs and dimensions are easily accessible through predefined variables

Index limits are associated to the GPU architecture
programmer must learn basic features of GPU architecture to determine the limits
- Number of SMs (1-80)
- Max number of concurrent threads per SM (1024 - 2048)
- Max number of warp per SM (32)
- Max number of concurrent blocks per SM (1 - 8)
- Max number of blocks (64k)
- Max number of grids (64k)

Memory Management:
host and device memory are separate entities
- host pointers point to CPU memory
- device pointers point to GPU memory
simple CUDA API for handling device memory


![[Screenshot 2025-06-19 at 2.03.06 PM.png]]
![[Screenshot 2025-06-19 at 2.02.57 PM.png]]

Thread Global IDs
in general 2D grid of 2D blocks -->
- grid sizes `gridDim
- block sizes `blockDim`
- `blockIdx` and `threadIdx`

Global X,Y identifier can be computed:
```c
blockIdx = (bx, by, 1);
threadIdx = (tx, ty, 1);

int globalIdx_x = bx * blockDim.x + tx;
int globalIdx_y = by * blockDim.y + ty;

int totalThreadInX = gridDim.x * blockDim.x;

int globalThreadIdx = 
	(by * blockDim.y + ty) * totalThreadsInX + (bx * blockDim.x + tx) 
```

for 3D:

```c++
blockIdx = (bx, by, bz);
threadIdx = (tx, ty, tz);

int globalIdx_x = bx * blockDim.x + tx;
int globalIdx_y = by * blockDim.y + ty;
int globalIdx_z = bz * blockDim.z + tz;

int totalThreadInX = gridDim.x * blockDim.x;
int totalThreadInXY = (gridDim.x * blockDim.x) * (gridDim.y * blockDim.y);

int globalThreadIdx = 
	(globalIdx_z * numThreadsInXY) + (globalIdx_y * numThreadsInX) + globalIdx_x
```

# Memory Models

global host memory (CPU) managed by host code. 
Object can be copied from global host memory to the device and vice-versa using `cudaMemcpy`
There is a global device memory (GPU) managed by the device code. 
Objects must be allocated using `cudaMalloc`

NVIDIA GPUs feature a memory hierarchy designed for parallel processing
- efficiency is profoundly influenced by this hierarchy
Execution speed relies on exploring data locality
- Temporal locality : data item just accessed is likely to be used again shortly, so keep it in a fast memory
- Spatial locality : neighboring data is likely to be used soon so load it finto a fast memory as well

GPU Memories:
- Global Memory
- Shared Memory
- Constant Memory
- Texture Memory
- Local Memory
- Registers
![[Screenshot 2025-06-19 at 2.46.17 PM.png]]
![[Screenshot 2025-06-19 at 2.46.33 PM.png]]

Global Memory: `__global__`
largest and slowest, accessible by all threads in all grids, lifetime of the application, includes objects allocated with `cudaMalloc`
widest scope, variable can be read and modified by any kernel, and also accessible to the host using special CUDA functions

Shared Memory: `__shared__`
fast, on-chip memory shared by all threads within a block
scope limited to thread block, very high efficiency and low latency

Constant memory: `__constant__`
similar to global, but can't be modified
high efficiency if used correctly. almost as fast as register

Texture memory:
read only memory space optimized for specific access patterns, one based on spatial locality, common in graphic applications
Not separate physical memory, rather a way to access global memory thru a dedicated cache that provides faster access and optional filtering capabilities

Local Memory:
private to each individual thread
lifetime is the lifetime of thread that owns it
physically located in off-chip device DRAM
slower than on-chip registers or shared memory

Registers: 
fastest memory elements on the GPU, local to each thread, extremely high efficiency
# CUDA Synchronization

Synchronization is crucial in parallel programming, including CUDA
Many strategies:
- Host-Device Coordination
- Barrier Synchronization
- Atomic Operations
- Thread Fences
- Synchronization Across All Blocks

### Host-Device Coordination
CPU launches kernels and memory transfers on the GPU asynchronously by default.
We need to know when device operations have completed before using the results or freeing resources

Several host-side functions blocks the CPU until all prev issued CUDA tasks on the device have been completed

Kernel launches are asynchronous
- control returns to the CPU immediately
- CPU needs to synchronize before consuming results
- notice that all CUDA API calls return an error code which the CPU can use to check for correctness
### Barrier Synchronization

Problem
one set of threads produce data that another set needs to consume
different stages of an algorithm within a block need to complete before the next stage begins
Threads in a block need to cooperate

`__synchthreads()`

Act as a barrier for all threads within the same thread block: thread encounters it and pause until all other threads in its block have reached that point
it provides no synchronization between different blocks

Ensures all memory operations performed by threads before the `__synchthreads` call are visible to all threads in the block after the barrier
Allows for safe data sharing and coordination between different phases of computation within a block
### Atomic Operations

Problem
multiple threads attempt to read and modify the same memory location without coordination, race condition can occur
final value depends on the non-deterministic order of execution, leading to incorrect results

atomic functions:
```c++
atomicAdd();
atomicSub();
atomicExch(); // exchange
atomicCAS();  // compare and swap
atomicMin();
atomicMax();
```
### Thread Fences

Problem:
CUDA has a relaxed memory consistency model --> hardware might reorder memory operations for performance reasons

```c++
__threadfence_block()
__threadfence()
__threadfence_system()
```

enforce memory ordering constraints

- threadfence block :  block level, ensures all writes to shared/global mem. are in order
- threadfence : grid level, stronger guarantee than the block
- threadfence system : strongest fence, extends visibility guarantee to the host CPU and other GPUs in the system
### Synchronization Across All Blocks

Problem
an algorithm requres all thread blocks to complete a phase before any block can procede to the next phase
CUDA doesn't provide a direct barrier that works across blocks

Primary method: terminate all kernels and relaunch them. 
Alternative: use some smart programming strategies
	devs implement complex software-based synchronization using global memory atomics or persistent thread techniques
	generally less efficient and harder to implement correctly than kernel termination

# CUDA Execution Model and Efficiency

Streaming Multiprocessors (SMs)

GPU hardware made of multiple SMs
Each SM is an independent processing unit with its cores (CUDA Cores) shared memory registers and warp schedulers

programming model guarantees that blocks can execute independently in any order relative to each other, concurrently or sequentially on the same or different SMs
![[Screenshot 2025-06-19 at 3.47.52 PM.png]]

most critical factors on CUDA Efficiency:
- Maximize Memory Coalescing
- Minimize Warp Divergence

### Maximize Memory Coalescing

ensure that threads within the same warp access contiguous, aligned locations in global memory simultaneously
when accesses are coalesced, the GPU can satisfy multiple memory requests with a single transaction, maximizing bandwidth
### Minimize Warp Divergence

Warp = group of threads (generally 32)
GPUs execute threads in warps using a SMIT (Single Instruction Multiple Threads)
ideally 32 threads in a warp execute the same instruction at the same time but on different data

when threads within the same warp take different execution paths, we have warp divergence
the goal is to serialize the execution of each distinct path taken by threads within that warp
	execute one path and the other path is idle
	avoid conditional statements or loops where threads within the same warp take different execution paths based on their threadID or data


