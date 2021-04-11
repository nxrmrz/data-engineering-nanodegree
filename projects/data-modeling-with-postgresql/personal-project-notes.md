## Objectives

- [ ] spend a week on this project (3 Apr - 10 Apr)
   

1. [x] Install postgresql on this machine
2. [x] Figure out how to .gitignore all `__pycache__` and `.ipynb_checkpoint` files (basically just don't specify `/` so all nested directories with these are ignored)
3. [ ] Create star schema optimised for queries on song play analysis
   1. Have fact table: songplays 
   2. Have dimensions tables: users, songs, artists, time
4. [x] Coding for the project
5. [ ] Fill in README.md for project

### Run Postgres Locally
Commands below work if we've installed postgres through homebrew
- `/usr/local/opt/postgresql/bin/createuser -s postgres` to create the `postgres` superuser  (once only)
- log into psql as the `postgres` user: `psql -U postgres`. To connect to the `postgres` database, specify additionally `-d postgres`
- `pg_ctl -D /usr/local/var/postgres start && brew services start postgresql` (if we want postgres to start every startup), otherwise
- `pg_ctl -D /usr/local/var/postgres start`
- `pg_ctl -D /usr/local/var/postgres stop`
- `pg_ctl` - utility to initialize, start, stop, or control a PostgreSQL server
- `psql` - utility letting you carry out admin functions without needing to know their actual SQL commands. PostgreSQL interactive terminal. We also execute SQL interactively here
  - `\du` to list all users
  - `\l` to list all databases
  - end all SQL statements with `;` otherwise they wont execute
- there are command line utilities like `createdb` we can execute as an alternative to above
- popular GUIs for postgres in MacOSX: Postico, pgAdmin, Navicat

## Set up roles and dbs
- log in as the `postgres` user
  - `CREATE ROLE student with LOGIN PASSWORD 'student';` to create role with a password
  - `alter user student createdb;` to apply `createdb` permissions to the student role
  - [ ] To create a db called `studentdb` with psql, then connect to it with `pscyopg2`. My problem is that pscyopg2 is installed via conda's pip whilst the python3 runtime is the `usr/local/bin` one. So everytime i run the `create_tables.py` script in this directory, it can't find the `pscyopg2` install :c. Debug later but since i'm running out of time, I'll work from the project workspace for now (4 apr 2021)

## SQL
- before specifying foreign key constraints, appropriate columns must already be available in the tables
- `UPSERT `vs `UPDATE:` when updating a record in the db, it must exist. if updating with `UPDATE`, it'll throw an error if it doesn't. If updating with `UPSERT`, it WON'T throw an error if it doesn't, and inserts the missing record instead.
- `DROP TABLE IF EXISTS` vs `DROP TABLE`: former won't throw an error if the table we want to drop doesn't exist, whilst latter will. Former and latter will both throw errors if there are other objects (i.e. foreign key constraints, and views) that depend on the table we want to drop. To remove despite those dependencies, do `DROP TABLE IF EXISTS tablename CASCADE`. Use `CASCADE` with great care.
- [This link](https://dba.stackexchange.com/questions/140410/what-are-the-consequences-of-not-specifying-not-null-in-postgresql-for-fields-wh) says that it's best to always **maximise** the constraint enforcement mechanisms of the db, and enforce data integrity as close to the data as possible. Frameworks, programmers, languages, come and go, but data and databases tend to persist. Slow performance < giving you the correct result. 
  - there are performance improvements to specifying a column as `NOT NULL` w
  - methodological reasons for writing `NOT NULL`: new programmer who writes an app that uses that db, might make that app write `NULL`s to the db, causing errors that are hard to trace, for e.g.

## Python
- `os.walk()` outputs  generator object that can be unpacked to `root`, `dir`, `files`. Function successively clicks on each folder and lists all folders (i.e. `dirs`) and files (i.e. `files`) inside them 
- have to reload modules you're importing into your project with the `importlib.reload()` function if you've changed the module(s) source code

## Project learnings
- worth it to look at the data and see what columns are not null and otherwise, because referencing the constraints at the SQL level is so important (see above)
- if we get a subset of the data, and that subset is used to fill in a database table, that table might have nulls for columns not normally having nulls (i.e. ids)
- implementing the ON CONFLICT, DO ... statements only comes after you're feeding in the whole batch of data to the ETL
  - did mostly ON CONFLICT, DO NOTHING on duplicates for artist and song ids, but wondering if time table, user table, songplay table can have duplicates? have to print out duplicated data to find out?
- Have to consider how the data is being fed--one at a time by a loop? Or multiple at a time. this affects the implementation of your ETL (the reading and processing statements)