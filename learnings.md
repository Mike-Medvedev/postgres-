## Chapter 1

- Postgresql is an Open source relational database
- called postgres because the inventor created it after his first project called ingres, (post ingres)
- postgres is maintained by a open source community that push features each year
- postgres can be downlaoded in many ways, developed for unix
- postgres binary package download install pre compiled binaries that the os can run
- users can download source code to compile themselves for extra optimizations
- downlaoded postgres installs the postgres server, also clients like psql and pgAdmin4 a gui client

## Chapter 2

- PostgreSQL is a service, running postgresql creates an instance of postgresql
- an instance of postgresql is called a cluster because it can contain multiple databases (cluster of db's)
- a postgresql is a process that runs via a daemon ( background process) that accepts clients to connect to its databases
- pg_clt is a command that allows to view information on the status of the postgresql server itself such as the process
- the first process that runs when starting postgres is called the postmaster process which spawns other important processes
- A database itself is just a space where users can permanently store data
- postgres uses the underlying file system for storage
- postgres stores all postgres related stuff in a folder called PGDATA
- PGDATA contains things like postgres conf to hold configurations, a base folder that holds the database files, and things like write ahead logs

## Chapter 3

- In Postgresql users and groups are categorized as roles.
- roles permit users/groups certain actions such as creating tables, objects, or even creating roles
- to permit users to connect to a db they must be authorized to do so via a role
- CREATE ROLE creates a role and futher options can configure the type of role
- a role can be a single users or a group of users, they are both called a role
- the db allows incoming connections for a host-based access firewall called hba
- pg_hba.conf shows these rules, which can be queried with pg_hba_file_rules

## Chapter 4 Basic Statements

- Basic SQL syntax allows you to write DDL (Data Definition Language) and DML (Data Manipulation Language) commands
- Creating a database in sql can be done with Create database databasename and drop database databasename.
- database are directories stored within the pgdata directory. the databases can be viewed with select \* from pg_database;
- each object like a database or table has a OID (object id) and when created, gets created as a directory/file in PGDATA on the filesystem
- so a database with OID 16400 that contains a table with a OID of 16307 can be found at /Library/PostgreSQL/16/data/base/16400/16307 where
  the database is the directory named 16400 and the table is the file named 16307 and its contents is binary containing all the data of each row
- the actual structure such as columns of a table are contained in the postgres catalog in a different table containing information about other tables,
  its got its own OID as well as its also a table and object
- Schemas represent a namespace for organizing objects such as tables and views and can be created with CREATE SCHEMA myschema;
- normal users by default cant make new schema and tables so they must be granted permission in the schema like CREATE SCHEMA myschema authorization username;
- There are 3 types of tables in PostgreSQL, temp tables, unlogged tables, logged tables
- Creating a table can be created via CREATE TABLE tablename (columns...) and GENERATED AS IDENTITY auto assigned a unique value to a column;
- Create table can be skipped if it already exists with IF EXISTS, CREATE TABLE IF NOT EXISTS USERS; for example
- temp tables only exist during a transaction
- There are common DML SQL statments to manipulate data. Select Insert Update and Delete
- to insert data users can write INSERT INTO "tablename" (column1, column2) values (data1, data2);
- you can insert multiple records by passing multiple tuples INSERT INTO "tablename" (column1, column2) values (data1, data2), (data1, data2);
- to select data users can write SELECT \* FROM TABLENAME
- you can select specific records based on a column with the WHERE clause. Select \* from table where id = 5;
- you can order the results of a select with ORDER BY, select \* from table ORDER BY columnname ASC; (ascending)
- Missing data is expressed as a NULL value, and you can order by columname NULLS FIRST; to sort by nulls first
- You can add null checks such as SELECT \* FROM tablename where columname IS NULL;
- you can update data with UPDATE tablename SET columnname = 0 WHERE name = 'fred';
- you can delete data with DELETE FROM tablename WHERE columnname = 'fred'
- if you dont want to specify row(s) you can use truncate which is also way faster than deleting, but it deletes the whole table, TRUNCATE TABLE;
