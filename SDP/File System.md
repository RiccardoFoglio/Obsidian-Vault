## File Concept

contiguous logical address space:
types:
- Data --> numeric, char, binary
- Program

Contents defined by file's creator. Many types: text file, source file, executable files...

Attributes:

| Name                              | only info kept in human readable form               |
| --------------------------------- | --------------------------------------------------- |
| Identifier                        | unique tag                                          |
| Type                              | needed for systems that support different types     |
| Location                          | pointer to file location on device                  |
| Size                              | current file size                                   |
| Protection                        | who can read, write and execute                     |
| Time Date and User Identification | data for protection, security, and usage monitoring |
Operations:

Create
Write : at write pointer location
Read : at read pointer location
Reposition within file -- Seek
Delete
Truncate
Open(F) : search the directory structure on disk for entry F and move content to memory
Close(F) : move content of entity F from memory to directory structure on disk

Open File: several pieces of data needed to open files
- open-file table that tracks open files
- file pointer: pointer tho last read/write location per process that has te file open
- file-open count: counter of number of times a file is open (to allow removal of data from open0file table when last processes closes it)
- disk location of the file: cache of data access info
- access rights: per-process access mode info

Open File Locking is provided by some OSs
- shared lock : similar to reader lock, several processes can acquire concurrently
- Executive lock : similar to writer lock

- Mandatory : access is denied depending on locks held and requested
- Advisory : processes can find status on locks and decide what to do

![[Screenshot 2025-06-20 at 2.56.28 PM.png]]

File Structure:
- None - sequence of words, bytes
- Simple record structure: lines, fixed length, variable length
- Complex structures: formatted document, relocatable load file
## Access Methods

- Sequential access
```
read next
write next
reset

no read after last write
```
- Direct access - file is fixed in length logical records
```
read n
write n
position to n
	read next
	write next
rewrite n
```
n = relative block number

![[Screenshot 2025-06-20 at 3.02.36 PM.png]]

Other access methods: can be built on top of base methods
General involve creation of an index for the file
index in memory for fast determination of location of data to be operated on
## Disk and Directory Structure

Directory = collection of nodes containing information about all files
![[Screenshot 2025-06-20 at 3.12.59 PM.png]]

Disk Structure: disks subdivided into partitions. can be RAID protected against failure.
can be user raw (without file system) or formatted with a file system
entity containing file system is known as volume
each volume containing file system also tracks that file system's info in device directory or volume table for contents
![[Screenshot 2025-06-20 at 3.15.26 PM.png]]

mostly general-purpose file systems, but also some special-purpose

Operations performed on Directory: search for file, create file, delete file, list a directory, rename file, traverse the file system

directory organized logically to obtain: efficiency (locating files quickly), naming (convenient to users) and grouping (logical grouping of files by properties)

### Single Level Directory
![[Screenshot 2025-06-20 at 3.23.18 PM.png]]
naming and grouping issues
### Two-Level Directory
![[Screenshot 2025-06-20 at 3.23.43 PM.png]]
Path name, can have the same file name for different users, efficient searching, no grouping
### Tree-Structured Directories
![[Screenshot 2025-06-20 at 3.24.22 PM.png]]
efficient searching, grouping capability current directory (working directory)
absolute or relative path name
creating a new file is done in current directory
delete a file --> `rm <filename>`
create a new subdirectory in current directory --> `mkdir <dirname>`
### Acyclic-Graph Directories
![[Screenshot 2025-06-20 at 3.26.10 PM.png]]
two different names (aliases), if **dict** deletes **list** : dangling pointer
Solution: Backpointers, so we can delete all pointers.
New directory entry type:
- Link : another name to an existing file
- Resolve the link : follow pointer to locate the file
### General Graph Directory
![[Screenshot 2025-06-20 at 3.28.16 PM.png]]
To guarantee no cycles: allow only links to files and not subdirectories, use a garbage collection and every time a new link is added, use a cycle detection algorithm to determine whether it's okay.
## File-System Mounting

File system must be mounted before it can be accessed 
unmounted file system is mounted at a mount point
![[Screenshot 2025-06-20 at 3.29.59 PM.png]]
## File Sharing

Sharing of files on multi-user systems is desirable. Sharing may be done through a protection scheme. On distributed systems, files may be shared across a network
Network File System (NFS) is a common distributed file-sharing method
if multi-user system:
- User IDs identify users, allowing permissions and protections to be per-user
  Group IDs allow users to be in groups, permitting group access rights
- Owner of a file / directory
- Group of a file / directory

### Remote File Systems
networking to allow file system access between systems: manually via programs like FTP, automatically, seamlessly using distributed file systems, semi automatically via the WWW

Client-server model allows clients to mount remote file systems from servers:
- server can serve multiple clients
- client and user-on-client identification is insecure or complicated
- NFL is standard UNIX client-server file sharing protocol
- CIFS is standard Windows protocol
- Standard OS file calls are translated into remote calls

Distributed Information Systems (distributed naming services) such as LDAP, DNS, NIS, Active Directory implement unified access to information needed for remote computing
### Failure Modes

All file systems have failure modes
Remote file systems add new failure modes due to network failure and server failure
Recovery from failure can involve state information about status of each remote request
Stateless protocols include all information in each request, allowing easy recovery but less security

### Consistency Semantics

How multiple users are to access a shared file simultaneously
- Andrew File System (AFS) implemented complex remote file sharing semantics
- Unix File System (UFS) implements
	- write an pen file visible immediately to other users of the same open file
	- share file pointer to allow multiple users to read and write concurrently
- AFS has session semantics: write only visible to sessions starting after the file is closed
## Protection

File owner/creator should be able to control what can be done and by whom
Types of access : Read, Write, Execute, Append, Delete, List

mode of access: read write execute
3 classes of users on unix / linux

![[Screenshot 2025-06-20 at 3.44.02 PM.png]]

for a particular file (ex: *game*) define the appropriate access:
![[Screenshot 2025-06-20 at 3.44.54 PM.png]]
