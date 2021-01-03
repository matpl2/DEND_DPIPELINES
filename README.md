# DEND Data Pipelines Project
This file is a documentation file for the exercise performed as a part of Data Engineer NanoDegree Project - Data Pipelines. 

Purpose of this project is to create high grade data pipelines that are dynamic and built from reusable tasks, can be monitored, and allow easy backfills. These pipelines should also ensure the data quality check.

## Files
This project includes following files:
 - Create_tables.sql
 - Dags folder with dag.py and load_dimension_table_subdag.py
 - Helpers folder (under plugins) with data_quality_tests.py and sql_queries.py
 - Operators folder (under plugins) with  data_quality.py, load_dimension.py, load_fact.py and stage_redshift.py


Create_tables.sql is a sql file that stores SQL code to create tables.
Dags folder consist 2 dags (main and subdag).
Helpers and Operators folder support dag execution.


## Data sets
There are two datasets used in this project that reside in S3. Here are the S3 links for each:

Song data: s3://udacity-dend/song_data
Log data: s3://udacity-dend/log_data

Song Dataset
The first dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. 

Log Dataset
The second dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate app activity logs from an imaginary music streaming app based on configuration settings.

## Redshift instance
For the purpose of this project it is necessary to create Redshift database. Database creadentials, and server paths should be stored in dwh.cfg file. For my work I have created free trier 1 node Redshift instance that run in defalut VPC. In such config it was necessary to configure VPC components to get access to Redshift(editing Security group for public access, attaching subnets to IGW). 

Redshift instance used in this excercise:
![alt text](https://github.com/matpl2/DEND_Datawarehouse/blob/main/picts/redshift.png)

## Tables
Using the song and event datasets, I have created a star schema optimized for queries on song play analysis. This includes the following tables.

Fact Table
1. songplays - records in event data associated with song plays i.e. records with page NextSong
   * songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent
   
Dimension Tables
1. users - users in the app
   * user_id, first_name, last_name, gender, level
   
2. songs - songs in music database
   * song_id, title, artist_id, year, duration
   
3. artists - artists in music database
    * artist_id, name, location, lattitude, longitude
    
4. time - timestamps of records in songplays broken down into specific units
    * start_time, hour, day, week, month, year, weekday
    
This star schema would be the most optimal for the analytics team. Also, because orginal dataset is in S3 the most efficient way for ETL process is to create first staging tables and use COPY command to load them. Once staging tables are created then core tables are crated with Insert stament.

## Dends in Airflow
Dend designed for this excercise can be described with following graph:

![alt text](https://github.com/matpl2/DEND_DPIPELINES/blob/main/picts/example-dag.png)

In this excercise it was optimized by placing subdag:


![alt text](https://github.com/matpl2/DEND_DPIPELINES/blob/main/picts/subdag.png)

