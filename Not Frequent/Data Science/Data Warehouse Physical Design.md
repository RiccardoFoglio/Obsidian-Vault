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