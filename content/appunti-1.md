# 001

When working with UNION, you can only Order By:
- any column name of the first query
- any number that represents a column of the first query
- any alias of the first query

You cannot use:
- any column name of the second (, third, etc.) query
- any alias of the second (, third, etc.) query


- A is true:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY "Last name";
-- No data found
~~~

- B is true:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY 2, cust_id;
-- No data found
~~~

- C is false:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY CUST_NO;
-- ORA-00904: "CUST_NO": invalid identifier
~~~

- D is true:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY 2, 1;
-- No data found
~~~

- E is false:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY "CUST_NO";
-- ORA-00904: "CUST_NO": invalid identifier
~~~

# 002

- A is false:
~~~sql
select count(cust_id), cust_first_name
from sh.customers
where cust_gender = 'M'
group by cust_first_name
having count(1) > 60;
-- COUNT(CUST_ID) CUST_FIRST_NAME 
-- -------------- --------------- 
--             64 Antony          
--             63 Arnand          
--             63 Baldwin         
--             63 Bartholomew     
--             64 Baxter   
~~~

- B is true:
~~~sql
select count(cust_id), cust_first_name
from sh.customers
group by cust_first_name
having count(1) > 60;
-- COUNT(CUST_ID) CUST_FIRST_NAME 
-- -------------- --------------- 
--             63 Angela          
--             64 Antony          
--             63 Arnand          
--             65 Ashley          
--             64 August  
~~~

- C is false:
~~~sql
select count(cust_id) cnt, cust_first_name
from sh.customers
group by cust_first_name
having cnt > 60;
-- ORA-00904: "CNT": invalid identifier
~~~

- D is true and E is false. Oracle's SQL queries are processed with this order:
	- FROM
	- WHERE
	- GROUP BY
	- SELECT
	- HAVING
	- ORDER BY

# 003

# 004

- A is false:
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

- C is false: DELETE FROM statement can specify multiple conditions.
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

- D is false: you cannot specify any condition in an INSERT INTO ... VALUES .. statement.

- F is false: you can specify multiple conditions in an UPDATE statement (*see example above*).

# 005

# 006

- A is false: a Constraint is enforced for every DML operation.
~~~sql
create table t (t1 number not null);
-- Table T created.

insert into t values (1);
-- 1 row inserted.

update t set t1 = null;	
-- ORA-01407: cannot update ("NICO"."T"."T1") to NULL
~~~

- B is false: a Foreign Key allows NULL values.
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