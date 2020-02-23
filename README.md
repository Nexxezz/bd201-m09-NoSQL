# bd201-m09-NoSQL
  * Please download the file  
    * download cassandra docker container:  
    `docker pull cassandra`
    * run cassandra container:
    `docker run --name cassandra --network some-network -d cassandra`
    * copy labwork files to container:
    `docer cp /home/labwork/* cassandra:/home/`  
  * Create a Keyspace for KillrVideo
    * use command `CREATE KEYSPACE killrvideo`
  * Create a table to store video metadata 
    * go to videos.csv directory:  
       `/home/labwork/exersise-2`
    * run cqlsh with `cqlsh` command 
    * use killrvideo keyspace:
      * `use killrvideo;` 
    * create table schema for videos:  
      * `CREATE TABLE videos(
      video_id timeuuid PRIMARY KEY,
      added_date timestamp,
      desription text, 
      title text, user_id uuid);`
    * `SELECT * FROM videos LIMIT 10;`  
    * `SELECT COUNT(*) FROM videos;`
    * `SELECT * FROM videos WHERE video_id=6c4cffb9-0dc4-1d59-af24-c960b5fc3652;`  
    * `TRUNCATE videos;`
  * Load the data for the video table from a CSV file
    * `COPY videos FROM 'videos.csv' WITH HEADER=true;`
  * Create a new table that allows querying videos by title and year using a composite partition key
    * `CREATE TABLE videos_by_title_year(
     title text, 
     added_year int ,
     added_date timestamp, 
     desription text, 
     user_id uuid, 
     video_id uuid ,
     PRIMARY KEY (title,added_year));`  
    * `COPY videos_by_title_year FROM 'videos_by_title_year.csv' WITH HEADER=true;`
    * `SELECT * from videos_by_title_year LIMIT 10;`
    * `SELECT * FROM videos_by_title_year WHERE added_year =2015;`     