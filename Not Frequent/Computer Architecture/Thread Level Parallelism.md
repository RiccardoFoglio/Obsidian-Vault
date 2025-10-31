It is unlikely that further evolutions in processor or compiler architecture could achieve significant improvements in the exploitation of the available ILP.

On the other side, in many applications there is a significant amount of parallelism available at a higher level (Thread-Level Parallelism, or TLP).

Threads : A thread is a separate process with its own instructions and data. A thread may correspond to:
- A process within a parallel program consisting of multiple processes, or
- An independent process.

There are applications that naturally expose a high degree of TLP (e.g., many server applications, OSs with multiple applications in parallel).

In other cases, the software is written in such a way to explicitly express the parallelism (e.g., some embedded applications).

In some existing applications it can be relatively difficult to identify and exploit TLP.

## Multithreading

In principle, ILP and TLP can be combined:

> an ILP-oriented processor could use TLP as a source of independent instructions to feed the available functional units as much as possible.

Multithreading allows multiple threads to share the functional units of a single processor in an overlapping fashion, duplicating only threads private state.

It requires: 
- Independently storing the state of each thread (register file, program counter, page table, etc.)
- Independent access to memory (e.g., through the virtual memory mechanism)
- Effective mechanism for switching from one thread to another.

Types of multithreading:
- Fine-grained multithreading: the switch from one thread to another happens at every clock cycle
- Coarse-grained multithreading: the switch happens only when an expensive stall arises
- Simultaneous multithreading (SMT): it mixes fine-grained multithreading and superscalar ideas.

### Fine-grained Multithreading

The execution of multiple threads is interleaved (e.g., in a round-robin fashion, skipping any thread which is stalled at a given time).
The CPU must be able to efficiently switch between tasks at any clock cycle.

**Advantage**: When an instruction is stalled, other instructions can be found in other threads, avoiding any performance loss.

**Disadvantage**: Individual threads could be slowed down by the concurrent execution of other threads.

### Coarse-grained Multithreading

The switch to a new thread is performed only on costly stalls.

**Advantage** : Thread switch can be less efficient.

**Disadvantage** : Performance loss from short stalls can not be avoided.

### Simultaneous Multithreading (SMT)
It is a type of fine-grained multithreading enabling a superscalar processor to exploit ILP and multithreading at the same time.
Multiple instructions from independent threads can be issued using dynamic scheduling capabilities to resolve dependencies among them.
Instructions from multiple threads can be mixed in the data path thank to the high number of available registers and the renaming mechanism

![[Pasted image 20250101181647.png]]

SMT can be implemented on the top of an out-of-order processor by 
- Adding a per-thread renaming table
- Keeping separate PCs
- Allowing instructions from multiple threads to commit (which requires a separate ROB for every thread)

The advantages of SMT wrt ST are higher for integer, than for floating point programs 
SMT advantages vary more widely for floating point programs

Industry Trend:
The current trend is not towards embedding SMT in aggressive speculative processors. 
Rather, designers are opting to implement multiple CPU cores on a single die with less aggressive support for multiple issue and multithreading.