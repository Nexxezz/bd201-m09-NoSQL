# bd201-m09-NoSQL
  * Please download the file  
    * download cassandra docker container:  
    `docker pull cassandra`
    * run cassandra container:
    `docker run --name cassandra --network some-network -d cassandra`
    * copy labwork files to container:
    `docer cp /home/labwork/* cassandra:/home/`  
  * Create a Keyspace for KillrVideo
    * `CREATE KEYSPACE killrvideo 
    WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 1 };`
  * Create a table to store video metadata 
    * go to videos.csv directory:  
       `/home/labwork/exersise-2`
    * run cqlsh with `cqlsh` command 
    * use killrvideo keyspace:
      * `USE killrvideo;` 
    * create table schema for videos:  
      * `CREATE TABLE videos(
      video_id TIMEUUID PRIMARY KEY,
      added_date TIMESTAMP,
      desription TEXT, 
      title TEXT, user_id UUID);`
    * `SELECT * FROM videos LIMIT 10;`  
    * `SELECT COUNT(*) FROM videos;`
    * `SELECT * FROM videos WHERE video_id=6c4cffb9-0dc4-1d59-af24-c960b5fc3652;`  
    * `TRUNCATE videos;`
  * Load the data for the video table from a CSV file
    * `COPY videos FROM 'videos.csv' WITH HEADER=true;`
  * Create a new table that allows querying videos by title and year using a composite partition key
    * `CREATE TABLE videos_by_title_year(
     title TEXT, 
     added_year INT ,
     added_date TIMESTAMP, 
     desription TEXT, 
     user_id UUID, 
     video_id UUID,
     PRIMARY KEY (title,added_year));`  
    * `COPY videos_by_title_year FROM 'videos_by_title_year.csv' WITH HEADER=true;`
    * `SELECT * from videos_by_title_year LIMIT 10;`
    * `SELECT * FROM videos_by_title_year WHERE added_year =2015;`  
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
        WITH CLUSTERING ORDER BY(added_year DESC);`  
  * Create a user defined type  
    * `CREATE TYPE video_encoding ( 
    bit_rates SET<TEXT>, 
    encoding TEXT, 
    height INT, 
    width INT,);`
    * alter videos table(add columns 'tag' and 'video_ecoding)  
      * `ALTER TABLE videos ADD tag TEXT`
      * ` ALTER TABLE videos ADD encoding video_encoding;`
    * load the data from videos_encoding.csv
      * `COPY videos(video_id,encoding) FROM 'videos_encoding.csv' WITH HEADER = TRUE;`
    * print resulting table  
      * `SELECT * FROM videos LIMIT 10;`
       