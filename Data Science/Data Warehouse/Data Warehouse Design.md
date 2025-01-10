![[Pasted image 20241006150218.png]]

- Conceptual Design : extract initial requirements
- Logical Design : convert into a logical model
- Physical Design : physical layout

## Conceptual Design

Can't use the ER model, not adequate. 

*Dimensional Fact Model*
- graphical model supporting conceptual design
- defines a fact schema modelling
	- dimensions
	- hierarchies
	- measures
- provides design documentation both for requirement review with users and after deployment

### Dimensional Fact Model

- **Fact** -> models a set of relevant events, evolves with time
- **Dimension** -> describes the analysis coordinates of a fact, characterized by many categorical attributes
- **Measure** -> describes a numerical property of a fact, aggregates are frequently performed on measures
![[Pasted image 20241006153638.png]]

DFM Hierarchy
- Each dimension can have a set of associated attributes
- Attributes describe the dimension at different abstraction levels and can be structured as a hierarchy
- the hierarchy represents a generalization relationship among a subnet of attributes in a dimension
- the hierarchy represents a functional dependency (1:n)

Comparison between ER and DFM
![[Pasted image 20241006170455.png]]
### Advanced DFM

![[Pasted image 20241006170517.png]]

- optional dimension -> dimension that is applicable only in some specific combinations
- optional edge -> value that makes sense only for some instances of dimension
- descriptive attribute -> not used for patterns, only additional information
- convergence -> attribute branches converge to the same bigger node

#### Aggregation

computes measures with a coarser granularity than those in the original fact schema. detail reduction is obtained by climbing the hierarchy. 
standard aggregate operators: SUM MIN MAX AVG COUNT

Measure characteristics:
- additive
- non additive -> cannot be aggregated a long a given hierarchy by means of SUM
- not aggregable

Example: number of customers non additive wrt product: one customer can buy multiple products so if you sum by products, you would sum the same customer over and over!
#### Measure Classification
- Stream Measures : 
	- can be evaluated cumulatively at the end of a time period
	- can be aggregated by means of all standard operators
	- sold quantity, sale amount etc...
- Level Measures : 
	- evaluated at a given time (snapshot)
	- not additive along the time dimension
	- inventory level, account balance etc...
- Unit Measures : 
	- evaluated at a given time and expressed in relative terms
	- not additive along any dimension
	- unit price of a product etc...
#### Aggregate Operators
- Distributive : 
	- can always compute higher level aggregations from more detailed data
	- sum min max
- Algebraic : 
	- can compute higher level aggregations from more detailed data ONLY when supplementary support measures are available
	- avg (requires count)
- Olistic : 
	- cannot compute higher level aggregations from more detailed data
	- mode, median
#### Advanced Features
- shared node : phone number both caller or called number
- multiple edge : one to many going up the hierarchy (book <--> author)
- configuration attribute : multi-valued categorical attribute (set amount of possible values), 
### Factless Fact Schema
Some events are not characterized by measures (empty fact schema)
Used for counting occurred events (course attendance) or representing events not occurred (coverage set)
### Time
Data modification over time is explicitly represented by event occurrences (time dimension, events stored as facts)

Dimensions may change over time
- modifications are typically slower
- client demographic data, product description etc...
- if required, dimension evolution should be explicitly modeled with these types

- Type 1 -> snapshot of current value
	- data overwritten with current value
	- overrides past with current situation
	- used when explicit representation of the data change is not needed
- Type 2 -> events are related to the temporally corresponding dimension value
	- after each state change in a dimension
		- new dimension instance created
		- new events are related to the new dimension instance
	- events are partitioned after the changes in dimensional attributes
- Type 3 -> al events are mapped to a dimension value sampled at a given time
	- requires explicit management of dimension changes during time
	- dimension schema modified
		- 2 timestamps, validity start and validity end
		- new attribute which allows identifying the sequence of modifications on a given instance
	- each state change in the dimension requires the creation of a new instance

## Logical Design

We address the Relational Model (ROLAP)

Inputs:
- conceptual fact schema
- workload
- data volume
- system constraints

Output:
- relational logical schema

based on different principles with respect to traditional logical design
- data redundancy
- table denormalization

**Dimensions** :
- one table for each dimension
- generated primary key
- contains all dimension attributes
- hierarchies not explicitly expressed
- totally denormalized representation

**Facts** :
- one fact table for each fact schema
- primary key composed by foreign keys of all dimensions
- measures are attributes of the fact table

**Star Schema**
![[Pasted image 20241006183616.png|600]]
Flat dimensions, represented in tables

**Snowflake Schema**
![[Pasted image 20241006184414.png|600]]

Partitioning dimensional table in further tables, connecting them via foreign key
Reduces data redundancy, but expensive to join tables to reach data
Not recommended, storage space decrease is rarely beneficial, and the cost of join execution may be significant
### Multiple Edges

Two ways to deal with multiple edges:
![[Pasted image 20241007135432.png]]

- Bridge Table to represent the many to many relationship of a multiple edge
This way we can use:
**Weighted Query**: consider the weight of multiple edges
Example: Author Income --> 
```SQL
SELECT Author_ID, SUM(Income*Weight)
FROM Sales, Books, Bridge, Authors
WHERE JOIN ...
GROUP BY Author_ID
```
**Impact Query**: don't consider weight of multiple edges
```SQL
SELECT Author_ID, SUM(Income)
FROM Sales, Books, Bridge, Authors
WHERE JOIN ...
GROUP BY Author_ID
```
Cons: additional JOIN operations and new tables introduced

- Multiple Edge is pushed down in the fact table, it also has the foreign key
Increasing the size of Fact Table
Reduces the amount of JOIN operations
Difficult to implement Impact Queries
Not very used
### Dimensions with Single Attribute

- Integration into the Fact Table
- Junk Dimension : collects all single attributes. (used when attribute domains have small cardinality) (typically used)

