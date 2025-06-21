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

## Chapter 5 Advanced Statements

- you can filter select statements with the where clause and coniditons like > or <
- the LIKE clause can search for similiar words like select \* from table where title LIKE "%data% which will find all titles with the word data in there
- use ILIKE for case insensitive searches instead of using upper()
- the DISTINCT command will filter out all duplicates of the column(s), so SELECT DISTINCT title from table, will only return rows with unique title's ommiting any with duplicate title
- DISTINCT works on combinations for columns too like SELECT DISTINCT title,name
- the COALESCE function coalesce() takes two parameters and returns the one that is not null.
  so you can do select coalesce(title, 'No Title') from tables and it will return the title if true or if title is null return 'No Title' or whatever value you pass in
- you should alias coalesce because the result set will contain the column name coalesce, so coalesce(title, 'Null Title') as title.
- the LIMIT and OFFSET keywords will limit the rows returned in a query and the offset controls which row to start at, which helps with pagination.
- for example select \* from table LIMIT 50, OFFSET 20 will return 50 records starting from the 20th record
- SUBQUERIES are queries that can be run inside a query, they are very powerful but hinder performance and JOINING is preferred where possible
- you can subquery with IN/NOT IN keywords such as: SELECT user_id FROM users where name IN ('mike', 'paul')
- the parentheses will contain the subquery, instead of ('mike', 'paul') you can do
  SELECT student_id FROM students WHERE student_name NOT IN (select \* from class_a_students)
- EXISTS/NOT EXISTS are keywords to evaluate if a subquery is true or false, for example select id, name from users where exists(select user_id from posts where posts.user_id = users.id )
- a join in sql combines tables together to form a result set
- select users._, posts._ from users, posts is a CROSS-JOIN and returns a cartesian product of
  both tables such that there exists every combination of rows from users and posts, so for the first user record there is every post record
- INNER JOINS join tables on a common value for example, select users._, posts._ from users JOIN posts ON posts.user_id = users.id. -> the resulting rows are such that each row contains a users.id value that equals a posts.user_id value in that row.
- Another way to think of INNER JOINS is two overlapping circle creating a venn diagram, the overlap is the result of an INNER JOIN
- LEFT JOINS return the same result as an INNER JOIN + the rest of the records of the LEFT table even if they dont have a common value with the RIGHT Table, so if you wanted to get all the users that have not written a post, you can join users with posts and left join to view all users and with and without posts and filter that result by users where posts are null.
- SELECT users._, posts._ from users LEFT JOIN posts on posts.user_id = users.id where posts.user_id IS NULL
- if you just used an inner join then you would never get null posts because inner join only returns the rows with common values

- A right join is the twin of a left join, it returns the right tables records as well as common values with the left

- A FULL OUTER JOIN combines both tables but its not a CROSS-JOIN, it just returns all records of both tables as well as their common values, but not every combination like a cross join, just left table records, right table records, and the records that share a common value deduped.

- A LATERAL JOIN allows you to join a table with the result of a subquery,
  select users._, q._ from users LATERAL JOIN (select posts.\* from posts where posts.user_id = users.id AND posts.likes > 2) as q on true;
- this will join users table with the result of the subquery, so we can see all the users who have posts with likes greater than 2 for each post.
- ths query runs for each row of users, so it checks for each user records, if the result of the subquery satisfies him. So if theres a user MIKE, then for MIKE check if any posts have the same user_id as mike.id and if for that post the like > 2
- this is useful because we can get the values in the subquery, in the main query with q

- Upsert in postgreSQL means inserting a record, and updating if theres a conflict.
- in postgres you can do insert into table (column) values (data) ON CONFLICT DO UPDATE set column = 'some data';

- RETURNING allows you to return the values from columns in an insert, update, or delete. delete from table where id = 5 returning \*; this will return the row that was deleted
- You can MERGE to do a bulk update of properties by merging a table into another table and cherry picking which values to update
- CTE's are Common Table Expressions, they are temporary queries you can use in a statement and only exists for the lifetime of the query
- WITH customers as (select _ from customers_1 union select _ from customers_2) select id from customers where name = 'kent';

## Chapter 5 (PostgreSQL Tutorial on Constraints and Columns)

- When defining a column in a table, and Identity column is a special column, to give it an autoincrement sequence you can to GENERATED ALWAYS AS IDENTITY;
- You can make computed columns rely on another column by doing GENERATED ALWAYS AS (other_column \* 2)
- Constraints give you fine-grained control over what data is entered into Columns and Tables, since a data type is not enough of a restriction.
- CHECK Constraint is a constraint that evaluates a boolean expression on a column or table. EXAMPLE: price int CHECK (price > 0) will restrict negative prices on the price column
- You can add a CHECK constraint on a table by doing CHECK (price > 0) or to name it you can do CONSTRAINT positive_numbers CHECK (price > 0)
- NOT NULL is a constraint that ensures a column is NOT NULL
- UNIQUE constraint ensures theres only one row that contains that value
- UNIQUE constraints also works for composites like, column_1 integer, column_2 integer, CONSTRAINT unique_pair UNIQUE (column_1, column_2)
- PRIMARY KEY constraint applies UNIQUE NOT NULL and also tables can only have one PRIMARY KEY, for referential integrity so foreign keys can reference it
- FOREIGN KEY constraint means the values of this column must shadow a value in the referenced table, so if a table has id int PRIMARY KEY, then table_2 needs CONSTRAINT fk_id FOREIGN KEY (some_id) REFERENCES table(id)
- ON DELETE is part of foreign keys and provide options for handling deletions that have references. IF the referenced table deletes a row, the referencing table will delete its rows with the corresponding foreign keys if ON DELETE CASCADE is present on that constraint
- ON DELETE RESTRICT disallows the deletion of a row in the referenced table if the referencing row has foreign keys depending on it

## Chapter 8 (Transactions)

- A transaction is an atomic unit of work and can be single or mulitple statements of SQL
- Every statement is an implicit transaction which the postgresql engine handles itself which automatically applies changes or rollsback changes if theres an error
- An explicit transaction can be done with BEGIN statment and end it with COMMIT or ROLLBACK statements
- each transaction is given a txid which is the transaction identifier. Commit permanently saves changes and roll back drops them
- if theres like a syntax error or anything bad happening in a transaction, the transaction will roll back and nothing is saved
- MVCC is the concurrency in postgresql and is a mechanism in place to handle conflicting updates
- for example when an update is made to a tuple, it actually makes a copy and negates the old tuple, any other concurrent transaction does the same thing
- then the conflicting transactions will enter a DEADLOCK and the second transaction will wait for the first to complete

## Chatper
