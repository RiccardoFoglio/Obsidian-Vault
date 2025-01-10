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
- Empowering users to collaborate and analyze data in different ways : self service analytics
- Integration happens outside the storage environment
- Minimal involvement of IT : wrangling with data is a self-service function
- Sandboxed for self-service analytics : need well defined problems
## Cons
- Raw data is stored with no oversight of the contents. storing data doesn't provide business value on its own. Need data governance, semantic consistency, mechanism to catalog data
- Consistency and data quality are uncertain : data brought into a data lake is co-located not integrated
- Business users don't have time/willingness to learn
- Rogue queries can bring down big clusters

Central question is whether collecting and storing data without a pre-defined business purpose is a good idea