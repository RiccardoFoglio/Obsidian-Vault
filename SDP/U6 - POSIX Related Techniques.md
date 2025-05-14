
Modern OSs hardware-related services are performed with system calls
	access a hard disk drive or device, create and execute a new process etc...

System calls = how a program requests a service from the OS
- provide an essential interface between a process and the OS
- well-defined and limited in number

similar to functions but:
- require intervention of kernel: transfer of control from user space to kernel space
- need some architecture-specific features like a software interrupt or trap
- by nature slow, they require intervention of kernel
	- divided into slow and all other system calls

Slow system calls = can block forever, many related to IO Operations
- Opens can block until some condition occurs on certain files
- Reads can block if the data is not present
- Writes can block if they cannot be accepted

Standard IO System Calls are usually synchronous
- task waits until the operation completes
- synchronous operations are inherently slow compared to the other processing
- Delay may be caused by
	- hardware devices
	- data transfer between physical and system memory
	- network transfer between file servers and computing devices

IO is fundamental and being inefficient implies enormous time penalty
standard UNIX-POSIX introduces numerous advanced IO functionalities such as:
- non-blocking and asynchronous IO
- File Locking
- Multiplexing IO
- Memory-Mapped IO

# File Locking

2 tasks editing the same file --> final state = last task that wrote the file

Always possible to consider a file as critical section and protect its access with a lock
	a file can easily be protected entirely or not at all
	Partial protection is difficult to grant with standard techniques

Some applications would be more efficient with protecting only sections of a file
--> file locking

- ability of a process to prevent other processes from modifying a region of a file
- file locking is a limited form of synchronization

System calls in POSIX standard:
- Flock : locks an entire file
- Lockf : locks a whole file or part of it
- Fcntl : locks a whole file or parts of it (What we will use)

```c++
int fcntl(int df, int cmd, struct flock *flockptr);
```
byte-range locking
- cmd = F_GETLK, F_SETLK, F_SETLKW
- flock structure

![[Screenshot 2025-05-14 at 4.31.13 PM.png|600]]

`F_GETLK` :  Check whether the lock described by `flockptr` is blocked by some other lock
- If a lock exists that would prevent ours from being created, the information on that existing lock overwrites the information pointed to by `flockptr`

`F_SETLK` :  Set the lock described by `flockptr`
- If the compatibility rule prevents the system from giving us the lock fcntl returns immediately with errno set to either `EACCES` or `EAGAIN`
- This command is also used to clear the lock described by `flockptr` (`l_type` of `F_UNLCK`)

`F_SETLKW` :  The blocking version of `F_SETLK`
- The W in the command name means wait
- Set the lock described by `flockptr` with a blocking operation
If the lock cannot be granted, the calling process is put to sleep and the process wakes up when the lock becomes available


```c++
struct flock {
	short l_type;     // shared read lock / exclusive write lock / unlocking
	short l_whence;   // SEEK_SET, SEEK_CUR or SEEK_END
	off_t l_start;    // offset in bytes relative to l_whence
	off_t l_len;      // length in butes, 0 --> lock to EOF
	pid_t l_pid;      // ID of process holding the lock that can block the                                current process (returned by F_GETLK only)
}
```

![[Screenshot 2025-05-14 at 4.38.09 PM.png|500]]

### Guidelines

two parameters specifying start offset of the region are similar to the last two arguments of the `lseek` function
```c++
off_t lseek(int fd, off_t offset, int whence);
```

locks can start and extend beyond the current end of file, but cannot start or extend before the beginning of file

Lock requests: granted or refused
![[Screenshot 2025-05-14 at 4.45.15 PM.png]]

Locks belong to processes
- compatibility rule applies to lock requests made from different processes, not to multiple locks requests made by a single process
- if a process has an existing lock on a range of a file, a subsequent attempt to place a lock on the same range by the same process will replace the existing lock with the new one
	- if a process has a write lock on bytes 16-32 of a file and then tries to place a read lock on bytes 16-32, the request will succeed, and the write lock will be replaced by a read lock

![[Screenshot 2025-05-14 at 4.47.46 PM.png]]

Life locking can produce
- starvation 
- deadlock

# Multiplexing IO




# File Mapping