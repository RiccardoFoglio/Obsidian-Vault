the header thread defines the class `std::thread` that can be used to start new threads

Using this class is the best way to use platform-independent threads

```c++
#include <thread>
```

Using it may require additional compiler flags

```c++
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=C++14 -pthread")
```

The library is based on objects of type `std::thread` : works with any callable object like a function, instance of a class, lambda expression

Library covers all main functionalities but there is no way to automatically capture the data computed by a thread

![[Screenshot 2025-04-11 at 4.37.10 PM.png]]
![[Screenshot 2025-04-11 at 4.37.28 PM.png]]

```c++
std::thread t1; // creates an object that doesn't refer to a thread

std::thread t2(f1); // starts an object thread that calls f1()

std::thread t3(f2, 123, 456); // starts a thread that calls f2(123, 456)

std::thread t4([] { f2(123, 456); }); // works also with lambda functions
```

## Join and Move

The member function Join can be used to wait for a thread to finish.
Function join must be called exactly once for each thread

Standard threads are not copyable, but movable so they can be used in containers
	Moving an `std::thread` transfers all resources associated with the running thread
	Only the new thread can be joined

```c++
std::thread t1; 
std::thread t2(f2, 123, 456); 
// Alternative syntax 
std::thread t3{f2(123, 456)}; 

t1 = std::move(t2);  
t1.detach(); // trying to join after will result in bug
t3.join();   // after it's done i can read the content and use it

// pass by reference
std::thread t (f, std::ref(i));
...
t.join();



```

