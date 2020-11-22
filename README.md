# Project: Data Modeling with Postgres

## Introduction

A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

The purpose of this project is to create a Postgres database with tables designed to optimize queries on song play analysis. To accomplish this, I will create a database schema and ETL pipeline for analysis. The database and ETL pipeline will be tested by running queries given to myself by the analytics team from Sparkify in order to compare results.

## Database Design

The directory of JSON logs is split up into two subdirectories: <code>song_data</code> and <code>log_data</code>. <code>song_data</code> contains metadata about a song and the artist of that song. <code>log_data</code> contain activity logs based on the songs in <code>song_data</code>. In order to optimize for queries on song play analysis, I have created a star schema based on the song and log datasets. The star schema will have a fact table (<code>songplays</code>) and dimension tables (<code>users, songs, artists, time</code>). The structure of this database is outlined below (**note:** rows highlighted in red are primary keys):

![Database Schema](UdacityDataEngProjDBSchema.png)

The tables you see in the diagram above are:

<code>songplays</code>: fact table; records in log data associated with song plays
<code>users</code>: information on Sparkify users
<code>songs</code>: information on songs in the database
<code>time</code>: timestamps of records broken down into various units
<code>artists</code>: information on artists in the database

The lines between rows indicate how the tables are related to one another.

## ETL Pipeline

The notebook <code>etl.ipynb</code> explains in more detail the full ETL pipeline. In summary, we first examine the <code>song_data</code> directory in order to create the <code>songs</code> and <code>artists</code> tables. This is simply done by examining the JSON files in the <code>song_data</code> directory and selecting out the columns we want using Python.

Next, we examine the <code>log_data</code> directory to create <code>time</code>, <code>users</code>, and <code>songplays</code> tables. The <code>time</code> and <code>users</code> tables are created in a very similar way as the above tables, with data modification needed to the <code>time</code> table to ensure the datatypes are correct. The <code>songplays</code> table is created by joining the <code>artists</code> and <code>songs</code> tables in combination with the data in the <code>log_data</code> directory.

## Instructions

Run the script <code>create_tables.py</code> to create the database and tables.
Next, run the script <code>etl.py</code> which will process the entire dataset by importing the data from the JSON files and saving them to the Postgres database.