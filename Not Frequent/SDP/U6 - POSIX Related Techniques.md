
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

In many applications: read from one descriptor, write to another and so on
IO can be performed using blocking, non blocking or asynchronous IO

sometimes we need to perform IO on different descriptors at the same time.
How can we read/write from/to 2 separate descriptors?

Common problem for telnet command:
![[Screenshot 2025-05-16 at 5.50.23 PM.png]]
- reads from terminal and write to a network connection
- reads from network connection and write to terminal

Solution 1: multi-threading --> run two threads with the parent handling one direction of data and the child to the other one

![[Screenshot 2025-05-16 at 5.52.28 PM.png]]

Each thread can perform blocking reads

Forced to use multi-threading or multi-processing
	may need to synchronize the process or the threads
	may need to perform a clean termination when EOF is received in one direction, we must stop both workers


Solution 2: Non-blocking IO --> nonblocking IO in a single process

- set both descriptors to be nonblocking
- issue a read on the first descriptor:
	- if data present: read and process it
	- if there is no data to read, the call returns and we do the same on the second descriptor
- wait for some amount of time and then we try to read from the first descriptor again

With async IO we avoid polling by letting the kernel notify us with a signal when a descriptor is ready

Async IO is far to be easy to implement
- Portability issue between systems
- polling --> decide how often we want to poll
- signals --> number of signals allowed for each process is limited

Solution 3 : Multiplexing IO --> use a function that accepts more than one IO descriptor, doesn't return until one of the descriptors is ready for IO, on return it tells us which descriptors are ready for IO

Logic flow:
- build a list of the descriptors that we are interested in
- use multiplexing IO function
		`select` `pselect` `poll`

select function:
```c++
#include <sys/select.h>
int select (
	int maxfdp1,
	fd_set *restrict readfds,
	fd_set *restrict writefds,
	fd_set *restrict exceptfds,
	struct timeval *restrict tvptr);
```

### Example

how to prepare a read set
```c++
fd_set rset;
int fd;

FD_ZERO(&rset);
FD_SET(fd, &rset);
FD_SET(STDIN_FILENO, &rset);
```

once, select returns, we test whether a given bit is still set
```c++
if (FD_ISSET(fd, &rset)){
	...
}
```

```c++
fd_set set;
FD_ZERO(&set);
FD_SET(fd1, &set);
FD_SET(fd2, &set);
FD_SET(fd3, &set);
size = max(fd1, fd2, fd3)+1;
while (1){
	if (select(size, &set, NULL, NULL, NULL) < 0) {
		// handle error
	}
	for (i=0; i<size; ++i){
		if (FD_ISSET(i, &set)){
			// there is data for fd in position i
		}
	}
}
```

# File Mapping

OSs provide memory-mapped files to associate a file with the process' address space
Allow the OS to manage all data movement between file and memory while user copes with memory address space
permit the programmer to manipulate the files without file IO functions

![[Screenshot 2025-05-16 at 6.14.56 PM.png|400]]

 advantages to mapping normal files to virtual memory space directly:
 - applications faster, in-memory algorithms can directly process files
 - manipulate larger quantity of data
 - There is no need to manage buffers and the file data they contain
 - multiple processes can share memory and the file views will be coherent

```c++
#include <sys/mman.h>
void *mmap (
	void *addr, size_t len, int prot, int flag, int fd, off_t off
);
```

![[Screenshot 2025-05-16 at 6.18.12 PM.png]]

- addr : address where we want the mapped region to start
- len : number of bytes to map
- prot : protection of the mapped region (read, write, exec...)
- flag : FIXED, SHARED, PRIVATE 
- fd : file descriptor specifying the file that is to be mapped
- off : offset in the fle of the bytes to map

memory-mapped region is unmapped when
- process terminates
- unmap a region by calling the munmap function
- closing the file descriptor doesn't unmap the region

Return value: 0 if success, -1 if errors

### Example

using memory mapping, copy a source file into a destination (equivalent) file
(implement the shell command "cp") adopting memory mapped files
program receives source and destination file names on the command line

```c++
#include "apue.h"
#include <fcntl.h>
#include <sys/mman.h>

#define COPYINCR (1024*1024*1024)

int main (int argc, char *argv[]){
	int fdin, fdout;
	void *src, *dst;
	 size_t copysz;
	 struct stat sbuf;
	 off_t fsz = 0;

	fdin = open(argv[1], O_RDONLY);
	fdout = open(argv[2], O_RDWR|O_CREAT|O_TRUNC, FILE_MODE);
	if (fdin < 0 || fdout < 0) error ...
	if (fstat(fdin, &sbuf) < 0) error ...

	while (fsz < sbuf.st_size){
		if ((sbuf.st_size -fsz) > COPYINCR)
			copysz = COPYINCR;
		else
			copysz = sbuf.st_size - fsz;
		if ((src - mmap (0, copysz, PROT_READ, MAP_SHARED, fdin, fsz)) == MAP_FAILED) ... error ...
		if ((dst = mmap (0, copysz, PROT_READ | PROT_WRITE, MAP_SHARED, fdout, fsz)) == MAP_FAILED) ... error ...
		memcpy(dst, src, copysz);
		munmap(src, copysz);
		munmap(sdt, copysz);
		fsz += copysz;
	}
	return 0
}



```