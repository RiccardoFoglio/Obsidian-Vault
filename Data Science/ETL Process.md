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
