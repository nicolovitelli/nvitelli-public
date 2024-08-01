---
title: Other Database Objects
---

## CREATE INDEX
Indexes are optional structures associated with tables and clusters that allow SQL queries to execute more quickly against a table.

**Notes about Indexes**
- Indexes are logically and physically independent of the data in the associated table. Being independent structures, they require storage space.
- You can create or drop an index without affecting the base tables, database applications, or other indexes.
- The database automatically maintains indexes when you insert, update, and delete rows of the associated table.
- If you drop an index, all applications continue to work. However, access to previously indexed data might be slower.
- To create an index in your own schema, you must have the CREATE ANY INDEX system privilege.
- LONG columns cannot be indexed.
- By default, Oracle creates B-Tree Indexes.

**Syntax**
```sql
CREATE [UNIQUE] INDEX index_name
	ON table_name (column_1, column_2, ... column_n)
	[COMPUTE STATISTICS];
```

- UNIQUE: Indicates that the combination of values in the indexed columns must be unique.
- *index_name*: the name to assign to the index.
- *table_name*: the name of the table in which to create the index.
- COMPUTE STATISTICS: tells Oracle to collect stats during the creation of the index. They are then used by the optimizer to choose a "plan of execution" when SQL Statements are executed.

**Function-based Index Example**
```sql
CREATE INDEX customer_name_idx
	ON customers (UPPER(customer_name));
```

**Sources**
- [Oracle Documentation - CREATE INDEX](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/CREATE-INDEX.html)

---

## ALTER INDEX
```sql
ALTER INDEX index_name
	RENAME TO new_index_name;
```

**Sources**
- [Oracle Documentation - ALTER INDEX](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/ALTER-INDEX.html)

---

## DROP INDEX
```sql
DROP INDEX index_name;
```

**Sources**
- [Oracle Documentation - DROP INDEX(https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/DROP-INDEX.html)

---

## Synonyms
A Synonym is an alternative name for objects such as tables, views, sequences, etc.
You can perform tasks such as creating synonyms, using synonyms, and dropping synonyms.

Synonyms allow underlying obejcts to be renamed or moved, where only the synonym must be redefined and applications based on the synonym continue to function without modification.

To create a Private Synonym, you need privileges depending on where you are creating the Synonym:
- In your schema: CREATE SYNONYM
- In another's user schema: CREATE ANY SYNONYM

**Data Dictionary Views**
The following are Data Dictionary Views related to Synonyms:
- DBA_SYNONYMS
- ALL_SYNONYMS
- USER_SYNONYMS

**Sources**
- [Oracle Documentation - Managing Synonyms](https://docs.oracle.com/en/database/oracle/oracle-database/21/admin/managing-views-sequences-and-synonyms.html)

---

## CREATE SYNONYM
```sql
CREATE [OR REPLACE] [PUBLIC] SYNONYM [schema.]synonym_name
	FOR [schema.]object_name;
```

| **KEYWORD OR ARGUMENT** |                              **DESCRIPTION**                              |   **DATATYPE**   | **OPTIONAL?** |   **DEFAULT VALUE**   |                                                                         **NOTES**                                                                         |
|:-----------------------:|:-------------------------------------------------------------------------:|:----------------:|:-------------:|:---------------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------:|
|        OR REPLACE       |          allows you to recreate the Synonym if it already exists          |         -        |      Yes      |           -           |                                                                             -                                                                             |
|          PUBLIC         | if specified, the Synonym is a Public Synonym and accessible to all users |         -        |      Yes      |           -           | If omitted, the Synonym will be a Private Synonym.                                                                                                        |
|          *schema*         |                           the appropriate schema                          | Any String value |      Yes      | Current user's schema |                                                                             -                                                                             |
| *object\_name* |       the name of the object for which you are creating the Synonym       | Any String value |       No      |           -           | Can be one of the following: table view sequence stored procedure function package materialized view java class schema object user-defined object synonym |

**Example**
```sql
CREATE PUBLIC SYNONYM public_emp FOR scott.emp;
```

The above statement creates a Public Synonym named public_emp on the emp table contained in the schema of scott.

**Sources**
- [Oracle Documentation - CREATE SYNONYM](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/CREATE-SYNONYM.html#GUID-A806C82F-1171-478E-A910-F9C6C42739B2)

---

## DROP SYNONYM
```sql
DROP [PUBLIC] SYNONYM synonym_name;
```

**Sources**
- [Oracle Documentation - DROP SYNONYM](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/DROP-SYNONYM.html)

---

## Views
A View is a logical representation of a table or combination of tables. In essence, a View is a stored query.

**Notes about Views**
- A View derives its data from the tables on which it is based. These tables are called base tables.
- All operations performed on a View actually affect the base table of the View.
- You can use Views in almost the same way as tables. You can query, update, insert into, and delete from Views.

**Restrictions on DML Operations**
Rows can be inserted into, updated in, or deleted from a base table using a View. Although there are some restrictions:
- You need the appropriate privigiles on the base tables to run DML Statements.
- DML Operations are not allowed on Views with SET or DISTINCT operators, GROUP BY clause, Group Functions or Subqueries.
- If a View is defined with WITH CHECK OPTION, DML Operations are not allowed if the View cannot select the row from the base table.
- If a NOT NULL column that does not have a DEFAULT clause is omitted from the View, then a row cannot be inserted into the base table using the View.
- INSERT and UPDATE Operations are not allowed on Views with expressions such as DECODE.
- Any DML Operation on a Join View can modify only one underlying base table at a time.
	- Rows from a Join View can be updated if the Join column keys in the base tables are UNIQUE. This does not apply on Views with the WITH CHECK OPTION clause.
	- Rows from a Join View can be deleted can be deleted as long as there is exactly one key-preserved table in the Join. This does not apply on Views with the WITH CHECK OPTION clause.
	- An INSERT Statement must not explicitly or implicitly refer to the columns of a non-key-preserved table. This does not apply on Views with the WITH CHECK OPTION clause.

**Key-Preserved Tables**
A table is key-preserved if every key of the table can also be a key of the result of the Join that is based on the table.

Given these two tables:\
CUSTOMERS
![CUSTOMERS](https://i.imgur.com/AIUfGT4.png)

ORDERS
![ORDERS](https://i.imgur.com/dx5XsN0.png)

And the following view:
```sql
CREATE OR REPLACE VIEW cust_ord AS
SELECT o.order#, o.orderdate, o.shipcost, c.customer#, c.lastname, c.firstname
FROM orders o JOIN customers c ON o.customer# = c.customer#;
```

The key-preserved table is Customers, because customer# is used in the Join condition.
Therefore you cannot perform any DML Operation on the Customers table, but only on the Orders table.

```sql
> INSERT INTO cust_ord (customer#, lastname, firstname) VALUES ('1234', 'John', 'Smith');
-- ORA-01779: cannot modify a column which maps to a non key-preserved table

> INSERT INTO cust_ord (order#, orderdate, shipcost) VALUES ('1235', TO_DATE('31-DEC-2022'), 19.99);
-- 1 row inserted.
```

**Sources**
- [Oracle Documentation - Managing Views](https://docs.oracle.com/en/database/oracle/oracle-database/21/admin/managing-views-sequences-and-synonyms.html)

---

## CREATE VIEW
You can create Views using the CREATE VIEW Statement. Each View is defined by a query that references tables, materialied Views, or other Views.

To create a View, you need privileges depending on where you are creating the View:
- In your schema: CREATE VIEW
- In another's user schema: CREATE ANY VIEW

**Syntax**
```sql
CREATE [OR REPLACE] VIEW view_name AS
query
[WITH CHECK OPTION];
```
- WITH CHECK OPTION: can be given for an updatable View to prevent inserts to rows for which the WHERE clause in the SELECT Statement is not true.

**Sources**
- [Oracle Documentation - CREATE VIEW](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/CREATE-VIEW.html)