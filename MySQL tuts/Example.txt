CREATE DATABASEIF NOT EXISTS college;

USE college;

CREATE TABLE student(
    roll_no INT PRIMARY KEY, 
    name VARCHAR(50),
    age INT NOT NULL
);

--PRIMARY KEY - a constraint that defines a column as a unique identifier for a table, basically through which a record/row can be accessed (non null)
--AUTO_INCREMENT - a constraint that automatically increments the value of the column by 1 for each new record/row
/*
Two or more columns can be defined as primary key, but combined they should be unique
Ex: roll_no
    name
    PRIMARY KEY(roll_no, name)
*/

-- Select and view columns

SELECT * FROM student; -- Displays all columns from student table

INSERT INTO student VALUES(1, "ROB", 17);
INSERT INTO student VALUES(2, "BOB", 17); 
INSERT INTO student VALUES(2, "DAVE", 17)(3, "TOM", 17);

-- CONSTRAINTS

--UNIQUE - a constraint that defines a column to have unique values, no two rows can have the same value for that column
CREATE TABLE temp1(
    id int PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(50) UNIQUE
    name VARCHAR(50) NOT NULL,
    /*
    or PRIMARY KEY(id)
    */
)

INSERT INTO temp1 VALUES(1, "xyz", "ROB");
INSERT INTO temp1 VALUES(2, "abc", "BOB");
INSERT INTO temp1 VALUES(3, "abc", "DAVE"); -- Error: Duplicate entry 'abc' for key 'email'
INSERT INTO temp1 VALUES(3, "def", ""); -- Error: Column 'name' cannot be null and Error: Duplicate entry 3 for key id


--FOREIGN KEY - a constraint that defines a column to be used from a primary key in another table, helps in linking two tables


--DEFAULT - a constraint that sets a default value for a column if no value is provided
--CHECK - a constraint that checks the value of a column to be within a certain range or condition

CREATE TABLE emp(
    emp_id INT PRIMARY KEY,
    emp_salary INT DEFAULT 20000
    emp_age INT CHECK(emp_age >= 18 AND emp_age =< 60)
);

INSERT INTO emp (emp_id, emp_age) VALUES(1, 21);
INSERT INTO emp (emp_id, emp_age) VALUES(2, 17); -- Error: CHECK constraint failed: emp_age > 18 AND emp_age < 60


--SELECT - a command that retrieves data from a table
--FROM - a command that specifies the table from which data is to be retrieved

SELECT * FROM emp; -- Displays all columns from emp table
SELECT emp_id, emp_salary FROM emp; -- Displays emp_id and emp_salary columns from emp table
SELECT DISTINCT emp_salary FROM emp; -- Displays distinct/unique values of emp_salary column from emp table

--WHERE - a command that specifies a condition to filter the data
SELECT * FROM emp WHERE emp_salary > 20000; -- Displays all columns from emp table where emp_salary is greater than 20000
SELECT * FROM emp WHERE emp_salary > 20000 AND emp_age < 30; -- Displays all columns from emp table where emp_salary is greater than 20000 and emp_age is less than 30
SELECT * FROM emp WHERE emp_salary > 20000 OR emp_age < 30; -- Displays all columns from emp table where emp_salary is greater than 20000 or emp_age is less than 30
SELECT * FROM emp WHERE emp_salary BETWEEN 20000 AND 30000; -- Displays all columns from emp table where emp_salary is between 20000 and 30000
SELECT * FROM emp WHERE emp_salary IN (20000, 25000, 30000); -- Displays all columns from emp table where emp_salary is 20000, 25000, or 30000
SELECT * FROM emp WHERE emp_salary NOT IN (20000, 25000, 30000); -- Displays all columns from emp table where emp_salary is not 20000, 25000, or 30000
SELECT * FROM emp WHERE emp_salary IS NULL; -- Displays all columns from emp table where emp_salary is null


--LIMIT - a command that specifies the number of rows to be displayed

SELECT * FROM emp LIMIT 5; -- Displays first 5 rows from emp table
SELECT * FROM emp LIMIT 5 OFFSET 5; -- Displays 5 rows from emp table starting from the 6th row
SELECT * FROM emp WHERE emp_salary > 20000 LIMIT 5; -- Displays first 5 rows from emp table where emp_salary is greater than 20000


--ORDER BY - a command that specifies the order in which data is to be displayed

SELECT * FROM emp ORDER BY emp_name ; -- Displays all columns from emp table in ascending order of emp_name
SELECT * FROM emp ORDER BY emp_salary ASC; -- Displays all columns from emp table in ascending order of emp_salary
SELECT * FROM emp ORDER BY emp_salary DESC; -- Displays all columns from emp table in descending order of emp_salary


--AGREGATE FUNCTIONS - functions that perform calculations on a set of values and return a single value

--COUNT - a function that returns the number of rows in a table
SELECT COUNT(*) FROM emp; -- Returns the number of rows in emp table

--SUM - a function that returns the sum of values in a column
SELECT SUM(emp_salary) FROM emp; -- Returns the sum of emp_salary column in emp table

--AVG - a function that returns the average of values in a column
SELECT AVG(emp_salary) FROM emp; -- Returns the average of emp_salary column in emp table

--MIN - a function that returns the minimum value in a column
SELECT MIN(emp_salary) FROM emp; -- Returns the minimum value of emp_salary column in emp table

--MAX - a function that returns the maximum value in a column
SELECT MAX(emp_salary) FROM emp; -- Returns the maximum value of emp_salary column in emp table


--GROUP BY - a command that groups rows that have the same values into summary rows

SELECT emp_salary, COUNT(*) FROM emp GROUP BY emp_salary; -- Returns the number of rows in emp table that have the same emp_salary
-- basically makes a table of emp_salary and the number of times it occurs in the emp table

SELECT emp_salary, emp_age, COUNT(*) FROM emp GROUP BY emp_salary, emp_age; -- Returns the number of rows in emp table that have the same emp_salary and emp_age

--HAVING - a command that filters the data after grouping has been done, similar to WHERE but for GROUP BY

SELECT emp_salary, COUNT(*) FROM emp GROUP BY emp_salary HAVING COUNT(*) > 1; -- Returns the number of rows in emp table that have the same emp_salary and the count is greater than 1

/*
GENERAL SYNTAX:
SELECT column(s)
FROM table_name
WHERE condition
GROUP BY column(s)
HAVING condition
ORDER BY column(s);*/


--UPDATE - a command that updates the values of a column in a table
/*
GENERAL SYNTAX:
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
*/

SET SQL_SAFE_UPDATES = 0; -- Disables safe mode to update the table

UPDATE emp SET emp_salary = 25000 WHERE emp_id = 1; -- Updates the value of emp_salary column to 25000 in emp table where emp_id is 1
UPDATE emp SET emp_salary = emp_salary + 5000; -- Updates the value of emp_salary column to emp_salary + 5000 in emp table


--DELETE - a command that deletes rows from a table
/*
GENERAL SYNTAX:
DELETE FROM table_name
WHERE condition;
*/

DELETE FROM emp WHERE emp_id = 1; -- Deletes the row from emp table where emp_id is 1
DELETE FROM emp; -- Deletes all rows from emp table

--DROP - a command that deletes a table
/*
GENERAL SYNTAX:
DROP TABLE table_name;
*/

DROP TABLE emp; -- Deletes the emp table

--FOREIGN KEY - a constraint that defines a column to be used from a primary key in another table, helps in linking two tables

CREATE TABLE dept(
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50)
);

CREATE TABLE teachers(
    teacher_id INT PRIMARY KEY,
    teacher_name VARCHAR(50),
    dept_id INT,
    -- FOREIGN KEY of the current table is the primary key of the other table  
    FOREIGN KEY(dept_id) REFERENCES dept(dept_id)
    ) 

/*
To visualise foreign keys: 
Go to Database: Click on Reverse Engineer: Select Scheme: Continue: Click on Execute: Click on Finish

You can view the ER (Entity Relationship) diagram of the database with teachers and dept tables linked by the foreign key dept_id 
Dept (Parent table) ---> Teachers (Child table)
*/

--CASACADING - a command that specifies the action to be reflected in the child table/taken when a row in the parent table is deleted or updated

--ON DELETE CASCADE - deletes the rows in the child table when the corresponding row in the parent table is deleted
--ON UPDATE CASCADE - updates the rows in the child table when the corresponding row in the parent table is updated

CREATE TABLE dept(
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50)
);

CREATE TABLE teachers(
    teacher_id INT PRIMARY KEY,
    teacher_name VARCHAR(50),
    dept_id INT,
    FOREIGN KEY(dept_id) REFERENCES dept(dept_id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);



--ALTER - a command that modifies a table/changes the SCHEMA of a table

--ADD col - a command that adds a column to a table
ALTER TABLE dept
ADD dept_head VARCHAR(50) NOT NULL DEFAULT "Principal"; -- Adds dept_head column to dept table

--MODIFY col - a command that modifies the data type of a column in a table
ALTER TABLE dept
MODIFY dept_id VARCHAR(50); -- Changes the data type of dept_id column in dept table to VARCHAR(50)

--DROP col - a command that deletes a column from a table
ALTER TABLE dept
DROP COLUMN dept_head; -- Deletes dept_head column from dept table

--RENAME - a command that renames a table
ALTER TABLE dept
RENAME TO department; -- Renames dept table to department

--CHANGE - a command that changes the name and data type of a column in a table
ALTER TABLE dept
CHANGE dept_id department_id INT; -- Changes the name and data type of dept_id column in dept table to department_id VARCHAR(50)



--TRUNCATE - a command that deletes all rows from a table, DROP deletes the table itself
TRUNCATE TABLE dept; -- Deletes all rows from dept table



--JOIN - a command that combines rows from two or more tables based on a related column between them

--INNER JOIN - a command that returns rows when there is at least one match in both tables
SELECT *
FROM teachers
INNER JOIN dept
ON teachers.dept_id = dept.dept_id;

--LEFT JOIN - a command that returns all rows from the left table and the matched rows from the right table
SELECT *
FROM teachers
LEFT JOIN dept
ON teachers.dept_id = dept.dept_id;
-- values from teachers table will be displayed even if there is no match in dept table

--RIGHT JOIN - a command that returns all rows from the right table and the matched rows from the left table
SELECT *
FROM teachers
RIGHT JOIN dept
ON teachers.dept_id = dept.dept_id;
-- values from dept table will be displayed even if there is no match in teachers table

--FULL JOIN - a command that returns rows when there is a match in one of the tables
SELECT * 
FROM teachers
FULL JOIN dept -- this will throw an error in MySQL, use LEFT JOIN and RIGHT JOIN instead, but works in PostgreSQL
ON teachers.dept_id = dept.dept_id;

-- Use the below syntax instead of FULL JOIN in MySQL

SELECT *
FROM teachers
LEFT JOIN dept
ON teachers.dept_id = dept.dept_id
UNION
SELECT *
FROM teachers
RIGHT JOIN dept
ON teachers.dept_id = dept.dept_id;

--SELF JOIN - a command that joins a table with itself
SELECT a.teacher_id, a.teacher_name, b.teacher_name
FROM teachers a, teachers b
WHERE a.dept_id = b.dept_id;

--UNION - a command that combines the result of two or more SELECT statements
SELECT teacher_id, teacher_name
FROM teachers
UNION
SELECT dept_id, dept_name
FROM dept;


--LEFT EXCLUDING JOIN - a command that returns all rows from the left table that do not have a match in the right table
SELECT *
FROM teachers as t
LEFT JOIN dept as d
ON teachers.dept_id = dept.dept_id
WHERE d.dept_id IS NULL;

--RIGHT EXCLUDING JOIN - a command that returns all rows from the right table that do not have a match in the left table
SELECT *
FROM teachers as t
RIGHT JOIN dept as d
ON teachers.dept_id = dept.dept_id
WHERE t.dept_id IS NULL;

--SELF JOIN - a command that joins a table with itself
SELECT *
FROM teachers as t1
JOIN teachers as t2
ON t1.dept_id = t2.dept_id
WHERE t1.teacher_id != t2.teacher_id;


--UNION - a command that combines the unique result of two or more SELECT statements
SELECT teacher_id, teacher_name
FROM teachers
UNION
SELECT dept_id, dept_name
FROM dept;

--UNION ALL - a command that combines the result of two or more SELECT statements including duplicates
SELECT teacher_id, teacher_name
FROM teachers
UNION ALL
SELECT dept_id, dept_name
FROM dept;

--INTERSECT - a command that returns the common rows between two SELECT statements
SELECT teacher_id, teacher_name
FROM teachers
INTERSECT
SELECT dept_id, dept_name
FROM dept;

--EXCEPT - a command that returns the rows that are in the first SELECT statement but not in the second SELECT statement
SELECT teacher_id, teacher_name
FROM teachers
EXCEPT
SELECT dept_id, dept_name
FROM dept;

--SUB QUERY/ INNER QUERY/ NESTED QUERY - a command that nests a SELECT statement within another SELECT statement

/*
GENERAL SYNTAX:
SELECT column_name(s)
FROM table_name
WHERE column_name operator
(SELECT column_name(s)
FROM table_name
WHERE condition);
*/

SELECT teacher_name
FROM teachers
WHERE dept_id IN
(SELECT dept_id
FROM dept
WHERE dept_name = "Computer Science");

-- another example of where we need to find the teacher who has the highest salary

SELECT teacher_name
FROM teachers
WHERE teacher_salary =
(SELECT MAX(teacher_salary)
FROM teachers);

-- another example of where we need to find the teacher who has the highest salary in a particular department

SELECT teacher_name
FROM teachers
WHERE dept_id = 1 AND teacher_salary =
(SELECT MAX(teacher_salary)
FROM teachers
WHERE dept_id = 1);

-- another example is where we need to find names and salaries of teachers above the average salary

-- typical static way 

SELECT teacher_name, teacher_salary
FROM teachers
WHERE teacher_salary > 25000;


-- More dynamic

SELECT teacher_name, teacher_salary
FROM teachers
WHERE teacher_salary >
(SELECT AVG(teacher_salary)
FROM teachers);



-- VIEWS - a command that creates a virtual table based on the result of a SELECT statement


-- CREATE VIEW - a command that creates a virtual table based on the result of a SQL statement, a smaller table that is derived from a larger table

-- For example, the sales team doesnt wanna know the Credit Card details of the customers, so we can create a view without the credit card details, and for the accounting team, we can create a view with the credit card details

/*
GENERAL SYNTAX:
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;

SELECT * FROM view_name;
*/

CREATE VIEW sales_team AS
SELECT customer_id, customer_name, customer_email
FROM customers;

SELECT * FROM sales_team;

-- DROP VIEW - a command that deletes a view
DROP VIEW sales_team;

-- For Accounts team

CREATE VIEW accounts_view AS
SELECT customer_id, customer_name, customer_email, customer_credit_card
FROM customers;

SELECT * FROM accounts_view;


-- Ex:

CREATE VIEW student_view AS
SELECT roll_no, name
FROM student;

SELECT * FROM student_view;


-- INDEX - a command that creates an index on a table, helps in faster retrieval of data

-- CREATE INDEX - a command that creates an index on a table
CREATE INDEX index_name
ON table_name (column1, column2, ...);

-- DROP INDEX - a command that deletes an index
DROP INDEX index_name
ON table_name;

-- Ex:
CREATE INDEX student_index
ON student (roll_no, name);

-- Ex:
DROP INDEX student_index
ON student;
