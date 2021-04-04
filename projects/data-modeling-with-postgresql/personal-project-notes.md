## Objectives

- [ ] spend a week on this project (3 Apr - 10 Apr)
  

1. [x] Install postgresql on this machine
2. [x] Figure out how to .gitignore all `__pycache__` and `.ipynb_checkpoint` files (basically just don't specify `/` so all nested directories with these are ignored)
3. [ ] Create star schema optimised for queries on song play analysis
   1. Have fact table: songplays 
   2. Have dimensions tables: users, songs, artists, time
4. [ ] Coding for the project
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