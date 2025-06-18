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

