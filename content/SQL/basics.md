## SELECT Statement
```sql
SELECT      [DISTINCT | UNIQUE] (*, columnname [AS alias], …)
FROM        tablename
[WHERE      condition]
[GROUP BY   group_by_expression]
[HAVING     group_condition]
[ORDER BY   columnname];
```

- [Oracle Documentation - SELECT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SELECT.html)

---

## Column Aliases
Column Aliases can be used to create a temporary name for columns or expressions.

### Syntax
*column_name* [AS] *alias_name*

### Examples
- `first_name AS Employee`
- `first_name Employee`
- `first_name || ' ' || last_name AS "Employee Details"`
- `first_name "Employee's First Name"`

### Restrictions
- If *alias_name* contains spaces, you must enclose *alias_name* in quotes.

### Sources
- [Oracle Documentation - SELECT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SELECT.html)

---

## DISTINCT
The DISTINCT clause is used to remove duplicates from the result set.
This clause can only be used within SELECT Statements.

### Syntax
```sql
SELECT DISTINCT column_name
FROM table_name;
```

### Example
```sql
SELECT DISTINCT first_name
FROM employees;
```

### Notes about DISTINCT
- When only one expression is provided in the DISTINCT clause, the query will return the unique values for that expression.
- When more than one expression is provided in the DISTINCT clause, the query will retrieve unique combinations for the expressions listed.
- DISTINCT does **not** ignore NULL values. If there are multiple NULL values, it will return only one NULL.
- UNIQUE is a synonym of DISTINCT.
- You cannot specify DISTINCT if the SELECT list contains LOB columns.

### Sources
- [Oracle Documentation - SELECT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SELECT.html)

---

## Query Processing Order / Order of Execution
```sql
FROM
WHERE
GROUP BY
SELECT
HAVING
ORDER BY
```

### Sources
- [Oracle SQL and PL/SQL Optimization for Developers - Query Processing Order](https://oracle.readthedocs.io/en/latest/sql/basics/query-processing-order.html)

---

## Alternative Quote Operator (*q*)

### Syntax
```sql
SELECT  q'{text}', column_1, ..., column_n
FROM    tablename;
```

### Oracle Live SQL
- [Alternative Quote Operator](https://livesql.oracle.com/apex/livesql/s/pdt89lso0zy6ooir8lidd8j1l)

---

## Operator and Condition Precedence

- \*, /\*
- +, -
- =, !=, <, >, <=, >=
- IS [NOT] NULL, LIKE, [NOT] BETWEEN, [NOT] IN, EXISTS, IS OF
- NOT
- AND
- OR

### Sources
- [Oracle Documentation - Condition Precedence](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/About-SQL-Conditions.html)
- [Oracle Documentation - Operator Precedence](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/About-SQL-Operators.html)

---

## DESCRIBE

### Syntax
DESC[RIBE] [*schema*.]*object*;

- *object* can be:
	- a table
	- a view
	- a synonym
	- a PL/SQL type
	- a PL/SQL procedure
	- a PL/SQL function
	- a PL/SQL package

### Command Output
The description for *object* contains the following information:
- each column's name
- whether or not null values are allowed (NULL or NOT NULL) for each column
- datatype of columns and their precision (and scale, if any, for a numeric column)

### Oracle Live SQL
- [DESC](https://livesql.oracle.com/apex/livesql/s/pdt89lso3vvk4893eazxegs4p)

### Sources
- [Oracle Documentation - DESCRIBE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqpug/DESCRIBE.html)

---

## DDL - Data Definition Language
Data Definition Language (DDL) statements let you to perform these tasks:
- Create, alter, and drop schema objects
- Grant and revoke privileges and roles
- Analyze information on a table, index, or cluster
- Establish auditing options
- Add comments to the data dictionary

The CREATE, ALTER, and DROP commands require exclusive access to the specified object. For example, an ALTER TABLE statement fails if another user has an open transaction on the specified table.

The GRANT, REVOKE, ANALYZE, AUDIT, and COMMENT commands do not require exclusive access to the specified object. For example, you can analyze a table while other users are updating the table.

Oracle Database implicitly commits the current transaction before and after every DDL statement.

The DDL Statements are:
- ALTER ... (*all statements beginning with ALTER*)
- ANALYZE
- ASSOCIATE STATISTICS
- AUDIT
- COMMENT
- CREATE ... (*all statements beginning with CREATE*)
- DISASSOCIATE STATISTICS
- DROP ... (*all statements beginning with DROP*)
- FLASHBACK ... (*all statements beginning with FLASHBACK*)
- GRANT
- NOAUDIT
- PURGE
- RENAME
- REVOKE
- TRUNCATE
- UNDROP

### Sources
- [Oracle Documentation - Data Definition Language (DDL) Statements](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Types-of-SQL-Statements.html)

---

## CREATE TABLE Statement

### Syntax
CREATE TABLE [*schema*.]*tablename*
    ( *columnname* *datatype* [DEFAULT *value*]
    [, *columnname* *datatype* [DEFAULT *value*]] );

### Sources
- [Oracle Documentation - CREATE TABLE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/CREATE-TABLE.html)

---

## Temporary Tables
A Temporary Table holds data that exists only for the duration of a Transaction or session.
Data in a Temporary Table is private to the session.
Each session can only see and modify its own data.

You cannot specify any FOREIGN KEY constraints on Temporary Tables.

You can create either a Global Temporary Table or a Private Temporary Table.
The following table shows the essential differences between them:
| **Characteristic**             | **Global Temporary Table**                                                                 | **Private Temporary Table**                                                                          |
|--------------------------------|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Naming rules                   | Same as for permanent tables                                                               | Must be prefixed with ORA$PTT_                                                                       |
| Visibility of table definition | All sessions                                                                               | Only the session that created the table                                                              |
| Storage of table definition    | Disk                                                                                       | Memory only                                                                                          |
| Types                          | Transaction-specific (ON COMMIT DELETE ROWS) or session-specific (ON COMMIT PRESERVE ROWS) | Transaction-specific (ON COMMIT DROP DEFINITION) or session-specific (ON COMMIT PRESERVE DEFINITION) |

### Sources
- [Oracle Documentation - Overview of Temporary Tables](https://docs.oracle.com/en/database/oracle/oracle-database/23/cncpt/tables-and-table-clusters.html#GUID-97709804-7430-4BD0-AFF4-727B74F6997E)