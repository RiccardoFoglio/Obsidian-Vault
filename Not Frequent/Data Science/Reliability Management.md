![[Pasted image 20241229170922.png]]

Reliability manager is responsible of the atomicity and durability ACID properties.
It implements the following transactional commands:
- begin transaction (B, usually implicit)
- commit work (C)
- rollback work (A, for abort)

It provides the recovery primitives
- warm restart for main memory failures
- cold restart

It manages the reliability of read/write requests by interacting with the buffer manager 
It may generate new read/write requests for reliability purposes

It exploits the **log file**
- a persistent archive recording DBMS activity 
- stored on **stable memory**

It prepares data for performing recovery by means of the operations 
- checkpoint 
- dump

**Stable Memory** = memory that is resistant to failure. abstraction, approximated by means of redundancy and robust write protocols. Failures in stable memory are considered catastrophic.

**Log File** = sequential file written in stable memory. It records transaction activities in chronological order
Log record types can be transaction records or system records
Writing the log:
- records are written in the current block in sequential order
- Records belonging to different transactions are interleaved

Transaction Records : describe activities performed by each transaction in execution order. Transaction delimiters:
- Begin B(T)
- Commit C(T)
- Abort/Rollback A(T)
where T is the transaction identifier

Data modifications: 
- Insert I(T,O,AS)
- Delete D(T,O,BS)
- Update U(T,O,BS,AS)

O = written object (RID)
AS = After State (state of object O after the modification)
BS = Before State (state of object O before the modification)


System Records : operations saving data on disk or other tertiary storage
- Dump
- Checkpoint CK(L)
L = set of TIDs of active transactions

![[Pasted image 20250101163142.png]]

![[Pasted image 20250101163338.png]]

Undo or Redo can be repeated an arbitrary number of times without changing the final outcome
Useful for managing crashes during the recovery process
> UNDO (UNDO(action)) = UNDO(action)

Checkpoint : operation periodically requested by reliability manager to buffer manager: allows faster recovery process.
During checkpoint, DBMS writes data on disk for all completed transactions (sync write) and records the active transactions

Checkpoint Execution:
1. TIDs of all active transactions are recorded
2. the pages of concluded transactions are synchronously written on disk
3. checkpoint record is synchronously written on the log

After checkpoint, the effect of all committed transactions is permanently stored on disk

Dump : creates a complete copy of the DB, typically performed when the system is offline. DB copy is stored in stable memory, and it may be incremental.

Rules for writing the log: designed to allow recovery in presence of failure
- WAL
- Commit precedence

Write Ahead Log : before state of data in a log record is written in stable memory before database data is written on disk. During recovery it allows execution of undo operations on data already written on disk

Commit precedence : The after state (AS) of data in a log record is written in stable memory before commit. During recovery, it allows the execution of redo operations for transactions that already committed, but were not written on disk

The log is written synchronously (force) for data modifications written on disk, and on commit
The log is written asynchronously for abort/rollback operations

The commit record on the log is a border line: 
- If it is not written in the log, the transaction should be undone upon failure
- If it is written, the transaction should be redone upon failure

Protocols for writing the log and the database:
![[Pasted image 20250101173249.png]]
- All database disk writes are performed before commit : doesn't require redo of committed transactions.
- All database disk writes are performed after commit : doesn't require undo of uncommitted transactions.
- Disk writes for the database take place both before and after commit : It requires both the undo and redo operations.

Mixed approach adopted in real systems

The usage of robust protocols to guarantee reliability is costly, comparable with database update cost 

It is required to guarantee the ACID properties: Log writing is optimized 
- Compact format 
- Parallelism 
- Commit of groups of transactions

# Recovery Management

- System Failure : It causes losing the main memory content (DBMS buffer) but not the disk (both database and log). Software problems or power supply interruptions.

- Media Failure : It causes losing the database content on disk, but not the log content (stored in stable storage). failure of devices managing secondary memory

![[Pasted image 20250101175324.png]]

Recovery : when failure occurs the system is stopped.
recovery depends on the failure type:
- warm restart -> system failures
- cold restart -> media failures
After recovery, system becomes again available to transactions
## Warm Restart
![[Pasted image 20250101175511.png]]
- Transactions completed before the checkpoint : **no recovery** needed (T1)
- Transactions committed but writes on disk haven't been done yet : **redo** needed (T2, T4)
- Active transactions at time of failure: didn't commit, **undo** is needed (T3, T5)

The checkpoint record is not needed to enable recovery, It provides a faster warm restart
Without checkpoint record the entire log needs to be read until the last dump

Algorithm:
1. Read backwards the log until the last checkpoint record
2. Detect transactions which should be undone/redone
	- At the last checkpoint : 
		UNDO = { active transactions at checkpoint }
		REDO = {} (empty)
	- read forward the log
		UNDO = Add all transactions for which the begin record is found
		REDO = Move transactions from UNDO to REDO list when the commit record is found
	- at the end of step 2 :
		UNDO = list of transactions to be undone
		REDO = list of transactions to be redone
3. Data Recovery : 
	- log is read backwards from time of failure until beginning of oldest transaction in UNDO list.
	- log is read forward from beginning of oldest transaction in REDO list
## Cold Restart
It manages failures damaging (a portion of) the database on disk
Main steps:
1. Access the last dump to restore the damaged portion of the database on disk
2. Starting from the last dump record, read the log forward and redo all actions on the database and transaction commit/rollback
3. Perform a warm restart

Alternative to step 2 and 3: perform only actions of committed transactions. 
It requires 2 log reads: detect committed transactions and redo actions of transactions in redo list

