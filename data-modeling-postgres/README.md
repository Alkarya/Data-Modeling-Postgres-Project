# Project - Data Modeling Postgres

## Project Description

This project has to purpose to support a startup called Sparkify that needs to analyze their data. 
This data is related to the songs and the users activity on their app. Currently this data is only available in JSON and this project will help "transalate" it into a SQL database.

## Repo Structure

* `/data` - data source used to feed the database
* `/scripts`- source code, includes python scripts and jupyter notebooks files

### Scripts
* `create_tables.py` - python script that manages the objects of the DB, it drops and creates de DB when called
* `etl.ipynb` - reads a processes a single file into the tables
* `etl.py` - reads and processes all files into the tables
* `sql_queries.py` - defines all the sql queries need to create the tables and all insert statements
* `test.ipynb` - tests the data inserted to the tables

## DB Design

The schema design is based on the concept of a star schema. It is desgin to analyse the data regarding song playing.
This type of modeling takes advantage of denormalization and fast aggregation.
It will help Sparkify answer their questions with fast and reliable data.

## Tables 

### Fact Table

1. **songplays** - records in log data associated with song plays i.e. records with page `NextSong`
   * songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

### Dimension Tables

2. **users** - users in the app
   * user_id, first_name, last_name, gender, level

3. **songs** - songs in music database
   * song_id, title, artist_id, year, duration

4. **artists** - artists in music database
   * artist_id, name, location, latitude, longitude

5. **time** - timestamps of records in songplays broken down into specific units
   * start_time, hour, day, week, month, year, weekday


## ETL Pipeline

The purpose of this ETL process, as the name says is to **E**xtract, **T**ransform and **L**oad the data into the tables in a postgres DB

1. Connects to the Database
2. Processes song data
   1. Gets all files
   2. Iterates through all files
      1. reads file
      2. inserts song data
      3. inserts artist data
3. Processes log data
   1. Gets all files
   2. Iterates through all files
      1. reads file
      2. filters song data by `NextSong`
      3. insert time data records
      4. inserts user data
      5. inserts songplay data

## How to run locally

1. Clone repo and change diretory

```bash
git clone https://github.com/Alkarya/Data-Engineering-NanoDegree.git

cd data-modeling-postgres
```
2. Start postgres docker image

```bach
docker-compose up
```

3. Create Python venv

On a new terminal run:
```bash
python3 -m venv python-venv            
source python-venv/bin/activate 
```

4. Install requisits

```bash
pip install -r requirements.txt
```

5. Run python scripts

```bash
cd scripts
python -m create_tables
python -m etl
```

6. Launch Jupyter Notebooks

```bash
jupyter lab
```
This should open on the browser jupyter notebooks

7. Closing and cleaning the local environment

Press `cntr + c` in the terminal to close jupyter notebooks

```bash
cd ..
deactivate
rm -r python-venv
```
On the other terminal press `cntr + c` to close docker

```bash
docker kill
docker system prune -a
docker network prune
```

## References

* Set up [Postgres Docker Compose](https://github.com/khezen/compose-postgres/tree/master) 
* Insert [on conflict](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-upsert/)
  