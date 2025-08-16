# Azure Data Warehouse Technologies

# 1. Introduction

![Untitled](Azure%20Data%20Warehouse%20Technologies%2067fc6654105d4ef18603d3a65b5e2721/Untitled.png)

- **Data source** is located on-premises or cloud-based.
- **Data ingestion** is often accomplished using **blob storage** or similar technologies.
- **Data storage** can be accomplished using the warehouse platform, **Azure Synapse**.
- **Data analysis** can utilize **Azure Analysis Services**.
- Finally, **data visualization** is made possible using **Microsoft Power BI**.

# **2. Data Warehouse Analytics Solutions**

![Untitled](Azure%20Data%20Warehouse%20Technologies%2067fc6654105d4ef18603d3a65b5e2721/Untitled%201.png)

![Untitled](Azure%20Data%20Warehouse%20Technologies%2067fc6654105d4ef18603d3a65b5e2721/Untitled%202.png)

- Azure Synapse for comprehensive, integrated data warehousing and analytics
    - Synapse integrates with a broad range of data storage and pipelines technologies within the Azure environment
    - Synapse also provides analytics capabilities on top of the selected architecture

- Azure Databricks for analytics built on Apache Spark
    - Microsoft Azure provides Databricks optimized for that Azure environment
    - There are 3 flavors of Databricks optimized in turn for:
        - SQL
        - Data Science and Engineering
        - Machine Learning

# 3. **Azure Data Warehouse Components**

## **3.1. Cloud Data Storage**

| Purpose | Product |
| --- | --- |
| Traditional data warehouse architectures | Azure Data Warehouse Gen 2 |
| Relational data storage | Azure Dedicated SQL Pools |
| Column-oriented or document databases | CosmoDB for NoSQL |
| File-based storage | Blob storage |

## **3.2. ETL / ELT Pipelines**

| Purpose | Product |
| --- | --- |
| Data integrations and data flows | Azure Data Factory |
| ETL pipelines with Spark | Azure Databricks |
| Query blob storage with TSQL | Azure Polybase |

# 4. When To Use Azure DWH Components

Azure Data Warehouse components are similar to offerings by both AWS and Google. 

You should use Azure Data Warehouse components to build a data warehouse solution when:

- The data infrastructure already contains Microsoft technologies such as Microsoft SQL Server
- An on-premise solution needs to be moved to the cloud for scaling
- You have large amounts of data that need an ELT solution to quickly ingest data from a wide variety of sources