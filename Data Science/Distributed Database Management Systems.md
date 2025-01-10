
Data and computation are distributed over different machines.

Advantages:
- performance improve,ent
- increased availability
- stronger reliability

Client / Server
- simplest and more widespread
- server : manages database
- client  : manages user interface

Different DBMS servers on different network nodes
- autonomous
- able to cooperate
Guaranteeing the ACID properties requires more complex techniques

Data replication
- replica = copy of data stored on a different network node
- replication server autonomously manages copy update
- simpler architecture than distributed database

Parallel architectures:
- performance increase is the only objective
- Different architectures: multiprocessor machines or CPU clusters

Data Warehouses:
- servers specialized in decision support
- perform OLAP (on line analytical processing)

# Distributed Database Systems

Client transactions access more than one DBMS server. Different complexity of available distributed services.

*Local Autonomy* : each DMBS server manages its local data in an autonomous way

Functional advantages:
- appropriate localization of data and applications

Technological advantages:
- increased data availability
- enhanced scalability

# Distributed Database Design

Data Fragmentation
given a relation R, a data fragment is a subset of R in terms of tuples, or schema, or both.
Different criteria to perform fragmentation
- horizontal : subset of tuples
- vertical : subset of schema
- mixed : both horizontal and vertical together

Horizontal fragmentation : subset of tuples in R with same schema of R and obtained by means of $\sigma_p$ . Fragments are not overlapped
Union of all fragments provides the original table.

Vertical fragmentation : subset of schema of R, obtained by means of $\pi_X$. Fragments are overlapping on the primary key.
Join on the primary key provides the original table

Fragmentation properties:
- completeness : each information in relation R is contained in at least one fragment $R_i$
- correctness : the information in R can be rebuilt from its fragments

Distributed database design is based on data fragmentation: data distribution over different servers. Each fragment of a relation R is usually stored in a different file, and possibly on a different server.
Relation R doesn't exist, it may be rebuilt from fragments


Allocation of fragments : the allocation schema describes how fragments are stored on different server nodes.

- Non redundant mapping if each fragment is stored on one single node
![[Pasted image 20250102145441.png]]

- Redundant mapping if some fragments are replicated on different servers
	- increased data availability
	- complex maintenance (copy sync is needed)
![[Pasted image 20250102145510.png]]

Transparency levels descrive the knowledge of data distribution
An application should operated differently depending on the transparency level supported by the DBMS
- **fragmentation transparency** : app knows existence of table and not of their fragments
- **allocation transparency** : app knows existence of fragments but not their allocation
- **language transparency** : all details specified, format in which higher level queries are transformed by a distributed DBMS

# Transaction Classification

The client requests the execution of a transaction to a given DBMS server, the DBMS server is in charge of redistributing it

Classes define different complexity levels in the interaction among DBMS servers.
They are based on the type of SQL instruction which the transaction is allowed to contain.

- **Remote Request** : single remote server, read only request 
- **Remote Transaction** : single remote server, any SQL command

- **Distributed Transaction** : Any SQL command, each SQL statement is addressed to one single server (global atomicity needed: 2 phase commit protocol)
- **Distributed Request** : Each SQL command may refer to data on different servers, distributed optimization needed, fragmentation transparency is in this class only

# Distributed DBMS Technology

ACID properties:
- Atomicity : requires distributed techniques like 2 phase commit
- Consistency : Constraints are currently enforced only locally
- Isolation : It requires strict 2PL and 2 Phase Commit
- Durability : It requires the extension of local procedures to manage atomicity in presence of failure

**Distributed query optimization** is performed by the DBMS receiving the query execution request 
It partitions the query in subqueries, each addressed to a single DBMS 
It selects the execution strategy 
	order of operations and execution technique 
	order of operations on different nodes 
		transmission cost may become relevant 
	(optionally) selection of the appropriate replica 
It coordinates operations on different nodes and information exchange

## Atomicity

All nodes (i.e., DBMS servers) participating to a **distributed transaction** must implement the same decision (commit or rollback) 
Coordinated by 2 phase commit protocol

Failure causes: 
- Node failure 
- Network failure which causes lost messages
	- Acknowledgement of messages (ack) 
	- Usage of timeout
- Network partitioning in separate subnetworks

2 Phase Commit protocol:
Objective = coordination of the conclusion of a distributed transaction
Parallel with a wedding: priest celebrating the wedding (coordinates the agreement) and couple to be married (participate to the agreement)

Distributed transaction:
- One coordinator : ==Transaction Manager (TM)==
- Several DMBS servers which take part to the transaction : ==Resource Managers (RM)==
Any participant may take the role of TM, also the client requesting the transaction execution

TM and RM have separate private logs 

Records in the TM log 
- Prepare :  it contains the identity of all RMs participating to the transaction (Node ID + Process ID) 
- Global commit/abort : final decision on the transaction outcome 
- Complete : written at the end of the protocol

New records in the RM log 
- Ready 
	- The RM is willing to perform commit of the transaction 
	- The decision cannot be changed afterwards 
	- The node has to be in a reliable state 
		- WAL and commit precedence rules are enforced
		- Resources are locked 
	- After this point the RM loses its autonomy for the current transaction

## Phase I

1. The TM
- Writes the prepare record in the log
- Sends the prepare message to all RM (participants)
- Sets a timeout, defining maximum waiting time for RM answer
![[Pasted image 20250103151242.png|300]]

2. The RMs 
- Wait for the prepare message
- When they receive it 
	- If they are in a reliable state 
		- Write the ready record in the log 
		- Send the ready message to the TM 
	- If they are not in a reliable state
		- Send a not ready message to the TM 
		- Terminate the protocol 
		- Perform local rollback 
	- If the RM crashed 
		- No answer is sent
![[Pasted image 20250103151307.png|300]]

3. The TM
- Collects all incoming messages from the RMs 
- If it receives ready from all RMs 
	- The commit global decision record is written in the log 
- If it receives one or more not ready or the timeout expires 
	- The abort global decision record is written in the log
![[Pasted image 20250103151442.png|300]]
## Phase II

1. The TM
- Sends the global decision to the RMs 
- Sets a timeout for the RM answers
![[Pasted image 20250103151701.png|300]]

2. The RM
- Waits for the global decision
- When it receives it 
	- The commit/abort record is written in the log 
	- The database is updated 
	- An ACK message is sent to the TM
![[Pasted image 20250103151751.png|300]]

3. The TM 
- Collects the ACK messages from the RMs 
- If all ACK messages are received 
	- The complete record is written in the log 
- If the timeout expires and some ACK messages are missing 
	- A new timeout is set 
	- The global decision is resent to the RMs which did not answer until all answers are received
![[Pasted image 20250103151919.png|400]]

uncertainty window is critical: start after ready msg is sent and ends upon receipt of global decision
Local resources in the RM are locked during the uncertainty window, it should be small
### Failure of a participant (RM)

Warm restart procedure is modified with a new case:
- if the last record in the log for transaction T is "ready" then T doesn't know the global decision of its TM

Log file : ready | undo | redo

Recovery:
- Ready List : new list collecting the IDs of all transactions in ready state
- For all transactions in the ready list, the global decision is asked to the TM at restart (remote recovery request)
### Failure of the coordinator (TM)

Messages that can be lost 
- Prepare (outgoing) (Phase I)
- Ready (incoming) (Phase I)
- Global decision (outgoing) (Phase II)

Recovery 
- If the last record in the TM log is prepare 
	- The global abort decision is written in the log and sent to all participants 
	- Alternative: redo phase I (not implemented) 
- If the last record in the TM log is the global decision 
	- Repeat phase II
### Network Failures

Any network problem in phase I causes global abort 
	The prepare or the ready msg are not received 
	
Any network problem in phase II causes the repetition of phase II 
	The global decision or the ACK are not received
# X-Open-DTP

The previous 2 phase commit won't work if the system is heterogeneous.

Protocol for the coordination of distributed transactions 
It guarantees interoperability of distributed transactions on heterogeneous DBMSs 

Based on 
- One client 
- One TM 
- Several RMs

X-Open-DTP defines interfaces for the communication 
- between client and TM : TM interface 
- between TM and RM : XA interface 
DBMS servers provide the XA interface 
Specialized products implement the TM and provide the TM interface

Standard Features:
- RMs are passive and only answer to remote procedure invocations from the TM 
- The control of the protocol is embedded in the TM 
- The protocol implements **two optimizations** of 2 Phase Commit 
	- Presumed abort 
	- Read only
- Heuristic decision to allow controlled transaction evolution in presence of failures

## Presumed Abort
The TM, when no information is available in the log, answers abort to a remote recovery request by a RM 
- Reduces the number of synchronous log writes : prepare, global abort, complete are not synchronous 
- Synchronous writes are still needed :
	- global, commit in TM log 
	- ready, commit in RM log
## Read Only
Exploited by a RM that did not modify its database during the transaction 
The RM 
- answers read only to the prepare request
- does not write the log
- locally terminates the protocol 

The TM will ignore the RM in phase II of the protocol
## Heuristic decision
Allows transaction evolution in presence of TM failures 
During the uncertainty window, a RM may be blocked because of a TM failure 
Locked resources are blocked until TM recovery 

The blocked transaction evolves locally under operator control 
Transaction end is forced by the operator 
Typically rollback, rarely commit 
Heuristic decision, because actual transaction outcome is not known 
Blocked resources are released

During TM recovery, decisions are compared to the actual TM decisions
- If TM decision and RM heuristic decision are different, atomicity is lost 
- The protocol guarantees that the inconsistency is notified to the client process 
Resolving inconsistencies caused by a heuristic decision is up to user applications
# Parallel DBMS

Parallel computation increases DBMS efficiency
Queries can be effectively parallelized
Different technological solutions are available
- Multiprocessor systems
- Computer clusters

**Inter-query parallelism**
Different queries are scheduled on different processors
Used in OLTP systems
Appropriate for workloads characterized by
- simple, short transactions
- high transaction loads (100-1000tps)
Load balancing on the pool of available processing units

**Intra-query parallelism**
Subparts of the same query are executed on different processors
Used in OLAP systems
Appropriate for workloads characterized by
- complex queries
- reduced query load
Complex queries are partitioned in subqueries. each subquery performs one or more operations on a subset of data.
- group by and join are easily parallelizable
- pipelining operations is possible

# DBMS benchmarks

Benchmarks describe the conditions in which performance is measured for a system
DBMS benchmarks are standardized by the TPC (Transaction Processing Council)
Each benchmark is characterized by
- Transaction load
- Database size and content
- Transaction code
- Techniques to measure and certify performance

![[Pasted image 20250104154656.png]]

