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

![[Pasted image 20250104141937.png]]

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
![[Pasted image 20250104151326.png]]

A cluster is a collection of multiple machines (nodes in the cluster)
Shards can be stored in any node within the cluster
![[Pasted image 20250104151351.png]]

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
