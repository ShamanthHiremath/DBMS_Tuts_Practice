# SQL Queries with Examples

```shell
# General Commands

--(not case sensitive)

-- IN-LINE COMMENT
/*
MULTI-LINE COMMENT
*/

//To create a new database
CREATE DATABASE db_name;

//To create already existing database
CREATE DATABASE IF NOT EXISTS db_

//To Drop/Delete a database
DROP DATABASE db_name;

//To Drop/Delete an already existing database
DROP DATABASE IF EXISTS db_name;

//To display the existing databases
SHOW DATABASE;

//SCHEMA - Table Design with FIELDS (columns) & RECORDS (rows)

//To use the database for the session  
USE db_name;

//To create a table
CREATE TABLE table_name(
column1 datatype constarint,
column2 datatype constarint,
column3 datatype constarint,
...
);

//PRIMARY KEY - defines a column as a unique identifier for a table, basically through which a record can be accessed and cannot be null
Ex: Id's, roll no's, etc. but not names, etc.

//To create a table with auto increment primary key
CREATE TABLE table_name(
column1 int PRIMARY KEY AUTO_INCREMENT, --Auto increments as registered 
column2 datatype,
column3 datatype,
...
);

//To create a table with multiple primary keys
CREATE TABLE table_name(
column1 datatype,
column2 datatype,
column3 datatype,
PRIMARY KEY(column1,column2)
);

//Select and view all columns from a table
SELECT * FROM table_name;

//Select specific columns from a table
SELECT column1, column2, column3 FROM table_name;

//Insert a record into a table
INSERT INTO table_name(column1, column2, column3) VALUES(value1, value2, value3);

//Insert multiple records into a table
INSERT INTO table_name(column1, column2, column3) VALUES(value1, value2, value3), (value1, value2, value3), (value1, value2, value3);

//FOREIGN KEYS - A column or group of columns in a table that refers to the PRIMARY KEY in another table (There can be multiple foreign keys, can be duplicate, can be null)
Ex: 

//To create a table with foreign key
CREATE TABLE table_name(
column1 datatype,
column2 datatype,
column3 datatype,
FOREIGN KEY(column1) REFERENCES table_name(column1)
);
