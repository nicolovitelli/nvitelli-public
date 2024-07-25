# 001

When working with UNION, you can only Order By:
- any column name of the first query
- any number that represents a column of the first query
- any alias of the first query

You cannot use:
- any column name of the second (, third, etc.) query
- any alias of the second (, third, etc.) query

C and E would return the error `ORA-00904: "CUST_NO": invalid identifier`.

# 002

WHERE and HAVING have two main differences:
- WHERE is used to filter data before it is aggregated and does not allow Aggregate Functions
- HAVING is used to filter the data after it is aggregated and does allow Aggregate Functions
- both can be used in the same SQL query
- neither WHERE nor HAVING can use column aliases since they're processed before SELECT:
	- FROM
	- WHERE
	- GROUP BY
	- SELECT
	- HAVING
	- ORDER BY

# 003

# 004

- A is incorrect:
When using INSERT INTO ... VALUES .. you are only specifying values for ONE row.
~~~sql
create table t (t1 number);
-- Table T created.

insert into t (t1) values (1);
-- 1 row inserted.
~~~

- B is true:
UPDATE statements can modify multiple rows in one statement. Multiple conditions are also allowed in an UPDATE statement.
~~~sql
create table t (t1 number);
-- Table T created.

insert into t (t1) values (1);
insert into t (t1) values (2);
insert into t (t1) values (3);
-- 1 row inserted.
-- 1 row inserted.
-- 1 row inserted.

update t set t1 = 0
where t1 = 1 or t1 = 3;
-- 2 rows updated.
~~~

- C is incorrect: DELETE FROM statement can specify multiple conditions.
~~~sql
create table t (t1 number);
-- Table T created.

insert into t (t1) values (1);
insert into t (t1) values (2);
insert into t (t1) values (3);
-- 1 row inserted.
-- 1 row inserted.
-- 1 row inserted.

delete from t where t1 = 1 or t1 = 3;
-- 2 rows deleted.
~~~

- D is incorrect: you cannot specify any condition in an INSERT INTO ... VALUES .. statement.

- F is incorrect: you can specify multiple conditions in an UPDATE statement (*see example above*).

# 005

# 006

- A is incorrect: a Constraint is enforced for every DML operation.
~~~sql
create table t (t1 number not null);
-- Table T created.

insert into t values (1);
-- 1 row inserted.

update t set t1 = null;	
-- ORA-01407: cannot update ("NICO"."T"."T1") to NULL
~~~

- B is incorrect: a Foreign Key allows NULL values.
~~~sql
create table t (t1 number unique);
-- Table T created.
create table x (x1 number, foreign key (x1) references t (t1));
-- Table X created.

insert into t values (null);
-- 1 row inserted.
~~~

- C is true (*see example above*)
- D is true.
~~~sql
create table t (t1 number, t2 number, primary key (t1, t2));
-- Table T created.
~~~

# 007

# 008

[Oracle Documentation - Wildcard Characters](https://docs.oracle.com/cd/F49540_01/DOC/inter.815/a67843/cqspcl.htm#20687)

# 009

# 010

~~~sql
create table members (
    member_id varchar2(6) not null,
    first_name varchar2(50),
    last_name varchar2(50) not null,
    address varchar2(50)
);
-- Table MEMBERS created.

SELECT member_id, ' ', first_name, ' ', last_name "ID FIRSTNAME LASTNAME"
FROM members;
-- ORA-00918: column ambiguously defined
~~~

original answer was D - It executes successfully and displays the column details in three separate columns and replaces only the last column heading with the alias.

# 011

When a DROP TABLE statement is executed:
- the table data and structure (including indexes) are deleted
- all views and synonyms related to the table remain but are invalidated
- any pending transaction is committed

[Oracle Documentation - DROP TABLE](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/DROP-TABLE.html)

# 012

# 013

- A is incorrect: you cannot specify more than one table in a DELETE statement
- B is incorrect: the WHERE clause is missing in the condition
- C is correct; the statement will delete all rows from the ORDERS table which have an order_total less than 1000
- D is incorrect: you cannot specify a column name in a DELETE statement

# 014

# 015

# 016

# 017

# 018

the correct answer is D:
- despite the statement does not have any syntax issue, the condition `(category_id = 12 and category_13)` doesn't make sense. a row cannot have both values in the category_id column, therefore the statement would return no rows. the correct condition would be `(category_id = 12 or category_13)`.

# 019

# 020

# 021

correct answer is D:
- when using ORDER BY, you can only specify:
	- a column name
	- a number that represents a column (1 = first column of the table)
	- a column alias
moreover, by default the ORDER BY clause uses an ASCENDING order.

