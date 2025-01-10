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
![[Pasted image 20241012155110.png]]
### Ranking
```sql
SELECT COD_I, SUM(SoldAmount), 
RANK() OVER ( 
	ORDER BY SUM(SoldAmount) 
	) AS SalesRank 
FROM Facts 
GROUP BY COD_I;
```
![[Pasted image 20241012155216.png]]
### Dense Ranking
```sql
SELECT COD_I, SUM(SoldAmount), 
DENSE_RANK() OVER ( 
	ORDER BY SUM(SoldAmount) 
	) AS DenseSalesRank 
FROM Facts 
GROUP BY COD_I;
```
![[Pasted image 20241012155258.png]]
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
![[Pasted image 20241012155400.png]]
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
![[Pasted image 20241012155926.png]]
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
![[Pasted image 20241012163050.png]]
### NTILE
Allows splitting each partition in n subgroups containing the same number of records. An identifier is associated to each subgroup
```sql
SELECT Type, Weight, NTILE(3) OVER ( 
	PARTITION BY Type 
	ORDER BY Weight 
	) AS Ntile3Weight 
FROM ITEM;
```
![[Pasted image 20241012163210.png]]
# Materialized Views
The result is precomputed and stored on the disk
They improve response times
Usually they are associated to queries with aggregations
They may be used also for non aggregating queries
Materialized views can be used as a table in any query
The DBMS can change the execution of a query to optimize performance
Materialized views can be automatically used by the DBMS without user intervention
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
- must be aware of the constraint existence!
### Example
![[Pasted image 20241012164549.png]]

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
