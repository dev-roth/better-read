# better-read

- Java Brains - Spring Boot + Cassandra CodeWithMe Example App
- <https://www.youtube.com/watch?v=LxVGFBRpEFM>

## Step-by-Step approach

- Start with the UI/ UX  
=> at least for Big Data apps one should know what data is actually needed by the user and the relationship between the data tables is not the main focus resp. the driven factor
- Design the system around the UI/ UX.
  - Quality Goals:  
    Being high performant and reliable although using large amount of data (> several million entries)
  - Techstack:  
    Spring Boot, Spring MVC (Web - server generated HTML pages), Spring Security, Spring Data Cassandra, Apache Cassandra (NoSQL)

## Apache Cassandra

### Basics/ Concepts

- Links:
  - <https://cassandra.apache.org/>
  - <https://cassandra.apache.org/_/cassandra-basics.html>
  - <https://hub.docker.com/_/cassandra>
- **Apache Cassandra is an open source NoSQL distributed database** trusted by thousands of companies for **scalability (horitonal/ scale-out) and high availability (running on multiple nodes)** without compromising performance.  
=> linear scalability applies essentially indefinitely. This capability has become one of Cassandra’s key strengths (Want more power? Add more nodes!).
- Although being a NoSQL database, Cassandra is not a key-value store or a document-based DB. Cassandra is a **transactional DB that supports full-typed schemas** including tables, rows and columns.
- "Distributed" means that **Cassandra can run on multiple machines** while appearing to users as a unified whole. It´s possible to run Cassandra on a single node, but this would not be really useful and should only be done for dev/ test purposes.
- Since it is a distributed database, Cassandra can (and usually does) have multiple nodes. A node represents a single instance of Cassandra. **These nodes communicate with one another through a protocol called gossip, which is a process of computer peer-to-peer communication**.
- Cassandra has a **masterless architecture**.

- In Cassandra, the **data itself is automatically distributed**, with (positive) performance consequences. It accomplishes this **using partitions**.
- One piece of data can be replicated to multiple **replica nodes, ensuring reliability and fault tolerance**. Cassandra supports the notion of a **replication factor (RF)**, which describes how many copies of your data should exist in the database.

- When developing/ designing the data schema/ structures for Cassandra, one if often approaching the task by **thinking about the queries first**, so having the app usage in mind (after a rough first data structure design, e.g. an ERM) and optimize the data structures for that.  
=> e.g. fetch something by id, name, whatever when browsing (prio 1), follow up queries by related lookup using the result of the first query,...  
=> e.g. **normalization is not of any importance** here, so it´s perfectly fine to violate normalization rules like adding properties from other entities directly in the tables and not use FK for that...
- The primary keys of the entities may be used as **partition keys**. In this case **single record partitions** are used. Single record partitions make sense for cases, where one is always fetching the data of one particular record.  
  When the use case actually fetches multiple records, one should try to store them in one partition (=> **multi record partition**), because the partitions are stored on different nodes (=> performance).  
=> **partitions := distributed data buckets** (on different nodes)  
=> **one need to watch out for partitions that might get too big**. A couple of hundred or even thousand records might be still ok, but more than that should be split in order to provide a reasonable performance.  
   This can easily be done by adding another partition key/ col like a time related col (year, month,...), a category or group info,...  
=> having not too many different partitions on one hand and partitions that are not too big on the other hand is the **tradeoff** one need to do in order to have a good performance
- The order of the entries are actually already taken into consideration in Cassandra when saving the data (compared to SQL DBs where this is determined at read/ runtime). A **clustering column** is used for this purpose.

### CQL - The Cassandra Query Language

- **CQL offers a model similar to SQL. The data is stored in tables containing rows of columns.**
- The tables itself are **organized in keyspaces** that are similar to the concept of schema in SQL.

- Commands:
  - `describe keyspaces;`  - show all available keyspaces
  - `describe <keyspace>;` - show a particular keyspace and its config in form of a `CREATE KEYSPACE` statement.
  - `use <keyspace>;` -       use/ connect to a particularkeyspace
  - `describe tables;`  -    show all available tables in the used keyspace

## DataStax Astra - Multi-cloud DBaaS built on Apache Cassandra

- <https://www.datastax.com/>
- <https://astra.datastax.com/>
- Multi-cloud DBaaS built on Apache Cassandra.
- Free tier available with currently up to 80GB storage and/or 20 million operations monthly.
- Data APIs via REST and GraphQL APIs.
- Deployed on major public cloud provier of own choice (AWS, Azure, Google Cloud).
