![[Pasted image 20241128122134.png]]

The workload of operational DBMSs is measured in tps, i.e., transactions per second ≈ $10$-$10^3$ for banking applications and flight reservations 
Concurrency control provides concurrent access to data
	It increases DBMS efficiency by maximizing the number of transactions per second (throughput) and minimizing response time

Elementary operations are 
- Read of a single data object x : r(x) 
- Write of a single data object x : w(x) 
They may require reading from disk or writing to disk an entire page

The **scheduler** is a block of the concurrency control manager is in charge of deciding if and when read/write requests can be satisfied 
The absence of a scheduler may cause correctness problems, also called anomalies

- Lost Update : transaction lost because both transactions read the same initial value
- Dirty Read : reads the value of X in an intermediate state which never becomes stable
- Inconsistent Read : Transaction read the object twice, with different values
- Ghost Update : partial observation of the effect of other transactions (also due to insertion of new data during transactions)

## Theory of Concurrency Control

Transaction = sequence of read and write operations characterized by same TID (Transaction ID)
$$
r_1(x)r_1(y)w_1(x)w_1(y) 
$$
Schedule = sequence of read/write operations presented by concurrent transactions
$$
r_1(z)r_2(z)w_1(y)w_2(z) 
$$ operations in schedule appear in the arrival order of requests

Concurrency Control accepts or rejects schedules to avoid anomalies
Scheduler has to accept or reject operation execution without knowing the outcome of the transactions (abort/commit)

Commit projection is a simplifying hypothesis:
	*Schedule only contains transactions performing commits*
The dirty read anomaly is not addressed
This hypothesis will be removed later

Serial schedule : transactions appear in sequence, without interleaved actions belonging to a different transaction. This never makes anomalies because the transactions don't work in parallel.
$$
r_0(x)r_0(y)w_0(x)\quad r_2(x)r_2(y)r_2(z)\quad r_1(y)r_1(x)w_1(y)
$$
An arbitrary schedule is correct when it yields the same result as an arbitrary serial schedule of the same transactions.
Schedule is serializable when is equivalent to an arbitrary serial schedule of the same transactions

Different equivalence classes between two schedules:
- view equivalence
- conflict equivalence
- 2 phase locking
- timestamp equivalence

Each equivalence class
- detects a set of acceptable schedules
- is characterized by a different complexity in detecting equivalence

### View Equivalence Approach

Definitions
- **Reads-from** : $r_i(x)$ reads-from $w_j(x)$ when $w_j(x)$ precedes $r_i(x)$ and $i\neq j$ and there is no $w_k(x)$ between them
- **final write** : $w_i(x)$ is the final write if it's the last write of x appearing in the schedule

Two schedules are view equivalent if they have the same reads-from set and same final write set

A schedule is view serializable if it's view equivalent to an arbitrary serial schedule of the same transactions

Detecting view equivalence to a given schedule has linear complexity.
Detecting view equivalence to an arbitrary serial schedule is NP complete, not feasible in real systems
Less accurate but faster techniques should be considered
### Conflict Equivalence Approach

Action $A_j(x)$ is in conflict with action $A_i(x)$ ($i \neq j$) if
- both actions operate on the same object X
- at least one action is a write
	- RW or WR conflicts
	- WW conflicts
- there is no other write between the actions

Two schedules are conflict equivalent if they have the same conflict set and each conflict pair is in the same order in both schedules

A schedule is conflict serializable if it's equivalent to an arbitrary serial schedule of the same transactions

To detect conflict serializability it's possible to exploit the conflict graph:
- node for each transaction
- an edge $T_i$ to $T_j$ if there exists at least a conflict between an action $A_i$ in $T_i$ and $A_j$ in $T_j$
- $A_i$ precedes $A_j$
If the conflict graph is acyclic the schedule is CSR
Checking graph cyclicity is linear in the size of the graph

Computationally expensive

VSR = view serializable
CSR = conflict serializable
![[Pasted image 20241226155627.png]]

## 2 Phase Locking

Lock = block on a resource which may prevent access to others
Lock operation:
- Lock : read or write (R-Lock, W-Lock)
- Unlock

Each read operation is preceded by a request of R-Lock and is followed by a request of unlock.
Similarly for write operations with W-Lock

The read lock is shared among different transactions
The write lock is exclusive, not compatible with any other lock on the same data
Lock escalation request of R-Lock followed by W-Lock on the same data

Scheduler becomes a lock manager : receives transaction requests and grants locks based on locks already granted to other transactions
When the lock request is granted:
- corresponding resource is acquired by the requesting transaction
- when the transaction performs unlock, the resource becomes available again
When the lock request is not granted:
- the requesting transaction is put in a waiting state
- wait terminates when the resource is unlocked and becomes available

The lock manager exploits 
- the information in the lock table to decide if a given lock can be granted to a transaction.
- the conflict table to manage lock conflicts

![[Pasted image 20241227180622.png]]

Read Locks are shared : other transactions may lock the same resource. A counter is used to count the number of transactions currently holding the R-Lock (free when count=0)
The lock manager exploits the information in the lock table to decide if a given lock can be granted to a transaction
- stored in main memory
- for each data object
	- 2 bits to represent the 3 possible object states (free, r_locked, w_locked)
	- a counter to count the number of waiting transactions

2 phase locking is exploited by most commercial DBMS
It's characterized by 2 phases:
- growing phase : needed locks are acquired
- shrinking phase : all locks are released
![[Pasted image 20241227181251.png]]

2 Phase Locking guarantees serializability
A transaction cannot acquire a new lock after having released any lock
![[Pasted image 20241227181419.png]]

Strict 2 Phase Locking allows dropping the commit projection hypothesis
- A transaction locks may be released *only at the end of the transaction* (After COMMIT/ROLLBACK)
After the end of the transaction, data is stable. 
It avoids the dirty read anomaly

Lock manager service interface:
Primitives:
- R-Lock(T, x, ErrorCode, TimeOut)
- W-Lock(T, x, ErrorCode, TimeOut)
- UnLock(T, x)
Parameters:
- T: Transaction ID of the requesting transaction
- x: requested resource
- ErrorCode: return parameter (ok or not ok when request not satisfied)
- TimeOut: maximum time for which the transaction is willing to wait

If the request can be satisfied:
- lock manager modifies state of resource
- returns control to the requesting transaction
- delay is small

if it can't be satisfied:
- requesting transaction inserted in a waiting queue and suspended
- when resource becomes available: first transaction in waiting queue is resumed and granted the lock on the resource
Probability of a conflict: (KxM)/N
- K = number of active transaction
- M = average number of objects accessed by a transaction
- N = number of objects in the database

When timeout expires while transaction is waiting, the lock manager extracts the waiting transaction from the queue, resumes it and returns a not ok error code.
The requesting transaction may perform rollback and possibly restart, or request again the same lock after some time without releasing locks on other acquired resources

## Hierarchical Locking

Table locks can be acquired at different granularity levels:
- table
- group of tuples (fragment)
- single tuple
- single field in a tuple

![[Pasted image 20241227210303.png]]

Hierarchical locking is an extension of traditional locking.
It allows a transaction to request a lock at the appropriate level of the hierarchy
It is characterized by a larger set of locking primitives

- Shared Lock (SL)
- eXclusive Lock (XL)
- Intention of Shared Lock (ISL) : shows the intention of shared locking on an object which is in a lower node in the hierarchy
- Intention of eXclusive Lock (IXL)
- Shared Lock and Intention of eXclusive Lock (SIXL) : shared lock of the current object and intention of exclusive lock for one or more objects in a descendant node

Request Protocol
1. Locks are always requested starting from the tree root and going down the tree
2. Locks are released starting from the blocked node of smaller granularity and going up the tree
3. To request a SL or an ISL on a given node, a transaction must own an ISL (or IXL) on its parent node in the tree
4. To request an XL, IXL or SIXL on a given node, a transaction must own an IXL or SIXL on its parent node in the tree

![[Pasted image 20241227210629.png]]
![[Pasted image 20241227210637.png]]

Selection of lock granularity depends on the application type:
- if it performs localized reads or updates of few objects --> low levels in the hierarchy (detailed granularity)
- if it performs massive reads or updates --> high levels in the hierarchy (rough granularity)

Effect of lock granularity:
- if it's too coarse, it reduces concurrency
- if it's too fine, it forces a significant overhead on the lock manager

Predicate locking addresses the ghost update of type b (insert) anomaly
	for 2PL a read operation is not in conflict with the insert of a new tuple (the new tuple can’t be locked in advance)
Predicate locking allows locking all data satisfying a given predicate 
implemented in real systems by locking indices

Isolation level of a transaction specifies how it interacts with the other executing transactions (may be set by means of SQL statement)
- SERIALIZABLE : highest isolation level, includes predicate locking
- REPEATABLE READ : strict 2PL without predicate locking, reads of existing objects can be correctly repeated, no protection against ghost update (b) anomaly
- READ COMMITTED : not 2PL, read lock is released as soon as the object is read, reading intermediate states of a transaction is avoided (dirty reads avoided)
- READ UNCOMMITTED : not 2PL, data is read without acquiring the lock (dirty reads allowed), only allowed for read only transactions

## Deadlock
concurrent systems managed by means of locking and waiting conditions: all transactions waiting on unlock by the others, so all waiting.

Solutions: 
1. **timeout** --> transaction waits for a given time, after expiration it performs rollback.
typically adopted in commercial DBMS, length of timeout interval:
- long : long waiting before solving the deadlock
- short : overkill, overloads the system

2. pessimistic 2PL --> all needed locks are acquired before the transaction starts (not always feasible)

3. timestamp --> only "younger" transactions are allowed to wait, may cause overkill

4. based on wait graph --> nodes are transactions, an edge represents a waiting state between two transactions. a cycle in the graph represents a deadlock, expensive to build and maintain (used in distributed DBMS)