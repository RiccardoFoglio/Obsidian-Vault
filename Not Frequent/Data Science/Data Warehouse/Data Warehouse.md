
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

![[Pasted image 20241006142909.png]]

Dimension tables used to represent specific portions or properties of the database

![[Pasted image 20241006143458.png]]

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
![[Pasted image 20241006145934.png]]

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
![[Pasted image 20241006145902.png]]

Staging Area: where ETL operations take place. DW fed only when data is ready, not on the fly like in 2 level arch.

