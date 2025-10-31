barriers can be used to coordinate multiple threads working in parallel
	a barrier allows each thread to wait until all cooperating threads have reached the same point, and then continue executing from there

![[Screenshot 2025-05-10 at 3.31.32 PM.png|400]]

barriers can be seen as a generalization of the `pthread_join` function
- the `pthread_join` acts as a barrier to allow 1 thread to wait until another thread completes their processing
- barriers allow an arbitrary number of threads to wait until all of the threads have completed their processing
- the threads don't have to exit, as they can continue working after all threads have reached the barrier

Trivial solution: use one semaphore for each thread + semB that acts as a barrier
too many semaphores --> expensive

`std::latch` = one-time synchronization primitive. counts down from initial value set in its constructor. ideal when threads need to wait for specific number of tasks to complete
`std::barrier` = similar to latch but reusable. after the counter reaches 0, resets to initial value, suitable for phased computations where threads need to synchronize multiple times
`std::flex_barrier` = extension of barrier, allowing the execution of a function when the counter reaches zero

barriers used to coordinate multiple threads working in parallel
- all threads to wait until everyone has arrived to a certain point
- simple semaphore would do the opposite, each thread would keep running and the last one will go to sleep
-  