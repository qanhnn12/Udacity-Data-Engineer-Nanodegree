# Data Model

# 1. What is a data model?

> An abstraction that organizes elements of data and how they will relate to each other.
> 

The process of creating data models for an information system.

Data modeling can easily translate to database modeling, as this is the essential end state.

# 2. Data modeling

![Untitled](Data%20Model%2031a3483f88294b5fb15fbd5b8c2e4f1f/Untitled.png)

- Process to support business and user applications
- Gather requirements from the application team, business users and end users
- **Conceptual modeling**: entity mapping
- **Logical modelling**: tables, schemas, columns
- **Physical modeling**: transforming logical model to the database Data Definition Language (DDL). We write code to create tables.

# 3. Why is data modeling important?

- Data organization is critical
- Organized data determines later data use
- Begin prior to building out application, business logic, and analytical model
- Iterative process

# 3. Relational databases

![Untitled](Data%20Model%2031a3483f88294b5fb15fbd5b8c2e4f1f/Untitled%201.png)

![Untitled](Data%20Model%2031a3483f88294b5fb15fbd5b8c2e4f1f/Untitled%202.png)

## 3.1. When to use Relational Database?

- **Flexibility for writing in SQL queries:** SQL - most common database query language
- **Ability to do JOINS**
- **Modeling the data not modeling queries**
- **Ability to do aggregations and analytics**
- **Easier to change to business requirements**
- **Secondary Indexes available** : add another index to help with quick searching
- **Smaller data volumes: s**maller data volume (and not big data) → use a relational database for its simplicity.
- **ACID Transactions**: Allows you to meet a set of properties of database transactions intended to guarantee validity even in the event of errors, power failures, and thus maintain data integrity.

### 3.1.1. ACID Transactions

Properties of database transactions intended to guarantee validity even in the event of errors or power failures.

- **Atomicity:** The whole transaction is processed or nothing is processed.
- **Consistency:** Only transactions that abide by constraints and rules are written into the database, otherwise the database keeps the previous state.
- **Isolation:** Transactions are processed independently and securely, order does not matter.
- **Durability:** Completed transactions are saved to database even in cases of system failure.

## 3.2. When Not to Use a Relational Database

- **Have large amounts of data:** Relational Databases are not distributed databases and because of this they can only scale vertically by adding more storage in the machine itself. You are limited by how much you can scale and how much data you can store on one machine. You cannot add more machines like you can in NoSQL databases.
- **Need to be able to store different data type formats:** Relational databases are not designed to handle unstructured data.
- **Need high throughput -- fast reads:** While ACID transactions bring benefits, they also slow down the process of reading and writing data. If you need very fast reads and writes, using a relational database may not suit your needs.
- **Need a flexible schema:** Flexible schema can allow for columns to be added that do not have to be used by every row, saving disk space.
- **Need high availability:** The fact that relational databases are not distributed (and even when they are, they have a coordinator/worker architecture), they have a single point of failure. When that database goes down, a fail-over to a backup system occurs and takes time.
- **Need horizontal scalability:** Horizontal scalability is the ability to add more machines or nodes to a system to increase performance and space for data.

## 3.3. Normalization vs. Denormalization

| Normalization | Denormalization |
| --- | --- |
| Reduce data redundancy & increase data integrity | Duplicate data in many tables |
| Need to JOINs many table → Performance issues | Reduce JOINs and aggregations → Improve performance  |
| Slow Reads because we have to JOINs many tables to perform aggregation; Fast Writes because data is only at a place, update quickly | Fast Reads because of high availability, Slow Writes because of updating many tables to ensure consistency |
- 1NF - primary key, no duplicate data in a single cell
- 2NF - no partial dependency
- 3NF - no transitive partial dependency

# **4. NoSQL Databases**

**NoSQL** has a simpler design, simpler horizontal scaling and finer control of availability. Data structures used are different than those in Relational Database, which makes the operations faster. 

NoSQL = Not Only SQL = Non Relational 

**Various NoSQL Database:**

- Apache Cassandra (Partition Row store)
- MongoDB (Document store)
- DynamoDB (Key-Value store)
- Apache HBase (Wide Column Store)
- Neo4J (Graph Database)

## 4.1. Apache Cassandra

Use its own language CQL

![https://video.udacity-data.com/topher/2021/August/612dd186_use-this-version-data-modeling-lesson-1-1/use-this-version-data-modeling-lesson-1-1.png](https://video.udacity-data.com/topher/2021/August/612dd186_use-this-version-data-modeling-lesson-1-1/use-this-version-data-modeling-lesson-1-1.png)

## 4.2. When to Use a NoSQL Database

- **Need to be able to store different data type formats**: NoSQL was also created to handle different data configurations: structured, semi-structured, and unstructured data. JSON, XML documents can all be handled easily with NoSQL.
- **Large amounts of data**: Relational Databases are not distributed databases and because of this they can only scale vertically by adding more storage in the machine itself. NoSQL databases were created to be able to be horizontally scalable. The more servers/systems you add to the database the more data that can be hosted with high availability and low latency (fast reads and writes).
- **Need horizontal scalability**: Horizontal scalability is the ability to add more machines or nodes to a system to increase performance and space for data
- **Need high throughput**: While ACID transactions bring benefits, they also slow down the process of reading and writing data. If you need very fast reads and writes, using a relational database may not suit your needs.
- **Need a flexible schema**: Flexible schema can allow for columns to be added that do not have to be used by every row, saving disk space.
- **Need high availability**: Relational databases have a single point of failure. When that database goes down, a failover to a backup system must happen and takes time.

## 4.3. CAP Theorem

- **Consistency**: Every read from the database gets the latest (and correct) piece of data or an error
- **Availability**: Every request is received and a response is given -- without a guarantee that the data is the latest update
- **Partition Tolerance**: The system continues to work regardless of losing network connectivity between nodes

## 4.4. Denormalization in Apache Cassandra

- Denormalization is not just okay -- it's a must
- ALWAYS think Queries first
- Apache Cassandra does **not** allow for JOINs between tables
- Denormalization must be done for fast reads
- Apache Cassandra has been optimized for fast writes (slow writes with Denormalization in Relational Database)
- One table per query is a great strategy

![https://video.udacity-data.com/topher/2021/August/612ea2b6_use-this-version-data-modeling-lesson-3-1/use-this-version-data-modeling-lesson-3-1.png](https://video.udacity-data.com/topher/2021/August/612ea2b6_use-this-version-data-modeling-lesson-3-1/use-this-version-data-modeling-lesson-3-1.png)

## 4.5. CQL - Cassandra Query Language

Cassandra query language is the way to interact with the database and is very similar to SQL. The following are **not** supported by CQL: JOINS, GROUP BY, Subqueries

## 4.6. Primary key

- Must be unique
- The PRIMARY KEY is made up of either just the PARTITION KEY or may also include additional CLUSTERING COLUMNS
- A Simple PRIMARY KEY is just one column that is also the PARTITION KEY. A Composite PRIMARY KEY is made up of more than one column and will assist in creating a unique value and in your retrieval queries
- The PARTITION KEY will determine the distribution of data across the system

[Difference between partition key, composite key and clustering key in Cassandra?](https://stackoverflow.com/questions/24949676/difference-between-partition-key-composite-key-and-clustering-key-in-cassandra)

[Partition Key vs Composite Key vs Clustering Columns in Cassandra](https://www.bmc.com/blogs/cassandra-clustering-columns-partition-composite-key/)

[Basic Rules of Cassandra Data Modeling | Datastax](https://www.datastax.com/blog/basic-rules-cassandra-data-modeling)