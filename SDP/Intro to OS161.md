# Threads

represents the control state of an executing program
Has an associated context (state)
- processor's CPU state: program counter, stack pointer, other registers, execution mode (privileged / non-privileged)
- Stack, located in the address space of the process

Memory:
- program code (out of context)
- program data (out of context)
- Program stack containing procedure activation records (within context)

User Threads : management done by user-level threads library
- POSIX Pthreads
- Windows threads
- Java Threads

Kernel threads -- supported by the Kernel
- virtually all general purpose OS including
	- Windows
	- Linux
	- mac OS x
	- iOS
	- Android

![[Screenshot 2025-04-02 at 12.27.21 PM.png|400]]

Multithreading Models:
- many-to-one
- one-to-one
- many-to-many

## Process Concept

An OS executes a variety of programs that run as a process
Process = program in execution, process execution must progress in sequential fashion

- program code = text section
- current activity including program counter, processor registers
- stack containing temporary data
	- function parameters, return addresses, local variables
- data section containing global variables
- Heap containing memory dynamically allocated during run time

Program is a passive entity stored on disk (EXE file)
Process is active
	Program becomes process when executable file loaded into memory
Execution of program started vie GUI mouse clocks, command line entry etc...

![[Screenshot 2025-04-02 at 1.43.19 PM.png|400]]

![[Screenshot 2025-04-02 at 1.43.41 PM.png|500]]


Thread Context
![[Screenshot 2025-04-02 at 1.46.22 PM.png|500]]
### Thread Library

Thread library provides programmer with API for creating and managing threads
Two primary ways of implementing:
- library entirely in user space
- kernel-level library supported by the OS

Threads are implemented by a thread library
Thread library stores thread's contexts when not running
Data structure used by thread library to store a thread context (thread control block)
In OS161 thread control block is a thread structure

```c++
/* see kern/include/thread.h */ 
struct thread { 
	char *t_name;                  /* Name of this thread */ 
	const char *t_wchan_name;      /* Name of wait channel, if sleeping */ 
	threadstate_t t_state;         /* State this thread is in */ 
	/* Thread subsystem internal fields. */ 
	struct thread_machdep t_machdep; 
	struct threadlistnode t_listnode; 
	void *t_stack;                 /* Kernel-level stack */ 
	struct switchframe *t_context; /* Saved register context (on stack) */ 
	struct cpu *t_cpu;             /* CPU thread runs on */ 
	struct proc *t_proc;           /* Process thread belongs to */ 
	...
};
```

![[Screenshot 2025-04-02 at 8.55.59 PM.png|500]]

Interface for bootstrap/shutdown (or panic) 
`thread_bootstrap`
`thread_start_cpus`
`thread_panic thread_shutdown`

Interface for thread handling 
`thread_fork`
`thread_exit`
`thread_yield`
`thread_consider_migration`

Internal functions 
`thread_create`
`thread_destroy`
`thread_make_runnable`
`thread_switch`



OS161 Thread Interface

- Make a new thread that will start executing at "entrypoint". The thread will belong to the process "proc" or to the current thread's process if "proc" is null
  The "data" arguments are passed to the function
```c++
int thread_fork(
	const char *name,
	struct proc *proc,
	void (*entrypoint) (void *, unsigned long),
	void *data1,
	unsigned long data2);
```

- Cause the current thread to exit. Interrupts need not to be disabled
```c++
__DEAD void thread_exit(void);
```

- Cause the current thread to yield to the next runnable thread, but itself stay runnable. Interrupts need not to be disabled
```c++
void thread_yield(void);
```

Creating threads using `thread_fork()`

```c++
runthreads(int doloud){
	char name[16];
	int i, result;
	for (i=0; i<NTHREADS; i++){
		snprintf(name, sizeof(name), "threadtest%d", i);
		result = thread_fork(name, NULL, doloud ? loudthread : quietthread, NULL, i);
		if (result) {
			panic("threadtest: thread_fork failed %s)\n", strerror(result));
		}
	}
	for (i=0; i<NTHREADS; i++){
		P(tsem);
	}
}
```


rimasto a 22/72

fino a 60/72