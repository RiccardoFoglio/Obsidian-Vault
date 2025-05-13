Multi-threading limitations:
- over-subscription : number of SW threads may be higher than number of HW threads
	- --> To solve these issues C++11 introduced **task-based** parallel programming
- threads don't offer any direct way to return a value to the caller
	- --> To solve these issues C++11 introduced **futures** and **promises**

Task = entity that runs asynchronously producing output data that will become available at a later time
- OS associate thread to a task automatically
- balancing tasks is automatic, through work-stealing features
- tasks have possibility of handling return values

1. Thread-based parallel programming relies on **std::thread** objects
```c++
std::thread t(thread_function);
```
2. task based parallel programming relies on **std::asynch** objects
```c++
auto fut = std::asynch(thread_function);
```

36:15 - slide 7 u5e7