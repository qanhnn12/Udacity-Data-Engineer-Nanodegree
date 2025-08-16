# ELT and Data Warehouse Technology in the Cloud

# 1. Introduction

- Database storage technologies for ingesting data as well as making it available to analytics consumers.
- Data pipeline technologies to move data from source to warehouse, as well as between the stages of the Extract, Transform and Load (ETL) processes.
- End-to-end data warehouse solution that provides the ability to manage the various parts of a data warehouse from a single application.

# 2. From ETL to ELT

- ETL: happens on an intermediate server
- ELT: happens on the destination server

This means rather than loading data directly into the final format of the destination data warehouse, data are loaded into the destination as either raw data or staging tables (or sometimes both). Only after loading is transformation performed.

## Benefits of ELT

- **Scalability** - massive amounts of data can be loaded into the data warehouse environment with relative ease
- **Flexibility** - the Transform step takes place using the same tech stack as the data warehouse runs on allowing for more flexibility in the process as business needs change.
- **Cost shifting** - the Transform step is often the most costly and by doing it last, Data Engineers can perform Just In Time transformations to meet the highest priority business needs first
- Better performance for large datasets
- More flexibility for unstructured (NoSQL) datasets

# 3. Cloud Managed SQL Storage

Data Warehouses in the cloud leverage many of the same SQL style, relational databases that are used for OLTP systems.

- Oracle
- Microsoft SQL Server
- PostgreSQL
- MySQL
- MariaDB

The major cloud providers provide all of these databases as managed databases meaning the user doesn't have to manage the hardware resources to gain optimal performance.

- Microsoft Azure
    - Azure SQL Database (MS SQL Server)
    - Azure Database for MySQL
    - Azure Database for MariaDB
    - Azure Database for PostgreSQL
- GCP
    - Cloud SQL (MySQL, PostgreSQL, and MS SQL Server)
- AWS
    - Amazon RDS (MySQL, PostgreSQL, MariaDB, Oracle, MS SQL Server)

# 5. Cloud Managed NoSQL Storage

ELT makes it easier to use many NoSQL database management systems in Data Warehousing scenarios. These database come in many flavors such as:

- Key value
- Document
- Column oriented
- Graph
- Time series

Each of the major cloud providers offers a variety of managed NoSQL databases:

- Azure - CosmosDB
    - Gremlin - graph database
    - MongoDB - document
    - Cassandra - column oriented
- GCP
    - Big Table - column oriented
    - Firestore - document
    - MongoDB Atlas - document
- AWS
    - DynamoDB - Key value
    - DocumentDB - document
    - Keyspaces = column oriented
    - Neptune - graph
    - Time stream - time series

# 6. Cloud ETL Pipeline Services

ETL / ELT processes rely on data pipelines often built using cloud-based tools.

Major Cloud providers:

- Azure Data Factory
- AWS Glue
- GCP Dataflow

In addition to these tools, a large number of companies offer cloud-based tools for solving ETL / ELT challenges. Some of the major tool providers in this space are:

- Informatica
- Talend
- Xplenty
- Matillion

One advantage of doing ELT over ETL is the ability to load large amounts of data quickly. One excellent example of this is ingesting streaming data. 

In modern architectures, this streaming data is often coming from Internet of Things (IoT) devices; however, it could be coming from more traditional sources such as server or transaction logs.

Each of the major cloud platforms has offering for ingesting large amounts of streaming data:

- Azure - Streaming Analytics
- AWS - Kinesis
- GCP - Dataflow

# 7. Cloud Data Warehouse Solutions

Modern cloud data warehouse solutions seamlessly combine elements from Cloud Storage and Cloud Pipelines with powerful analytics capabilities. Each of the three major cloud providers has its own flavor of Cloud Data Warehouse that works seamlessly with its other cloud data engineering offerings.

- Azure Synapse
- Amazon Redshift
- GCP Big Query

[ETL vs ELT: Key Differences, Comparisons, & Use Cases](https://rivery.io/blog/etl-vs-elt/)