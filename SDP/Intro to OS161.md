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







fino a 60/72

## Kernel Threads
### Thread Library

## User Processes

### Context Switch
### Syscalls & Traps
### Address spaces and address translation
### ELF files and exec load