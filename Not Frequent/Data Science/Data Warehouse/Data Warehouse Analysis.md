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

![[Pasted image 20241007143240.png|400]]
### Drill Down

Data detail increase by:
- increasing detail in a dimension, by walking down a hierarchy
	`group by city, month` --> `group by store, month`
- adding a whole dimension
	`group by product` --> `group by product, city`
Frequently drill down operates on a subset of data produced by the initial query

![[Pasted image 20241007143634.png|400]]
### Slice and Dice

Selection of a data subset by means of selection predicates
- Slice = equality predicate selecting a "slice" --> `Year = 2005`
- Dice = predicate expression selecting a "dice" --> `Category = 'Food' and City = 'Torino'`

![[Pasted image 20241007143932.png|400]]
### (Table) Pivot

Reorganization of the multidimensional structure without varying the detail level.
Increases readability of the same information
Multidimensional representation is always based on a "grid" (hierarchical spreadsheet)
- 2 dimensions are the main grid axes
- position of dimensions in the grid are changed

![[Pasted image 20241007144206.png|400]]
# Extensions of the SQL Language

Interface tools require operations for the computation of different Group Bys at the same time.
The SQL-99 (SQL3) standard has extended the SQL Group By clause
[[Group By recap]]

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
![[Pasted image 20241012150510.png]]
#### Comparison
```SQL
SELECT City, Month, Amount, 
	SUM(Amount) OVER (PARTITION BY City) 
	AS TotalAmount 
FROM Sales
```
![[Pasted image 20241012150454.png]]

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
![[Pasted image 20241012150440.png]]
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
![[Pasted image 20241012153609.png]]
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

