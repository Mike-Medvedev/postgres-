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
