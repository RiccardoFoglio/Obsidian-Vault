# Mutex
![[Screenshot 2025-04-11 at 5.59.08 PM.png]]
![[Screenshot 2025-04-11 at 6.00.27 PM.png]]

Mutex offers exclusive, non-recursive ownership semantics

They implemented in the class std::mutex

- calling thread owns a mutex from the time that it successfully calls either **lock** or **try_lock** until it calls **unlock**
- when a thread owns a mutex all other threads will block or receive a false return value if the attempt to claim the mutex
- a calling thread must not own the mutex prior to calling **lock** or **try_lock**

The behavior of a program is undefined if a mutex is destroyed while still owned by any threads or if a thread terminates while owning a mutex

The class `std::mutex` is neither copyable nor movable

## Recursive Mutex

```c++
std::recursive_mutex rm; 

// F1 must protect IO and then call F2
void f1() { 
	rm.lock(); 
	std::cout << "foo\n"; 
	f2(); 
	rm.unlock(); 
} 

// F2 must also lock and unlock IO because can be called directly (not thru F1)
void f2() { 
	rm.lock(); 
	std::cout << "bar\n"; 
	rm.unlock(); 
}
// standard mutex will block f2 forever
```

## Shared Mutex

A shared mutex is a mutex that allows shared and exclusive locks
- An **exclusive** lock can be obtained by a single thread
- A **shared** can be obtained by more than one thread

It is also possible to use the “try” versions during locking and returns true or false
- Functions `try_lock` try to get an exclusive lock
- Function `try_lock_shared` try to get a shared lock

Shared mutexes are mostly used to implement reader and writer locks 
- Readers use shared locks 
- Writers use exclusive locks

```c++

int value = 0; 
std::shared_mutex sm; 
std::vector tv; 
for (int i = 0; i < 5; ++i) 
	tv.emplace_back([&] { 
		sm.lock_shared(); 
		... use value ... 
		sm.unlock_shared(); 
	}) 
	
for (int i = 0; i < 5; ++i) 
	tv.emplace_back([&] { 
		sm.lock(); 
		++value; 
		sm.unlock(); 
	})
```

## Timed Mutex

The library `std::timed_mutex`, similarly to `std::mutex`, offers exclusive, non-recursive ownership semantics

In addition, `std::timed_mutex` provides the ability to attempt to claim ownership of a `std::timed_mutex` with a timeout via the member functions

RAII Wrappers:
Mutexes can be thought of resources that can be acquired and freed with lock and unlock
Thus, they are easily subject to errors
![[Screenshot 2025-04-11 at 6.27.16 PM.png]]

To avoid these problems it's possible to use RAII paradigm, wrappers for mutexes
- `lock_guard`, `unique_lock`, `shared_lock`, `scoped_lock`

These are RAII-based mutex wrappers that simplify thread synchronization

Their differences lie in flexibility, use cases, and supported mutex types

![[Screenshot 2025-04-12 at 11.40.32 AM.png]]

```c++
std::mutex mtx;
{
	std::lock_guard<std::mutex> lock(mtx);
	// Locks immediately
	// Critical section**
}   // Automatically unlocks
```

The main advantage is not needing to unlock the lock, out of scope unlocks it automatically


![[Screenshot 2025-04-12 at 11.43.58 AM.png]]

```c++
std::mutex mtx;
std::unique_lock<std::mutex> lock(mtx, std::defer_lock);
// Defer locking
if (condition) {
	lock.lock(); // Manual lock
	// Critical section
	lock.unlock(); // Manual unlock
}
```

![[Screenshot 2025-04-12 at 11.47.10 AM.png]]
```c++
std::shared_mutex sh_mt
{
	std::shared_lock<std::shared_mutex> lock(sh_mtx);
	// Share read access
	// Read-only critical section
} // Automatically unlocks
```

![[Screenshot 2025-04-12 at 11.48.33 AM.png]]

```c++
std::mutex mtx1, mtx2;
{
	std::scoped_lock lock(mtx1, mtx2);
	// Locks both mutexes
	// Critical section using both resources
} // Automatically unlocks both
```

![[Screenshot 2025-04-12 at 11.52.42 AM.png]]

# Semaphores

Introduced in C++20

they contain a counter initialized by the constructor 
When the counter is zero they acquire locks until the counter is incremented
![[Screenshot 2025-04-12 at 11.59.02 AM.png]]

Semaphores are not tied to threads, acquiring a semaphore can occur on a different thread than releasing the semaphore

![[Screenshot 2025-04-12 at 11.59.59 AM.png]]

!! compile with `-std=c++20`!!