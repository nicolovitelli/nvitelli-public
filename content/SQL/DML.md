---
title: DML – Data Manipulation Language
---

Data Manipulation Language (DDL) statements access and manipulate data in existing schema objects.

DML Statements do not implicitly commit the current transaction.

The DML Statements are:
- CALL
- DELETE
- EXPLAIN PLAN
- INSERT
- LOCK TABLE
- MERGE
- SELECT
- UPDATE

**Sources**
- [Oracle Documentation - Data Manipulation Language (DML) Statements](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Types-of-SQL-Statements.html#GUID-E1749EF5-2264-44DF-99EF-AEBEB943BED6)

---

## INSERT ALL
**Unconditional INSERT ALL**\
When using an unconditional INSERT ALL statement, each row produced by the driving query results in a new row in each of the tables listed in the INTO clauses.

**Unconditional INSERT ALL Syntax**
```sql
INSERT ALL
INTO table_1 (column_a, column_b) VALUES (column_1, column_2)
	INTO table_2 (column_c, column_d) VALUES (column_1, column_2)
	INTO table_3 (column_e, column_f) VALUES (column_1, column_2)
SELECT column_1[, column_n]
FROM source_table;
```

**Conditional INSERT ALL**\
In a conditional INSERT ALL statement, conditions can be added to the INTO clauses, which means the total number of rows inserted may be less that the number of source rows multiplied by the number of INTO clauses.

**Conditional INSERT ALL Syntax**
```sql
INSERT ALL
	WHEN condition THEN
		INTO table_1 (column_a, column_b) VALUES (column_1, column_2)
	WHEN condition THEN
		INTO table_2 (column_c, column_d) VALUES (column_1, column_2)
	WHEN condition THEN
		INTO table_3 (column_e, column_f) VALUES (column_1, column_2)
SELECT column_1, column_2
FROM source_table;
```

**Restrictions**
- Multitable inserts can only be performed on Tables, not on views or Materialized Views.
- Sequences cannot be used in the multitable insert statement. It is considered a single statement, so only one sequence value will be generated and used for all rows.

**Oracle Live SQL**
- [Unconditional INSERT ALL](https://livesql.oracle.com/apex/livesql/s/o9ninb4mh0btszce7to6vvo2q)
- [Conditional INSERT ALL](https://livesql.oracle.com/apex/livesql/s/o9nn25s98zpbyva2s8qe5fbqc)

**Sources**
- [Oracle Documentation - INSERT: multi_table_insert](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/INSERT.html#GUID-903F8043-0254-4EE9-ACC1-CB8AC0AF3423)

---

## MERGE
The MERGE Statement selects data from one or more source tables and updates or inserts it into a target table.

The MERGE statement allows you to specify a condition to determine whether to update data from or insert data into the target table.

**Syntax**
```sql
MERGE INTO target_table
USING source_table
ON (search_condition)
	WHEN MATCHED THEN
		UPDATE SET column_1 = value_1, column_2 = value_2, ...
		WHERE [condition]
		[DELETE WHERE condition]
	WHEN NOT MATCHED THEN
		INSERT (column_1, column_2, ...)
		VALUES (value_1, value_2, ...)
		WHERE [condition];
```
- *target_table*: Table on which you want to update or insert into.
- *source_table*: Source of data to be updated or inserted.
- *search_condition*: Search condition upon which the merge operation either updates or inserts.

For each row in the target table, Oracle evaluates the search condition:
- If the result is true, then Oracle updates the row with the corresponding data from the source table.
- In case the result is false for any rows, then Oracle inserts the corresponding row from the source table into the target table.

**Example**
```sql
MERGE INTO author a
USING customers c
ON (a.authorid = TO_CHAR(c.customer#))
    WHEN MATCHED THEN
        UPDATE SET a.lname = c.lastname, a.fname = c.firstname
    WHEN NOT MATCHED THEN
        INSERT (a.authorid, a.lname, a.fname)
        VALUES (c.customer#, c.lastname, c.firstname);
```
For each row in Author:
- If authorid and customer# are equal, then update the Author's lastname (a.lname) and first name (a.fname) with the Customer's last name (c.lastname) and first name (c.firstname).
- Otherwise inserts a new row in Author with Customer's data.

**Sources**
- [Oracle Documentation - MERGE](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/MERGE.html)