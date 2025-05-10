multi-threading should improve performance, but implies overheads
1. waiting on a lock slows down the process
2. create and destroy a thread can be time-consuming
	- over-subscription : number of SW threads ready to start is higher than number of HW threads available in the system
3.  running threads can exceed system resources in some code section

atomic operations --> cannot be interrupted, on single variables (Problem 1)

Thread Pools --> design pattern for achieving concurrency but reducing overheads (Problem 2)

Semaphore Throttles --> strategy to manually reduce the number of workers in critical situation (Problem 3)

### Atomic Operations

`std::atomic<T>` provides atomic operations on single variables of tyoe T
- atomic operation is guaranteed to execute fully without interruption from other threads
- relies on special hardware instructions
- provided by cpu architecture
- operations typically avoid involving the OS scheduler for blocking and are very efficient

### Thread Pools




### Semaphore Throttles