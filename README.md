# Learning_PostgreSQL
Learning SQL Database and exploring the PostgreSQL

# Contents
- Database
- PostgreSQL
- Installation
- Running the psql
- Managing Users&Roles

# Database
A database is an organized collection of structured information, or data, typically stored electronically in a computer system. A database is usually controlled by a database management system (DBMS). Together, the data and the DBMS, along with the applications that are associated with them, are referred to as a database system, often shortened to just database.

Data within the most common types of databases in operation today is typically modeled in rows and columns in a series of tables to make processing and data querying efficient. The data can then be easily accessed, managed, modified, updated, controlled, and organized. Most databases use structured query language (SQL) for writing and querying data.

Details can be found [here!](https://www.oracle.com/database/what-is-database.html) 

# PostgreSQL
PostgreSQL is a powerful, open source object-relational database system that uses and extends the SQL language combined with many features that safely store and scale the most complicated data workloads. PostgreSQL has earned a strong reputation for its proven architecture, reliability, data integrity, robust feature set, extensibility, and the dedication of the open source community behind the software to consistently deliver performant and innovative solutions. PostgreSQL runs on all major operating systems.

More about [PostgreSQL!](https://www.postgresql.org/about/)

# Installation[Ubuntu/WSL2]
I have installed the PostgreSQL on Ubuntu on Windows System for Linux. Deatils can be found [here!](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database)
Necessary Commands,
```
sudo apt update
sudo apt install postgresql postgresql-contrib
psql --version

sudo service postgresql status
sudo service postgresql start
sudo service postgresql stop
```

# Running the psql
To run PostgreSQL with psql shell:  
Start your postgres service: `sudo service postgresql start`  
Connect to the postgres service and open the psql shell: `sudo -u postgres psql`  
To exit postgres=# enter: `\q` or use the shortcut key: Ctrl+D  
Please make sure the set the default admin password prio to connect the database, process explained in below topic.

# Managing Users&Roles
The default admin user, ```postgres```, needs a password assigned in order to connect to a database. To set a password:  
Enter the command: ```sudo passwd postgres```  

Create new user  
`CREATE USER name [ [ WITH ] option [ ... ] ]`  
Where options can be:  
   `SUPERUSER | NOSUPERUSER  
    | CREATEDB | NOCREATEDB  
    | CREATEROLE | NOCREATEROLE  
    | CREATEUSER | NOCREATEUSER  
    | INHERIT | NOINHERIT  
    | LOGIN | NOLOGIN  
    | CONNECTION LIMIT connlimit  
    | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'  
    | VALID UNTIL 'timestamp'   
    | IN ROLE role_name [, ...]  
    | IN GROUP role_name [, ...]  
    | ROLE role_name [, ...]  
    | ADMIN role_name [, ...]  
    | USER role_name [, ...]  
    | SYSID uid     
    `

To assign new role:
`ALTER USER #username WITH #Role;`
Example:
`ALTER USER eleus WITH SUPERUSER;`

Lets create a user who can login and create database and interact with them.  
`CREATE ROLE $username WITH LOGIN PASSWORD = 'password-goes-here';`  
example: `CREATE ROLE eleus WITH LOGIN PASSWORD = '***';`  

CREATE USER is now an alias for CREATE ROLE. The only difference is that when the command is spelled CREATE USER, LOGIN is assumed by default, whereas NOLOGIN is assumed when the command is spelled CREATE ROLE.

To list all user accounts (or roles) in the current PostgreSQL database serve: `\du`

# Create/Delete DATABASE
To show current databases `\l`

To create a new databse:
`CREATE DATABASE testdb;`
![CreateDB](https://user-images.githubusercontent.com/35254833/90165425-71230f00-ddba-11ea-85ac-4baf78dba748.PNG)

To create a database with specific owner:
`CREATE DATABASE testdb2 WITH OWNER eleus;`

To delete any databse:
`DROP DATABASE testdb2;`

# Connect to a Database
To connect to a database:
`psql -h $host_ip_or_domain -p $port -U $username -W $dbname
# using -W (capital W) will ask for a password, if you don't have any password use -w `
Example: `psql -h localhost -p 5432 -U eleus myfdb`
![CreateDB](https://user-images.githubusercontent.com/35254833/90166602-518ce600-ddbc-11ea-83fa-db32634ae9b6.PNG)

To swithc between databases, type `\c $dbname` ex-`\c testdb`.

# Create Tables
To create data table:

`CREATE`

`
CREATE TABLE [IF NOT EXISTS] table_name (  
   column1 datatype(length) column_contraint,  
   column2 datatype(length) column_contraint,  
   column3 datatype(length) column_contraint,  
   table_constraints  
);
`
Example:
`
CREATE TABLE "person"(
   id int,
   first_name VARCHAR(50),
   last_name VARCHAR(50),
   gender VARCHAR(10),
   date_of_birth TIMESTAMP);
`
## Creating Table with COnstraints
`
CREATE TABLE "person"(
   id SERIAL NOT NULL PRIMARY KEY,
   first_name VARCHAR(50) NOT NULL,
   last_name VARCHAR(50) NOT NULL,
   gender VARCHAR(10) NOT NULL,
   date_of_birth TIMESTAMP NOT NULL,
   email VARCHAR(150));
`

To view the newly created data table: `\d $tablename`, in our case `\d person`
![Capture_](https://user-images.githubusercontent.com/35254833/90171605-8ea8a680-ddc3-11ea-82ea-9038e69fd73e.PNG)

# Inserting data into Table
To insert a single row data into the table:
`
INSERT INTO $table_name ($column_1, $column_2, ... $column_N) VALUES ($value_of_column_1, $value_of_column_2, ... $value_of_column_N);
`
To insert multiple rows into the table:
`
INSERT INTO $table_name ($column_1, $column_2, ... $column_N) VALUES ($row_1_column_1, $row_1_column_2, ... $row_1_column_N),($row_2_column_1, $row_2_column_2, ... $row_2_column_N),($row_N_column_1, $row_N_column_2, ... $row_N_column_N);
`
Example:
`
INSERT INTO person (first_name, last_name, gender, date_of_birth) VALUES ('Eleus', 'Apon', 'Male', date '1993-09-23');

`
It should through an output `INSERT 0 1`, which means it was successfull. 
## Genrate and insert 1000 of data into the table:
To generate data [Mockaroo](https://www.mockaroo.com/)

After generating data from mockaru, dowloaded that as SQL and modified on Visual Studio Code and then created the data table on the database.
To see the full table in the Database:
`SELECT * FROM $table_name;` , Ex: `SELECT * FROM person;`
![Capture__](https://user-images.githubusercontent.com/35254833/90176910-8c4a4a80-ddcb-11ea-83ac-8cb313de6b33.PNG)

# Querying Data







