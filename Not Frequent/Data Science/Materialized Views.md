Precomputed summaries for the fact table
- explicitly stored in data warehouse
- provide a performance increase for aggregate queries
![[Pasted image 20241013141950.png]]

They are defined in SQL statements
Materialized views may be exploited for answering several different queries (not for all aggregation operators)

![[Pasted image 20241013142124.png]]

Huge number of allowed aggregations, most attribute combinations are eligible
Select of the "best" materialized view set
Cost function minimization: query execution cost, view maintenance (update) cost
Constraints: available space, time window for update, response time, data freshness


