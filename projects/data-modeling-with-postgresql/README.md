## Data Modeling with Postgres

In this project, I've created a PostgreSQL database containing information about songs and songplays from a music streaming app owned by a hypothetical startup called Sparkify. The database is in star schema design. I also create a script to perform ETL on raw songs and songplay data, transforming and loading them into the said database.

The project has the following files and folders:
* `/data` - contains raw song (i.e. from the `song_data` sub-folder) and songplay (i.e. from the `log_data` sub-folder) information
* `create_tables.py` - creates our database, called `sparkifydb` and its tables
* `etl.ipynb` - a jupyter notebook for creating, testing and detailing the various steps of our ETL pipeline
* `etl.py` - contains final ETL code. Runs our ETL pipeline end-to-end, loading data into `sparkifydb`
* `sql_queries.py` - contains our SQL commands for creating and dropping tables from `sparkifydb`, as well as inserting data into them
* `test.ipynb` - a jupyter notebook for testing the successful creation of tables and the insertion of values in them. 

### Purpose of this database in the context of Sparkify and their analytical goals

Sparkify is a startup wanting to have an easy process for analysing song and user activity data they've collected from their new music streaming app. This analysis includes knowing what songs their users are listening to, for example. The database I've created has tables optimised for SQL querying. There are tables on songplays, users, songs, song artists, and songplay timestamps. Unlocking all these features from the data, as well as having the data stored in a structured format allows Sparkify analysts to better understand their user activity, hence helping make data-driven business decisions.

### Database Schema Design and ETL Pipeline** 
The database is relational, as Sparkify is a new startup with low numbers of data at this stage. The relational set-up, apart from having the advantages of ACID transactions, allows Sparkify analysts the flexibility of writing SQL queries on the fly and engineers changing the data schema as required, useful for an early stage company without established practices. 

The database design is a star schema, with a fact table containing songplay information, that link to dimension tables containing user information, song information, artist information, and timestamp information. This design allows the data to be denormalised, allowing for simplified queries, faster aggregation, and reduction of errors due to reduced copies of data. 

Our ETL pipeline is composed of a file, `etl.py` that processes raw song data and user data and loads them into the created tables in the `sparkifydb` database. Song data populates the songs and artists tables, whilst user activity data populates the songplays, user and timestamp tables. The ETL process is created and tested first on a jupyter notebook `etl_pipeline.ipynb`, before transferring to the `etl.py` file, which runs ETL end-to-end. There are no integrity checks in the ETL pipeline, as these checks are at the database level. 

### Example Queries and Results

Here are example SQL queries we can run against `sparkifydb`

**What songs do users listen to on the app?**
```
QUERY_1 = """
SELECT songplays.songplay_id, songs.song_id, songs.title FROM songplays
JOIN songs ON songplays.song_id = songs.song_id;
"""
```

Result:
| songplay_id |            song_id |          title |
|------------:|-------------------:|---------------:|
|        4108 | SOZCTXZ12AB0182364 | Setanta matins |


**Give us the top 3 users by activity**
```
QUERY_2 = """
SELECT users.user_id, users.first_name, users.last_name, count(*) as num_songs_listened_to
FROM songplays
JOIN users on songplays.user_id = users.user_id
GROUP BY users.user_id
ORDER BY num_songs_listened_to DESC
LIMIT 3;
"""
```

Result:
| user_id | first_name | last_name | num_songs_listened_to |
|--------:|-----------:|----------:|----------------------:|
|      49 |      Chloe |    Cuevas |                   689 |
|      80 |      Tegan |    Levine |                   665 |
|      97 |       Kate |   Harrell |                   557 |
