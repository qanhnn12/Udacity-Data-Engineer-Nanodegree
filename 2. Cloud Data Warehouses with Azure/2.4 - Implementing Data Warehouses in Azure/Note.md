# Implementing Data Warehouses in Azure

# 1. Azure Data Warehousing components

![Untitled](Implementing%20Data%20Warehouses%20in%20Azure%209b30737d2cbb4955bc559b8810170347/Untitled.png)

| Ingestion | Storage | Analytics |
| --- | --- | --- |
| Batch | SQL | Big Data analytics |
| Streaming | NoSQL | Log and telemetry streaming analytics |

Data warehouse solution in Azure may include all or just some of these options. 

The **pricing models** vary from the **amount of data** stored/processed to **time-based billing**.

**Benefits** of the mix and match approach:

- Pay for just what you need
- Tailor solution to business needs not the platform

**Cautions** for the mix and match approach:

- Need to understand a variety of billing models
- Must understand data and time needs for each of the components to understand the total cost for the solution

# 2. Enterprise BI Architecture

| Data Source | Ingestion | Data Storage | Analysis | Visualization |
| --- | --- | --- | --- | --- |
| On-premise and cloud (Microsoft SQL Server) | Storage blob | Azure Synapse  | Azure Analysis services | Power BI |

![Untitled](Implementing%20Data%20Warehouses%20in%20Azure%209b30737d2cbb4955bc559b8810170347/Untitled%201.png)

![Untitled](Implementing%20Data%20Warehouses%20in%20Azure%209b30737d2cbb4955bc559b8810170347/Untitled%202.png)

# 3. Ingesting Data at Scale into Azure Synapse

![Untitled](Implementing%20Data%20Warehouses%20in%20Azure%209b30737d2cbb4955bc559b8810170347/Untitled%203.png)

![Untitled](Implementing%20Data%20Warehouses%20in%20Azure%209b30737d2cbb4955bc559b8810170347/Untitled%204.png)

- Creating linked services
    - A linked service is where you define your connection information to other services
- Creating a pipeline
    - A pipeline contains the logical flow for an execution of a set of activities
- Using a trigger or a one-time data ingestion
    - You can manually start a data ingestion or you can schedule a trigger

# 4. SQL to SQL ELT in Azure

![Untitled](Implementing%20Data%20Warehouses%20in%20Azure%209b30737d2cbb4955bc559b8810170347/Untitled%205.png)

Doing a SQL to SQL ELT in Azure involves:

- Starting with data ingested into either Blob Storage or Azure Delta Lake Gen 2
    - Once the extract process has landed data into Azure Blob Storage, SQL can be used for the final two steps: load and transform.
- Create EXTERNAL staging tables in the Data Warehouse
    - For the load step, MS SQL Server Polybase provides a fast, highly efficient method for loading data from Azure Blob Storage into staging tables created in Azure Dedicated SQL Pools as external tables.
- Transform data from staging tables to DW tables
    - When transforming data, MS SQL Server T-SQL can be used again in Azure Dedicated SQL Pools to transform data from staging table schemas to the final data warehouse schema.

## 4.1. Dedicated or Serverless SQL Pools?

In Azure Synapse Analytics, there are two types of SQL pools to choose from: dedicated SQL pools and serverless SQL pools. You can use either for ELT pipeline processes and staging data. They offer different functionalities, performance characteristics, and pricing models.

- Serverless SQL Pools are created when you create a Synapse Analytics resource in Azure.
- Dedicated SQL Pools have to be created as a separate resource.

### **4.1.1. Azure Dedicated SQL Pools**

Dedicated SQL pools are designed to provide a high-performance, scalable, and cost-effective solution for big data workloads. 

They utilize a Massively Parallel Processing (MPP) architecture. This architecture enables users to perform queries faster, especially for complex analytical queries. 

Dedicated SQL pools are provisioned with a fixed amount of resources and are billed based on the resources allocated, regardless of usage.

> In a Synapse Analytics production data warehouse, you likely would use Dedicated SQL Pools.
> 

### **4.1.2. Azure Synapse Analytics Serverless SQL Pools**

Serverless SQL pools provide a pay-per-query model, which means you only pay for the resources used by each executed query. They are designed to handle both small and large-scale data processing tasks and can automatically scale resources based on workload requirements. Serverless SQL pools do not require any upfront provisioning or resource allocation.

> For development workloads, ad-hoc querying, or volatile workloads, use serverless SQL pools.
> 

[Extract, transform, load (ETL) - Azure Architecture Center](https://docs.microsoft.com/en-us/azure/architecture/data-guide/relational-data/etl)

```sql
IF NOT EXISTS (SELECT * FROM sys.external_file_formats 
												WHERE name = 'QuotedCsvWithHeaderFormat') 
	CREATE EXTERNAL FILE FORMAT [QuotedCsvWithHeaderFormat] 
	WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS ( FIELD_TERMINATOR = ',', STRING_DELIMITER = '"', FIRST_ROW = 2   )
)
GO

IF NOT EXISTS (SELECT * FROM sys.external_data_sources 
								WHERE name = 'udacitylearning_udacitylearning_dfs_core_windows_net') 
	CREATE EXTERNAL DATA SOURCE [udacitylearning_udacitylearning_dfs_core_windows_net] 
	WITH (
		LOCATION = 'abfss://udacitylearning@udacitylearning.dfs.core.windows.net' 
	)
GO

CREATE EXTERNAL TABLE dbo.staging_accidents (
	[ID] nvarchar(4000),
	[Severity] nvarchar(4000),
	[Start_Time] nvarchar(4000),
	[End_Time] nvarchar(4000),
	[Start_Lat] nvarchar(4000),
	[Start_Lng] nvarchar(4000),
	[End_Lat] nvarchar(4000),
	[End_Lng] nvarchar(4000),
	[Distance(mi)] nvarchar(4000),
	[Description] nvarchar(4000),
	[Number] nvarchar(4000),
	[Street] nvarchar(4000),
	[Side] nvarchar(4000),
	[City] nvarchar(4000),
	[County] nvarchar(4000),
	[State] nvarchar(4000),
	[Zipcode] nvarchar(4000),
	[Country] nvarchar(4000),
	[Timezone] nvarchar(4000),
	[Airport_Code] nvarchar(4000),
	[Weather_Timestamp] nvarchar(4000),
	[Temperature(F)] nvarchar(4000),
	[Wind_Chill(F)] nvarchar(4000),
	[Humidity(%)] nvarchar(4000),
	[Pressure(in)] nvarchar(4000),
	[Visibility(mi)] nvarchar(4000),
	[Wind_Direction] nvarchar(4000),
	[Wind_Speed(mph)] nvarchar(4000),
	[Precipitation(in)] nvarchar(4000),
	[Weather_Condition] nvarchar(4000),
	[Amenity] nvarchar(4000),
	[Bump] nvarchar(4000),
	[Crossing] nvarchar(4000),
	[Give_Way] nvarchar(4000),
	[Junction] nvarchar(4000),
	[No_Exit] nvarchar(4000),
	[Railway] nvarchar(4000),
	[Roundabout] nvarchar(4000),
	[Station] nvarchar(4000),
	[Stop] nvarchar(4000),
	[Traffic_Calming] nvarchar(4000),
	[Traffic_Signal] nvarchar(4000),
	[Turning_Loop] nvarchar(4000),
	[Sunrise_Sunset] nvarchar(4000),
	[Civil_Twilight] nvarchar(4000),
	[Nautical_Twilight] nvarchar(4000),
	[Astronomical_Twilight] nvarchar(4000)
	)
	WITH (
	LOCATION = 'us_accidents_small.csv',
	DATA_SOURCE = [udacitylearning_udacitylearning_dfs_core_windows_net],
	FILE_FORMAT = [QuotedCsvWithHeaderFormat]
	)
GO

SELECT TOP 100 * FROM dbo.staging_accidents
GO
```

```sql
-- Use CETAS to export select statement
IF OBJECT_ID('dbo.fact_accident_dates') IS NOT NULL
BEGIN
  DROP EXTERNAL TABLE [dbo].[fact_accident_dates];
END

CREATE EXTERNAL TABLE dbo.fact_accident_dates
WITH (
    LOCATION    = 'fact_accident_dates',
    DATA_SOURCE = [udacitylearning_udacitylearning_dfs_core_windows_net],
	FILE_FORMAT = [SynapseDelimitedTextFormat]
)  
AS
SELECT sa.[ID] AS AccidentId, sa.[Start_Time] AS AccidentDate
FROM [dbo].[staging_accidents] sa;
GO

-- Query the newly created CETAS external table
SELECT TOP 100 * FROM dbo.fact_accident_dates
GO
```