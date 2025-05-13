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

![[Screenshot 2025-05-13 at 2.06.23 PM.png]]

maintains multiple threads waiting for tasks to be allocated for concurrent execution
- one main thread generates the tasks (enqueued in a FIFO queue)
- thread pool organizes the working threads manipulating the tasks

main thread:
- initializes the task queue and working threads
- creates work objects (tasks)
- inserts task into the queue

worker threads in the pool:
- get tasks from the queue

size of thread pool is the number of threads ready to execute tasks
- tunable parameter
- crucial to optimize performance
	- creation and destruction is restricted to initial and final generation of pool
	- better performance and better system stability
	- excessive number of threads may waste memory and increase context-switching incurring in performance penalties

Thread pools are not directly available in C++ standard library
- implementable using standard library's facilities

smart implementations of a thread pool may use specific tasks and specific function
	queue stores the tasks to solve but also the functions to solve it
	functions are usually called callback functions and are used to solve the tasks

3rd party libraries implement thread pools:
- Poco::ThreadPool 
- CTPL
- BS:thread_pool

#### Create a basic Thread Pool

```c++
// SOLUTION Part A

class ThreadPool {
private:
	// The pool of threads
	std::vector<std::thread> threads;
	// The task queue
	std::queue<std::function<void()>> tasks;
	// Mutex to protect the task queue
	std::mutex queueMutex;
	// Condition variable for synchronization
	std::condition_variable condition;
	// Flag to request the threads to stop
	bool stop;
```

```c++
// SOLUTION Part B

public:
	// constructor
	ThreadPool(size_t numThreads) : stop(false) {
	for (size_t i = 0; i<numThreads; ++i) {
		threads.emplace_back([this] {
			while (true) {
				std::function<void()> task;
				{
					std::unique_lock<std::mutex> lock(queueMutex);
					// Wait until a task is available or the pool is stopped
					condition.wait(lock, [this] { 
						return stop || !tasks.empty(); 
					});
					if (stop && tasks.empty()) {
						return; // Exit the thread
					}
					task = std::move(tasks.front());
					tasks.pop();
				}
				// execute task
				task();
			}
		});
	}
}
```

```c++
template <class F>
void enqueue(F&& f){
	{
		std::unique_lock<std::mutex> lock(queueMutex);
		if(stop){
			throw std::runtime_error("enqueue on stopped ThreadPool");
		}
		tasks.emplace(std::forward<F>(f));
	}
	condition.notify_one(); 
}
```
allows a function template to accept any kind of argument without writing separate overload functions

```c++
~ThreadPool(){
	{
		std::unique_lock<std::mutex> lock(queueMutex);
		stop = true;
	}
	condition.notify_all();
	for(std::thread& thread : threads){
		thread.join();
	}
}
```

![[Screenshot 2025-05-13 at 2.44.09 PM.png]]
### Semaphore Throttles

Semaphore used to reduce/fix the maximum amount of running Threads in specific (critical) section of code
![[Screenshot 2025-05-13 at 2.47.54 PM.png]]

The number of worker is tunable : the boss T may dynamically decrease or increase the number of active workers by waiting or releasing semaphore units
Workers using more resources:
- should acquire multiple semaphore units
- caution: heavy threads can wait more on the throttles and generate deadlocks

Could be useful to acquire and release semaphores atomically with locks

```c++
m.lock();
cs.acquire();
cs.acquire();
m.unlock();

mtx.lock();
cs.release();
cs.release();
mtx.unlock();
```