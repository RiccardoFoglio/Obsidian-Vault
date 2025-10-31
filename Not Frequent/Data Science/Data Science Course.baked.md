
Preprocessing
	Data Cleaning -> reduces noise effect, removes outliers, solves inconsistencies
	Data Integration -> reconciles data extracted, integrates metadata, solves conflicts, manages redundancy

Association Rules
- analyze correlations or patterns from transactional database

Classification
	Objectives: prediction of class label, definition of an interpretable model of a given phenomenon

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006135409.png)

Clustering
	detecting groups of similar data objects, identifying exceptions and outliers

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006140129.png)

Other Data Mining Techniques:
- Sequence Mining -> ordering criteria on analyzed data taken into account
- Time Series and Geospatial Data -> temporal and spatial information are considered
- Regression -> prediction of a continuous value
- Outlier Detection

Decision support systems: huge operational databases available in most companies, may provide a large wealth of useful information.

Decision Support Systems provide means for:
- in depth analysis of a company's business
- faster and better decisions

Business Intelligence (BI) provides support to strategic decisions in companies.
transforming company data into actionable information at different detail levels, for analysis applications.
Users may have heterogeneous needs, BI requires an appropriate hardware and software infrastructure.

Data Warehouse = Database devoted to decision support, which is kept separate from company operational databases.
Data which is:
- devoted to a specific subject
- integrated and consistent
- time dependent, non volatile
used for decision support in a company

### Multidimensional Representation
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006142909.png)

Dimension tables used to represent specific portions or properties of the database

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006143458.png)

All attributes of entries are stored in the dimension tables
The fact table records the fact of interest, it connects the 3 dimension tables together

This way we don't represent empty cells

Data Warehouse Size: typically it covers around 2 years of data
- 2 x 365days
- shop dimension: 300 shops
- product dimensions: 3000 sold every day in every shop
Total rows: 365x2x300x3000 = 657 Millions --> 21GB size of fact table

### Data Analysis Tools

OLAP analysis: Complex aggregate function computation
	support to different types of aggregate functions

Data analysis by means of data mining techniques
	various analysis types
	significant algorithmic contribution

Presentation
	separate activity: data returned by a query may be rendered by means of different presentation tools

Motivation search
	Data exploration by means of progressive "incremental" refinements

# Data Warehouse Architecture

Separation between transactional computing and data analysis (avoid one level architectures)

Architectures characterized by two or more levels
	Separate to a different extent data incoming into the data warehouse from analyzed data, more scalable

## Two Level Architecture

External Data Sources (Source Lev.) ----> ETL Tools -----> Warehouse Architecture (DW Lev.)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006145934.png)

### Data Warehouse VS Data Mart

*Company Data Warehouse* contains all information on the company business
- extensive functional modelling process
- design and implementation require a long time

*Data Mart* departmental information subset focused on a given subject
- 2 architectures: dependent (fed by company data warehouse) and independent (fed by sources)
- faster implementation
- requires careful design to avoid subsequent data mart integration problems

### Servers for Data Warehouses

- ROLAP (Relational OLAP) Servers
	- extended relational DBMS (compact representation for sparse data)
	- SQL Extensions for aggregate computation
	- specialized access methods which implement efficient OLAP data access
- MOLAP (Multidimensional OLAP) Servers
	- data represented in proprietary (multidimensional) matrix format (sparse data require compression)
	- special OLAP primitives
- HOLAP (Hybrid OLAP) Server

### Extraction Transformation and Loading (ETL)

Prepares data to be loaded into the warehouse:
- Data extraction (From OLTP and external) sources
- data cleaning
- data transformation
- data loading
Performed when the DW is first loaded, and during periodical DW refresh

### Metadata
data about data
Different types:
- data transformation and loading : describe data sources and needed transformation operations
- data management : describe structure of the data in the DW
- query management : data on query structure and to monitor query execution

## Three Level Architecture

Addition of Staging Area between source level and DW level.
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006145902.png)

Staging Area: where ETL operations take place. DW fed only when data is ready, not on the fly like in 2 level arch.


![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006150218.png)

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
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006153638.png)

DFM Hierarchy
- Each dimension can have a set of associated attributes
- Attributes descrive the dimension at different abstraction levels and can be structured as a hierarchy
- the hierarchy represents a generalization relationship among a subnet of attributes in a dimension
- the hierarchy represents a functional dependency (1:n)


Comparison between ER and DFM
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006170455.png)

### Advanced DFM

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006170517.png)

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
	- can be evaluated cumulatively ant the end of a time period
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
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006183616.png)
Flat dimensions, represented in tables

**Snowflake Schema**
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241006184414.png)

Partitioning dimensional table in further tables, connecting them via foreign key
Reduces data redundancy, but expensive to join tables to reach data
Not recommended, storage space decrease is rarely beneficial, and the cost of join execution may be significant
### Multiple Edges

Two ways to deal with multiple edges:
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241007135432.png)

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


- OLAP analysis: complex aggregate function computation. support to different types of aggregate functions
- Data mining techniques
#### Controlled Query Environment
pre-written queries that run periodically for common tasks.
used to realize diagrams in dashboards.
requires ad-hoc code development: stored procedures, application packages, predefined joins and aggregations. Flexible tools for report management are available, which allow defining report layout, publication periodicity and distribution list
#### Ad-Hoc Query Environment
arbitrary OLAP queries defined, designed on demand by users using point and click techniques, which automatically generate SQL instructions. Complex queries may be defined.
An OLAP session allows successive refinements of the same query, used when predefined reports are not enough
# OLAP Analysis

Available query operations:
- roll up, drill down
- slice and dice
- (table) pivot
- sorting

Operations may be:
- used together in the same query
- exploited in sequence to refined the same query which builds up the OLAP session
### Roll Up

Data detail reduction by:
- decreasing detail in a dimension, by climbing up a hierarchy
	`group by store, month` --> `group by city, month`
- dropping a whole dimension
	`group by product, city` --> `group by product`

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241007143240.png)
### Drill Down

Data detail increase by:
- increasing detail in a dimension, by walking down a hierarchy
	`group by city, month` --> `group by store, month`
- adding a whole dimension
	`group by product` --> `group by product, city`
Frequently drill down operates on a subset of data produced by the initial query

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241007143634.png)
### Slice and Dice

Selection of a data subset by means of selection predicates
- Slice = equality predicate selecting a "slice" --> `Year = 2005`
- Dice = predicate expression selecting a "dice" --> `Category = 'Food' and City = 'Torino'`

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241007143932.png)
### (Table) Pivot

Reorganization of the multidimensional structure without varying the detail level.
Increases readability of the same information
Multidimensional representation is always based on a "grid" (hierarchical spreadsheet)
- 2 dimensions are the main grid axes
- position of dimensions in the grid are changed

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241007144206.png)
# Extensions of the SQL Language

Interface tools require operations for the computation of different Group Bys at the same time.
The SQL-99 (SQL3) standard has extended the SQL Group By clause
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241007144536.png)

New class of aggregate functions characterized by:
- computation window, inside which the computation of aggregate functions is performed (cumulative totals and moving average can be computed)
- new aggregate functions to compute the rank in a given sort order

New `WINDOW` clause, characterized by:
- `partitioning` : rows are grouped without collapsing them (different from group by)
	no partitioning: a single group is defined
- `row ordering` : separately in each partition (similar to order by)
- `aggregation window` : for each row in the partition, it defines the row group on which the aggregate function is computed
### Example
Show for each city and month the sale amount and average on the current month and the two previous months, separately for each city

- Partitioning on city: average computation is reset when the city changes
- Ordering by month: to compute moving average on the current month and 2 preceding
- Aggregation window size: the current row and the two preceding rows

```SQL
SELECT City, Month, Amount, AVG(Amount) OVER Wavg AS MovingAVG
FROM Sales
WINDOW Wavg AS (PARTITION BY City
				ORDER BY Month
				ROWS 2 PRECEDING)

/*  OR  */

SELECT City, Month, Amount, 
	AVG(Amount) OVER   (PARTITION BY City
						ORDER BY Month
						ROWS 2 PRECEDING)
	AS MovingAvg
FROM Sales
```

Sort order is required, because the computation of the moving average considers rows in an ordered fashion.
When the window is not complete, the computation takes place on the available rows
Several different computation windows may be specified
## Aggregation Window

The moving window on which the aggregate function is computed may be defined
- at the physical level : it builds the group by counting rows
- at the logical level : it builds the group by defining an interval on the sort key
### Physical Interval Definition

- Between the a lower bound and the current row
  `ROWS 2 PRECEDING`
- Between a lower and an upper bound
  `ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING`
- Between the beginning (or end) of a partition and the current row 
  `ROWS UNBOUNDED PRECEDING`

Appropriate for sequence data with no gaps
### Logical Interval Definition

Uses the `RANGE` clause, with the same syntax as the physical interval.
A distance on the sort key between the interval bounds and the current value should be defined
`RANGE 2 MONTH PRECEDING`

Appropriate for "sparse" data, with gaps in the sequence

### Applications

- Moving aggregate computations (computations on a window which moves over data)
- Cumulative total computations (the cumulative total)
- Comparison between detailed data and aggregated data

#### Cumulative 
```SQL
SELECT City, Month, Amount, 
	SUM(Amount) OVER (PARTITION BY City 
					  ORDER BY Month 
					  ROWS UNBOUNDED PRECEDING) 
	AS CumeTot
FROM Sales
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5CData%20Warehouse%5Cattachments%5CPasted%20image%2020241012150510.png)
#### Comparison
```SQL
SELECT City, Month, Amount, 
	SUM(Amount) OVER (PARTITION BY City) 
	AS TotalAmount 
FROM Sales
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5CData%20Warehouse%5Cattachments%5CPasted%20image%2020241012150454.png)

```SQL
SELECT City, Month, Amount
	Amount/SUM(Amount) OVER () 
	AS TotalFract 
	Amount/SUM(Amount) OVER (PARTITION BY City) 
	AS CityFract 
	Amount/SUM(Amount) OVER (PARTITION BY Month) 
	AS MonthFract 
FROM Sales
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5CData%20Warehouse%5Cattachments%5CPasted%20image%2020241012150440.png)
### Group By and Window

Windows can be used together with grouping performed by `group by`
the temporary table generated by the execution of the `group by` clause (possible with aggregate function computation) becomes the operand to which the computations in the `window` clause are applied.

```SQL
SELECT City, Month, SUM(Amount) AS TotMonth, 
	AVG(SUM(Amount)) OVER (PARTITION BY City 
							ORDER BY Month 
							ROWS 2 PRECEDING) 
	AS MovingAvg 
FROM Sales 
WHERE <join conditions, but actually write them>
GROUP BY City, Month
```
### Ranking Functions

Functions computing the rank of a value inside a partition
- `rank()` function : tie --> 1 1 3
- `denserank()` function : tie --> 1 1 2

```SQL
SELECT City, Amount,
	RANK() OVER (ORDER BY Amount DESC)
	AS Ranking
FROM Sales
WHERE Month = 12
```

### Group By Cause Extension

Multidimensional spreadsheets compute several partial totals in one shot.
For sake of efficiency avoid multiple data reads and redundant data sorts.
SQL-99 standard extended the syntax of the `group by` clause
- `rollup` -> computes aggregations on all groups obtained by removing one by one the columns in the grouping clause
- `cube` -> computes aggregations on all combination of the columns in the grouping clause
- `grouping sets` -> computes aggregations on the group list in the grouping clause

#### Rollup
```SQL
SELECT City, Month, Pkey, 
	SUM(Amount) AS TotSales 
FROM Time T, Shop S, Sales V 
WHERE T.Tkey = V.Tkey 
	AND S.Skey = V.Skey 
	AND Year = 2000 
GROUP BY ROLLUP (City,Month,Pkey)
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5CData%20Warehouse%5Cattachments%5CPasted%20image%2020241012153609.png)
Super aggregates are represented by `NULL`
#### Cube
```sql
SELECT City, Month, Pkey, 
	SUM(Amount) AS TotSales 
FROM Time T, Shop S, Sales V 
WHERE T.Tkey = V.Tkey 
	AND S.Skey = V.Skey 
	AND Year = 2000 
GROUP BY CUBE (City,Month,Pkey)
```

- distributive aggregate functions (min, max, sum, count) : may be computed from aggregations on a larger set of attributes
- algebraic aggregate functions (avg) : may be computed on larger set of attributes if appropriate support aggregations are stored

To increase efficiency of cube computation, the distributive/algebraic properties of the aggregate functions are exploited.
- previously computed `group by` are exploited
- `rollup` requires single sort operation
- `cube` is a combination of several `rollup` operations
- previously executed sort operations exploited
#### Grouping Sets
```sql
SELECT City, Month, Pkey, 
	SUM(Amount) AS TotSales 
FROM Time T, Shop S, Sales S 
WHERE T.Tkey = S.Tkey 
	AND S.Skey = S.Skey 
	AND Year = 2000 
GROUP BY GROUPING SETS (Month, (City,Month,Pkey))
```


- Computation windows
- Raking functions
- Group by clause extensions

### Physical aggregation
```sql
SELECT Date, Amount, 
	AVG(Amount) OVER ( 
		PARTITION BY City 
		ORDER BY Date 
		ROWS 2 PRECEDING 
	) AS MovingAverage 
FROM Sales 
ORDER BY Date;
```

### Logical aggregation
```sql
SELECT Date, Amount, 
	AVG(Amount) OVER ( 
		PARTITION BY City 
		ORDER BY Date 
		RANGE BETWEEN INTERVAL ‘2’ DAY PRECEDING AND CURRENT ROW 
	) AS Last3DaysAverage
FROM Sales 
ORDER BY Date;
```

### Table Schema
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241012155110.png)
### Ranking
```sql
SELECT COD_I, SUM(SoldAmount), 
RANK() OVER ( 
	ORDER BY SUM(SoldAmount) 
	) AS SalesRank 
FROM Facts 
GROUP BY COD_I;
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241012155216.png)
### Dense Ranking
```sql
SELECT COD_I, SUM(SoldAmount), 
DENSE_RANK() OVER ( 
	ORDER BY SUM(SoldAmount) 
	) AS DenseSalesRank 
FROM Facts 
GROUP BY COD_I;
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241012155258.png)
### Double Ranking
```sql
SELECT Item.COD_I, Item.Weight, 
	RANK() OVER (ORDER BY Item.Weight ) AS WeightRank 
	RANK() OVER (ORDER BY SUM(SoldAmount) ) AS SalesRank 
FROM Facts, Item 
WHERE Facts.COD_I = Item.COD_I 
GROUP BY Item.COD_I, Item.Weight 
ORDER BY WeightRank;
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241012155400.png)
### Top N Ranking Selection
```sql
SELECT * 
FROM (SELECT COD_I, SUM(SoldAmount), 
		 RANK() OVER (ORDER BY SUM(SoldAmount)) AS SalesRank 
	 FROM Facts 
	 GROUP BY COD_I) 
 WHERE SalesRank<=2;
```
### ROW_NUMBER
In each partition it assigns a progressive number to each row
Partition the items according to their type and enumerate in progressive order the data in each partition. In each partition the rows are sorted according to the weight

```sql
SELECT Type, Weight, 
	ROW_NUMBER OVER ( PARTITION BY Type ORDER BY Weight ) AS RowNumberWeight 
FROM Item;
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241012155926.png)
### CUME_DIST
In each partition it assigns a weight between 0 and 1 to each row according to the number of values which precede the value of the attribute employed for the sorting in the partition
Given a partition with N rows, for each x the CUME_DIST is computed as follows:
	CUME_DIST(x) = number of values which precede or have the same value of the attribute employed for sorting, divided by N

```sql
SELECT Type, Weight, CUME_DIST() OVER ( 
	PARTITION BY Type 
	ORDER BY Weight 
	) AS CumeWeight 
FROM Item;
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241012163050.png)
### NTILE
Allows splitting each partition in n subgroups containing the same number of records. An identifier is associated to each subgroup
```sql
SELECT Type, Weight, NTILE(3) OVER ( 
	PARTITION BY Type 
	ORDER BY Weight 
	) AS Ntile3Weight 
FROM ITEM;
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241012163210.png)
# Materialized Views
The result is precomputed and stored on the disk
They improve response times
Usually they are associated to queries with aggregations
They may be used also for non aggregating queries
Materialized views can be used as a table in any query
The DBMS can change the execution of a query to optimize performance
Materialized views can be automatically used by the DMBS without user intervention
## Creation
```sql
CREATE MATERIALIZED VIEW Name 
[BUILD {IMMEDIATE|DEFERRED}] 
[REFRESH {COMPLETE|FAST|FORCE|NEVER} 
		 {ON COMMIT|ON DEMAND}] 
 [ENABLE QUERY REWRITE] 
 AS 
	 Query
```

- **NAME** = materialized view name
- **QUERY** = query associated to the materialized view (query that creates the view)
- **BUILD**
	- **IMMEDIATE** --> creates the materialized view and immediately loads the query results
	- **DEFERRED** --> creates the materialized view but doesn't load query results immediately
- **REFRESH**
	- **COMPLETE** --> recomputes query result by executing it on all data
	- **FAST** --> updates the content using changes since last refresh
	- **FORCE** --> FAST if possible, otherwise COMPLETE
	- **NEVER** --> content is not updated using Oracle standard procedures

	- **ON COMMIT** --> automatic refresh when SQL operation affect content
	- **ON DEMAND** --> only upon explicit request of the user issuing command
- **ENABLE QUERY REWRITE** = enables the DBMS to automatically use the materialized view as a basic block to improve other queries performance. Only available in high-end versions of DBMS. When unavailable, the query must be rewritten by the user to access the materialized view.
## Constraints

Depending on the DBMS and the query. you can create a materialized view associated to the query if some constraints are satisfied
- constraints on the aggregating attributes
- constraints on the tables and the joins
- ... other constraints
- must ve aware of the constraint existence!
### Example
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241012164549.png)

```sql
SELECT Cod_S, Cod_I, SUM(Measure) 
FROM Facts 
GROUP BY Cod_S, Cod_I;
```
OPTIONS:
- Immediate data loading
- Complete refresh only upon request
- DBMS can use materialized view to optimize queries

```sql
CREATE MATERIALIZED VIEW Sup_Item_Sum
BUILD IMMEDIATE
REFRESH COMPLETE ON DEMAND 
ENABLE QUERY REWRITE 
AS 
	SELECT Cod_S, Cod_I, SUM(Measure) 
	FROM Facts 
	GROUP BY Cod_S, Cod_I;
```
## Fast Refresh

Requires proper structures to log changes to the tables involved by the materialized view query
MATERIALIZED VIEW LOG
- There is a log for each table of a materialized view
- each log is associated to a single table and some of its attributes
- it stores changes to the materialized view table

The **REFRESH FAST** option can be used only if the materialized view query satisfies some constraints: 
- materialized view **logs** for the tables and attributes of the query must exist
- when the GROUP BY clause is used in the SELECT statement an **aggregation** function must be specified (COUNT, SUM etc...)

Examples:
Create a materialized view log associated to the FACTS table, on Cod_S, Cod_I and MEASURE attributes 
- enable the options SEQUENCE and ROWID 
- enable new values handling
```sql
CREATE MATERIALIZED VIEW LOG 
ON Facts 
WITH SEQUENCE, ROWID (Cod_S, Cod_I, Measure) 
INCLUDING NEW VALUES;
```


The materialized view query is 
```sql
SELECT Cod_S, Cod_I, SUM(Measure) 
FROM Facts 
GROUP BY Cod_S, Cod_I; 
```
Options:
- Immediate data loading 
- Automatic fast refresh 
- The DBMS can use the materialized view to optimize other queries

```sql
CREATE MATERIALIZED VIEW LOG ON Facts 
WITH SEQUENCE, ROWID (Cod_S, Cod_I, Measure) 
INCLUDING NEW VALUES; 

CREATE MATERIALIZED VIEW Sup_Item_Sum2 
BUILD IMMEDIATE 
REFRESH FAST ON COMMIT 
ENABLE QUERY REWRITE 
AS 
	SELECT Cod_S, Cod_I, SUM(Measure) 
	FROM Facts 
	GROUP BY Cod_S, Cod_I;
```

## Other Commands

- ALTER to change the view (not the query, but the options)
- DROP to delete the view
- DBMS_MVIEW.EXPLAIN_MVIEW allows the materialized view inspection:
	- refresh type
	- operations on which the fast refresh is enabled
	- query rewrite status (enabled, allowed, disabled)
	- errors

Precomputed summaries for the fact table
- explicitly stored in data warehouse
- provide a performance increase for aggregate queries
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241013141950.png)

They are defined in SQL statements
Materialized views may be exploited for answering several different queries (not for all aggregation operators)

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241013142124.png)

Huge number of allowed aggregations, most attribute combinations are eligible
Select of the "best" materialized view set
Cost function minimization: query execution cost, view maintenance (update) cost
Constraints: available space, time window for update, response time, data freshness



Workload characteristics
- aggregate queries which require accessing a large fraction of each table
- read-only access
- periodic data refresh, possible rebuilding physical access structures (indices, views)

Physical structures:
- index types different from OLTP
	- bitmap index, join index, bitmapped join index...
	- $B^+$-tree index not appropriate for attributes with low cardinality domains and queries with low selectivity
- Materialized Views
	- query optimizer should be able to exploit them

Optimizer characteristics should consider statistics when defining the access plan (cost based) and aggregate navigation

Physical design procedure:
- selection of physical structures supporting most frequent (or most relevant) queries
- selection of structures improving performance of more than one query
- constraints: disk space, available time window for data update

Tuning: a posteriori change of physical access structures, workload monitoring tools are needed, frequently required for OLAP applications

Parallelism: data fragmentation, query parallelization (inter and intra query), join and group by lend themselves well to parallel execution

Indexing dimensions:
- attributes frequently involved in selection predicates
- if domain cardinality is high: B-tree index
- if domain cardinality is low: bitmap index

Indices for join: indexing only foreign keys in the fact table is rarely appropriate, bitmapped join index is suggested (if available)

Indices for group by: use materialized views
 prepares data to be loaded into the data warehouse
Eased by exploiting the staging area.
Performed when the DW is first loaded and during periodical DW refresh
# Extraction

Data acquisition from sources. Extraction methods:
- **static** : snapshot of operational data (performed during first DW population)
- **incremental** : selection of updates that took place after last extraction (exploited for periodical DW refresh, immediate or deferred)
The selection of which data to extract is based on their quality.

It depends on how operational data is collected:
- **historical** : all modifications are stored for a given time in the OLTP system (ex: bank transactions, insurance data... operationally simple)
- **partly historical** : only a limited number of states is stored in the OLTP system (operationally complex)
- **transient** : the OLTP system only keeps the current data state (ex: stock inventory, operationally complex)
## Incremental Extraction

- Application assisted : only option for legacy systems, data modifications are captured by ad hoc application functions, requires changing OLTP applications, increases application load
- Log based : log data is accessed by means of appropriate APIs, log data format is usually proprietary, efficient, no interference with application load
- Trigger based : triggers capture interesting data modifications, doesn't require changing OLTP applications, increases application load
- Timestamp based  : modified records are marked by the (last) modification timestamp, requires modifying the OLTP database schema, deferred extraction --> may lose intermediate states if data is transient
# Transformation

## Data Cleaning

Techniques for improving data quality (correctness and consistency)
- Duplicate data
- Missing data
- Unexpected use of a field
- Impossible or wrong data values
- Inconsistency between logically connected data

Problems due to data entry errors, different field formats and evolving business practices

Each problem is solved by an ad hoc technique:
- data dictionary : appropriate for data entry errors or format errors, can be exploited only for data domains with limited cardinality
- approximate fusion : appropriate for detecting duplicates / similar data correlations, *approximate join*, *purge/merge problem*
- outlier identification, deviations from business rules
## Data Transformation

Data conversion from operational format to data warehouse format (requires data integration)

A uniform operational data representation is needed (reconciled schema)

2 steps:
- from operational sources to reconciled data in the staging area : conversion and normalization, matching, possible significant data selection
- from reconciled data to the data warehouse : surrogate keys generation, aggregation computation
# Loading

Update propagation to the data warehouse
Update order that preserves data integrity: dimensions - fact tables - materialized views and indices
Limited time window to perform updates
Transactional properties are needed : reliability and atomicity

 Data repository for original data in raw format, transformed data used for various types of reporting

Data formats:
- structured data : relational data
- semi-structured data : CVS, JSON, XML
- unstructured data : text documents, emails
- binary data : images, audio files

Query = similar to google search

Often not all questions data can answer are known a-priori. Hard to store data in some "optimal" form.
An attempt to break down information silos, information not adequately shared among data systems.
Based on exploiting massive cheap data storage
## Characteristics

- Store all data : DW design requires deciding what data to include in the warehouse, data lakes include also data that might be used some day
- Manage all data types
- Provide service to all users, Users process a variety of different types of data and answer new questions
- Adapt easily to changes : alla data is stored in its raw form and is always accessible, users are empowered to explore data in novel ways
- Provide faster insight, but early access to the data comes at a price
## Data Warehouse

Relational data coming from transactional systems, operational databases, and line of business applications
Schema designed prior to the DW implementation
High cost storage
Data quality : highly curated data that serves as the central version of the truth
Users are business analysts
Analytics : BI and visualization, batch reporting
## Data Lake

Data is both non-relational and relational, coming from IoT devices, websites, mobile apps, social media and corporate applications.
Schema is written at the time of analysis
Low-cost storage
Data quality: any data that may or may not be curated (raw data)
Users are data scientists, data developers : business analysts, if using curated data
Analytics : full-text search, machine learning, predictive analytics, data discovery and profiling

## Pros
- ability to harness more data, from more sources, in less time
- Data structures and business requirements are defined only when needed
- Empowering users to collaborate and analyze data data in different ways : self service analytics
- Integration happens outside the storage environment
- Minimal involvement of IT : wrangling with data is a self-service function
- Sandboxed for self-service analytics : need well defined problems
## Cons
- Raw data is stored with no oversight of the contents. storing data doesn't provide business value on its own. Need data governance, semantic consistency, mechanism to catalog data
- Consistency and data quality are uncertain : data brought into a data lake is co-located not integrated
- Business users don't have time/willingness to learn
- Rogue queries can bring down big clusters

Central question is whether collecting and storing data without a pre-defined business purpose is a good idea
 Data mining = algorithms to discover information hidden into a data collection. The extraction is automatic and is represented using patterns.

Data --> Selected data --> preprocessed data --> transformed data --> pattern --> knowledge

Techniques for: association rule extraction, classification, clustering

- Descriptive methods : extract interpretable models describing data
- Predictive methods : exploit some known variables to predict unknown or future values of other variables
# Data Processing

Data = collection off data objects and their attributes, property or characteristic of the object. A collection of attributes describe an object.

Types:
- Categorical: symbols
	- Nominal : distinctness --> eye color, id number etc...
	- Ordinal : distinctness & order --> rankings, grades etc...
- Numeric: numbers
	- Interval : distinctness & order & addition --> calendar dates
	- Ratio : all 4 --> temperature, length, time, counts etc...

Type of attribute depends on which of the following it possesses:
- Distinctness : either the same or different
- Order : can be ordered, so greater or lesser than
- Addition : add or subtract
- Multiplication : multiply or divide values

- Discrete Attribute
	- has only a finite or countably infinite set of values
	- often represented as integer variables
	- binary attributes are a special case of discrete
- Continuous Attribute
	- has real numbers as attribute values
	- finite number of digits to represent them
	- floating-point variables
### Data Set Types
- Record
	- Tables : each record has a fixed set of attributes
	- Document Data : semi-structured or unstructured, like Textual Data, still representable in a tabular way (words and frequency in document)
	- Transaction Data : each record is a set of items, ex: grocery store
- Graph
	- World Wide Web : information are nodes, and connections are links with weight
	- Molecular Structures
- Ordered
	- Spatial Data
	- Temporal Data : evolution of value over time
	- Sequential Data
	- Genetic Sequence Data
### Data Quality
Poor data quality negatively affects many data processing efforts
Data quality problems:
- Noise and Outliers
- Missing values
- Duplicate data
- Wrong data
#### Outliers
data objects with characteristics very different than most other data objects in data set
- noise that interferes with data analysis
- goal of the analysis: find the frauds, intrusions etc..
#### Missing Values
information not collected, attributes not applicable to all cases
#### Duplicate Data
Data set may include data objects that are duplicates, or almost duplicates of one another, major issue when merging data from heterogeneous sources
Data Cleaning : process of dealing with duplicates
# Data Preparation

### Aggregation
combining two or more attributes into a single one
- data reduction --> reduced representation of the dataset
	- sampling : reduce cardinality of set
	- feature selection : reduce number of attributes
	- discretization : reduce cardinality of attribute domain
- change of scale
- more stable data
#### Sampling
Main technique for data selection, obtaining and processing full data set is too expensive and time consuming.
Must represent the sample as a representative of the full dataset

- Simple Random Sampling : equal probability of selecting any particular item
	- with replacement : removes from the population
	- without replacement : just copy, no removal (items can be picked more than once)
- Stratified sampling : split data into several partitions, draw random samples from each partition
#### Dimensionality Reduction
avoid curse of dimensionality, reduce amount of time and memory required by data mining algorithms, allow data to be more easily visualized, may help eliminate irrelevant features and reduce noise.

Techniques:
- Principal Component Analysis (PCA) --> goal is to find a projection that captures the largest amount of variation in data
- Singular Value Decomposition
- Others: supervised and non-linear techniques

Curse of Dimensionality: When dimensionality increases, data becomes increasingly sparse in the space that it occupies.
#### Feature Subset Selection
Redundant features duplicate much of the info contained in one or more attributes
Irrelevant features contain no info that is useful for the data mining task at hand
- Brute force approach : try all possible feature subsets as input to data mining algorithm
- Embedded approaches : feature selection occurs naturally as part of data mining algorithm
- Filter approaches : features selected before data mining algorithm is run
- Wrapper approaches : use the data mining alg. as black-box to find best subset

Feature Creation: create new attributes that can capture the important information in a data set much more efficiently than the original attributes
#### Discretization
Process of converting a continuous attribute into an ordinal attribute.
Potentially infinite number of values mapped into a small number of categories.

Discretization is commonly used in classification: many classification algorithms work best if both the independent and dependent variables have only a few values

- N intervals with the same with width : $W = (V_{max}-V_{min}) / N$
- N intervals with the same cardinality : each interval contains the same n of objects
- clustering
- analysis of data distribution

Binarization : maps an attribute into one or more binary variables
- Continuous attribute : first map the attribute to a categorical one {low medium high}
- Categorical attribute : mapping to a set of binary attributes {high 1 0 0; medium 0 1 0; low 0 0 1}
one-hot encoding, only 1 bit takes the value 1
### Attribute Transformation

function that maps the entire set of values of a given attribute to a new set of replacement values such that old value can be identified with one of the new values

Normalization : type of data transformation.
Values of an attribute are scaled to fall into a small range \[-1, +1] or \[0, +1]
Techniques:
- min-max normalization : $v' = \frac{v-\min_A}{\max_A-\min_A}(new\_\max_A - new\_\min_A) + new\_\min_A$
- z-score normalization : $v' = \frac{v-\min_A}{stand\_dev_A}$
- decimal scaling : $v' = \frac{v}{10^j}$ where j is the smallest integer such that $\max(|v'|)<1$
## Similarity and Dissimilarity

Similarity = numerical measure of how alike two data objects are, it's higher when objects are more alive, often falls in the range \[0,1]

Dissimilarity = numerical measure of how different are two data objects. Minimum dissimilarity is often 0. Upper limit varies.

Proximity refers to a similarity or dissimilarity

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241022145559.png)

#### Euclidian Distance

$$d(X,Y) = \sqrt{(\sum^{n}_{k=1}(x_k-y_k)^2}$$

where n is the number of dimensions (attributes) and $x_k$ and $y_k$ are the attributes of data object X and Y

#### Minkowski Distance
$$
d(X,Y) = (\sum^n_{k=1}|x_k-y_k|^r)^{1/r}
$$
where r is a parameter.

- R = 1 --> City Block distance : common example is the hamming distance, number of bits that are different between two binary vectors
- R = 2 --> Euclidean distance
- R = $\infty$ --> "supremum" distance : maximum difference between any component of the vectors

Common Properties of a Distance:
- $d(x,y) \ge 0$ Positive Definiteness
- $d(x,y) = d(y,x)$ Symmetry
- $d(x,z) \le d(x,y) +d(y,z)$ Triangle Inequality

Common Properties of a Similarity
- $s(x,y) = 1$ only if $x=y$
- $s(x,y) = s(y,x)$

SMC versus Jaccard
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241022153658.png)
Cosine Similarity
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241022153707.png)
## Correlation

Measure of the linear relationship between two data objects having binary or continuous variables.
Useful during the data exploration phase to be better aware of data properties
Analysis of feature correlation. Correlated features should be removed

Pearson's Correlation
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241022154501.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241022154606.png)

 Extraction of frequent correlations or patterns from a transactional database

A collection of transactions is given, transaction = set of items not ordered.
A, B ==> C
A,B items in the rule body
C item in the rule head
the ==> means co-occurrence, not causality

- Itemset = set including one or more items
- k-itemset = contains k items
- support count = frequency of occurrence of an itemset
- support = fraction of transactions that contain an itemset
- frequent itemset = itemset whose support is greater than or equal to a minsup threshold

Given the association rule A ==> B
- Support : fraction of transactions containing both A and B
- Confidence : frequency of B in transactions containing A, represents the strength of the ==>

Example:
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241023102900.png)

Given a set of transactions T, association rule mining is the extraction of the rules satisfying the constraints:
- support $\ge$ minsup threshold
- confidence $\ge$ mincoft threshold
result is : complete (all rules satisfy both constraints), correct (only the rules satisfying both constraints)

Brute-force approach: enumerate all possible permutations, compute support and confidence for each rule, prune the rules that don't satisfy the minsup and minconf constraints
Computationally unfeasible
Given an itemset, the extraction process may be split:
	first generate frequent itemset, most computationally expensive step
	next generate rules for each frequent itemset

frequent itemset generation:
Brute-force approach: each itemset in the lattice is a candidate frequent itemset. Scan the database to count the support of each candidate, match each transaction against every candidate.
  Complexity : $\approx O(|T|2^dw)$  where |T| number of transactions, d num. of items, w transaction length
Improve efficiency:
- reduce number of candidates
- reduce number of transactions
- reduce the number of comparisons

A Priori Principle : "if an itemset is frequent, then all of its subsets must also be frequent"
The support of an itemset can never exceed the support of any of its subsets
It holds due to the antimonotone property of the support measure
It reduces the number of candidates

Apriori Algorithm
Level-based approach, at each iteration extracts itemsets of a given length k
Two main steps for each level: 
1. candidate generation: 
	   Join Step: generate candidates of length k+1 by joining frequent itemsets of length k
	   Prune Step: apply Apriori principle, prune length k+1 candidate itemsets that contain at least one k-itemset that is not frequent
2. frequent itemset generation: scan DB to count support for K+1 candidates, prune candidates below minsup
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241023111310.png)

Performance: extracting long frequent itemsets requires generating alla frequent subsets.
- minimum support threshold : increases number of frequent itemsets
- dimensionality : more space needed to support count of each item
- size of database : run time may increase with number of transactions
- average transaction width : transaction width increases in dense data sets

Improvements:
- Hash-Based itemset counting: k-itemset whose corresponding hashing bucket count below the threshold cannot be frequent
- Transaction reduction : transaction that doesn't contain any frequent k-itemset is useless in subsequent scans
- Partitioning: any itemset that is potentially frequent in DB must be frequent in at least one of the partitions of DB
- Sampling: mining on a subset of given data
- Dynamic Itemset Counting: add new candidate itemsets only when all of the subsets are estimated to be frequent

## FP-Growth Algorithm

Exploits a main memory compressed representation of the database, the FP-tree
high compression for dense data distributions
complete representation for frequent pattern mining
frequent pattern mining by means of FP-growth
- recursive visit of FP-tree
- applies divide-and-conquer approach
Only two database scans: count item supports + build FP-tree

Scan Header Table from lowest support item up
For each item i in header table extract frequent itemsets including item i and items preceding it in Header Table
1. build conditional pattern base for item i (i-CPB)
2. recursive invocation of FP-growth on i-CPB


An itemset is closed if none of its immediate supersets has the same support as the itemset

Selection of the appropriate minsup threshold is not obvious:
- too high: itemsets including rare but interesting items may be lost
- too low: computationally very expensive, large number of frequent itemsets

Large number of patterns may be extracted, rank them by level inf interestingness.
Objective Measures:
- rank patterns based on statistics computed from data
- initial framework only considered support and confidence
Subjective Measures:
- rank patterns according to user interpretation


Correlation: $\frac{P(A,B)}{P(A)P(B)}$ = $\frac{confr(r)}{sup(B)}$ 
- statistical independence: correlation =1
- positive correlation : correlation > 1
- negative correlation : correlation < 1


Weight could be considered
items or transactions may be weighted

Taxonomy: set of hierarchies that aggregate data items into higher-level concepts

Generalized Itemsets: sets of items at different generalization levels. May contain data items together with generalized items defined in the taxonomy. Summarize knowledge represented by a set of lower-level descendants
A generalized itemset covers a transaction when all:
- its generalized items are ancestors of items included in the transaction
- its data items are included in the transaction
Generalized itemset support ration between number of covered transactions and dataset cardinality

 Objectives:
- prediction of a class label
- definition of an interpretable model of a given phenomenon
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241029154408.png)

Give a collection of class labels and a collection of data objects labelled with a class label:
find a descriptive profile of each class, which will allow the assignment of unlabeled objects to the appropriate class

- Training set: collection of labeled data objects used to learn the classification model
- Test set: collection of labeled data objects used to validate the classification model

Evaluation of classification techniques:
- Accuracy : quality of prediction
- Interpretability : model interpretability and compactness
- Incrementality : model update in presence of newly labeled record
- Efficiency : model building time and classification time
- Scalability : training set size, attribute number
- Robustness : noise, missing data
## Decision Tree

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241029163744.png)
### Hunt's Algorithm
- If $D_t$ contains record that belongs to more than one class:
	- select the "best" attribute A on which to split $D_t$ and label node t as A
	- split $D_t$ into smaller subsets and recursively apply the procedure to each subset
- If $D_t$ contains records that belong to the same class $y_t$ then t is a leaf node labeled $y_t$
- if $D_t$ is an empty set then t is a leaf node labeled as the default class

Adopts a greedy strategy: best attribute for the split is selected locally at each step (Not a global optimum)

Issues: 
- structure of test condition
- selection of best attribute for the split
- stopping condition for the algorithm

Structure of test condition depends on attribute type: nominal ordinal or continuous. It also depends on the number of outgoing edges:
- Multi-way split: as many partitions as distinct values
- Binary split: divides values into two subsets, need to find optimal partitioning

Splitting on continuous attributes: different techniques
- Discretization : to form ordinal categorical attribute (static: discretize once at beginning) (dynamic: discretize during tree induction)
- Binary decision (A $<$ v or A $\ge$ v)

Selection of best attribute
Attributes with homogeneous class distribution are preferred. need a measure of node impurity.
Many different measures available
- GINI index
- Entropy
- Misclassification error
Different algorithms rely on different measures
#### GINI Impurity Measure
$$
GINI(t) = 1-\sum_{j}[p(j|t)]^2
$$
- Max value: $1-1/n_c$ when records are equally distributed among all classes
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241030112637.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241030112716.png)

Split based on GINI:
$$
GINI_{split} = \sum^k_{i=1}\frac{n_i}{n}GINI(i)
$$
where $n_i$ = number of records at child *i*
*n* = number of records at node *p*

#### Entropy Impurity Measure (INFO)
$$
Entropy(t) = -\sum_jp(j|t)\log_2p(j|t)
$$
- Max: $\log(n_c)$ when records are equally distributed
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241030113937.png)

Split based on INFO
$$
GAIN_{split} = Entropy(p)-(\sum^k_{i=1}\frac{n_i}{n}Entropy(i))
$$
$$
GainRATIO_{split} = \frac{GAIN_{split}}{SplitINFO}
$$
$$
SplitINFO = -\sum^k_{i=1}\frac{n_i}{n}\log\frac{n_i}{n}
$$
### Stopping Criteria for Tree Induction

Stop expanding a node when all the records belong to the same class
Stop expanding a node when all the records have similar attribute values

Underfitting : model is too simple, both training and test errors are large
Overfitting : decision boundary is distorted due to noise point

- Pre-Pruning: stop algorithm before it becomes a fully-grown tree. Typical stopping condition for a node: all instances in the same class or all attribute values are the same
   
- Post-Pruning: grow decision tree to its entirety, then trim the nodes in a bottom-up fashion. If generalization error improves after trimming, replace sub-tree by a leaf node. Class label of leaf node is determined from majority class of instances in the sub-tree.

Number of instances gets smaller as u traverse down the tree.
Number of instances at the leaf nodes could be too small to make any statistically significant decision
Missing values affect decision tree construction in 3 ways:
- affect how impurity measures are computed
- affect how to distribute instances with missing value to child nodes
- affect how a test instance with missing value is classified

Accuracy : For simple datasets, comparable to other classification techniques 
Interpretability : Model is interpretable for small trees, Single predictions are interpretable
Incrementality : Not incremental
Efficiency : Fast model building, very fast classification
Scalability : Scalable both in training set size and attribute number
Robustness : Difficult management of missing data

## Random Forest

Ensemble learning technique: multiple base models combined to improve accuracy and stability and to avoid overfitting.
Random forest = set of decision trees: a number of decision trees are built at training time, the class is assigned by majority voting

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241031112310.png)

Bootstrap aggregation: given a training set of instances, it selects B times a random sample with replacement from D and trains trees on these dataset samples. 
For each candidate split in the learning process, a random subset of the features is selected.
Trees are decorrelated: feature subset are sampled randomly, hence different features can be selected as best attributes for the split.

Algorithm
- given a training set D of n instances with p features
- b = 1...B
	- Sample randomly with replacement n' training examples. A subset $D_b$ is generated
	- Train a classification tree on $D_b$
		- during tree construction, for each candidate split
			- $m<<p$ random features are selected ($m \sim \sqrt(p)$)
			- the best split is computed among these m features
- Class is assigned by majority voting among the B predictions
### Evaluation of random forests

- **Accuracy** : Higher than decision trees
- **Interpretability** : 
	- Model and prediction are not interpretable. 	A prediction may be given by hundreds of trees. 
	- Provide global feature importance : an estimate of which features are important in the classification
- **Incrementality** : Not incremental Evaluation of random forests
- **Efficiency** : Fast model building, very fast classification
- **Scalability** : Scalable both in training set size and attribute number
- **Robustness** : Robust to noise and outliers
## Rule-Based Classification

Classify records by using a collection of "if then" rules : $(Condition) \rightarrow y$ 
Examples:
- (Blood Type=Warm) ^ (Lay Eggs=Yes) → Birds

A rule r covers an instance X if the attributes of the instance satisfy the condition of the rule

- Mutually exclusive rules: two conditions can't be true at the same time
- Exhaustive rules: classifier rules account for every possible combination of attribute values

Rules can be simplified, but become no longer mutually exclusive or exhaustive.

Ordered rule set: rules are rank ordered according to their priority (an ordered rule set is known as a decision list.) When a test record is presented to the classifier, it's assigned to the class label of the highest ranked rule it has triggered. If none of the rules fired, it's assigned to the default class

Building classification rules:
- Direct method : extract rules from data
- Indirect method : extract rules from other classification models (decision trees, neural networks...)

- Accuracy : Higher than decision trees
- Interpretability : Model and prediction are interpretable
- Incrementality : Not incremental Evaluation of rule based classifiers
- Efficiency : Fast model building, very fast classification 
- Scalability : Scalable both in training set size and attribute number
- Robustness : Robust to outliers

## K-Nearest Neighbor


## Bayesian Classification

Bayes Theorem: let C and X be random variables 

$P(C,X) = P(C|X) P(X)$ and $P(C,X) = P(X|C) P(C)$

Hence $$P(C|X) P(X) = P(X|C) P(C)$$ 
and also $$P(C|X) = P(X|C) P(C) / P(X)$$
How the classification works:
- let the class attribute and all data attributes be random variables. C = any class label, X = records to be classified
- Compute $P(C|X)$ for all classes
- Assign X to the class with Maximal $P(C|X)$
- Applying Bayes Theorem: $P(C|X) = P(X|C)\cdot P(C)/P(X)$

How to estimate P(X|C) ?
- Naive Hypothesis : $P(x_1,...,x_k|C) = P(x_1|C)P(x_2|C)...P(x_k|C)$, not always true
- Bayesian networks : allow specifying a subset of dependencies among attributes

Example:
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241031121337.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241031121357.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241031121415.png)

- Accuracy : Similar or lower than decision trees, naïve hypothesis simplifies model
- Interpretability : Model and prediction are not interpretable, the weights of contributions in a single prediction may be used to explain 
- Incrementality : Fully incremental, does not require availability of training data  
- Efficiency : Fast model building, very fast classification
- Scalability : Scalable both in training set size and attribute number
- Robustness : Affected by attribute correlation

## Support Vector Machines

find a linear hyperplane that separates the data

- Accuracy : Among best performers 
- Interpretability : Model and prediction are not interpretable, **Black box model**
- Incrementality : Not incremental Evaluation of Support Vector Machines 
- Efficiency : Model building requires significant parameter tuning, very fast classification 
- Scalability : Medium scalable both in training set size and attribute number
- Robustness : Robust to noise and outliers

## Artificial Neural Networks
inspired to the structure of the human brain
- neurons are elaboration units
- synapses as connection network
Different tasks --> different architectures

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241105152609.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241105152625.png)

Activation: stimulates biological activation to input stimuli, provides non-linearity to the computation, may help saturate neuron outputs in fixed ranges
Different functions:
- Sigmond ( $\tanh$ ) : saturate input value in a fixed range, non linear for all the input scale, typically used by FFNNs for both hidden and output layers
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108143642.png)

- Binary Step : outputs 1 when input is non-zero. Useful for binary outputs. Issues: not appropriate for gradient descent (derivative not defined in 0, and =0 in every other position

- ReLU (Rectified Linear Unit) : used in deep networks (CNNs), avoids vanishing gradient, doesn't saturate. Neurons activate linearity only for positive input

- SoftMax : differently to other activation functions it's applied only to the output layer. After SoftMax, the output vector can be interpreted as a discrete **distribution of probabilities**
- ![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108144044.png)
### Building a FFNN
for each node: define weights and offset value
providing the highest accuracy on the training data
Iterative approach on training data instances
algorithm:
```
assign random values to weights and offsets
process instances in the training set one at a time
for each neuron
	compute result when applying weights, offset and activation function
forward propagation until the output is computed
compare the output with expected output and evaluate error
backpropagation of the error, by updating weights and offset
```
Process ends when:
- % accuracy above given threshold
- % of parameter variation below given threshold
- max number of epochs is reached


- Accuracy : among best performers 
- Interpretability : Model and prediction are not interpretable, Black box model 
- Incrementality : Not incremental Evaluation of Feed Forward NN 
- Efficiency : Model building requires very complex parameter tuning, it requires significant time, very fast classification
- Scalability : Medium scalable both in training set size and attribute number 
- Robustness : Robust to noise and outliers, requires large training set, otherwise unstable when tuning parameters
### Convolutional Neural Networks (CNN)
Allow automatically extracting features from images and performing classification
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108144847.png)

Typical convolutional layer:
- convolution stage - feature extraction by means of sliding filters
- sliding filters activation - apply activation functions to input tensor
- pooling  - tensor downsampling 
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108145049.png)

**Tensors** : data flowing through CNN layers is represented in the form of tensors, N-dimensional vectors.
**Rank** = number of dimensions (0 : scalar, 1 : 1D vector, 2 : 2D vector (matrix))
**Shape** = number of elements for each dimension (ex: vector of length 5 has shape 5)

Images = rank-3 tensors with shape \[depth, height, width]
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108145355.png)

**Convolution** : processes data in form of tensors.
Input : input image or intermediate features (tensors)
Output : tensor with extracted features
Sliding filter produces the values of the output tensor. contain the trainable weights of the NN. Each convolutional layer contains many (hundreds) filters
after convolving tensor with N filters we obtain:
- rank-3 tensor with shape \[N,h,w]
- each filter generates a layer in the depth of the output tensor

**Activation** : ReLU is typically used for CNNs (faster training, doesnt saturate, faster computation of derivatives)

**Pooling** : performs tensor down-sampling. sliding filter which replaces sensor with a summary statistic of nearby outputs. maxpool is the most common : computes the max value as statistic
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108150030.png)

Convolutional layers training : during training each sliding filter learns to recognize a particular pattern in the input tensor.
Filters in shallow layers recognize textures and edges
Filters in deeper layers recognize objects and parts

Sematic segmentation CNNs : allow assigning a class to each pixel of the input image.
Composed of 2 parts:
- encoder network : convolutional layers to extract abstract features
- decoder network : deconvolutional layers to obtain the output image from the extracted features

### Recurrent Neural Networks (RNN)
allow processing sequential data $x(t)$. Differently from normal FFNN they are also able to keep a state which evolves during time.
Applications : machine translation, time series prediction, speech recognition, part of speech tagging

A RNN receives as input a vector $x(t)$ and the state at previous time step $s(t-1)$ 
A RNN typically contains many neurons organized in different layers
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108150941.png)

Training performed with **Backpropagation Through Time**.
Given a pair training sequence $x(t)$ and expected output $y(t)$ : error is propagated through time, weights are updated to minimize the error across all the time steps

Issues
- vanishing gradient : errore gradient decreases rapidly over time, weights are not properly updated. This makes harder having RNN with long-term memories
Solution : LSTM (Long Short Term Memories)
- RNN with gates which encourage the state information to flow through long time intervals

Autoencoders will allow compressing input data by means of compact representations and from them reconstruct the initial input. 
- for feature extraction : the compressed representation can be used as significant set of features representing unput data
- for image (or signal) denoising: the image reconstructed from the abstract representation is denoised with respect to the original one
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108151301.png)

### Word Embeddings (Word2Vec)
word embeddings associate words to n-dimensional vectors
- trained on big text collections to model the word distributions in different sentences and contexts.
- able to capture the semantic information of each word
- words with similar meaning share vectors with similar characteristics
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108151708.png)

Each word is represented with a vector, so operations among words are allowed
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108151737.png)

Semantic relationships among words are captured by vector positions
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108151759.png)






# Model Evaluation

Methods for performance evaluation : partitioning techniques for training and test sets
Metrics for performance evaluation: accuracy, other measures
Techniques for model comparison: ROC curve
Objective: reliable estimate of performance
Performance of a model may depend on other factors besides the learning algorithm: class distribution, cost of misclassification, size of training and test sets...

Learning curve shows how accuracy changes with varying training sample size.
Requires a sampling schedule for creating learning curve:
- arithmetic sampling
- geometric sampling
Effect of small sample size: bias in the estimate, variance of estimate

Methods of estimation:
- Partitioning labeled data for training, validation and test
- several partitioning techniques: holdout, cross validation
- Stratified sampling to generate partitions, without replacement
- Bootstrap, sampling with replacement

Holdout: fixed partitioning (80% training, 20% test). Appropriate for large datasets. May be repeated several times

Cross Validation: 
	partition data into k disjoint subsets
	k-fold: train on k-1 partitions, test on the remaining one
	reliable accuracy estimation, not appropriate fore very large datasets
Leave-One-Out:
	cross validation for k=n
	only appropriate for very small datasets

Model performance estimation:
- Model training step : building a new model
- Model validation step : hyperparameter tuning, algorithm selection
- Model test step : estimation of model performance

typical dataset size:
- training set : 60%
- validation set : 20%
- test set : 20%

Splitting labeled data: use hold-out to split in: training+validation, test
Use cross validation to split in training and validation

Binary classifier:
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241105145505.png)

Accuracy = Num. of correctly classified objects / Num. of classified objects = $\frac{a+d}{a+b+c+d}$

Recall (r) = Num. of objects correctly assigned to C / Num. of objects belonging to C = $\frac{a}{a+c}$

Precision (p) = Num. of objects correctly assigned to C / Num. of objects assigned to C = $\frac{a}{a+b}$

F - measure (F) = $\frac{2rp}{r+p}$ = $\frac{2a}{2a+b+c}$

Receiver Operating Characteristic (ROC) : characterizes the trade-off between positive hits and false alarms
- TPR : True Positive Rate : TPR = TP / (TP + FN)
- FPR : False Positive Rate : FPR = FP / (FP + FN)

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241105151540.png)
(FPR, TPR)
(0,0) : declare everything to be negative class
(1,1) : declare everything to be positive class
(0,1) : ideal

To build ROC Curve:
- use classifier that produces posterior probability for each test instance P(+|A)
- sort the instances according to P(+|A) in decreasing order
- Apply threshold at each unique value of P(+|A)
- count the number of TP, FP, TN, FN and find TPR, FPR

No model consistently outperforms the other
Ideally area under ROC is 1.0, with random guess is 0.5
 finding groups of objects such that the objects in a group will e similar to one another and different from other groups
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241106110208.png)

Clusters used for:
- understanding data, group related documents etc...
- summarize large data sets

A clustering is a set of clusters
important distinction between:
- Partitional Clustering : divides data objects into non-overlapping subsets such that each data object is exactly one subset
- Hierarchical Clustering : set  of nested clusters organized as a hierarchical trees

Other distinctions between sets of clusters:
- Exclusive / Non-Exclusive : in non-ex, points can belong to multiple clusters
- Fuzzy / Non-Fuzzy : point belong to every cluster with a weight 0-1
- Partial / Complete : cluster only a part of the data
- Heterogeneous / Homogeneous : cluster of widely different sizes, shapes and densities

### Well Separated
cluster is a set of points such that any point in a cluster is closed to every other point in the cluster than to any point not in the cluster
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241106115209.png)
### Center-Based
The center of a cluster is called centroid, the average of all the points in the cluster, or medoid, the most representative point of a cluster

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241106115306.png)
### Contiguity-Based
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241106121005.png)
### Density-Based
dense region of points, which is separated by low-density regions, from other regions of high density.
Used when the clusters are irregular or intertwined, and when noise and outliers are present.

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241106121018.png)
### Conceptual Cluster
finds clusters that share some common property or represent a particular concept
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241106121130.png)

# Clustering Algorithms

## K-Means Clustering
- partitional clustering approach
- each cluster is associated with a centroid (center point)
- each point is assigned to the cluster with the closest centroid
- number of clusters K must be specified

algorithm:
```
Select K points as initial centroids
repeat
	form K clusters by assigning all points to the closest centroid
	recompute centroid for each cluster
until : centroids don't change
```

- Initial centroids are often chosen randomly
- The centroid is (typically) the mean of the points in the cluster
- ‘Closeness’ is measured by Euclidean distance, cosine similarity, correlation, etc...
- K-means will converge for common similarity measures mentioned above.
- Most of the convergence happens in the first few iterations
- Complexity is $O( n * K * I * d )$
	- n = number of points
	- K = number of clusters
	- I = number of iterations
	- d = number of attributes
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241107110501.png)

Most common measure is Sum of Squared Error (SSE)
$$
SSE = \sum^K_{i=1}\sum_{x\in C_i}dist^2(m_i, x)
$$
- For each point, the error is the distance to the nearest cluster
- x is a data point in cluster $C_i$ and $m_i$ is the representative point for cluster $C_i$
- Given two clusters, we can choose the one with the smallest error
- One easy way to reduce SSE is to increase K, the number of clusters

Initial Centroids Problem
- Multiple runs - helps but not on my side
- Sample and use hierarchical clustering to determine initial centroids
- select more than k initial centroids and then select the most widely separated
- postprocessing
- Bisecting K-means

Elbow Graph (Knee approach)
- plotting the quality measure trend (SSE) against K
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241107113724.png)

Handling Empty Clusters
- Basic K-means algorithm can yield empty clusters
- several strategies:
	- Choose the point that contributes most to SSE
	- Choose a point from the cluster with the highest SSE
	- If there are several empty clusters, the above can be repeated several times.

Pre-processing : normalize the data, eliminate outliers
Post-processing : eliminate small clusters that may represent outliers, split "loose" clusters (high SSE), merge clusters that are close and have relatively low SSE

### Bisecting K-Means
variant of k-means algorithm, can produce a partial or hierarchical clustering
```
initialize the list of clusters to contain the cluster containing all points
repeat
	select a cluster from list of clusters
	for i = 1 to num_iterations do
		bisect the selected cluster using basic K-means
	end for
	add the 2 clusters from the bisection with the Lowest SSE to list of clusters
until : list of cluster contains K clusters
```

Limitation of K-means
- differing sizes
- differing densities
- differing non-globular shapes
- data outliers
## Hierarchical Clustering

Produces a set of nested clusters organized as a hierarchical tree
Can be visualized as a dendrogram
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241107114638.png)

Strengths:
- Do not have to assume any particular number of clusters
- They may correspond to meaningful taxonomies

Two main types of hierarchical clustering:
- Agglomerative : Start with the points as individual clusters, At each step, merge the closest pair of clusters until only one cluster (or k clusters) left
- Divisive : Start with one, all-inclusive cluster, At each step, split a cluster until each cluster contains a point (or there are k clusters)
Traditional hierarchical algorithms use a similarity or distance matrix: Merge or split one cluster at a time
### Agglomerative Clustering Algorithm
More popular hierarchical clustering technique
```
Compute the proximity matrix
Let each data point be a cluster
repeat
	merge two closest clusters
	update the proximity matrix
until : only a single cluster remains
```
Different methods to define inter-cluster similarity:
- MIN : can handle non-elliptical shapes BUT is sensitive to noise and outliers
- MAX : less susceptible to noise and outliers BUT tends to break large clusters, biased towards globular clusters
- Group Average :  less susceptible to noise and outliers BUT biased towards globular clusters
- Distance between Centroids
- Other methods driven by an objective function

### Hierarchical Clustering : Group Average
Proximity of two clusters is the average of pairwise proximity between points in the two clusters
$$
proximity(Cluster_i, Cluster_j) = \frac{\sum proximity(p_i, p_j)}{|Cluster_i|\cdot|Cluster_j|}
$$
Need to use average connectivity for scalability since total proximity favors large clusters
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108152141.png)

Compromise between Single and Complete Link.
- Strengths : less susceptible to noise and outliers
- Limitations : biased towards globular clusters

### Cluster Similarity : Ward's Method
Similarity between two clusters is based on the increase in squared error when two clusters are merged. Similar to group average if distance between points is distance squared.

Les susceptible to noise and outliers
Biased towards globular clusters
Hierarchical analogue of K-Means --> can be used to initialize K-Means

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108152441.png)

### Time and Space Requirements

- $O(N^2)$ space since it uses the proximity matrix
- $O(N^3)$ time in many cases : there are $N$ steps and at each step the $N^2$ matrix must be updated 
## DBSCAN

DBSCAN is a density-based algorithm
- Density = number of points within a specific radius (Eps)
- a point is a **core point** if it has more than a specified number of points (MinPts) withing Eps
- a **border point** has fewer than MinPts within Eps, but is in the neighborhood of a core point
- A noise point is any point that is not a core point or a border point 
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108153149.png)

Algorithm:
```
current_cluster_label = 1
for all core points
	if core point has no cluster label
		current_cluster_label = current_cluster_label + 1
		label current core point with cluster label
	for all points in eps-neighborhood except i-th (point itself)
		if point doesn't have cluster label
			label the point with current_cluster_label
```
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241108153355.png)

Determining EPS and MinPts: idea is that for points in a cluster, their $k^th$ nearest neighbors are at roughly the same distance.
Noise points have the $k^th$ nearest neighbor at farther distance
Plot sorted distance of every point to its $k^th$ nearest neighbor
# Cluster Validity

For supervised classification we have a variety of measures to evaluate how good our model is.
For cluster analysis, the analogous question is how to evaluate the “goodness” of the resulting clusters?
We evaluate them:
- to avoid finding patterns in noise
- to compare clustering algorithms
- to compare two sets of clusters
- to compare two clusters

The aspects of cluster validation:
1. Determining the clustering tendency of a set of data, i.e., distinguishing whether non-random structure actually exists in the data.
2. Comparing the results of a cluster analysis to externally known results, e.g., to externally given class labels.
3. Evaluating how well the results of a cluster analysis fit the data without reference to external information. - Use only the data
4. Comparing the results of two different sets of cluster analyses to determine which is better.
5. Determining the ‘correct’ number of clusters.

For 2, 3, and 4, we can further distinguish whether we want to evaluate the entire clustering or just 
individual clusters.

Numerical measures are applied to judge various aspects of cluster validity
- External Index: Used to measure the extent to which cluster labels match externally supplied class labels
- Internal Index: Used to measure the goodness of a clustering structure without respect to external information
- Relative Index: Used to compare two different clusterings or clusters

**Cluster Cohesion**: Measures how closely related are objects in a cluster
$$
WSS=\sum_i \sum_{x\in C_i}(x-m_i)^2
$$
**Cluster Separation**: Measure how distinct or well separated a cluster is from other clusters – Separation is measured by the between cluster sum of squares
$$
BSS = \sum_i|C_i|(m-m_i)^2
$$
A proximity graph based approach can also be used for cohesion and separation.
- Cluster cohesion is the sum of the weight of all links within a cluster.
- Cluster separation is the sum of the weights between nodes in the cluster and nodes outside the cluster.
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241113114010.png)
## Silhouette
A succinct measure to evaluate how well each object lies within its cluster
It is defined for single points
It considers both cohesion and separation
Can be computed for:
- individual points
- individual clusters
- clustering results

For each object i:
- a(i) : the average dissimilarity of i with all other objects within the same cluster (the smaller the value, the better the assignment)
- b(i) : min(average dissimilarity of i to any other cluster, of which i is not a member)
$$
s(i) = \frac{b(i)-a(i)}{\max\{a(i),b(i)\}}
$$
Ranges between -1 and +1, typically between 0 and 1, the closer to 1 the better

Silhouette for clusters and clusterings
- The average s(i) over all data of a cluster measures how tightly grouped all the data in the cluster are
- The average s(i) over all data of the dataset measures how appropriately the data has been clustered
## Rand Index
Any two objects that are in the same cluster should be in the same class and vice versa
given:
- f00 = number of pairs of objects having a different class and a different cluster
- f01 = number of pairs of objects having a different class and the same cluster
- f10 = number of pairs of objects having the same class and a different cluster
- f11 = number of pairs of objects having the same class and the same cluster

The Rand Index:
$$
Rand\ Index= \frac{f_{00}+f_{11}}{f_{00}+f_{01}+f_{10}+f_{11}}
$$


 DBMS : Data Base Management System
A software package designed to store and manage databases
We are interested in internal mechanisms of a DBMS providing services to applications.
- Useful for making the right design choices
- Some services are becoming available also in operating systems
## Architecture

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241114105001.png)
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
 ![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241114110701.png)

Buffer Manager manages page transfer from disk to main memory and vice versa.
It's in charge of managing the DBMS buffer
Efficient buffer management is a key issue DBMS performance

Buffer:
- large main memory block
- pre-allocated to the DBMS
- shared among executing transactions

Buffer organization:
- memory is organized in pages
- the size of a page depends on the size of the operating system I/O block

Memory management strategies:
- data locality : data referenced recently is likely to be referenced again
- Empirical law: 20-80 : 20% of data is read/written by 80% of transactions

The buffer manager keeps additional "snapshot" information on the current content of the buffer
for each buffer page:
- physical location of the page on disk : File identifier, Block number
- state variables : 
	- Count of the number of transactions using the page
	- Dirty bit which is set if the page has been modified

It provides the following primitives to access methods to load pages from disk and vice versa:
- fix
- unfix
- force
- set dirty
- flush
Requires shared access permission from the concurrency control manager
### Fix Primitive
Used by transactions to require access to a disk page: the page is loaded into the buffer. A pointer to a page into the buffer is returned to the requesting transaction
At the end of the Fix primitive, the requested page is in the buffer, is valid and the Count state variable of the page is incremented by 1
The Fix primitive requires an I/O operation only if the requested page is not yet in the buffer

It looks for the requested page among those already in the buffer
if found: it returns ro the requesting transaction the address of the page in the buffer. It happens often because of data locality
if not found: page is searched into the buffer where the new page can be loaded. first among free pages, next among pages with count = 0 (victim pages, may still be locked).
If the selected page has Dirty = 1, it is synchronously written on disk
The new page is loaded in the buffer and its address is returned to the requesting transaction
### Unfix Primitive
It tells the buffer manager that the transaction is no longer using the page 
The state variable Count of the page is decremented by 1
### Dirty Primitive
It tells the buffer manager that the page has been modified by the running transaction 
The Dirty state variable of the page is set to 1
### Force Primitive
It requires a synchronous transfer of the page to disk 
- The requesting transaction is suspended until the Force primitive is executed 
- It always entails a disk write
### Flush Primitive
It transfer pages to disk, independently of transaction requests
It is internal to the Buffer Manager 
It runs when the CPU is not fully loaded In CPU idle time 
It downloads pages which are not valid (state variable Count=0) or are not accessed since a longer time

## Writing Strategies

- Steal : The Buffer Manager is **allowed** to select a locked page with Count=0 as victim. The page belongs to an active transaction
- No Steal : The Buffer Manager is **not allowed** to select pages belonging to active transactions as victims

The steal policy writes on disk dirty pages belonging to uncommitted transactions
In case of failure these changes must be undone: same operations as in transaction rollback

- Force : All active pages of a transaction are synchronously written on disk by the Buffer Manager during the commit operation
- No Force : Pages are written on disk asynchronously by the Buffer Manager by means of the Flush primitive

Pages belonging to a committed transaction may be written on disk after commit 
In case of failure these changes must be redone

Typical usage is steal/no force, because of its efficiency 
	No force provides better I/O performance 
	Steal may be mandatory for queries accessing a very large number of pages

## File System and Buffer Manager

The Buffer Manager exploits services provided by the file system: 
- Creation/deletion of a file
- Open/close of a file
- Read : provides a direct access to a block in a file, it requires file identifier, block number and buffer page where to load data in memory
- Sequential Read : provides sequential access to a fixed number of blocks in a file. It requires file identifier, starting block, count of the number of blocks to be read, starting buffer page where to load data in memory
- Write and Sequential Write : same as reading
- Directory management functions


 ![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241120155717.png)

Data may be stored on disk in different formats to provide efficient query execution: different formats are appropriate for different query needs
Physical access structures describe how data is stored on disk
### Access Method Manager

Transforms an access plan generated by the optimizer into a sequence of physical access requests to (database) disk pages. It exploits access methods 

An access method is a software module. It is specialized for a single physical data structure 
It provides primitives for reading and writing data

Access Method : 
1. Selects the appropriate blocks of a file to be loaded in memory 
2. Requests them to the Buffer Manager 
3. Knows the organization of data into a page, can find specific tuples and values inside a page

Organization of a disk page is different for different access methods. It's divided in:
- space available for data
- space reserved for access method control information
- space reserved for file system control information

Tuples may have varying size: Varchar types Presence of Null values 
A single tuple may span several pages when its size is larger than a single page
# Physical Access Structures

Physical access structures describe how data is stored on disk to provide efficient query execution 
In relational systems
- Physical data storage : Sequential structures / Hash structures 
- Indexing to increase access efficiency : Tree structures (B-Tree, B+-Tree) / Unclustered hash index / Bitmap index
## Sequential Structures : 
- Tuples are stored in a given sequential order 
- Different types of structures implement different ordering criteria 
- Available sequential structures 
	- Heap file (entry sequenced) 
	- Ordered sequential structure
### Heap file : 
- Tuples are sequenced in **insertion order** 
	- insert is typically an **append** at the end of the file 
- **All** the space in a block is completely exploited before starting a new block 
- Delete or update may cause wasted space 
	- Tuple deletion may leave unused space 
	- Updated tuple may not fit if new values have larger size 
- Sequential reading/writing is very efficient 
- Frequently used in relational DBMS 
	- jointly with unclustered (secondary) indices to support search and sort operations
## Ordered Sequential Structures:
- The order in which tuples are written depends on the value of a given key, called sort key 
	- A sort key may contain one or more attributes 
		- the sort key may be the primary key 
- Appropriate for 
	- Sort and group by operations on the sort key 
	- Search operations on the sort key 
	- Join operations on the sort key when sorting is used for join
- Problem : 
	- preserving the sort order when inserting new tuples, it may also hold for update
- Solution :
	- Leaving a percentage of free space in each block during table creation.
	  On insertion, dynamic (re)sorting in main memory of tuples into a block
	- Overflow file containing tuples which do not fit into the correct block

Typically used with $B^+$-Tree clustered (primary) indices : the index key is the sort key : the index key is the sort key.
Used by DBMS to store intermediate operation results
## Tree Structures
Provide “direct” access to data based on the value of a key field 
	The key includes one or more attributes 
It does not constrain the physical position of tuples 
The most widespread in relational DBMS
### General Characteristics
- One root node
- Many intermediate nodes
- Nodes have a large fan-out (each node has many children)
- Leaf nodes provide access to data : Clustered / Unclustered
### $B$-Tree and $B^+$-Tree
2 different tree structures for indexing:
- $B$-Tree : Data pages are reached only through key values by visiting the tree
- $B^+$-Tree : Provides a link structure allowing sequential access in the sort order of key values

B stands for Balanced : Leaves are all at the same distance from the root.
Access time is constant, regardless of the searched value
#### Clustered
The tuple is contained into the leaf node 
	Constrains the physical position of tuples in a given leaf node 
		The position may be modified by splitting the node, when it is full 
	Also called key sequenced 
Typically used for primary key indexing
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241121115938.png)
#### Unclustered
The leaf contains physical pointers to actual data 
	The position of tuples in a file is totally unconstrained 
	Also called indirect 
Used for secondary indices
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241121120008.png)

Pros:
- efficient for range queries
- appropriate for sequential scan in the order of key field : always for clustered, not guaranteed otherwise

Cons:
- Insertions may require a split of a leaf: possibly also intermediate nodes, computationally intensive
- deletions may require mergine uncrowded nodes and re-balancing
## Hash Structure
It guarantees direct and efficient access to data based on the value of a **key field**. The hash key may include one or more attributes.

Suppose the hash structure has B blocks:
- The hash function is applied to the key field value of a record : It returns a value between 0 and B-1 which defines the position of the record 
- Blocks should never be completely filled to allow new data insertion
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241121122239.png)
Pros:
- Very efficient for queries with equality predicate on the key 
- No sorting of disk blocks is required 
Cons:
- Inefficient for range queries 
- Collisions may occur
### Unclustered Hash Index
- guarantees direct and efficient access to data based on the value of a key field (similar to hash index)
- blocks contain pointers to data : actual data is stored in a separate structure, position of tuples is not constrained to a block (different from hash index)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241121122559.png)
## Bitmap Index
It guarantees direct and efficient access to data based on the value of a key field 
	It is based on a bit matrix 
The bit matrix references data rows by means of RIDs (**R**ow **ID**entifiers) 
	Actual data is stored in a separate structure 
	Position of tuples is not constrained

The bit matrix has one column for each different value of the indexed attribute and one row for each tuple. Position $(i,j)$ of the matrix is : 1 if tuple $i$ takes value $j$, 0 otherwise
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241121122929.png)
Pros:
- Very efficient for Boolean expressions of predicates --> Reduced to bit operations on bitmaps 
- Appropriate for attributes with limited domain cardinality
Cons:
- Not used for continuous attributes 
- Required space grows significantly with domain cardinality
 ![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241124152837.png)

It selects an efficient strategy for query execution
- It is a fundamental building block of a relational DBMS 

It guarantees the **data independence** property 
- The form in which the SQL query is written does not affect the way in which it is implemented 
- A physical reorganization of data does not require rewriting SQL queries

It automatically generates a query execution plan 
- It was formerly hard-coded by a programmer  

The automatically generated execution plan is usually more efficient
- It evaluates many different alternatives
- It exploits statistics on data, stored in the system catalog, to make decisions
- It exploits the best known strategies
- It dynamically adapts to changes in the data distribution

## Lexical, Syntactic and Semantic Analysis

Analysis of a statement to detect:
- **Lexical errors**
	e.g., misspelled keywords 
- **Syntactic errors**
	errors in the grammar of the SQL language 
- **Semantic errors** 
	references to objects which do not actually exist in the database (e.g, attributes or tables) information in the data dictionary is needed

Output: Internal representation in (extended) relational algebra

Why relational algebra? 
	It explicitly represents the order in which operators are applied. It is procedural (different from SQL)
	There is a corpus of theorems and properties exploited to modify the initial query tree
## Algebraic Optimization

Execution of algebraic transformations considered to be always beneficial 
	Example: anticipation of selection with respect to join 
Should eliminate the difference among different formulations of the same query 
This step is usually independent of the data distribution 

Output: Query tree in “canonical” form
## Cost based optimization

Selection of the “best” execution plan by evaluating execution cost
Selection of 
	the best access method for each table 
	the best algorithm for each relational operator among available alternatives 

Based on a cost model for access methods and algorithms 
Generation of the code implementing the best strategy

Output: Access program in executable format It exploits the internal structures of the DBMS 
Set of dependencies: conditions on which the validity of the query plan depends e.g., the existence of an index
## Execution modes

- **Compile and go** --> Compilation and immediate execution of the statement. 
  No storage of the query plan. 
  Dependencies are not needed
- **Compile and store** --> The access plan is stored in the database together with its dependencies. 
  It is executed on demand. 
  It should be recompiled when the data structure changes

# Algebraic Optimization - In Depth

It is based on equivalence transformations 
	Two relational expressions are equivalent if they both produce the same query result for any arbitrary database instance 

Interesting transformations
- reduce the size of the intermediate result to be stored in memory
- prepare an expression for the application of a transformation which reduces the size of the intermediate result

1. Atomization of selection
$$
\sigma_{F1 \  Ʌ \  F2 }(E) \equiv \sigma_{F2}(\sigma_{F1}(E)) \equiv \sigma_{F1}(\sigma_{F2}(E))
$$
2. Cascading projections
$$
\pi_X(E) \equiv \pi_X(\pi_{X,Y}(E))
$$
3. Anticipation of selection with respect to join (pushing selection down)
$$
\sigma_{F}(E_1 \bowtie E_2) \equiv E_1 \bowtie (\sigma_F(E_2))
$$
4. Anticipation of projection with respect to join
$$
\pi_L(E_1\bowtie_pE_2) \equiv \pi_L((\pi_{L1,J}(E_1))\bowtie_p(\pi_{L2,J}(E_2)))
$$
L1 = L - Schema($E_2$) 
L2 = L - Schema($E_1$) 
J = set of attributes needed to evaluate join predicate p

5. Join derivation from Cartesian product
$$
\sigma_F(E_1 \times E_2)\equiv E_1 \bowtie_F E_2
$$
predicate F only relates attributes in $E_1$ and $E_2$

6. Distribution of selection with respect to union
$$
\sigma_F(E_1 \cup E_2) \equiv (\sigma_F(E_1)) \cup (\sigma_F(E_2))
$$

7. Distribution of selection with respect to difference
$$
\sigma_F(E_1-E_2) \equiv (\sigma_F(E_1))-(\sigma_F(E_2)) \equiv (\sigma_F(E_1))-E_2
$$
8. Distribution of projection with respect to union
$$
\pi_X(E_1 \cup E_2) \equiv (\pi_X(E_1)) \cup (\pi_X(E_2))
$$


Can projection be distributed with respect to difference?
- $\pi_X(E_1-E_2) \equiv (\pi_X(E_1))-(\pi_X(E_2))$                                !!!NO!!!
This equivalence only holds if X includes the primary key or a set of attributes with the same properties (unique and not null)

9. Other properties:
$$
\sigma_{F1 \ V \ F2}(E) \equiv (\sigma_{F1}(E)) \cup (\sigma_{F2}(E))
$$
$$
\sigma_{F1 \ Ʌ \ F2}(E) \equiv (\sigma_{F1}(E)) \cap (\sigma_{F2}(E))
$$
10. Distribution of join with respect to union

$$
E \bowtie (E_1 \cup E_2) \equiv (E \bowtie E_1)\cup(E \bowtie E_2)
$$
All binary operators are commutative and associative except for difference

# Cost Based Optimization - In Depth

It is based on 
- Data profiles: statistical information describing data distribution for tables and intermediate relational expressions 
- Approximate cost formulas for access operations: Allow evaluating the cost of different alternatives for executing a relational operator
## Data Profiles
Quantitative information on the characteristics of tables and columns
- cardinality (# of tuples) in each table T, also estimated for intermediate relational expressions
- size in bytes of tuples in T 
- size in bytes of each attribute $A_j$ in T 
- number of distinct values of each attribute in T: cardinality of the active domain of the attribute 
- min and max values of each attribute $A_j$ in T

Table profiles are stored in the data dictionary 
Profiles should be periodically refreshed by reanalyzing data in the tables 
	Update statistics command 
	Executed on demand : immediate execution during transaction processing would overload the system

Table profiles are exploited to estimate the size of intermediate relational expressions

For the selection operator: $Card (\sigma A_i =_v (T)) ≈ Card(T) / Val (A_i \ in \ T)$
$Val (A_i \ in \ T)$ = # of distinct values of $A_i$ in T (active domain) 
It holds only under the hypothesis of **uniform distribution**
## Access operators

Internal representation of the relational expression as a query tree
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241124163825.png)
Leaves correspond to the physical structures: tables, indices 
Intermediate nodes are operations on data supported by the given physical structure :scan, join, group by...

Executes sequential access to all tuples in a table also called **full table scan**
Operations performed during a sequential scan 
- Projection: discards unnecessary columns 
- Selection on a simple predicate ($A_i=v$) 
- Sorting based on an attribute list 
- Insert, update, delete
### Sorting
Classical algorithms in computer science are exploited e.g., quick sort 
Size of data is relevant: memory sort, sort on disk
### Predicate evaluation
- If available, it may exploit index access $B^+$-tree, hash, or bitmap 
- Simple equality predicate $A_i=v$  --> Hash, $B^+$-tree, or bitmap are appropriate 
- Range predicate $v1 ≤ Ai ≤ v2$  --> only $B^+$-tree is appropriate 
- For predicates with limited selectivity full table scan is better : if available, consider bitmap

Conjunction of predicates $A_i = v_1 \quad Ʌ \quad A_j= v_2$ 
The most selective predicate is evaluated first : Table is read through the index 
Next the other predicates are evaluated on the intermediate result 

Optimization: 
First compute the intersection of bitmaps or RIDs coming from available indices 
Next table read and evaluation of remaining predicates

Disjunction of predicates $A_i = v_1 \quad V \quad A_j= v_2$ 
Index access can be exploited only if all predicates are supported by an index 
otherwise full table scan
### Join Operation
A critical operation for a relational DBMS : connection between tables is based on values instead of pointers. Size of the intermediate result is typically larger than the smaller table 
Different join algorithms 
- Nested loop 
- Merge scan join 
- Hash join 
- Bitmapped join
#### Nested Loop
A single full scan is done on the outer table 
For each tuple in the outer table a full scan of the inner table is performed, looking for corresponding values 
Also called “brute force”
Efficient when : 
- inner table is small and fits in memory optimized scan
- join attribute in the inner table is indexed index scan 

Execution cost : The nested loop join technique is not symmetric, so the execution cost depends on which table takes the role of inner table
#### Merge Scan
Both tables are sorted on the join attributes 
The two tables are scanned in parallel tuple pairs are generated on corresponding values 

Execution cost : The merge scan technique is symmetric requires sorting both tables 
- may be sorted by a previous operation 
- may be read through a clustered index on join attributes 
More used in the past efficient for large tables, because sorted tables may be stored on disk
#### Hash Join
Application of the same hash function to the join attributes in both tables 
- Tuples to be joined end up in the same buckets
  collisions are generated by tuples yielding the same hash function result with different attribute value 
- A local sort and join is performed into each bucket 
Very fast join technique
#### Bitmapped Join
Bit matrix that precomputes the join between two tables A and B 
- One column for each RID in table A 
- One row for each RID in table B 
Position (i, j) of the matrix is: 
- 1 if tuple with RID j in table A joins with tuple with RID i in table B 
- 0 otherwise 
Updates may be slow

Typically used in OLAP queries, joining several tables with a large central table 
Example: Exam table, joined to Student and Course tables 
Exploits one or more bitmapped join indices, one for each pair of joined tables 
Access to the large central table is the last step

Complex queries may exploit jointly 
- bitmapped join indices 
- bitmap indices for predicates on single tables
#### Group By
Sort based 
- Sort on the group by attributes 
- Next compute aggregate functions on groups 
Hash based 
- Hash function on the group by attributes 
- Next sort each bucket and compute aggregate functions 
Materialized views may be exploited to improve the performance of aggregation operations

# Execution Plan Selection

Cost based optimization:
- Inputs:
	- Data Profiles
	- Internal representation of the query tree
- Output
	- Optimal query execution plan
	- set of dependencies

It evaluates the cost of different alternatives for reading each table and executing each relational operator.
It exploits approximate cost formulas for access operations

The search for the optimal plan is based on the following dimensions 
- The way data is read from disk e.g., full scan, index 
- The execution order among operators e.g., join order between two join operations 
- The technique by means of which each operator is implemented e.g., the join method 
- When to perform sort (if sort is needed)

The optimizer builds a tree of alternatives in which 
- each internal node makes a decision on a variable 
- each leaf represents a complete query execution plan

The optimizer selects the leaf with the lowest cost
General formula 
$$
C_{Total} = C_{I/O} \times n_{I/O} + C_{cpu} \times n_{cpu}
$$
$n_{I/O}$ is the number of I/O operations 
$n_{cpu}$ is the number of CPU operations 

The selection is based on operation research optimization techniques e.g., branch and bound

The final execution plan is an approximation of the best solution 
The optimizer looks for a solution which is of the same order of magnitude of the “best” solution 

For compile and go : it stops when the time spent in searching is comparable to the time required to execute the current best plan
 ![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241128103255.png)

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


 ![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241128122134.png)

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

Two schedules are view equivalent if they have the same reads-from set and sane final write set

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
 ![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241229170922.png)

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

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250101163142.png)

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250101163338.png)

Undo or Redo can be repeated an arbitary number of times without changing the final outcome
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
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250101173249.png)
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

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250101175324.png)

Recovery : when failure occurs the system is stopped.
recovery depends on the failure type:
- warm restart -> system failures
- cold restart -> media failures
After recovery, system becomes again available to transactions
## Warm Restart
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250101175511.png)
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
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250102145441.png)

- Redundant mapping if some fragments are replicated on different servers
	- increased data availability
	- complex maintenance (copy sync is needed)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250102145510.png)

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
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250103151242.png)

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
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250103151307.png)

3. The TM
- Collects all incoming messages from the RMs 
- If it receives ready from all RMs 
	- The commit global decision record is written in the log 
- If it receives one or more not ready or the timeout expires 
	- The abort global decision record is written in the log
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250103151442.png)
## Phase II

1. The TM
- Sends the global decision to the RMs 
- Sets a timeout for the RM answers
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250103151701.png)

2. The RM
- Waits for the global decision
- When it receives it 
	- The commit/abort record is written in the log 
	- The database is updated 
	- An ACK message is sent to the TM
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250103151751.png)

3. The TM 
- Collects the ACK messages from the RMs 
- If all ACK messages are received 
	- The complete record is written in the log 
- If the timeout expires and some ACK messages are missing 
	- A new timeout is set 
	- The global decision is resent to the RMs which did not answer until all answers are received
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250103151919.png)

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

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250104154656.png)


 Not Only SQL = NoSQL

main features: no joins, and schema-less (no tables, implicit schema)
horizontal scalability (example: 4 2CPU machines, easier to scale and cheaper)

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228142619.png)

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228142655.png)

Types of NoSQL databases:
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228142801.png)

### Key-value DB
- Simplest NoSQL data stores
- Match keys with values
- No structure
- Great performance
- Easily scaled
- Very fast
- Examples: Redis, Riak, Memcached
### Column-oriented DB
- Store data in columnar format 
	- Name = “Daniele”:row1,row3; “Marco”:row2,row4; …
	- Surname = “Apiletti”:row1,row5; “Rossi”:row2,row6,row7…
- A column is a (possibly-complex) attribute
- Key-value pairs stored and retrieved on key in a parallel system (similar to indexes)
- Rows can be constructed from column values
- Column stores can produce row output (tables)
- Completely transparent to application 
- Examples: Cassandra, Hbase, Hypertable, Amazon DynamoDB
### Graph DB
- Based on graph theory
- Made up by Vertex and Edges
- Used to store information about networks
- Good fit for several real world applications
- Examples: Neo4J, Infinite Graph, OrientDB
### Document DB
- Database stores and retrieves documents
- Keys are mapped to documents
- Documents are self-describing (attribute=value)
- Has hierarchical-tree nested data structures (e.g., maps, lists, datetime, …)
- Heterogeneous nature of documents
- Examples: MongoDB, CouchDB, RavenDB.

## MapReduce

Scalable distributed programming model to process Big Data
Great for parallel jobs requiring pieces of computations to be executed on all data records
Move the computation (algorithm) to the data (remote node, PC, disk)
2 functions: Map and Reduce

**Map** : process each record and returns a list of key-value pairs
Map functions are called once for each document:
```
function(doc){
	emit(key_1, value_1);
	emit(key_2, value_2);
}
```
Maps cannot depend on any info outside the document --> allows parallel work

**Reduce** : for each key, reduces the list of its values to a "single" value. Returned value can be a complex piece of data, a list, tuple etc...
documents emitted by the map functions are sorted by key.
reduce inputs are the map outputs: a list of key-value documents
Each execution of the reduce function returns one key-value document
The most simple SQL-equivalent operations performed by means of reducers are group by aggregations, bur reducers are very flexible functions that can execute even complex operations


Replication:
- same data --> portions of the whole dataset
- in different places --> local and/or remote servers, clusters, data centers
- goals : redundancy helps surviving failures, better performance
- approaches:
	- master slave replication
	- A-Synchronous replication

Master-Slave Replication:
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228153051.png)
master server takes all the writes, updates, inserts
one or more slave servers take all the reads
only read scalability, master single point of failure

Synchronous replication
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228153230.png)
- master waits for all the slaves to commit
- performance killer for replication in the cloud

Asynchronous replication
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228153314.png)
- master commits locally, doesn't wait for any slave
- each slave independently fetches updates from master, which may fail
- faster but unreliable

## Distributed DB

Different autonomous machines, working together to manage the same dataset

3 typical problems:
- Consistency : All the distributed databases provide the same data to the application
- Availability : Database failures (e.g., master node) do not prevent survivors from continuing to operate
- Partition tolerance : The system continues to operate despite arbitrary message loss, when connectivity failures cause network partitions

CAP Theorem: it is impossible for a distributed system to simultaneously provide all three of the previous guarantees
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228160539.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228160903.png)

- CA : Consistency and Availability of the data shared by agents within each Partition, by ignoring other partitions
- CP : We claim to tolerate partitioning/faults, because we simply block all responses if a partition occurs, assuming that we cannot continue to function correctly without the data on the other side of a partition
- AP : If we don't care about global consistency (i.e. simultaneity), then every part of the system can make available what it knows

2 out of 3 view is misleading
- partitions are rare, so usually we have CA
- choice between C and A may occur at very fine granularity
- properties are more continuous than binary
### ACID vs BASE

ACID : Atomicity Consistency Isolation Durability --> traditional approach of databases

BASE : 
	Basically Available : system provides availability, in terms of the CAP theorem
	Soft state : the state of the system may change over time, even without input, because of the eventual consistency model
	Eventual consistency : the system will become consistent over time, given that the system doesn't receive input during that time
	Example : (DNS)

# MongoDB

- high performance
- high availability
- native scalability
- high flexivility
- open source

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228161828.png)

Document Data Design
Records are stored into BSON Documents (binary representation of JSON), field-value pairs, may be nested.
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241228161846.png)

High-level, business-ready representation of the data
Mapping into developer-language objects --> date, timestamp, array, sub-documents, etc.

Field Names
- The field name **\_id** is reserved for use as a primary key; its value must be unique in the collection, is immutable, possibly autogenerated, and may be of any type other than an array.
- Field names cannot contain the null character.
- The server **permits** storage of field names that contain dots (.) and dollar signs ($)
- BSON documents may have more than one field with **the same name**. Most MongoDB interfaces, however, represent MongoDB with a structure (e.g., a hash table) that does not support duplicate field names.
- The maximum BSON document size is **16 megabytes**. To store documents larger than the maximum size, MongoDB provides GridFS.
- Unlike JavaScript objects, the fields in a BSON document are **ordered**.

## Databases and collections

- each instance of MongoDB can manage multiple databases
- each DB is composed of a set of collections
- each collection contains a set of documents
	- documents of each collection represent similar "objects"
	- MongoDB is schema-less
	- no requirement to define schema of documents a-priori and objects of the same collections can be characterized by different fields
	- possible to enforce document validation rules for a collection during update and insert operations

Show list of available databases : `show databases`
Select the database you are interested in : `use <database_name>`

Create a database and a collection inside the database : select db by using the use <> command, then create a collection.

DB doesn't exist until a document is created inside.

- Delete/Drop a database : use<> then `db.dropDatabase()`
- Create collection : `db.createCollection(<collection_name>, <options>)`
- Show collections : `show collections`
- Drop collections : `db.<collection_name>.drop()`

## C.R.U.D. Operations

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230135814.png)
### Create
- Insert a single document in a collection
```
db.<collection name>.insertOne({<set of the field: value pairs of the new document>});

db.people.insertOne( { 
						user_id: "abc123", 
						age: 55, 
						status: "A",
						favorite_colors: ["blue", "green"],
						address: { street: "my street", city: "my city" }
						
					 } );
```

- insert many documents
```
db.<collection name>.insertMany([<comma separated list of documents>]);

db.products.insertMany([
		{user_id: "abc123", age: 30, status: "A"}, 
		{user_id: "abc456", age: 40, status: "A"}, 
		{user_id: "abc789", age: 50, status: "B"} 
	]);

```
### Delete
Delete existing data, in MongoDB corresponds to the deletion of the associated document

```
DELETE FROM <collection name> WHERE <condition>

db.<collection name>.deleteMany({<condition>})
```

### Querying Data
Most of the operations available in SQL language can be expressend in MongoDB language
`SELECT` --> `find()`
```
SELECT * FROM people

db.people.find()
```

`db.<collection name>.find({<conditions>},{<fields of interest>});` 
- Conditions : select documents satisfying the conditions
- Fields of interest : select only the specified fields of the documents

```
SELECT id, user_id, status FROM people WHERE status = "A"

db.people.find (
	{ 
		status: "A"
	},
	{
		user_id: 1,
		status: 1
	}
)
```
(by default the id is always returned, if you don't want it it must be specified with `_id: 0`)

To select a single document : `db.collection_name.findOne({condition}{fields of interest})`

NO JOIN OPERATIONS EXIST
Must write a program that
- selects the documents of the first collection u are interested in
- iterates over the documents returned by the first step, by using a loop statement
- executes one query for each of them to retrieve the corresponding documents in the other collection

Relations among documents / records are provided by:
- object_ID called "manual reference"
- DBRef, standard approach across collections and databases (check driver compatibility)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230141630.png)
Instead of creating a new collection, The user document will be modified and the two subdocuments will be embedded inside.

Comparison query operators
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230141642.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230142245.png)
($or requires a list, and at least 1 is verified)
```
db.people.find( 
	{ $or: 
		[ 
			{ status: "A" } , 
			{ age: 50 } 
		] 
	} 
)
```

### Read Operations

- Count `db.people.count({condition})`
- Comparison `db.people.find({$operator:...})`
- Logical `db.people.find({$or/$not...})`
- Element `db.inventory.find({item:null / item:{$exists:false} / item: {$type:10} })`
- Embedded Documents `db.inventory.find({size{h:14,w:21...}})` will select all documents with the exact subdocument `size{h:14,w:21...}`

`db.collection.find()` gives back a Cursor. it can be used to iterate over the result or as input for next operations 
```
cursor.sort()
cursor.count()
cursor.forEach() //shell method
cursor.limit()
cursor.max()
cursor.min()
cursor.pretty()
```

sort() : `sort(<field list>)` value -1=descending, 1=ascending

count() : `db.people.count(...)` or `db.people.find(...).count()`

forEach() : applies a JS function to apply to each document from the cursor:
```
db.people.find({status: "A“}).forEach( 
	function(myDoc){ 
		print( "user:”+myDoc.name ); 
	})
```

### Update

`db.collection.updateOne(<filter>, <update>, <options>)`

`db.collection.updateMany(<filter>, <update>, <options>)`

- `<filter>` : filter condition, specifies which documents must be updated
- `<update>` : specifies which fields must be updated and their new values
- `<options>` : specific update options

```
db.inventory.updateMany( 
	{ "qty": { $lt: 50 } },
	{
		$set: { "size.uom": "in", status: "P" }, 
		$currentDate: { lastModified: true } 
	}
)
```

## Data Aggregation Pipeline

Documents enter a multi-stage pipeline that transforms the documents of a collection into an aggregated result
Pipeline stages can appear multiple times in the pipeline

Pipeline expressions can only operate on the current document in the pipeline and cannot refer to data from other documents: expression operations provide in-memory transformation of documents (max 100 Mb of RAM per stage).

Generally, expressions are stateless and are only evaluated when seen by the aggregation process with one exception: accumulator expressions used in the $group stage (e.g. totals, maximums, minimums, and related data).

The aggregation pipeline provides an alternative to map-reduce and may be the preferred solution for aggregation tasks since MongoDB introduced the $accumulator and $function aggregation operators starting in version 4.4

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230145508.png)

`db.collection.aggregate({<set of stages>})`
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230145559.png)

Stages:
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230150133.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230150142.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230150557.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230150608.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230150617.png)
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230150628.png)
## Examples
Given this collection of books:
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020241230150842.png)

- For each country, select the average price and the average review_score.
- The review score should be rounded down.
- Show the first 20 results with a total number of books higher than 50.
```
db.book.aggregate(
	[
		{$unwind:"$price"},
		{group:{_id: "price.country",
				avg_price: {$avg:'$price.v'},
				bookcount: {$sum:1},
				review:{$avg:'$review_score'}
				}
		},
		{$match:{bookcount:{$gte:50}}},
		{$project:{avg_price:1, review:{$floor:'$review'}}},
		{$limit:20},
	])
```

- Compute the 95 percentile of the number of pages, 
- only for the books that contain the tag “guide”.
```
db.book.aggregate([
	{$match:{tag:"guide"}},
	{$sort:{n:1}},
	{$group:{_id:null,value:{$push:"$n"}}},
	{$project:
		{"n95p": {$arrayElemAt:
				["$value",
				{$floor:{multiply:[0.95,{$size:"$value"}]}}
				]
		}}
	},
])
```
## Indexing

Indexes are data structures that store a small portion of the collection's data set in a form easy to traverse.

They store ordered values of a specific field, or set of fields, in order to efficiently support equality matches, range-based queries and sorting operations.

MongoDB creates a unique index on the `_id` field during the creation of a collection.
The `_id` index prevents clients from inserting two documents with the same value for the `_id` field. 
You cannot drop this index on the `_id` field.

`db.collection.createIndex(<index keys>, <options>)`

Different types of indexes:
- single field : Support user-defined ascending/descending indexes on a single field of a document
- compound field : Support user-defined indexes on a set of fields
- multikey
- geospatial
- text : Support efficient searching for string content in a collection
- hashed


Take a query string
match it against a document collection
calculate a set of relevant results
display a sorted list

ElasticSearch : real-time distributed search and analytics engine
Scalable and efficient data exploration:
- full-text search
- structured search
- analytics
Document-oriented (JSON) search engine

Strong points:
- horizontal scaling capabilities make it suitable for a great variety of apps
- RESTful API allows programmers to write most of the operations in any programming language
- JSON-based interactions make it machine and human-friendly
- any modifications to stored documents is recorded in transaction logs that are replicated in multiple nodes to avoid data loss.

# Data Representation

`ElasticSearch: field` <==> `SQL: column` 
Data is stored in named entries belonging to a variety of **data types**
In ElasticSearch (similarly to other NoSQL databases) a field can contain multiple values of the same type (list of values)

`ElasticSearch: document` <==> `SQL: row`
Data objects are represented as rows (SQL) or documents (ElasticSearch)
Columns and fields are part of a row (SQL) or a document (ElasticSearch)
Row format in SQL is strict and follows a predefined schema. 
Documents are more flexible and can contain a variety of fields (they do not follow a strict schema)

`ElasticSearch: index` <==> `SQL: table`

`ElasticSearch: cluster` <==> `SQL: database`

![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250104141937.png)

Different meanings of "index"
- An index stores a collection of documents
- To index a document means to insert a document in an index
- (inverted index) Additional structure that accelerates data retrieval

Document = top-level (root) object serialized into JSON. Uniquely identified by pair:
- Index : where the document is stored
- ID : identifier of the document

# Querying and Searching

Search options: 
- Structured query on specific fields, possibly sorted
- Full-text query
- Combination of the two
- Mapping
- Analysis
- Query DSL (Domain Specific Language)

Full Text : Textual data, usually written in some human language
> “How well does this document match the query?”

it is also important understanding the intent : 
- abbreviations
- singulars/plurals, verb conjugation
- synonyms
- order of words building a context

Analysis : 
1. Tokenization of a block of text into individual terms suitable for an inverted index
2. Normalization into a standard form to improve their retrieval (or recall) in queries

A filter is used for fields containing exact values : boolean match / no match
A query is (typically) used for full-text search : concept of relevance is well suited to fulltext search

## ElasticSearch Query

Expressed in Query DSL
Submitted as formatted JSON in the body of an HTTP request

```
// empty query
POST /_search
{}

// search on specific index
POST index1/_search
{}

```

```
POST departments/_search
{
	"query": {
		"match" : {"name" : "John"}
	}
}
```

Compound queries :
```
POST departments/_search 
{ 
	"query": { 
		"bool": { 
			"should": [ 
				{"match": {"name": "John"}}, 
				{"match": {"name": "Mark"}} 
			], 
				"minimum_should_match":1, 
			"must":{ 
				{"match": {"title": "developer"}}
			 }
			"must_not":{ 
				{"match": {“lastname": “Smith"}}
			}
		}
	}
}
```

match can be used only on text fields

It is possible to specify multiple indices to be searched in the query URI
`POST rooms,students/_search {...}`
When a number of documents can be returned as query result, by default the top 10 relevant results are returned

# Data modifications

## Insert
```
POST /index_name/ 
{ 
	JSON document 
}
```
## Update

1. Retrieve old document
2. modify retrieved copy
3. delete old document
4. index the new document

name of index / Unique ID of document / -update
```
PUT index_name/123/_update 
{ 
	"color" : "red", 
}
```
## Delete

```
DELETE index_name/id
```

# Results Scoring

Relevance is represented by value:
- floating point number
- computed for each document matching the query
- stored as **\_store** for each document in the search result
- higher \_score values correspond to more relevant documents

Need to compute similarity between query and each document
To evaluate the importance of each term in a document wrt query:
- TF/IDF (Term Frequency / Inverse Document Frequency) score
- Document and query are represented in vector form (Vector Space Model)

$$
TF(t\ in\ d)=\sqrt{frequency}
$$
$$
IDF(t) = 1 + \log(numDocs / (docFreq+1))
$$
$$
norm(d) = 1 / \sqrt{numTerms}
$$

Vector Space Model : represents both query and document as term vectors. Provides a way to compare multi-term query against a document

# Horizontal Scalability

Sharding is a technique to divide an index in smaller partitions. 
Each partition is a shard
Each document belongs to a single shard
Each shard is an instance of a Lucene index

When data is written to a shard 
1. It is periodically (every 1 second) written into a new immutable Lucene segment on disk 
2. It becomes available for querying
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250104151326.png)

A cluster is a collection of multiple machines (nodes in the cluster)
Shards can be stored in any node within the cluster
![](file:///C:%5CUsers%5Cricca%5CDocuments%5CObsidian%20Vault%5CData%20Science%5Cattachments%5CPasted%20image%2020250104151351.png)

Sharding is important because:
- It allows splitting data in smaller chunks, and thereby scaling on large volumes of data 
	- Data may be distributed across multiple nodes within a cluster 
	- Shards can be stored on smaller disks 
		- E.g., it is possible to store 1TB of data even without a single node with that disk capacity 
- Operations can be distributed across multiple nodes and thereby parallelized 
	- Performance is increased, because multiple machines can potentially work on the same query.
- Shards may be replicated on different nodes to increase availability

# Document Versioning

ElasticSearch uses optimistic concurrency control
Different from ACID transactions that need locking
The process is “simple” for centralized data management

ElasticSearch data may be distributed on different nodes in a cluster
When documents are created, updated, or deleted, the new version of the document has to be replicated to other nodes in the cluster

Every document has a **\_version** number that is incremented whenever a document is changed

