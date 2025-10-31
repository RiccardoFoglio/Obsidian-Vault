DBMS : Data Base Management System
A software package designed to store and manage databases
We are interested in internal mechanisms of a DBMS providing services to applications.
- Useful for making the right design choices
- Some services are becoming available also in operating systems
## Architecture

![[Pasted image 20241114105001.png]]
### Optimizer
It selects the appropriate execution strategy for accessing data to answer queries
It receives in input a SQL instruction (DML)
It executes lexical, syntactic, and semantic parsing and detects (some) errors
It transforms the query in an internal representation (based on relational algebra)
It selects the “right” strategy for accessing data

This component guarantees the data independence property in the relational model
### Access Method Manager
It performs physical access to data
It implements the strategy selected by the optimizer
### Buffer Manager
It manages page transfer from disk to main memory and vice versa
It manages the main memory portion that is preallocated to the DBMS

The memory block pre-allocated to the DBMS is shared among many applications
### Concurrency Control
It manages concurrent access to data --> Important for write operations
It guarantees that applications do not interfere with each other, thus yielding consistency problems
### Reliability Manager
It guarantees correctness of the database content when the system crashes
It guarantees atomic execution of a transaction (sequence of operations)
It exploits auxiliary structures (log files) to recover the correct database state after a failure

## Transactions

A **transaction** is a logical unit of work performed by an application. It is a sequence of one or more SQL instructions, performing read and write operations on the database.

It is characterized by
- Correctness
- Reliability
- Isolation

Transaction start:
- Typically implicit
- First SQL instruction: at beginning of program, after the end of the former transaction

Transaction end:
- COMMIT: correct end of a transaction
- ROLLBACK: end with error
	- database state goes back to state at beginning of transaction

99.9% of transactions commit 
Remaining transactions rollback
	Rollback is required by the transaction (suicide)
	Rollback is required by the system (murder)

Transaction Properties: ACID
- **Atomicity** : transaction cannot be divided in smaller units. Guaranteed by:
	- Undo : The system undoes all the work performed by the transaction up to the current point
	- Redo : The system redoes all work performed by committed transactions
- **Consistency** : A transaction execution should not violate integrity constraints on a database.
	- Enforced by defining integrity constraints in the database schema
	- When a violation is detected, the system may Rollback the transaction or Automatically correct the violation
- **Isolation** : The execution of a transaction is independent of the concurrent execution of other transactions. Enforced by the Concurrency Control block of the DBMS
- **Durability** : The effect of a committed transaction is not lost in presence of failures. It guarantees the reliability of the DBMS. Enforced by the Reliability Manager block of the DBMS