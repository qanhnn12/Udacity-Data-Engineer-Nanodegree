# Building an Azure Data Warehouse for Bike Share Data Analytics

# Project Overview

Divvy is a bike sharing program in Chicago, Illinois USA that allows riders to purchase a pass at a kiosk or use a mobile application to unlock a bike at stations around the city and use the bike for a specified amount of time. The bikes can be returned to the same station or to another station. The City of Chicago makes the anonymized bike trip data publicly available for projects like this where we can analyze the data.

The Relational ERD for the Divvy Bikeshare Dataset at the beginning can be seen as below: 

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled.png)

The goal of this project is to develop a data warehouse solution using Azure Synapse Analytics. 

- Design a star schema based on the business outcomes listed below;
- Import the data into Synapse;
- Transform the data into the star schema;
- and finally, view the reports from Analytics.

# Datasets

Python script to load data into a PostgreSQL database on Azure to simulate OLTP data source: [LINK](https://github.com/udacity/Azure-Data-Warehouse-Project/tree/main/starter)

Divvy Bikeshare Dataset: [LINK](https://video.udacity-data.com/topher/2022/March/622a5fc6_azure-data-warehouse-projectdatafiles/azure-data-warehouse-projectdatafiles.zip)

# **Business Outcomes**

1. Analyze how much time is spent per ride
    - Based on date and time factors such as day of week and time of day
    - Based on which station is the starting and / or ending station
    - Based on age of the rider at time of the ride
    - Based on whether the rider is a member or a casual rider
2. Analyze how much money is spent
    - Per month, quarter, year
    - Per member, based on the age of the rider at account start
3. Analyze how much money is spent per member
    - Based on how many rides the rider averages per month
    - Based on how many minutes the rider spends on a bike per month

# Build the Data Warehouse step by step

## Task 1: **Create your Azure resources**

### 1.1 Create an Azure Database for PostgreSQL

On the **Basics** tab, create a ***Server name***, choose ***Workload type*** as *Development* since this is just a small project. 

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%201.png)

Then under ***Authentication***, fill in *Admin username*, *Password* and *Confirm password*.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%202.png)

On the **Networking** tab

- Under the ***Public Access***, remember to tick on *Allow public access this resource through this resource through the internet using a public IP address*.
- Under ***Firewall rules***, tick on *Allow public access from any Azure service within Azure to this server* and click on *Add current client IP address.*

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%203.png)

Review all the information and hit the Create button.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%204.png)

### 1.2 Create an Azure Synapse workspace

On the **Basics** tab, create the ***Workspace name***, set the ***Region*** as the PostgreSQL Database above (here I choose *East US*), then fill information for ***Data Lake Storage Gen2***.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%205.png)

On the Security tab, fill in information for ***Authentication***.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%206.png)

Review all necessary information and hit the **Create** button.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%207.png)

## **Task 2: Design a star schema**

After reading the Business Outcomes, I decided to create 2 facts tables: `fact_payment`, `fact_trip` and 3 dimension tables: `dim_date`, `dim_rider`, and `dim_station` as shown in the image below.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%208.png)

Noticed that I have added a few date columns as integers for the ease of join in data warehouse. They are:

- Table `fact_payment`: column `date_id`
- Table `fact_trip`: column `started_at_id` and `ended_at_id`. I keep column `started_at` and column `ended_at` as `VARCHAR` because they have time part, but Azure Synapse Analytics doesn’t allow us to cast them to `DATETIME`.
- Table `dim_date`: column `date_key`.
    
    Azure Synapse Analytics doesn’t support recursive CTEs like in SQL Server, I have to create the `dim_date` by using the `staging_payment` table, so the `date_key` here is not quite correct. In practice, we only need a unique `date_id` as ‘YYYYMMDD’ type in `dim_date` table.
    

## Task 3: **Create the data in PostgreSQL**

### 3.1 Download and run a Python script

On the Azure Portal, go the Azure Database for PostgreSQL, under Settings, click on Connect, you can see all the connection details

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%209.png)

Then go to **Visual Studio Code**, create a folder to store your script and copy the **Host, User and Password** based on ***Connection Details*** as shown on ******the image above. Noticed that the Password for PostgreSQL Database is the one you set under the **Authentication** field on step 1.1.

Also, modify the filename path for each csv file.

After running the Python Script, we’ve successfully copied data from csv files to PostgreSQL. 

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2010.png)

In this step, if you receive an error "p*ort 5432 failed: Connection timed out (0x0000274C/10060). Is the server running on that host and accepting TCP/IP connections?”*, check whether your PostgreSQL port is set in another port (watch the video guide [HERE](https://www.youtube.com/watch?v=jNbCH--WPLQ&ab_channel=OnlineStudyForCS)). By default, it should be port 5432.

If it is set in port 5432, but you still receive errors, on the Azure Portal, go to your **PostgreSQL database**, under **Settings**, click on **Networking**. Under **Firewall rule,** *Add your current public IP address*. You can find your current IP address by simply searching “What is my public IP address?” on Google. (This is actually what we’ve done in step 1.1 above)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2011.png)

### 3.2. Verify that data are copied/uploaded into PostgreSQL

Open **pgAdmin 4**, register a server, fill in all the connection details as shown on the Connect, tab pgAdmin 4 on the Portal.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2012.png)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2013.png)

We see that 4 tables has been there.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2014.png)

## Task 4: EXTRACT the data from PostgreSQL

Go to the Azure Portal, click on Azure Synapse Analytics we’ve created above, open Synapse Studio.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2015.png)

### 4.1. Create 2 Linked Services

Go to the Manage tab at the bottom on the left, let’s create Linked Services to our Azure Database for PostgreSQL and Azure Blob Storage.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2016.png)

Let’s create a Linked Service to our **Azure Database for PostgreSQL**. Fill in all the information as below. Noticed that the ***Database name*** is `udacityproject`, ***User name*** and ***Password*** are ones we’ve set for PostgreSQL Database. Then ***Test connection*** and hit the ***Create*** button.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2017.png)

Next, create a Linked Service to the **Azure Blob Storage.**

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2018.png)

The result would look like the image below.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2019.png)

Then, go to the Data tab (second button on the left), under tab **Linked**, you’ll see that we’ve created 2 linked services there.

### 4.2 Ingest data

From Home, click on Ingest button, then follow each step as below.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2020.png)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2021.png)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2022.png)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2023.png)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2024.png)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2025.png)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2026.png)

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2027.png)

On the Data tab, you will see 4 files available on the Blob Storage.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2028.png)

## Task 5: LOAD the data into external tables in the data warehouse

When you start the SQL script wizard to LOAD data into external tables, start the wizard from the data lake node, not the blob storage node.

So under Azure Data Lake Storage Gen2, click on each csv file and start generate the CREATE EXTERNAL TABLE scripts. 

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2029.png)

We connect to Built-in SQL pool since it is required (in reality, we may have link to a Dedicated SQL Pool). Remember to create a SQL Database if you haven’t, then fill in the external table name.  

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2030.png)

Continue doing similarly for other 3 tables. Noticed that we have to use CETAS instead of normal CREATE TABLE scripts. You can find my CETAS scripts to create 4 staging tables HERE.

The end result would be 4 staging tables in the SQL Database. The output is the same as we’ve seen in pgAdmin 4.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2031.png)

## **Task 6: TRANSFORM the data to the star schema using CETAS**

The serverless SQL pool won't allow us to create persistent tables in the database, as it has no local storage. So, use **CREATE EXTERNAL TABLE AS SELECT (CETAS)** instead. CETAS is a parallel operation that creates external table metadata and exports the SELECT query results to a set of files in your storage account.

Using 5 scripts HERE, we can create 3 dimension tables and 2 fact tables as below.

- Table `fact_payment`

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2032.png)

- Table `fact_trip`

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2033.png)

- Table `dim_date`

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2034.png)

- Table `dim_rider`

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2035.png)

- Table `dim_station`

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2036.png)

Confirm the number of rows as the same as Task 3 and Task 5, except `dim_date`.

![Untitled](Building%20an%20Azure%20Data%20Warehouse%20for%20Bike%20Share%20Da%200ca8f336fce84c50a7910313961603a2/Untitled%2037.png)