# Data Modeling with Apache Cassandra

Python program that that models data by creating tables in Apache Cassandra, a NoSQL database, to run queries. 

The aim is to create queries on song play data to analyze data collected by Sparkify with the intetion to understand what songs the users are listenign to.

## Required Libraries

1. pandas [https://pandas.pydata.org/]
2. cassandra [https://datastax.github.io/python-driver/installation.html]
3. numpy [https://www.numpy.org/]
   
### Installation

1. Ensure that Apache Cassandra is installed on the computer [(http://cassandra.apache.org/)]
2. Ensure that Java SE Development Kit 8 is installed on the computer [(https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)]
3. Ensure that the Cassandra-Driver Python module is installed [(https://datastax.github.io/python-driver/installation.html)]

## Files

- event_data: Folder containing all CSV files
- images: Folder containing related images
- Project_1B_FINAL.ipynb: Program file that runs the queries on Apache Cassandra database
- event_datafile_new.csv: File is generated after running **CREATE CSV FILE** code section of *Project_1B_FINAL.ipynb*

## Running the tests

1. Run cassandra.bat of Apache Cassandra installation
2. Run cqlsh of Apache Cassandra installation
3. Run Project_1B_FINAL.ipynb

# CSV Data File

## The event_datafile_new.csv contains the following columns (LINE\[n\]): 
#### 0. artist \[artist\]
#### 1. firstName of user \[firstname\]
#### 2. gender of user \[gender\]
#### 3. item number in session \[iteminsession\]
#### 4. last name of user \[lastname\]
#### 5. length of the song \[length\]
#### 6. level (paid or free song) \[level\]
#### 7. location of the user \[location\]
#### 8. sessionId \[sessionid\]
#### 9. song title \[song\]
#### 10. userId \[userid\]

## The image below is a screenshot of the denormalized data in event_datafile_new.csv after the code above is run:
<img src="images/image_event_datafile_new.jpg">

# Database Overview

## Query 1: Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4

### _song\_session\_hist_ Table

| col_name      | data_type | key     | key_type  |
|---------------|-----------|---------|-----------|
| sessionid     | int       | primary | partition |
| iteminsession | int       | primary | cluster   |
| artist        | text      | -       | -         |
| song          | text      | -       | -         |
| length        | float     | -       | -         |

### SELECT artist, song, length FROM song_session_hist WHERE sessionid = 338 AND iteminsession=4
    Row 1: Artist->Ava; Song->Music Matters (Mark Knight Dub); length->495.307312


## Query 2: Give me only the following: name of artist, song (sorted by iteminsession) and user (first and last name) for userid = 10, sessionid = 182

### _user\_session\_hist Table

| col_name      | data_type | key     | key_type  |
|---------------|-----------|---------|-----------|
| userid        | int       | primary | partition |
| sessionid     | int       | primary | partition |
| iteminsession | text      | primary | cluster   |
| artist        | text      | -       | -         |
| song          | text      | -       | -         |
| firstname     | text      | -       | -         |
| lastname      | text      | -       | -         |

### SELECT artist, song, firstname, lastname FROM user_session_hist WHERE userid=10 AND sessionid=182
    Row 1: Artist->Down To The Bone; Song->Keep On Keepin' On; firstname->Sylvie; lastname->Cruz
    Row 2: Artist->Three Drives; Song->Greece 2000; firstname->Sylvie; lastname->Cruz
    Row 3: Artist->Sebastien Tellier; Song->Kilometer; firstname->Sylvie; lastname->Cruz
    Row 4: Artist->Lonnie Gordon; Song->Catch You Baby (Steve Pitron & Max Sanna Radio Edit); firstname->Sylvie; lastname->Cruz

## Query 3: Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

### _song\_usage\_hist Table

| col_name      | data_type | key     | key_type  |
|---------------|-----------|---------|-----------|
| song          | text      | primary | partition |
| userid        | int       | primary | partition |
| firstname     | text      | -       | -         |
| lastname      | text      | -       | -         |

### SELECT firstname, lastname FROM song_usage_hist WHERE song='All Hands Against His Own'
    Row 1: firstname->Jacqueline; lastname->Lynch
    Row 2: firstname->Tegan; lastname->Levine
    Row 3: firstname->Sara; lastname->Johnson

# Project Overview

## Built With

* [Apache Cassandra](http://cassandra.apache.org/) - The database used
* [Python](https://www.python.org/) - Programming language

## Authors

* **Jobelle Lee** - [themaxermister](https://github.com/themaxermister/Data-Modelling-with-Apache-Cassandra)
