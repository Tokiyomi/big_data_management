# Designing Intensive Data Applications - 7
By toki
## Table of contents
- [Chapter 7](#chapter-5)
- [References](#references)

## Chapter 7

#### 1. What are transactions?
A transaction is a way for an application to group several reads and writes together into a logical unit. 
Transactions are not a law of nature; they were cre‐ ated with a purpose, namely to simplify the programming model for applications accessing a database. 
By using transactions, the application is free to ignore certain potential error scenarios and concurrency issues, because the database takes care ofthem instead 
(we call these safety guarantees).

#### 2. Describe each concept of ACID
- Atomicity: Transactions are all or nothing
- Consistency: Only valid data is saved
- Isolation: Transactions do not affect each other
- Durability: Written data will no be lost

#### 3. What are the different replication logs methods?
- Statement-based replication: the leader logs every write request (statement) that it executes and sends that statement log to its followers.

- Write-ahead log (WAL) shipping: the log is an append-only sequence of bytes containing all writes to the database. We can use the exact same log to build a replica on another node: besides writing the log to disk, the leader also sends it across the network to its followers.

- Logical (row-based) log replication: use different log formats for replication and for the storage engine, which allows the replication log to be decoupled from the storage engine internals.

- Trigger-based replication: A trigger lets you register custom application code that is automatically executed when a data change (write transaction) occurs in a database system. The trigger hasthe opportunity to log this change into a separate table, from which it can be read by an external process.

#### 4. Whats is the most basic level of transaction isolation and what its two guarantees?
Read committed. Its guarantees:

When reading from the database, you will only see data that has been committed (no dirty reads).
When writing to the database, you will only overwrite data that has been committed (no dirty writes).

#### 5. Describe the mechanisms used in Dynamo-style datastores?
Read repair: when a client makes a read from several nodes in parallel, it can detect any stale responses.

Anti-entropy process: some datastores have a background process that constantly looks for differences in the data between replicas and copies any missing data from one replica to another.

#### 6. What is hinted handoff?
Once the network interruption is fixed, any writes that one node temporarily accepted on behalf of another node are sent to the appropriate “home” nodes.

#### 7. How can we avoid partition with disproportionately high load?
Assign records to nodes randomly. That would distribute the data quite evenly across the nodes.But it's not the best way, thus let’s assume for now that you have a simple key-value data model, in which you always access a record by its primary key. For example, in an old-fashioned paper encyclopedia, you look up an entry by its title; since all the entries are alphabetically sorted by title, you can quickly find the one you’re looking for.

#### 8. Describe an example of partitioning by key range
A print encyclopedia is partitioned by key range. Volume 1 contains words starting with A and B, but volume 12 contains words starting with T, U, V, X, Y, and Z. Simply having one volume per two letters of the alphabet would lead to some volumes being much bigger than others.

#### 9. What are skewed workloads?
Some partitions that have more data or queries than others. The presence of skew makes partitioning much less effective. In an extreme case, all the load could end up on one partition, so 9 out of 10 nodes are idle and your bottleneck is the single busy node.

#### 10. What is snapshot isolation?
The idea is that each transaction reads from a consistent snapshot of the database—that is, the trans‐ action sees all the data that was committed in 
the database at the start of the transaction. Even if the data is subsequently changed by another transaction, each transaction sees only the old data 
from that particular point in time.

## References

Kleppmann, M. (2017). Designing Data Intensive Applications: Part 1. Foundations of Data Systems: Encoding and Evolution. O'Reilly, 1st Edition. Pp. 111-139
