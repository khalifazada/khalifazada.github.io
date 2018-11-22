---
layout: post
title: SQL Syntax
---

![SQL](https://azure.microsoft.com/svghandler/sql-database/?width=600&height=315 "SQL")

# Beginner

SQL is case insensitive

## Querying Databases with SQL

Writing SQL query is the primary way to interact with the database.
SQL is a [declarative](https://en.wikipedia.org/wiki/Declarative_programming) statement 
language.

### `SELECT`

	SELECT columns01, column02, ... FROM recent_grads;

The *query* word `SELECT`is the most basic statement in SQL. The language reads more like English. The semicolon `;` specifies where the query ends. Thus we can *structure* a query in several lines of length.

	note: order in which columns are called matters!

### `WHERE`

The `WHERE` statement allows *filtering* the database according to a specific *constrain*.

	SELECT col01, col02
	FROM db_nm
	WHERE col01 > 0.5;

The following operators are defined `> >= < <= = <>`. The last operator `<>` is *not equal to* can also be expressed as `!=`.

### `LIMIT`

SQL comes with a statement called `LIMIT` that allows us to specify how many results we'd like the database to return as an integer value.

	SELECT col02
	FROM db_nm
	WHERE col01 > 0.5
	LIMIT 10;

Notice that `col01` is filtered but the return result is `col02`.

## Logical Operators & Ordering

### `AND` & `OR`

	SELECT [col01,...]
	FROM [db_nm]
	WHERE [cond01] AND [cond02];

In the above example `AND` can be interchanged with `OR` depending which Boolean condition is needed.

### `ORDER BY`
The returned query is ordered according to the original ordering of the data. It is possible to specify desired ordering using a `ORDER BY` statement.

	select [col01,...] from [db_nm] where [cond01,...] order by [col_name] asc;

The above query will be ordered by values in `col_name` column in an *ascending* order. Specifying `desc` will order the results in a *descending* order.

## Aggregation Functions

### `COUNT()`

	SELECT COUNT(*) FROM tbl_nm;

Will return a total count of **all** rows.

	SELECT COUNT(col_nm) FROM tbl_nm;

Will return total count of **all non-null** values in a **column**.

### `MIN()` & `MAX()`

	SELECT MIN(col_nm) FROM tbl_nm;
	SELECT MAX(col_nm) FROM tbl_nm;

### `SUM()` & `AVG()`

	SELECT SUM(col_nm) FROM tbl_nm;
	SELECT AVG(col_nm) FROM tbl_nm;

### `DISTINCT`

	SELECT DISTINCT col_nm FROM tbl_nm;

This command will select unique values from the specified columns. We could also *aggregate* unique values by specifying `AVG(DISTINCT col_name)`.

### `GROUP BY`

	SELECT col_nm_01, ... FROM tbl_nm GROUP BY col_name;

The above query will *group* all selected columns by `col_name`

## Renaming Columns with `AS`

	SELECT col_name AS new_col_nm FROM tbl_nm;

## Performing Arithmetic

Arithmetic operations are performed on a column or between columns. The expression to perform an operation is intuitive.

	SELECT ((col_nm_01 + col_nm_02) / col_nm_03) FROM tbl_nm;

## Querying Virtual Columns

### HAVING

	SELECT col_nm_01, ... FROM tbl_nm GROUP BY col_nm HAVING (condition);

When using `GROUP BY` we create a virtual column, thus `WHERE` clause cannot be used. Instead, we use `HAVING` to filter results and select a subset.
 
## Querying SQLite from Python

Python comes with SQLite module which can be imported.

	import sqlite3 as sql

### Connecting to a Database

We use `connect()` function and pass the *path* to the filename.

	conn = sql.connect('db_nm.db')

We also need a *Cursor* object that will run and execute queries.

	cursor = conn.cursor()

To execute a query we pass a string of statements in SQL to an `execute()` method and then *fetch* the results into a list of *tuples*.

	query = 'select * from tbl_nm;'
	cursor.execute(query)
	results = cursor.fetchall()

The variable `results` will contain all the returned values from the declared query.

#### Shortcut

There is a way to run queries as a shortcut by-passing a *Cursor* object altogether.

	conn = sql.connect('db_nm.db')
	query = 'select * from tbl_nm;'
	results = conn.execute(query).fetchall()

### Retrieving Specific Number of Results
To retrieve one single result (as a tuple) we use the *Cursor* method `fetchone()`. To retrieve `n` results we use `fetchmany(n)`.
Both methods return results in *increments* meaning by calling `fetchone()` the second time, second tuple will be returned.

	import sqlite3 as sql
	
	conn = sql.connect('db_nm.db')
	cursor = conn.cursor()
	
	query = 'select * from tbl_nm;'
	cursor.execute(query)
	
	single_result = cursor.fetchone()
	five_results = cursor.fetchmany(5)
	all_results = cursor.fetchall()

### Closing Connection
	conn.close()

# Intermediate

## Modifying Data

### INSERT
Inserting data into a table is done through the `INSERT` statement.

	INSERT INTO [tablename] VALUES (primary_key, val01,...);

### UPDATE
We can use the `UPDATE` statement to *change* any existing values in a table.

	UPDATE (tbl_nm)
	SET (col_nm_01 = new_val_01,...)
	WHERE (col_nm_01 = old_val_01,...);

Note that **all** rows will be updated if `WHERE` clause is not specified.

### DELETE
If we need to delete any rows from a table we can use the `DELETE` statement.

	DELETE FROM (tbl_nm)
	WHERE (cond01,...);

### Missing Values
You can retrieve any rows where a specific column is `NULL` by using the following syntax:

	SELECT * from tableName
	WHERE columnName IS NULL;

Notice that `IS` statement is used to specify where values are equal to `NULL`.

## Table Schemas

### Adding Columns
We can add a column with the `ALTER TABLE` statement:

	ALTER TABLE tableName
	ADD columnName dataType;

The statement `ADD` will *add* a column to `tableName`

### Removing Column
We can alter the table to remove a column.

	ALTER TABLE tableName
	DROP COLUMN colName;

Note that the above command is only possible with SQL, not SQLite.

### Creating Tables
#### CREATE TABLE
The syntax to create a **new** table is the following

	CREATE TABLE db_nm.tb_nm(
	id integer PRIMARY KEY,
	col_nm_01 dataType,
	.
	.
	.
	col_nm_N dataType
	);

### Relations Between Tables
To query across multiple tables and define relations between columns we can create associations between those columns .

	CREATE TABLE db_nm.tbl_nm_01(
	id integer PRIMARY KEY,
	col_nm_01 dataType,
	.
	.
	.
	col_nm_N dataType	
	FOREIGN KEY(col_nm_N) REFERENCES tbl_nm_02(col_nm_M)
	);

We are referencing `col_nm_N` from `tbl_nm_01` with `col_nm_M` from `tbl_nm_02`.

### Querying Across Foreign Keys
#### Types of Joins
##### INNER JOIN
We can use the `INNER JOIN` statement to make querying across foreign key relationships easier:

	SELECT [column1, column2, ...] from tableName1
	INNER JOIN tableName2
	ON tableName1.some_col_nm == tableName2.some_col_nm;

The above indicates that we are trying to query columns from `tableName1` **joined** with `tableName2` on 3rd column from one table with 4th column of the other.

`INNER JOIN` displays rows **only** when there's a match on the condition in both tables. Non-matching rows are excluded

##### LEFT OUTER JOIN
Displays `NULL` for all **right** side values if left side row didn't find a match.

	SELECT [col1,...] FROM tbl_nm_01
	LEFT OUTER JOIN tbl_nm_02
	ON tbl_nm_01.some_col_nm == tbl_nm_02.some_col_nm;

##### RIGHT OUTER JOIN
All **left** side rows are filled with `NULL` values if no matching row on the right was found.

	SELECT [col01,...] FROM tbl_nm_01
	RIGHT OUTER JOIN tbl_nm_02
	ON tbl_nm_01.some_col_nm == rbl_nm_02.some_col_nm;

## Creating a Database

### CREATE DATABASE

	CREATE DATABASE db_nm;

#### OWNER

We can specify which user will be the *owner* of the database. The owner is the only that can access and modify the database with an exception of *superusers*.

	CREATE DATABASE db_nm OWNER user_nm;

## Deleting Database

### DROP DATABASE

	DROP DATABASE db_nm;

## PostgreSQL

### Importing, Connecting & Closing

Similar to SQLite we create a *connect* and a *cursor* objects. Difference being, we have to specify the database name we wish to connect to as well as the username.

	import psycopg2 as postgre
	conn = postgre.connect('dbname=db_nm user=usr_nm')
	cursor = conn.cursor()
	.
	.
	.
	conn.close()

Note: default database and username is *postgres*

### Committing
Changes made are auto rolled back upon closing the connection. To make all queries and changes to data permanent they need to be *committed*.

	import psycopg2 as postgre
	
	conn = postgre.connect("dbname=db_nm user=usr_nm")
	curs = conn.cursor()
	
	query = "sql_syntax"

	conn.execute(sql_syntax)
	curs.commit()
	conn.close()

Alternatively,

	conn.autocommit = True
  
### Command Line PostgreSQL

#### Starting, Running Queries & Closing

	psql
	.
	SQL_query_syntax;
	.
	\q

#### List of Commands

* **`\?`** Lists all available commands
* **`\l`** Lists all available databases
* **`\du`** Lists users
* **`\dt`** List tables
* **`\dp tbl_nm`** Lists permissions for a specific table

#### Connecting to a database

	psql -d db_nm

#### Creating Users

	CREATE ROLE user_nm WITH LOGIN PASSWORD 'password';

Adding `CREATEDB` after `WITH` will allow `user_nm` to create databases as well.

Complete list of commands are available [here](https://www.postgresql.org/docs/9.4/static/sql-createrole.html).

##### Adding Permissions
To *grant* permission to users for running queries the `GRANT` statement is used.

	GRANT SELECT, INSERT, DELETE ON tbl_nm TO user_nm;
	GRANT ALL PRIVILIGES ON tbl_nm TO user_nm;

The above queries grant *limited* or *complete* priviliges to a specific user.

##### Removing Permissions

To remove a permission we must *revoke* it.

	REVOKE SELECT, INSERT ON tbl_nm FROM user_nm;
	REVOKE ALL PRIVILEGES ON tbl_nm FROM user_nm;

#### Deleting Users

Once all permissions have been revoked the user can be *dropped*

	DROP ROLE user_nm;

#### Superusers

*Superusers* are able to override all access and restrictions.

	CREATE ROLE user_nm WITH LOGIN PASSWORD 'psswrd' SUPERUSER;

# Advanced

## Introduction to Indexing

### CREATE INDEX

	CREATE INDEX index_nm ON tbl_nm(col_nm);

The above query will *create* an index based on the specified `col_nm` within `tbl_nm` and name the index as `index_nm`.

To avoid creating an index name that already exists we can use the `IF NOT EXISTS` clause.

	CREAT INDEX IF NOT EXISTS index_nm ON tbl_nm(col_nm);

## Multi-Column Indexing

To create a multi-column index, we use the same `CREATE INDEX` syntax as before but instead specify **2** columns in the `ON` statement:

	CREATE INDEX index_nm ON tbl_nm(col_nm_1, col_nm_2);

### Covering Index

When an index contains all of the information necessary to answer a query, it's called a **covering index**.

	A sample query will produce the following answer:
```
query_plan = conn.execute(
				"EXPLAIN QUERY PLAN 
				SELECT col_01, col_02 
				FROM tbl_nm 
				WHERE col_01 > n 
				AND col_02 < m;").fetchall()
```

Its *covering* because the index covers all of the selected columns. 
