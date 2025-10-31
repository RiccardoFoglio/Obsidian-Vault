Not Only SQL = NoSQL

main features: no joins, and schema-less (no tables, implicit schema)
horizontal scalability (example: 4 2CPU machines, easier to scale and cheaper)

![[Pasted image 20241228142619.png]]

![[Pasted image 20241228142655.png]]

Types of NoSQL databases:
![[Pasted image 20241228142801.png]]

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
![[Pasted image 20241228153051.png]]
master server takes all the writes, updates, inserts
one or more slave servers take all the reads
only read scalability, master single point of failure

Synchronous replication
![[Pasted image 20241228153230.png]]
- master waits for all the slaves to commit
- performance killer for replication in the cloud

Asynchronous replication
![[Pasted image 20241228153314.png]]
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
![[Pasted image 20241228160539.png]]
![[Pasted image 20241228160903.png]]

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

![[Pasted image 20241228161828.png]]

Document Data Design
Records are stored into BSON Documents (binary representation of JSON), field-value pairs, may be nested.
![[Pasted image 20241228161846.png]]

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

![[Pasted image 20241230135814.png]]
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
![[Pasted image 20241230141630.png]]
Instead of creating a new collection, The user document will be modified and the two subdocuments will be embedded inside.

Comparison query operators
![[Pasted image 20241230141642.png]]
![[Pasted image 20241230142245.png]]
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

![[Pasted image 20241230145508.png]]

`db.collection.aggregate({<set of stages>})`
![[Pasted image 20241230145559.png]]

Stages:
![[Pasted image 20241230150133.png]]
![[Pasted image 20241230150142.png]]
![[Pasted image 20241230150557.png]]
![[Pasted image 20241230150608.png]]
![[Pasted image 20241230150617.png]]
![[Pasted image 20241230150628.png]]
## Examples
Given this collection of books:
![[Pasted image 20241230150842.png]]

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

