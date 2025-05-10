provide a place for threads to renderz-vous
allow threads to wait in a race-free way for an arbitrary condition to occur

efficiently wait for a condition to become true without constantly polling (busy-waiting)

- data shared between threads : fix condition 
- mutex protects access to data
- condition variable manages the waiting and notifying of threads

in C++ : 
- std::condition_variable
- in header <condition_variable>

Usage:

```c++
---------------
# define
---------------
#include <thread>
#include <mutex>
#include <condition_variable>
...
std::mutex mtx;
std::condition_variable cv;
bool dataReady = false; 
```

notify threads
```c++
mtx.lock();         // lock mutex to protect CV
done = true;        // modify shared data 
cv.notify_one();    // notify one or all threads waiting on the CV
mtx.unlock();       // release the mutex
```

`notify_one` will wake up at least 1 thread waiting on the condition
`notify_all` will wake up threads waiting on the condition

waiting threads:
```c++
mtx.lock();
while (!done)       // check the condition
	cv.wait(mtx);   // wait on the CV atomically releases the lock and puts                             thread to sleep
mtx.unlock();
```

checking the condition:
- done = false --> !done = true --> execute wait on cv --> release mutex and awaits on condition variable
	- mutex must be released to allow other threads to check the condition
	- when condition variable is signaled, thread wakes up and predicate is checked again
- done = true --> !done = false --> thread goes on and unlocks mutex

need this complexity because --> problem : condition variables have no memory

### Example

```c++
void child(){
	mtx.lock();
	done = true;
	cv.notify_one();
	mtx.unlock();
}

int main (int argc, char *argv[]){
	std::thread t(child);
	mtx.lock();
	while (!done)
		cv.wait(mtx);
	mtx.unlock();
	return 0;
}
```

1. Parent runs first : 
	- acquire lock as done = 0 (false) --> enters while loop and wait for notification
	- child run, done = true, notify threads and unlocks mutex 
	- wait receives signal, while loop one more check and now done = true, exit loop and unlock mutex
2. child runs first : 
	- set done = true, notify threads (nobody waiting rn so it's lost)
	- parent enters critical section, done = true so skip wait (would have missed the notify), unlocks


Mutex is used to protect condition variable and must be locked before waiting
	 wait will atomically unlock the mutex allowing other access to the condition variable
	 when condition variable is signaled, one or more of the threads on the waiting list will be woken up and the mutex will be locked again for that thread

Used to avoid busy waiting (`while (true)` )

CV vs Semaphores
- semaphores are very general and sophisticated, expensive, counter with a mutex, used for general synch schemes and to control the access to a finite number of resources
- CV represents a condition related to a shared data, needs a mutex, used to sync threads based on the condition of the shared data

