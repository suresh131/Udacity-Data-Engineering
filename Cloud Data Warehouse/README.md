In this project we are going to use two Amazon Web Services, S3 (Data storage)
and Redshift (Data warehouse with columnar storage)

Data sources are provided by two public S3 buckets. One bucket contains info about songs and artists, the second has info concerning actions doneby users (which song are listening, etc.. ). The objects contained in both bucketsare JSON files. The song bucket has all the files under the same directory butthe event ones don't, so we need a descriptor file (also a JSON) in order to extract data from the folders by path. We used a descriptor file because we don't have a common prefix on folders.

The Redshift service is where data will be ingested and transformed,in fact though COPY command we will access to the JSON files inside the buckets and copy their content on our staging tables

Schema and Table Details:

To represent this context a Star schema has been used

The songplays table is the core of this schema, is it our fact table and
it contains foreign keys to four tables;

start_time REFERENCES time(start_time)
user_id REFERENCES time(start_time)
song_id REFERENCES songs(song_id)
artist_id REFERENCES artists(artist_id)
There are also two staging tables; One for song dataset and one for
and one for event dataset

ETL process
In this project most of ETL is done with SQL (Python used just as bridge), transformation and data normalization is done by Query, check out the sql_queries python module


This will create our tables, this must be runned first
python create_tables.py

And this will execute our ETL process
python etl.py

Project structure

create_tables.py - This script will drop old tables (if exist) ad re-create new tables
etl.py - This script executes the queries that extract JSON data from the S3 bucket and ingest them to Redshift
sql_queries.py - This file contains variables with SQL statement in String formats, partitioned by CREATE, DROP, COPY and INSERT statements
dhw.cfg - Configuration file used that contains info about Redshift, IAM and S3

