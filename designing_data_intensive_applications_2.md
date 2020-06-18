# Designing Intensive Data Applications - 2
By toki
## Table of contents
- [Chapter 2: Data Models and Query Languages](#chapter-2-data-models-and-query-languages)
	- [Relational Model vs Document Model](#relational-model-vs-document-model)
	- [Query Languages for Data](#query-languages-for-data)
	- [Graph Models](#graph-models)
- [Extra](#extra)
- [References](#references)

## Chapter 2: Data Models and Query Languages
## Relational Model vs Document Model

### 1. For a data structure like a *résumé*, which is the most appropiate representation of information and why?

> A *résumé* is mostly a self-contained document, a JSON representation
> can be quite appropriate. Let's see an example:

```json
{"user_id": 251, 
"first_name": "Bill", 
"last_name": "Gates", 
"summary": "Co-chair of the Bill & Melinda Gates... Active blogger.", 
"region_id": "us:91", 
"industry_id": 131, 
"photo_url": "/p/7/000/253/05b/308dd6e.jpg",
"positions": [ 
	{"job_title": "Co-chair", "organization": "Bill & Melinda Gates Foundation"}, 
	{"job_title": "Co-founder, Chairman", "organization": "Microsoft"}
],
"education": [ 
	{"school_name": "Harvard University", "start": 1973, "end": 1975},
	{"school_name": "Lakeside School, Seattle", "start": null, "end": null} 
], 
"contact_info": {
	"blog": "http://thegatesnotes.com",
	"twitter": "http://twitter.com/BillGates" }
}
```
> The JSON representation has better locality than the multi-table
> schema RDMS. If you want to fetch a profile in the relational example,
> you need to either perform multiple queries (query each table by
> user_id) or perform a messy multiway join between the users table and
> its subordinate tables. In the JSON representation, all the relevant
> information is in one place, and one query is sufficient.

### 2. How do we express hierarchy in document models?

> Document databases reverted back to the hierarchical model in one
> aspect: storing nested records (one-to-many relationships, like
> positions, education, and contact_info) within their parent record
> rather than in a separate table.
> 
> Relational and document databases are not fundamentally different: in
> both cases, the related item is referenced by a unique identifier,
> which is called a *foreign key* in the relational model and a
> *document reference* in the document model.

### 3. List the pros of document and relational models
* Document
	* Schema flexibility
	* Better performance due to locality
	* For some applications it is closer to the data structures used by the application
* Relational
	* Better support for joins
	* Many-to-one and many-to-many relationships

### 4. Which data model leads to simpler application code?

> If the data in your application has a document-like structure (i.e., a
> tree of **one-to many relationships**, where typically the entire tree
> is loaded at once), then it’s probably a good idea to use a document
> model.
> 
> If your application does use **many-to-many relationships**, the
> document model becomes less appealing.
> 
> It’s not possible to say in general which data model leads to simpler
> application code; it depends on the kinds of relationships that exist
> between data items. For **highly interconnected data**, the document
> model is awkward, the relational model is acceptable, and graph models
> are the most natural.

### 5. Imagine you want to change the format of your data in a given application. For example, say you are currently storing each user’s full name in one field, and you instead want to store the first name and last name separately. How would yo do it, in both, document and relational schema?

> In a document database, you would just start writing new documents
> with the new fields and have code in the application that handles the
> case when old documents are read. For example:

```json
if (user && user.name && !user.first_name) 
{ 
// Documents written before Dec 8, 2013 don't have first_name 
user.first_name = user.name.split(" ")[0]; 
}
```

> On the other hand, in a “statically typed” database schema, you would
> typically per‐ form a migration along the lines of:

```sql
ALTER TABLE users ADD COLUMN first_name text; 
UPDATE users SET first_name = split_part(name, ' ', 1); -- PostgreSQL 
UPDATE users SET first_name = substring_index(name, ' ', 1); -- MySQL
```

### 6. How do different database services manage their data *locality* for queries?

|Service| Management |
|--|--|
| MongoDB | A document is usually stored as a single continuous string, encoded as JSON, XML, or a binary variant thereof (such as MongoDB’s BSON). |
| Google’s Spanner | Google’s Spanner database offers the same locality properties in a relational data model, by allowing the schema to declare that a table’s rows should be interleaved (nested) within a parent table |
| Oracle | Oracle allows the same, using a feature called multi-table index cluster tables |
| Cassandra & HBase| The column-family concept in the Bigtable data model (used in Cassandra and HBase) has a similar purpose of managing locality |

## Query languages for Data
### 7. Create an imperative query to get the movie titles directed by *Quentin Tarantino* that where released after 2010 and lasted more than 120 minutes.
```json
function GetTitles(movies) {
 var TarantinoMovies = [];
 for (var i = 0, i < movies.length, i++) {
     if (movies[i].director === "Quentin Tarantino") && (movies[i].release_date > 2010) && (movies[i].duration > 120) {
        TarantinoMovies.push(movies[i]);
       }
     }
 return TarantinoMovies;
}
```
### 8. Use MongoDB's MapReduce feature to get a report of how many yellow volkswagen you have seight per month
```json
db.observations.mapReduce( 
	function map() { 
		var year = this.observationTimestamp.getFullYear(); 	
		var month = this.observationTimestamp.getMonth() + 1; 
		emit(year + "-" + month, this.numYellowVolkswagens); 
		}, 
	function reduce(key, values) { 
		return Array.sum(values); 
	}, 
	{ 
		query: { type: "YellowVolkswagens" }, 
		out: "MonthlyYellowVolkswagensReport" 
	}
);
```
## Graph models

### 9. Describe a graph

> A graph consists of two kinds of objects: *vertices* (also known as
> nodes or entities) and *edges* (also known as relationships or arcs).
> Many kinds of data can be modeled as a graph. Typical examples
> include:
> * Social graphs
> * Web graphs
> * Road networks

### 10. List the proporties of vertices and edges

| Vertex | Edges |
|--|--|
| Each vertex consists of: a unique identifier, a set of outgoing edges, a set of incoming edges, a collection of properties (key-value pairs). | Each edge consists of: a unique identifier, the vertex at which the edge starts (the tail vertex), the vertex at which the edge ends (the head vertex), a label to describe the kind of relationship between the two vertices, a collection of properties (key-value pairs) |


### 11. Investigate 3 graph databases
| Database | Description |
|--|--|
| ArangoDB | NoSQL native multi-model database system developed by ArangoDB Inc. The database system supports three important data models (key/value, documents, graphs) with one database core and a unified query language called AQL (ArangoDB Query Language). |
| Neo4j| Open-source, supports ACID, has high-availability clustering for enterprise deployments, and comes with a web-based administration that includes full transaction support and visual node-link graph explorer |
| OrientDB | Second-generation distributed graph database with the flexibility of documents in one product (i.e., it is both a graph database and a document NoSQL database).|


### 12. Investigate 5 graph query languages

> * **Declarative** 	* **Cypher:** Cypher is a declarative query language for property graphs, created for the Neo4j graph database. 	*
> **SPARQL:** Pronounced sparkle is an RDF query language—that is, a semantic query language for  databases—able to retrieve and manipulate
> data stored in Resource Description Framework (RDF) format. 	*
> **Datalog:** Datalog is a declarative logic programming language that syntactically is a subset of Prolog. It is often used as a query
> language for deductive databases. In recent years, Datalog has found
> new application in data integration, information extraction,
> networking, program analysis, security, cloud computing and machine
> learning.
> * **Imperative** 	* **Gremlin:** Gremlin is a graph traversal language and virtual machine developed by Apache TinkerPop of the Apache
> Software Foundation. Gremlin works for both OLTP-based graph databases
> as well as OLAP-based graph processors.

## References
Kleppmann, M. (2017). _Designing Data Intensive Applications: Part 1. Foundations of Data Systems: Reliable, Scalable, and Maintainable Applications_. O'Reilly, 1st Edition. Pp. 27-63

Rodriguez, M.A., "[The Gremlin Graph Traversal Machine and Language](https://arxiv.org/abs/1508.03843)," Proceedings of the ACM Database Programming Languages Conference, October, 2015.

[SPARQL 1.0 Query language](http://www.w3.org/TR/rdf-sparql-query/) 

Huang, Green, and Loo, "Datalog and Emerging applications",  [_SIGMOD 2011_](http://www.cs.ucdavis.edu/~green/papers/sigmod906t-huang.pdf)  (PDF), UC Davis.

Leonard, Anghel (2013). _Pro Hibernate and MongoDB_. Apress. p. 384. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)")  [9781430257943](https://en.wikipedia.org/wiki/Special:BookSources/9781430257943 "Special:BookSources/9781430257943") – via Google Play. Multimodel databases (..., ArangoDB and more)
