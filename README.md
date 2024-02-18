
<details><summary>SQL Layout</summary><p>
  

```bash
___________________________________________________________________________________________________________________________________________________________________
Structure 			|Data				    |Query		       |Security	             |Consistency                         |
DDL : Data Definition Language	|DML : Data Manipulation Language   |DQL : Data Query Language |DCL : Data Control Language  |TCL : Transitional Control Language |
________________________________|___________________________________|__________________________|_____________________________|____________________________________|
Create				|Insert              		    |Select		       |Grand		             |*Commat			          |
Alter				|Updata				    |*Clause		       |*With Grand	             |*Rollback			          |	
Drop				|Delete				    |*Grouping		       |Deny   	                     |*Save Point			  |
Truncate			|Merge				    |*Distinct		       |Revoke		             |				          |
Select Into			|View				    |*Alias	               |			     |				          |
Index				|Temporary Tables		    |*Join 		       |			     |				          |
				|Procedure			    |*Set Operators            |			     |				          |
				|				    |*Sub Query		       |			     |				          |
				|				    |*Function	               |			     |				          |
________________________________|___________________________________|__________________________|_____________________________|____________________________________|


```

--------------------------------------------------------------------------------------------------
<p>
</details>

<details><summary>Constraints</summary><p>


```bash

What is a constraint, and why use constraints?
A set of conditions defining the type of data that can be input into each column of a table. 
Constraints ensure data integrity in a table and block undesired actions.

_____________________________________________________________________________________
Integrity	|Entity Integrity      |Referential Integrity     |Domain Integrity |
________________|______________________|__________________________|_________________|
--Constraint	|PK		       |FK	    	          |Data Type	    |
------------	|Identity	       |			  |Default	    |
------------	|Unique		       |			  |Null/Not Null    |
________________|______________________|__________________________|_________________|
--DB Object	|Index		       |Trigger			  |Rule		    |
------------	|Trigger	       |			  |Trigger	    |
________________|______________________|__________________________|_________________|
```
```bash
• NOT NULL Constraint: Ensures that a column cannot have a NULL value.
You must use the IS NULL or IS NOT NULL operators to check for a NULL value.

• DEFAULT Constraint: Provides a default value for a column when none is specified.

• UNIQUE Constraint: Ensures that all values in a column are different.

• PRIMARY Key: Uniquely identifies each row/record in a database table.

• FOREIGN Key: Uniquely identifies row/record in any of the given database tables. The relationship between 2 tables matches the Primary Key in one of the tables with a Foreign Key in the second table.

• CHECK Constraint: The CHECK constraint ensures that all the values in a column satisfy certain conditions.

• IDENTITY Constraint: a specific data type and constraint combination that automatically generates unique, incrementing values for a specific column in a table. 

• INDEX: Used to create and retrieve data from the database very quickly. it is assigned a ROWID for each row before it sorts out the data.

• TRIGGER: is like an automated sentry that reacts to specific events happening within the database
```
--------------------------------------------------------------------------------------------------

<p>
</details>

<details><summary>DDL : Data Definition Language</summary><p>

# Structure


## DDL => Database 
```bash

Syntax> CREATE DATABASE DatabaseName;
Syntax> DROP DATABASE DatabaseName;
Syntax> SHOW DATABASES;
Syntax> USE DatabaseName;
```
## DDL => Create Table
```bash
Syntax> Create Table:
CREATE TABLE table_ name(
column1 datatype,
column3 datatype,
.....
columnN datatype,
PRIMARY KEY( column_name ) );
----------------------------------------------------------------
SQL> CREATE TABLE CUSTOMERS(
ID 	INT NOT NULL,
NAME 	VARCHAR (20) NOT NULL,
AGE 	INT NOT NULL,
ADDRESS CHAR (25),
SALARY 	DECIMAL (18, 2),
PRIMARY KEY (ID));
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Creating a Table from an Existing Table:
----------------------------------------------------------------
Syntax> CREATE TABLE NEW_TABLE_NAME AS
SELECT [ column1, column 2...columnN ]
FROM EXISTING_TABLE_NAME
[WHERE condition]
```
## DDL => Drop or Truncate Table
```bash
Syntax> Drop or Truncate TABLE table_ name;
```


## DDL => Alter Table
```bash
Add New Column:
Syntax> ALTER TABLE EXISTING_TABLE_NAME
ADD COLUNM columnN datatype
```
```bash
Change Exisit Column Data Type:
Syntax> ALTER TABLE EXISTING_TABLE_NAME
ALTER COLUNM columnN datatype
```
```bash
Alter Table Add Constraint:
Syntax> ALTER TABLE EXISTING_TABLE_NAME
ADD CONSTRAINT columnN Constraint
```
```bash
Drop Constraint:
Syntax> ALTER TABLE EXISTING_TABLE_NAME
DROP CONSTRAINT columnN Constraint
```
```bash
Drop Column:
Syntax> ALTER TABLE EXISTING_TABLE_NAME
DROP COLUNM columnN
```


## DDL => SELECT INTO 
```bash
SELECT INTO: The Direct Copycat
----------------------------------------------------------------
Definition: Takes a SELECT statement and directly inserts the results into a new table (permanent or temporary).
----------------------------------------------------------------
Syntax> SELECT [ column1, column 2...columnN ]
FROM EXISTING_TABLE_NAME
[WHERE condition]
INTO NEW_TABLE_NAME;
```
## DDL => INDEX
```bash
What is an index?
----------------------------------------------------------------
A special data structure related to a database table and used for storing its important parts and enabling faster data search and retrieval. 
Indexes are especially efficient for large databases, where they significantly enhance query performance.
```
```bash
What types of indexes?
----------------------------------------------------------------
• Clustered Index: One special index that physically orders table data based on the indexed column(s). 
This can be very fast for retrieving data sorted by the indexed column but can impact write performance.
• Non-Clustered Index: The most common type, separate from the actual data, pointing to rows based on the indexed column(s). 
Offers a good balance between read and write performance.
• Unique Index: Ensures each value in the indexed column is unique, enforcing data integrity and uniqueness constraints
```
```bash
Syntax> CREATE INDEX:
CREATE INDEX [Index Type] index_name
ON table_ name;
----------------------------------------------------------------
Syntax> Single Column Indexes:
CREATE INDEX [Index Type] index_name
ON table_name (column_name);
----------------------------------------------------------------
Syntax> DROP INDEX:
DROP INDEX index_name;
```
```bash
When should indexes be avoided?
The following guidelines indicate when the use of an index should be reconsidered.
• Indexes should not be used on small tables.
• Tables that have frequent, large batch updates or insert operations.
• Indexes should not be used on columns that contain a high number of NULL values.
• Columns that are frequently manipulated should not be indexed.

```
<p>
</details>

<details><summary>DML : Data Manipulation Language</summary><p>

# Data
## DML => Insert 
```bash

Syntax> INSERT INTO TABLE_NAME
(column1, column2, column3,...columnN)]
VALUES (value1, value2, value3 ,...valueN);
```
##  DML => Update

```bash
Syntax> UPDATE table_name
SET column1 = value1, column2
= value2...., columnN = valueN
WHERE [condition];
```


## DML => Delete
```bash

Syntax> DELETE FROM table_name
WHERE [condition];
```


## DML => Merge 
```bash

Syntax> MERGE INTO target_table USING source_table
ON merge_condition
WHEN MATCHED THEN
  UPDATE SET column1 = value1, column2 = value2, ...
 
[WHEN NOT MATCHED THEN INSERT (column1, column2, ...) VALUES (value1, value2, ...)]
```

## MERGE for SCD:
```bash
Implementing SCD types: The MERGE statement is particularly helpful for implementing different types of Slowly Changing Dimensions (SCDs). Here's how:
Type 1: Use UPDATE within WHEN MATCHED to update existing dimensions with new values.
Type 2: Use INSERT within WHEN NOT MATCHED to create new records with new values, while marking existing ones as inactive.
Type 3: Use a combination of UPDATE and INSERT with different conditions to handle historical data and new values.
----------------------------------------------------------------
Limitations:
----------------------------------------------------------------
Complex transformations: While MERGE can handle basic transformations, complex data manipulations might require additional logic or separate steps in your ETL process.
Performance: For very large datasets, the MERGE statement might be less efficient than separate INSERT and UPDATE statements due to its overhead.
----------------------------------------------------------------
Alternatives:
----------------------------------------------------------------
Separate statements: For simple ETL jobs, using separate INSERT and UPDATE statements can be more straightforward and efficient.
ETL tools: Many ETL tools offer specialized features for SCD implementations and complex data transformations.
```

##  DML => Create or Drop Views or Temp Tables 
```bash
Views
----------------------------------------------------------------
Definition: A virtual table based on a stored SQL query. They do not store data themselves but dynamically retrieve it from underlying tables based on the query definition.
----------------------------------------------------------------
Types:
Simple views: Based on a single SELECT statement.
Joined views: Combine data from multiple tables using JOIN operations.
Union views: Merge data from multiple SELECT statements into a single result set.
Materialized views: Pre-computed versions of views, potentially improving performance but requiring maintenance.
Indexed views: Allow efficient access using indexes, but come with additional overhead.
Security views: Restrict data access based on user permissions.
----------------------------------------------------------------
Use Cases:
• Structure data in a way that users or classes of users find natural or intuitive.
• Restrict access to the data in such a way that a user can see and (somet imes) modify exactly what they need and no more.
• Summarize data from various tables which can be used to generate reports.
```
```bash
Syntax> Create Views 
CREATE [VIEW | TEMPORARY TABLE] view_name AS
SELECT column1, column2.....
FROM table_name
WHERE [condi tion];
```
```bash
Syntax> Dropping Views
DROP VIEW view_name;
```
## DML => Create or Drop Temporary Tables 
```bash

Temporary Tables
----------------------------------------------------------------
Definition: Tables created within a database session that exist only for the duration of that session. Disappear automatically when the session ends or is explicitly dropped.
----------------------------------------------------------------
Types:
Local temporary tables: Visible only to the current user session.
Global temporary tables: Accessible to all user sessions in the database, but exist only until the database is restarted.
----------------------------------------------------------------
Use Cases:
Storing intermediate results for complex calculations or data manipulation within a query.
Creating temporary workspaces for calculations or data transformations.
Sharing data within a session without modifying permanent tables.
```
```bash


Syntax> CREATE [GLOBAL] TEMPORARY TABLE AS Temp_Table_Name
column1 datatype,
column3 datatype,
.....
columnN datatype,
PRIMARY KEY( one or more columns ) );
```
```bash

Syntax> Dropping TEMPORARY TABLE
DROP TABLE Temp_Table_Name;
```
## Choosing between Views and Temporary Tables:		
```bash
Use a view when:
You need to present data in a specific format, filtered or aggregated, on a recurring basis.
You want to improve query readability and maintainability.
You need to restrict data access based on user permissions.
Use a temporary table when:
You need to store intermediate results for a specific query or session.
You need a temporary workspace for calculations or data transformations.
You don't want to modify permanent tables.
----------------------------------------------------------------
Key Considerations:
----------------------------------------------------------------
Performance: Views can be slower than accessing underlying tables directly, while temporary tables generally have lower performance than permanent tables.
Data Integrity: Views reflect changes in underlying tables, while temporary tables are isolated and do not affect data integrity.
Concurrency: Views and temporary tables can be used concurrently by different users, but be aware of potential locking issues.
```
## DML => Procedure

```bash
Procedures
----------------------------------------------------------------
Procedures are like pre-packaged mini-programs that encapsulate a series of SQL statements and logic. They offer several advantages for managing database operations and code reusability.
----------------------------------------------------------------
What Procedures Do:
----------------------------------------------------------------
Perform multiple actions: They execute a sequence of SQL statements, potentially including DML (Data Manipulation Language) for modifying data, DQL (Data Query Language) for retrieving data, and even control flow statements like loops and conditional branches.
Modularity and reusability: They encapsulate complex logic into reusable units, making code more organized and easier to maintain.
Parameters and input/output: They can accept input parameters and return output values, allowing flexibility in data manipulation and processing.
Transaction control: They can manage transactions explicitly using BEGIN, COMMIT, and ROLLBACK statements, ensuring data consistency.
Stored in the database: They are stored within the database itself, accessible by authorized users from different applications.
```

<p>
</details>

<details><summary> Data Query Language </summary><p>


# Query
## DQL => Select
```bash
Select is a fundamental statement used to retrieve data from tables and views.
----------------------------------------------------------------
SELECT:
Syntax> SELECT column1, column2,columnN FROM table_ name;
```
## DQL => Clause
```bash
What is a clause?
A condition imposed on a SQL query to filter the data to obtain the desired result.
Some examples are WHERE, LIMIT, HAVING, LIKE, AND, OR, ORDER BY, etc.
----------------------------------------------------------------
*WHERE Clause (Filter) is used within a SELECT, UPDATE, or DELETE statement to specify a condition
that must be true for a row to be included in the result set or affected by the operation.
```
```bash
Syntax> SELECT column1, column2,
column
FROM table_name
WHERE [condition]
```
```bash
You can specify a condition using 
----------------------------------------------------------------
Comparison operators: Use operators like =, >, <, >=, <=, and != to compare values from columns.
Logical operators: Combine conditions using AND, OR, and NOT to create complex filters.
Functions: Use built-in functions for calculations, string manipulations, and other operations within the condition.)
```
### **The AND | OR Operator
```bash
Logical Operators:
AND: Returns TRUE only if both conditions connected by AND are TRUE. 
OR: Returns TRUE if at least one of the conditions connected by OR is TRUE. 
```
```bash
Syntax> SELECT column1, column2,
column
FROM table_name
WHERE [condition1] AND | OR [condi tion2]...AND | OR [conditionN];
```
```bash

**LIKE | Wildcard
----------------------------------------------------------------
Used for pattern matching in strings.
• The percent sign (%)
• The underscore (_)
```
```bash
Syntax> SELECT FROM table_name
WHERE column [LIKE | Wildcard] ['XXXX%' | '%XXXX%' | 'XXXX_' | '_XXXX' | '_XXXX_']
```
```bash

*ORDER BY Clause & SORTING
----------------------------------------------------------------
Results ascending or descending order, ascending order by default.
```
```bash

Syntax>  SELECT column -list
FROM table_name
[WHERE condition]
[ORDER BY column1, column2,
.. columnN] [ASC | DESC];
```
```bash

*TOP, LIMIT or ROWNUM Clause
----------------------------------------------------------------
Limiting Results:
```
```bash
Syntax> SELECT TOP number | percent column name(s)
FROM table_name
WHERE [condition]
```
```bash

TOP (Transact-SQL): Returns the specified number of rows from the beginning.
SQL> SELECT TOP 3 * FROM CUSTOMERS;
```
```bash

LIMIT (MySQL, PostgreSQL): Returns the specified number of rows starting from a specific offset. 
SQL> SELECT * FROM CUSTOMERS LIMIT 3;
```
```bash

ROWNUM (Oracle): Returns a row number for each row, allowing filtering based on position.
SQL> SELECT * FROM CUSTOMERS WHERE ROWNUM <= 3;
```
## DQL => Grouping
```bash

GROUP BY: Groups rows based on specified columns. 
```
```bashSyntax> SELECT column1, column2
FROM table_name
WHERE [ conditions ]
GROUP BY column1, column2
ORDER BY column1, column2
```
```bash

**HAVING Clause
---------------------------------------------------------
HAVING: Filters groups based on conditions after GROUP BY.
```
```bash
Syntax> SELECT column1, column2
FROM table1, table2
WHERE [ conditions ]
GROUP BY column1, column2
HAVING [ conditions ]
ORDER BY column1, column2
```
## DQL => Distinct
```bash
DISTINCT: Removes duplicate rows from the result set.
```
```bash
Syntax> SELECT DISTINCT column1,
column 2,.....co lumnN
FROM table_name
WHERE [condi tion]
```


## DQL => Alias

```bash
What is an alias?
A temporary name given to a table (or a column in a table) while executing a certain SQL query. 
```
```bash
TABLE ALIAS:
----------------------------------------------------------------
Syntax> SELECT column1, column 2....
FROM table_name AS alias_name 
WHERE [condition];
```
```bash
COLUMN ALIAS:
----------------------------------------------------------------
Syntax> SELECT column name AS alias_name FROM table_name
WHERE [condi tion];
```


## DQL => Joins
```bash
What is a join?
A clause used to combine and retrieve records from two or multiple tables. SQL tables can be joined based
on the relationship between the columns of those tables. Check out our SQL joins tutorial for more context. 
----------------------------------------------------------------
What types of joins do you know?
• INNER JOIN:  – returns only those records that satisfy a defined join condition in both (or all) tables. It's a default SQL join.
• LEFT JOIN:  – returns all records from the left table and those records from the right table that satisfy a defined join condition.
• RIGHT JOIN: – returns all records from the right table and those records from the left table that satisfy a defined join condition.
• FULL JOIN: – returns all records from both (or all) tables. It can be considered as a combination of left and right joins.
```
```bash
Syntax> SELECT table1.column1, table2.colum n2... FROM table1
[INNER JOIN | LEFT JOIN | RIGHT JOIN | FULL JOIN] table2
ON table1.common_field = table2.common_field;
```
```bash
• SELF JOIN: is used to join a table to itself as if the table were two tables, temporarily renaming at least one table in the SQL statement.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
SQL> SELECT
a.ID, b.NAME, a.SALARY
FROM CUSTOMERS a, CUSTOMERS b
WHERE a.SALARY < b.SALARY;
```



## DQL => Set Operators

```bash
*Set Operators
----------------------------------------------------------------
The Set operator is used to combine and manipulate the results of two or more SELECT.
----------------------------------------------------------------
What set operators do you know?
UNION – returns the records obtained by at least one of two queries (excluding duplicates)
UNION ALL – returns the records obtained by at least one of two queries (including duplicates)
INTERSECT – returns the records obtained by both queries
EXCEPT (called MINUS in MySQL and Oracle) – returns only the records obtained by the first query but not the second one
----------------------------------------------------------------
To use this, each SELECT statement must have
• The same number of columns selected
• The same number of column expressions
• The same data type
• Have them in the same order
```
```bash
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]

[UNION | UNION ALL | INTERSECT | EXCEPT]

SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]
```



## DQL => Sub Query

```bash
*Sub Query	
----------------------------------------------------------------
What is a subquery?
Also called an inner query; a query placed inside another query, or an outer query. A subquery may occur in
the clauses such as SELECT, FROM, WHERE, UPDATE, etc. It's also possible to have a subquery inside another subquery.
The innermost subquery is run first, and its result is passed to the containing query (or subquery).
----------------------------------------------------------------
What types of SQL subqueries do you know?
Single-row – returns at most one row.
Multi-row – returns at least two rows.
Multi-column – returns at least two columns.
Correlated – a subquery related to the information from the outer query.
Nested – a subquery inside another subquery.
```
```bash
Syntax> SELECT outer_column1, outer_column2, ...
FROM outer_table_name
WHERE condition (
  SELECT inner_column1, inner_column2, ...
  FROM inner_table_name
  WHERE inner_condition
);
```
## DQL => Function

```bash
What types of SQL functions do you know?
----------------------------------------------------------------
Return a single value: Their primary purpose is to calculate or transform data and return a single result.
Aggregate functions – work on multiple, usually grouped records for the provided columns of a table, and return a single value (usually by group).
Scalar functions – work on each individual value and return a single value.
On the other hand, SQL functions can be built-in (defined by the system) or user-defined (created by the user for their specific needs).
```
```bash

What aggregate functions do you know?
----------------------------------------------------------------
AVG() – returns the average value
SUM() – returns the sum of values
MIN() – returns the minimum value
MAX() – returns the maximum value
COUNT() – returns the number of rows, including those with null values
FIRST() – returns the first value from a column
LAST()– returns the last value from a column
```
```bash
What scalar functions do you know?
----------------------------------------------------------------
LEN() (in other SQL flavors – LENGTH()) – returns the length of a string, including the blank spaces
UCASE() (in other SQL flavors – UPPER()) – returns a string converted to the upper case
LCASE() (in other SQL flavors – LOWER()) – returns a string converted to the lower case
INITCAP() – returns a string converted to the title case (i.e., each word of the string starts from a capital letter)
MID() (in other SQL flavors – SUBSTR()) – extracts a substring from a string
ROUND() – returns the numerical value rounded to a specified number of decimals
NOW() – returns the current date and time
```
```bash

What are character manipulation functions?
----------------------------------------------------------------
Character manipulation functions represent a subset of character functions, and they're used to modify the text data.
----------------------------------------------------------------
CONCAT() – joins two or more string values appending the second string to the end of the first one
SUBSTR() – returns a part of a string satisfying the provided start and end points
LENGTH() (in other SQL flavors – LEN()) – returns the length of a string, including the blank spaces
REPLACE() – replaces all occurrences of a defined substring in a provided string with another substring
INSTR() – returns the numeric position of a defined substring in a provided string
LPAD() and RPAD() – return the padding of the left-side/right-side character for right-justified/left-justified value
TRIM() – removes all the defined characters, as well as white spaces, from the left, right, or both ends of a provided string
```


</p>

</details>

<details><summary>DCL : Data Control Language
</summary><p>
  


# Security
## DCL => Grand
```bash
Grants specific privileges (e.g., SELECT, INSERT, UPDATE, DELETE) on a database object (e.g., table, view, procedure) to a user or group.
```
```bash
Syntax> GRANT privilege ON object_name TO user/group;
```
## DCL => Deny 
```bash
Explicitly prohibits specific privileges on an object to a user or group, even if they have been granted elsewhere.
```
```bash
Syntax> DENY privilege ON object_name TO user/group;
```


## DCL  => Revoke

```bash
Removes previously granted privileges from a user or group.
```
```bash
Syntax> REVOKE privilege ON object_name FROM user/group;
```

</p>
</details>


<details><summary>TCL : Transitional Control Language
</summary><p>

# Consistency
## TCL: Transitional Control Language
```bash
Transactions have four standard properties, usually referred to by the acronym ACID.
• Atomicity: ensures that all operations within the work unit are completed successfully.
Otherwise, the transaction is aborted at the point of failure, and all the previous
operations are rolled back to their former state.
• Consistency: ensures that the database properly changes states upon a successfully committed transaction.
• Isolation: enables transactions to operate independently of and transparent to each other.
• Durability: ensures that the result or effect of a committed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
transaction persists in case of a system failure.
----------------------------------------------------------------
The following commands are used to control transactions.
```
## TCL => COMMIT
```bash
• COMMIT: to save the changes.
----------------------------------------------------------------
Syntax> COMMIT;
```
## TCL => ROLLBACK
```bash
• ROLLBACK: to roll back the changes.
----------------------------------------------------------------
Syntax> ROLLBACK;
```
## TCL => SAVEPOINT
```bash
• SAVEPOINT: creates points within the groups of transactions to ROLLBACK.
----------------------------------------------------------------
Syntax> SAVEPOINT SAVEPO INT_NAME;
ROLLBACK TO SAVEPO INT_NAME;
```
## TCL => SET TRANSACTION
```bash
• SET TRANSACTION: Places a name on a transaction.
----------------------------------------------------------------
Syntax> SET TRANSACTION [ READ WRITE | READ ONLY ];
```
## TCL => The RELEASE SAVEPOINT
```bash
• The RELEASE SAVEPOINT
----------------------------------------------------------------
Syntax> RELEASE SAVEPOINT
SAVEPOINT NAME;
```

--------------------------------------------------------------------------------------------------
</p>
</details>  

