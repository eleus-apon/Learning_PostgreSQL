# Learning_PostgreSQL
Learning SQL Database and exploring the PostgreSQL

# Contents
- [Database](#Database)
- PostgreSQL
- Installation
- Running the psql
- Managing Users&Roles
- Create/Delete DATABASE
- Connect to a Database
- Create Tables
- Inserting data into Table
- Select From
- Order By
- Distinct
- Where Clause and AND
- Comparison Operators
- Limit, Offset & Fetch
- IN
- Between
- Like & ILike
- Group By
- Calculating Min, Max & Average
- Arithmatic Operators
- Time and Dates
- Age Functions
- Primary Keys
- Constraints
- Delete/Update Records
- Relationship/Foreign Keys
- Inner Joins
- Left Joins
- Serial & Sequesce
- Extensions
- UUID Data

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
    ```
    SUPERUSER | NOSUPERUSER  
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
    ```

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

```
CREATE TABLE [IF NOT EXISTS] table_name (  
   column1 datatype(length) column_contraint,  
   column2 datatype(length) column_contraint,  
   column3 datatype(length) column_contraint,  
   table_constraints  
);
```
Example:
```
CREATE TABLE "person"(
   id int,
   first_name VARCHAR(50),
   last_name VARCHAR(50),
   gender VARCHAR(10),
   date_of_birth TIMESTAMP);
```
## Creating Table with COnstraints
```
CREATE TABLE "person"(
   id SERIAL NOT NULL PRIMARY KEY,
   first_name VARCHAR(50) NOT NULL,
   last_name VARCHAR(50) NOT NULL,
   gender VARCHAR(10) NOT NULL,
   date_of_birth TIMESTAMP NOT NULL,
   email VARCHAR(150));
```

To view the newly created data table: `\d $tablename`, in our case `\d person`
![Capture_](https://user-images.githubusercontent.com/35254833/90171605-8ea8a680-ddc3-11ea-82ea-9038e69fd73e.PNG)

# Inserting data into Table
To insert a single row data into the table:
`
INSERT INTO $table_name ($column_1, $column_2, ... $column_N) VALUES ($value_of_column_1, $value_of_column_2, ... $value_of_column_N);
`
To insert multiple rows into the table:
```
INSERT INTO $table_name ($column_1, $column_2, ... $column_N) VALUES ($row_1_column_1, $row_1_column_2, ... $row_1_column_N),($row_2_column_1, $row_2_column_2, ... $row_2_column_N),($row_N_column_1, $row_N_column_2, ... $row_N_column_N);
```
Example:
```
INSERT INTO person (first_name, last_name, gender, date_of_birth) VALUES ('Eleus', 'Apon', 'Male', date '1993-09-23');

```
It should through an output `INSERT 0 1`, which means it was successfull. 
## Genrate and insert 1000 of data into the table:
To generate data [Mockaroo](https://www.mockaroo.com/)

After generating data from mockaru, dowloaded that as SQL and modified on Visual Studio Code and then created the data table on the database.
To see the full table in the Database:
`SELECT * FROM $table_name;` , Ex: `SELECT * FROM person;`
![Capture__](https://user-images.githubusercontent.com/35254833/90176910-8c4a4a80-ddcb-11ea-83ac-8cb313de6b33.PNG)

# Select From
Syntax:
```
SELECT * FROM $ table name
```
To select data more specifically:
```
SELECT $column_name_a, $column_name_b,... $column_name_n FROM $table_name;
```
Example: `SELECT email FROM person;`

# Order By
Syntax:
```
SELECT * FROM person;
1 2 3 4 5 ASC
5 4 3 2 1 DSC
#To represent data order by country in ascending mode
SELECT * FROM person ORDER BY country_of_birth;

#To represent data order by country in descending mode
SELECT * FROM person ORDER BY country_of_birth DSC;

```
More Example:
```
#To represent data order by name in ascending mode
SELECT * FROM person ORDER BY first_name;

#To represent data order by name in descending mode
SELECT * FROM person ORDER BY first_name DSC;
```
![Capture___](https://user-images.githubusercontent.com/35254833/90183938-2d3e0300-ddd6-11ea-8053-18e9b91ada92.PNG)

# Distinct
Syntax:

Example:
`
SELECT DISTINCT country_of_birth FROM person ORDER BY country_of_birth;
`
![Capture](https://user-images.githubusercontent.com/35254833/90184309-b8b79400-ddd6-11ea-9d13-d0fb72ab8ee3.PNG)

# Where Clause and AND
Where claus allows us to filter the data based on conditions.
Example:
```
SELECT * FROM person WHERE gender = 'Female';

#Multiple Condition
SELECT * FROM person WHERE gender = 'Female' AND country_of_birth = "Africa";
SELECT * FROM person WHERE gender = 'Female' AND (country_of_birth = 'Afganistan' OR country_of_birth ='China');
```

![Capture](https://user-images.githubusercontent.com/35254833/90185164-17c9d880-ddd8-11ea-8d47-a8525f805c6e.PNG)

# Comparison Operators
We can use below comparison operators to filter down our data in the Where Claus.
| syntax | what it means                            | example                              |
|--------|------------------------------------------|--------------------------------------|
| =      | equals to                                | id = 2                               |
| >      | greater than                             | id > 2                               |
| <      | less than                                | id < 10                              |
| >=     | greater than or equals to                | id >= 10                             |
| <=     | less than or equals to                   | id <= 100                            |
| <>     | not equals to                            | id <> 102                            |
| ||     | contact strings                          | 'hello ' || 'world'                  |
| !!=    | not in                                   | 3 !! = i                             |
| ~~     | like                                     | 'scrappy,marc,hermit' ~~ '%scrappy%' |
| !~~    | not like                                 | 'bruce' !~~ '%al%'                   |
| ~      | match (regex), case sensitive            | 'thomas' ~ '*.thomas*.'              |
| ~*     | match (regex), case insensitive          | 'thomas' ~* '*.thomas*.'             |
| !~     | Does not match (regex), case sensitive   | 'thomas' !~ '*.Thomas*.'             |
| !~*    | Does not match (regex), case insensitive | 'thomas' !~ '*.vadim*.'              |

# Limit, Offset & Fetch
To select only a specific number of rows we can use these Parameters.
Example:
```
#To filter only first 10 rows
SELECT * FROM person LIMIT 10;

#To filter data 15 to 25 number of rows
SELECT * FROM person OFFSET 15 LIMIT 10;

#To filter out data upto 15th row
SELECT * FROM person OFFSET 15;

#To filter data 15 to 25 number of rows
SELECT * FROM person OFFSET 15 FETCH FIRST 10 ROW ONLY;
```
# IN
Let's say we want to select all rows of specific countries. To do so instead of using Or in Where claus we can use IN Keyword.
Exmple:
```
SELECT * FROM person WHERE country_of_birth IN('China', 'Brazil', 'France');
```

# Between
To slect data from a range.
Example:
```
SELECT * FROM person WHERE date_of_birth BETWEEN DATE '2000-01-01' AND '2015-01-01';
```

# Like & ILike
Like: The PostgreSQL LIKE operator is used to match text values against a pattern using wildcards. If the search expression can be matched to the pattern expression, the LIKE operator will return true, which is 1.
Example:
```
SELECT * FROM person WHERE email LIKE '_____@%';

SELECT * FROM person WHERE email LIKE '%@facebook.com';
# It will find all users whose email address ends with facebook.com
```
![Capture](https://user-images.githubusercontent.com/35254833/90190290-8ca11080-dde0-11ea-8856-cd856e081bc5.PNG)


iLike:The key word ILIKE can be used instead of LIKE to make the match case-insensitive according to the active locale. This is not in the SQL standard but is a PostgreSQL extension. The operator ~~ is equivalent to LIKE , and ~~ corresponds to ILIKE. 
Examlple: 
```
SELECT * FROM person WHERE country_of_birth ILIKE 'p%';
```

# Group By
Allows us to group our data based on column.
Example:
```
SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth;
SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth ORDER BY country_of_birth;
```
![Capture](https://user-images.githubusercontent.com/35254833/90191116-1ac9c680-dde2-11ea-8613-22d94e8d37eb.PNG)

##Group By Having
For additional filtering on Group By, for example we want find all the countries with at-least 10 people.
```
SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth HAVING COUNT(8) > 40 ORDER BY country_of_birth;
# Having keyword must be placed before ORDER BY, we can set less than, equal or greater.
```
![Capture](https://user-images.githubusercontent.com/35254833/90191509-d854b980-dde2-11ea-9078-f037764bd9da.PNG)


# Calculating Min, Max & Average
MAX Example: `SELECT MAX(price) FROM car;`
Min Example: `SELECT MIN(price) FROM car;`
Avg Example: `SELECT AVG(price) FROM car;`
Avg Example in round figure: `SELECT ROUND(AVG(price)) FROM car;`

Grouping the info to see different price for individual make:
` SELECT make, model, MIN(price) FROM car GROUP BY make, model; `
` SELECT make, model, MAX(price) FROM car GROUP BY make, model; `

# Sum
`SELECT SUM(price) from car;`
`SELECT make, SUM(price) FROM car GROUP BY make;`


# Arithmatic Operators
| syntax | what it means              | example      |
|--------|----------------------------|--------------|
| +      | addition                   | SELECT 2 + 5 |
| -      | subtraction                | SELECT 5 -2  |
| /      | division                   | 10 / 2       |
| *      | multiplication             | 5 * 3        |
| %      | modulo                     | 12 % 5       |
| %      | truncate                   | % 4.5        |
| !      | factorial                  | 3!           |
| !!     | factorial (left operation) | !! 5         |
| :      | natural Exponentiation     | : 3.0        |
| ;      | natural Logarithm          | (; 5.0)      |
| @      | absolute value             | @ -5.0       |
| |/     | square root                | |/ 25.0      |
| ||/    | cubic root                 | ||/ 27.0     |

Arithmetic Operators (ROUND):
```
SELECT id, make, model, price, price * .10 FROM car;
#calculating the 10 % of the main prices

```

# Coalesce
COALESCE is a system in-built function that can be considered one of the conditional expressions available in PostgreSQL. NULLIF, GREATEST, LEAST, and COALESCE are the types of conditional expressions in PostgreSQL. The COALESCE function returns the first non-NULL expression in the specified list.
Syntax: `COALESCE (argument_1, argument_2, …);`
Example:`SELECT COALESCE(email, 'Email not provided)' FROM person`

# Null IF
The nullif() function returns a null value, if a the value of the field/column defined by the first parameter equals that of the second. Otherwise, it will return the original value.
Example: `SELECT COALESCE(10 / NULLIF(0,0), 0);`

# Time and Dates
To show current time:
```
SELECT NOW();
SELECT NOW()::DATE;
SELECT NOW()::TIME;
```
Convert between TimeZone:
```
SHOW TIMEZONE;
SELECT timezone('America/New_York','2016-06-01 00:00');
```

## Adding& Suntract Time
```
SELECT NOW();
SELECT NOW() - INTERVAL 1 'YEAR';
SELECT NOW() + INTERVAL 10 'MONTHS';
SELECT NOW()::DATE + INTERVAL 10 'MONTHS';
```

## Extracting Fields From a Date
```
SLECT NOW()'
SELECT EXTRACT(YEAR FROM NOW());
SELECT EXTRACT(DAY FROM NOW());

```

# Age Functions
he PostgreSQL age function returns the number of years, months, and days between two dates.
Example:
`
SELECT first_name, last_name, gender, country_of_birth AGE(NOW(), date_of_birth) AS age FROM person;
`
# Primary Keys
The PostgreSQL PRIMARY KEY is a column in a table which must contain a unique value which can be used to identify each and every row of a table uniquely. So it can be said that the PRIMARY KEY of a table is a combination of NOT NULL and UNIQUE constraint.


# Constraints
Constraints are the rules enforced on data columns on table. These are used to prevent invalid data from being entered into the database. This ensures the accuracy and reliability of the data in the database.
## NOT NULL Constraint
Ensures that a column cannot have NULL value.
## UNIQUE Constraint
Ensures that all values in a column are different.
## Primary Keys
The PostgreSQL PRIMARY KEY is a column in a table which must contain a unique value which can be used to identify each and every row of a table uniquely. So it can be said that the PRIMARY KEY of a table is a combination of NOT NULL and UNIQUE constraint.
## FOREIGN Key
Constrains data based on columns in other tables. 
## CHECK Constraint
The CHECK constraint ensures that all values in a column satisfy certain conditions.
## EXCLUSION Constraint
The EXCLUDE constraint ensures that if any two rows are compared on the specified column(s) or expression(s) using the specified operator(s), not all of these comparisons will return TRUE.

To Dropping Constraints:
`ALTER TABLE table_name DROP CONSTRAINT some_name;`

# Delete/Update Records
Delete:
```
SELECT * FROM person;
DELETE FROM person; # It will delete everyone from the table
DELETE FROM person WHERE id = 100; #To delete the row with id 100
DELETE FROM person WHERE gender = 'Female' AND country_of_birth = 'England';
DELETE FROM person WHERE gender = 'Male'; # To delete all Male from the table
```
Update:
```
SELECT * FROM person;
UPDATE person SET email = 'tbeere1@economist.com' WHERE id 20;
```

# Upsert
In relational databases, the term upsert is referred to as merge. The idea is that when you insert a new row into the table, PostgreSQL will update the row if it already exists, otherwise, it will insert the new row. That is why we call the action is upsert (the combination of update or insert).
Syntax:
```
INSERT INTO table_name(column_list) 
VALUES(value_list)
ON CONFLICT target action;
```

# Relationships [SQL Database]
When creating a database, common sense dictates that we use separate tables for different types of entities. Some examples are: customers, orders, items, messages etc... But we also need to have relationships between these tables. For instance, customers make orders, and orders contain items. These relationships need to be represented in the database. Also, when fetching data with SQL, we need to use certain types of JOIN queries to get what we need.

There are several types of database relationships are as follows:
- One to One Relationships
In a one-to-one relationship, one record in a table is associated with one and only one record in another table. For example, in a school database, each student has only one student ID, and each student ID is assigned to only one person.
![relational 07 03 2](https://user-images.githubusercontent.com/35254833/90256520-afc2d300-de67-11ea-934b-4563c04a2340.png)

- One to Many and Many to One Relationships
In a one-to-many relationship, one record in a table can be associated with one or more records in another table. For example, each customer can have many sales orders.
![relational 07 04 2](https://user-images.githubusercontent.com/35254833/90256641-d254ec00-de67-11ea-8cfc-a75b2b07358c.png)

- Many to Many Relationships
A many-to-many relationship occurs when multiple records in a table are associated with multiple records in another table. For example, a many-to-many relationship exists between customers and products: customers can purchase various products, and products can be purchased by many customers.

Relational database systems usually don't allow you to implement a direct many-to-many relationship between two tables. Consider the example of keeping track of invoices. If there were many invoices with the same invoice number and one of your customers inquired about that invoice number, you wouldn't know which number they were referring to. This is one reason for assigning a unique value to each invoice.

- Self Referencing Relationships
This is used when a table needs to have a relationship with itself. For example, let's say you have a referral program. Customers can refer other customers to your shopping website.

# PostgreSQL JOINS
The PostgreSQL Joins clause is used to combine records from two or more tables in a database. A JOIN is a means for combining fields from two tables by using values common to each.

There are 4 basic types of joins supported by PostgreSQL, namely:

- Inner Joins
the inner join returns a result set that contains row in the left table that matches the row in the right table.
![5311](https://user-images.githubusercontent.com/35254833/90266987-af7e0400-de76-11ea-8a5d-c5d8428cd485.png)
Syntax:
`SELECT $columns FROM $table1 JOIN $table2 ON $table1.joining_column = $table2.reference_column;
`

- Left (Outer) Joins
the left join returns a complete set of rows from the left table with the matching rows if available from the right table. If there is no match, the right side will have null values.
![5511](https://user-images.githubusercontent.com/35254833/90267097-e3f1c000-de76-11ea-9250-8fdf045926c6.png)
Syntax:
`SELECT $columns FROM $table1 LEFT JOIN $table2 ON $table1.joining_column = $table2.reference_column;
`

- Right (Outer) Joins
The RIGHT JOIN or RIGHT OUTER JOIN works exactly opposite to the LEFT JOIN. It returns a complete set of rows from the right table with the matching rows if available from the left table. If there is no match, the left side will have null values.
![5711](https://user-images.githubusercontent.com/35254833/90267165-0257bb80-de77-11ea-9b55-8113f38ee3d8.png)
Syntax:
`Select * FROM table1 RIGHT [ OUTER ] JOIN table2 ON table1.column_name=table2.column_name;`

- Full Outer Joins
The full outer join or full join returns a result set that contains all rows from both the left and right tables, with the matching rows from both sides where available. If there is no match, the missing side contains null values.
![592](https://user-images.githubusercontent.com/35254833/90267289-22877a80-de77-11ea-8ed5-9ac2dab97dcf.png)

Some special PostgreSQL joins are below:
- Cross Joins
- Natural Joins

# Export Query Result to CSV
In order to export a table or query to csv use one of the following commands:
`\copy [Table/Query] to '[Relative Path/filename.csv]' csv header `

# Serial & Sequesce
A sequence in PostgreSQL is a user-defined schema-bound object that generates a sequence of integers based on a specified specification.
PostgreSQL provides three serial pseudo-types SMALLSERIAL, SERIAL, and BIGSERIAL with the following characteristics:
| Name        | Storage Size | Range                          |
|-------------|--------------|--------------------------------|
| SMALLSERIAL | 2 bytes      | 1 to 32,767                    |
| SERIAL      | 4 bytes      | 1 to 2,147,483,647             |
| BIGSERIAL   | 8 bytes      | 1 to 9,223,372,036,854,775,807 |

# Conclusion
This was my first step into learning Database. I followd the video tutorial available [here](https://www.youtube.com/watch?v=qw--VYLpxG4&t=7991s)
To create table I have used this [link](https://thisdavej.com/copy-table-in-excel-and-paste-as-a-markdown-table/)

