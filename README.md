# Building an Azure Data Warehouse for Bike Share Data Analytics

## Summary
***
- [Introduction](#intro)
- [Project Description](#descript)
- [Project Steps](#phases)
- [Data](#data)
  - [Song Dataset](#song)
  - [Log Dataset](#log)
- [Project content](#content)
- [Schema for Song Play Analysis](#schema)
  - [Fact Table](#fact)
  - [Dimension Tables](#dim)
- [How to use](#usage)
  - [Requirements](#reg)
  - [Execution](#exec)
- [Conclusion](#end)
***

## Introduction <a name="intro"></a>

Divvy is a bike sharing program in Chicago, Illinois USA that allows riders to purchase a pass at a kiosk or use a mobile application to unlock a bike at stations around the city and use the bike for a specified amount of time. The bikes can be returned to the same station or to another station. The City of Chicago makes the anonymized bike trip data publicly available for projects like this where we can analyze the data.

Since the data from Divvy are anonymous, we have created fake rider and account profiles along with fake payment data to go along with the data from Divvy.

## Project Description <a name="descript"></a>

The goal of this project is to develop a data warehouse solution using Azure Synapse Analytics. We will:

Design a star schema based on the business outcomes listed below;
- *Import the data into Synapse;*
- *Transform the data into the star schema; and finally, view the reports from Analytics.*

The business outcomes we are designing for are as follows:

**Analyze how much time is spent per ride**
> Based on date and time factors such as day of week and time of day

> Based on which station is the starting and / or ending station

> Based on age of the rider at time of the ride

> Based on whether the rider is a member or a casual rider

**Analyze how much money is spent**
> Per month, quarter, year

> Per member, based on the age of the rider at account start

**EXTRA CREDIT - Analyze how much money is spent per member**
> Based on how many rides the rider averages per month

> Based on how many minutes the rider spends on a bike per month
 
 ## Project Steps <a name="phases"></a>
 
<p align="center">
  <img src="https://github.com/Gutelvam/Azure-Data-Warehouse/blob/main/imgs/steps.png?raw=true">  <br>
  <i>Project steps</i>
</p>

 
**1 - Create your Azure resources**
- Create an Azure PostgreSQL database
- Create an Azure Synapse workspace
- Create a Dedicated SQL Pool and database within the Synapse workspace

**2 - Design a Star Schema**

- With a set of business requirements related to the data warehouse. We are being asked to design a star schema using fact and dimension tables.

**3 - Ingest data in PostgreSQL**

- This can be done using the Python script provided for you in [Github: ProjectDataToPostgres.py](https://github.com/udacity/Azure-Data-Warehouse-Project/tree/main/starter), just change authentication credential to connect in database, and run the python script provided and the data will be inserted at postgreSQL created.

**4 - Extract Data from PostgreSQL and ingest into Azure Blob Storage**

- In your Azure Synapse workspace, we have to use the ingest wizard to create a one-time pipeline that ingests the data from PostgreSQL into Azure Blob Storage. This will result in all four tables being represented as text files in Blob Storage, ready for loading into the data warehouse.

**5 - Load Data into External tables in the Data Warehouse**
- Once in Blob storage, the files will be shown in the data lake node in the Synapse Workspace. From here, we use the script generating function to load the data from blob storage into external staging tables in the data warehouse you created using the Dedicated SQL Pool.

**6 - Transform the Data into Star Schema**

- Last step is to write SQL scripts to transform the data from the staging tables to the final star schema you designed.

## Data <a name="data"></a>
The data used for this project was provided from bike sharing program in Chicago, it's open data that was enhriched with fake data, to complete the ERD diagram below:

<p align="center">
  <img src="https://github.com/Gutelvam/Azure-Data-Warehouse/blob/main/imgs/divvy-erd.png?raw=true"><br>
  <i>Data Diagram</i>
</p>


#### Song Dataset  <a name="song"></a>

The first dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are file paths to two files in this dataset.
> song_data/A/B/C/TRABCEI128F424C983.json
> song_data/A/A/B/TRAABJL12903CDCF1A.json

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

> { "num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

#### Log Dataset <a name="log"></a>

The second dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations.

The log files in the dataset are partitioned by year and month. For example, here are filepaths to two files in this dataset.

> log_data/2018/11/2018-11-12-events.json
> log_data/2018/11/2018-11-13-events.json

And below is an example of what the data in a log file, 2018-11-12-events.json, looks like.
![dataframe log data](https://github.com/Gutelvam/etl_with_postegres/blob/master/img/log-data.png?raw=true "Log Data")

## Project content <a name="content"></a>
There are 3 files `.py` and 2 python jupter notebooks `.ipynb`, the first three  are descripted below:

- `sql_queries.py`: here is all  DDL and DML queries used on this project
- `create_tables.py`: Execution of DDL queries
- `etl.py`: here is all process of data, get json files (raw data) > process them and insert into tables.

and the two notebooks:
- `etl.ipynb`: notebook test to build **etl** and **sql_query**  python scripts
- `test.ipynb`: Execution of test if all process is doing right

## Schema for Song Play Analysis  <a name="schema"></a>
Using the song and log datasets, you'll need to create a star schema optimized for queries on song play analysis. This includes the following tables.

##### **Fact Table**  <a name="fact"></a>
**songplays** - records in log data associated with song plays i.e. records with page NextSong
-   **songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent**
##### **Dimension Tables**  <a name="dim"></a>

**users** - users in the app
-  ***user_id, first_name, last_name, gender, level***

**songs** - songs in music database
-  ***song_id, title, artist_id, year, duration***

**artists** - artists in music database
- ***artist_id, name, location, latitude, longitude***

**time** - timestamps of records in songplays broken down into specific units
-  ***start_time, hour, day, week, month, year, weekday***

## How to  use  <a name="usage"></a>
#### Requirements  <a name="req"></a>
        - numpy==1.16.2
        - psycopg2==2.7.6.1
        - pandas==0.24.2
#### Execution  <a name="exec"></a>

For this step first you must to clone all repository and execute in that order, utilizing pyhthon in cmd `python create_tables.py` , `python etl.py`

After execution of these scripts you can use test.ipynb to run some tests and do Sanity Check of solution.

## Conclusion  <a name="end"></a>
In this project we have modeled  and create a star schema database in Postegres RDBMS from two different raw data sources in JSON format, in this process we apply the most common data engineering process: Extract, transform and load by inserting transformed data into the business significant tables that can be used in the analysis process.
