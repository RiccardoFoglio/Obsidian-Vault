![[Pasted image 20241128103255.png]]

Goal: Providing good performance for database applications 
Application software is not affected by physical design choices, data independence 
It requires the selection of the DBMS product. Different DBMS products provide different 
- storage structures
- access techniques

INPUTS: 
- Logical schema of the database 
- Features of the selected DBMS product e.g., index types, page clustering 
- Workload 
	- Important queries with their estimated frequency 
	- Update operations with their estimated frequency 
	- Required performance for relevant queries and updates

OUTPUTS:
- Physical schema of the database: table organization, indices 
- Set up parameters for database storage and DBMS configuration e.g., initial file size, extensions, initial free space, buffer size, page size. Default values are provided

Selection dimensions
Physical file organization 
- unordered (heap) 
- ordered (clustered)
- hashing on a hash-key 
- clustering of several relations : Tuples belonging to different tables may be interleaved

Indices 
- different structures : e.g., $B^+$-Tree, hash 
- clustered (or primary) : Only one index of this type can be defined 
- unclustered (or secondary) : Many different indices can be defined


For each query 
	accessed tables 
	visualized attributes 
	attributes involved in selections and joins 
	selectivity of selections 

For each update 
	attributes and tables involved in selections 
	selectivity of selections 
	update type (Insert / Delete / Update) and updated attributes (if any)

Selection of 
	physical storage of tables 
	indices 

For each table 
	file structure heap or clustered 
	attributes to be indexed hash or $B^+$-Tree clustered or unclustered

Changes in the logical schema
	alternatives which preserve BCNF (Boyce Codd Normal Form) 
	alternatives not preserving BCNF e.g., in data warehouses
	 partitioning on different disks

Physical design strategies
No general design methodology is available 
	trial and error design process 
	general criteria 
	“common sense” heuristics 

Physical design may be improved after deployment : database tuning

The primary key is usually exploited for selections and joins
index on the primary key: clustered or unclustered, depending on other design constraints
Add more indices for the most common query predicates 
	Select a frequent query 
	Consider its current evaluation plan 
	Define a new index and consider the new evaluation plan : if the cost improves, add the index 
	Verify the effect of the new index on modification workload available disk space

Heuristics
Never index small tables 
	loading the entire table requires few disk reads 
Never index attributes with 
	low cardinality domains low selectivity e.g., gender attribute 
	not true in data warehouses different workloads and exploitation of bitmap indices

For attributes involved in simple predicates of a where clause 
	Equality predicate : Hash is preferred, $B^+$-Tree 
	Range predicate : $B^+$-Tree 
Evaluate if using a clustered index improves in case of slow queries

For where clauses involving many simple predicates
	Multi attribute (composite) index
	Select the appropriate key order the order of attributes affects the usability of the index 
Evaluate maintenance cost

To improve joins 
	Nested loop : Index on the inner table join attribute 
	Merge scan : $B^+$-Tree on the join attribute (if possible, clustered)

For group by : Index on the grouping attributes Hash index or $B^+$-tree 
Consider group by push down 
	anticipation of group by with respect to joins 
	not available in all systems observe the execution plan

