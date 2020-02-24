# bd201-m09-NoSQL
  * Please download the file  
    * download file `https://courses.epam.com/asset-v1:EPAM+201BD+2019.1+type@asset+block/NoSQL_labwork.7z`
    * download cassandra docker container:  
    `docker pull cassandra`
    * run cassandra container:
    `docker run --name cassandra --network some-network -d cassandra`
    * copy labwork files from downloaded archive to container:
    `docer cp /home/labwork/* cassandra:/home/`  
  * Create a Keyspace for KillrVideo
    * `CREATE KEYSPACE killrvideo 
    WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 1 };` (screenshot 1)
  * Create a table to store video metadata 
    * run cqlsh with `cqlsh` command 
    * use killrvideo keyspace:
      * `USE killrvideo;` (screenshot 1)
    * create table schema for videos:  
      * `CREATE TABLE videos(
      video_id TIMEUUID PRIMARY KEY,
      added_date TIMESTAMP,
      desription TEXT, 
      title TEXT, user_id UUID);` (screenshot 2)
  * Load the data for the video table from a CSV file
    * `COPY videos FROM 'videos.csv' WITH HEADER = TRUE;` (screenshot 2)
    * query table  
      * `SELECT * FROM videos LIMIT 10;` (screenshot 3) 
      * `SELECT COUNT(*) FROM videos;` (screenshot 4)
      * `SELECT * FROM videos WHERE video_id=6c4cffb9-0dc4-1d59-af24-c960b5fc3652;` (screenshot 5)
    * delete data from table  
      * `TRUNCATE videos;` (screenshot 6)
  * Create a new table that allows querying videos by title and year using a composite partition key
    * `CREATE TABLE videos_by_title_year(
     title TEXT, 
     added_year INT ,
     added_date TIMESTAMP, 
     desription TEXT, 
     user_id UUID, 
     video_id UUID,
     PRIMARY KEY (title,added_year));` (screenshot 7)
    * load the data from csv file  
      * `COPY videos_by_title_year FROM 'videos_by_title_year.csv' WITH HEADER = TRUE;` (screenshot 7)
    * query table  
      * `SELECT * from videos_by_title_year LIMIT 10;` (screenshot 8)
      * `SELECT * FROM videos_by_title_year WHERE added_year = 2015;` (screenshot 9)  
  * Create a 'videos_by_tag_year' table that allows range scans and ordering by year  
    * `CREATE TABLE videos_by_tag_year( 
        tag TEXT, 
        added_year INT, 
        video_id TIMEUUID, 
        added_date TIMESTAMP, 
        description TEXT, 
        title TEXT, 
        user_id UUID,  
        PRIMARY KEY(tag,added_year)) 
        WITH CLUSTERING ORDER BY(added_year DESC);` (screenshot 10) 
    * `COPY videos_by_tag_year FROM 'videos_by_tag_year.csv' WITH HEADER = TRUE;` (screenshot 11)
  * Create a user defined type  
    * `CREATE TYPE video_encoding ( 
    bit_rates SET<TEXT>, 
    encoding TEXT, 
    height INT, 
    width INT,);` (screehsot 12)
  * Alter an existing table and add additional columns
    * add columns 'tag' and 'video_ecoding' to videos table  
      * `ALTER TABLE videos ADD tag TEXT` (screenshot 13)
      * `ALTER TABLE videos ADD encoding video_encoding;` (screesnhot 13)
    * load the data from videos_encoding.csv
      * `COPY videos(video_id,encoding) FROM 'videos_encoding.csv' WITH HEADER = TRUE;` (screenshot 14)
    * query resulting table
      * `SELECT * FROM videos LIMIT 2;` (screenshot 14)
       