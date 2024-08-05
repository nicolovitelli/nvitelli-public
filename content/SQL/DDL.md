---
title: DDL Statements
---

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

**Sources**
- [Oracle Documentation - Data Definition Language (DDL) Statements](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Types-of-SQL-Statements.html)

---

## CREATE TABLE Statement
**Syntax**
```sql
CREATE TABLE [schema.]tablename
    ( columnname datatype [DEFAULT value]
    [, columnname datatype [DEFAULT value]] );
```

**Sources**
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

**Sources**
- [Oracle Documentation - Overview of Temporary Tables](https://docs.oracle.com/en/database/oracle/oracle-database/23/cncpt/tables-and-table-clusters.html#GUID-97709804-7430-4BD0-AFF4-727B74F6997E)

---

## Global Temporary Tables
Use the CREATE GLOBAL TEMPORARY TABLE Statement to create a Global Temporary Table.

The definition of a Global Temporary Table is visible to all sessions, but the data in a Global Temporary Table is visible only to the session that inserts the data into the table.

**Notes about Global Temporary Tables**
- DDL operations (except TRUNCATE) are allowed on an existing temporary table only if no session is currently bound to that temporary table. If you rollback a transaction, the data you entered is lost, although the table definition persists.
- Indexes can be created on global temporary tables. They are also temporary and the data in the index has the same session or transaction scope as the data in the underlying table.

**The ON COMMIT Clause**\
The ON COMMIT clause indicates if the data in the table is transaction-specific (the default) or session-specific, the implications of which are as follows:
- **DELETE ROWS**: The database truncates the table after each commit.
	- Rows will also be deleted after a failed DDL Statement.
	- N.B.: only certain failed DDL Statements will delete all rows.
- **PRESERVE ROWS**: The database truncates the table when you terminate the session.

**Global Temporary Tables Syntax**
```sql
CREATE GLOBAL TEMPORARY TABLE global_temp_table_name
    (column_1 datatype,
    column_2 datatype,
    … column_n datatype)
[ON COMMIT DELETE ROWS | PRESERVE ROWS];
```

---

## Private Temporary Tables
Private Temporary Tables are temporary database objects that are dropped at the end of a transaction or session.

**Notes about Private Temporary Tables**
- Private Temporary Tables are stored in memory and each one is visible only to the session that created it.
- The metadata and content of a Private Temporary Table is visible only within the session that created the it.

**Restrictions**
- Names of Private Temporary Tables must be prefixed according to the initialization parameter private_temp_table_prefix.
- You cannot create Indexes, Materialized Views on Private Temporary Tables.
- You cannot define column with default values.
- You cannot reference Private Temporary Tables in any permanent object (views or triggers).
- Only three DDL Statements are allowed: CREATE, DROP, and TRUNCATE.
	- Other DDL Statements will return `ORA-00942: table or view does not exist`.
- You must be a User other than SYS to create Private Temporary Tables.

**The ON COMMIT Clause**\
The ON COMMIT clause indicates if the data in the table is transaction-specific (the default) or session-specific, the implications of which are as follows:
- **DROP DEFINITION**: All data in the table is lost, and the table is dropped at the end of transaction.
- **PRESERVE DEFINITION**: All data in the table is lost, and the table is dropped at the end of the session that created the table.

**Private Temporary Tables Syntax**
```sql
CREATE PRIVATE TEMPORARY TABLE ORA$PTT_priv_temp_table_name
	(column_1 datatype,
	column_2 datatype,
	… column_n datatype)
[ON COMMIT DROP DEFINITION | PRESERVE DEFINITION];
```

---

## Global vs. Private Temporary Tables
![Global vs. Private Temporary Tables](https://i.imgur.com/0izzyrU.png)

---

## CREATE DIRECTORY
**Syntax**
```sql
CREATE [OR REPLACE] DIRECTORY directory_name AS 'path_name';
```

- OR REPLACE: Specify OR REPLACE to re-create the directory database obejct if it already exists.

**Restrictions**
- You must have the CREATE ANY DIRECTORY System Privilege to create directories.

**Sources**
- [Oracle Documentation - CREATE DIRECTORY](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/CREATE-DIRECTORY.html)

---

## DROP DIRECTORY
Use the DROP DIRECTORY Statement to remove a directory object from the Database.

**Syntax**
```sql
DROP DIRECTORY directory_name;
```

**Restrictions**
- You must have the DROP ANY DIRECTORY System Privilege to drop a directory.

**Sources**
- [Oracle Documentation - DROP DIRECTORY](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/DROP-DIRECTORY.html)

---

## ALTER TABLE Statement
```sql
ALTER TABLE tablename
	ADD|MODIFY|DROP COLUMN| columnname [definition];
```

**Sources**
- [Oracle Documentation - ALTER TABLE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/ALTER-TABLE.html)

---

## ALTER TABLE ... ADD
```sql
ALTER TABLE tablename
	ADD (columnname datatype, [DEFAULT] ...);
```

**Example**
```sql
ALTER TABLE publisher
	ADD (ext NUMBER(4));
```

---

## ALTER TABLE ... MODIFY
```sql
ALTER TABLE tablename
	MODIFY (columnname datatype [DEFAULT], …);
```

**Example**
```sql
ALTER TABLE publisher
	MODIFY (rating DEFAULT 'N');
```

---

## ALTER TABLE ... DROP
Specify DROP to remove the column descriptor and the data associated with the target column from each row in the table. If you explicitly drop a particular column, then all columns currently marked UNUSED in the target table are dropped at the same time.

When the column data is dropped:
- All indexes defined on any of the target columns are also dropped.
- All constraints that reference a target column are removed.

**Syntax - Dropping One Column**
```sql
ALTER TABLE tablename
	DROP COLUMN columnname [CASCADE CONSTRAINTS];
```

**Syntax - Dropping Multiple Columns**
```sql
ALTER TABLE tablename
	DROP (columnname, …) [CASCADE CONSTRAINTS];
```

---

## ALTER TABLE ... SET UNUSED
Specify SET UNUSED to mark one or more columns as unused.

Unused columns are treated as if they were dropped, even though their column data remains in the table rows. After a column has been marked UNUSED, you have no access to that column.

A SELECT * query will not retrieve data from unused columns. In addition, the names and types of columns marked UNUSED will not be displayed during a DESCRIBE, and you can add to the table a new column with the same name as an unused column.

**Syntax**
```sql
ALTER TABLE tablename
	SET UNUSED [COLUMN] (columnname);
```

---

## ALTER TABLE ... DROP UNUSED COLUMNS
Specify DROP UNUSED COLUMNS to remove from the table all columns currently marked as unused. Use this statement when you want to reclaim the extra disk space from unused columns in the table. If the table contains no unused columns, then the statement returns with no errors.

**Syntax**
```sql
ALTER TABLE tablename
	DROP UNUSED COLUMNS;
```

---

## ALTER TABLE ... EXCHANGE PARTITION
```sql
ALTER TABLE target_table EXCHANGE PARTITION partition_name
  WITH TABLE source_table;
```

- Data from the *source_table* goes into the *target_table*.

---

## RENAME Statement
```sql
RENAME old_object_name TO new_object_name;
```

**Sources**
- [Oracle Documentation - RENAME](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/RENAME.html)

---

## TRUNCATE TABLE
```sql
TRUNCATE TABLE tablename;
```

**Sources**
- [Oracle Documentation - TRUNCATE TABLE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/TRUNCATE-TABLE.html)

---

## DROP TABLE
```sql
DROP TABLE tablename [PURGE];
```

After a successful DROP TABLE…
- An uncommited transaction is automatically committed.
- All indexes and constraints defined on the table are also dropped.
- The dropped table may be moved to the Recycle Bin.
- The dropped table **cannot** be recovered using the ROLLBACK command.
- Sequences used in the dropped table **do remain** valid.

**Sources**
- [Oracle Documentation - DROP TABLE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/DROP-TABLE.html#GUID-39D89EDC-155D-4A24-837E-D45DDA757B45)

---

## PURGE Statement
The PURGE Statement is used to:
- Remove a table or index from your recycle bin and release all of the space associated with the object;
- Remove part or all of a dropped tablespace or tablespace set from the recycle bin;
- Remove the entire recycle bin.
	- See the contents in your recycle bin with `SELECT * FROM RECYCLEBIN`;

You cannot recover an object after it is purged.

**Syntax**
```sql
PURGE {
TABLE table_name |
INDEX idx_name |
TABLESPACE tablespace_name |
RECYCLEBIN
DBA_RECYCLEBIN };
```

**Sources**
- [Oracle Documentation - PURGE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/PURGE.html#GUID-9257F773-E019-4464-80F4-F5AB61D7D9B6)

---

## Add a PRIMARY KEY Constraint to Existing Table
```sql
ALTER TABLE table_name
	ADD [CONSTRAINT constraint_name] PRIMARY KEY (column_name);
```

---

## Add a FOREIGN KEY Constraint to Existing Table
```sql
ALTER TABLE table_name
	ADD [CONSTRAINT constraint_name] FOREIGN KEY (column_name)
	REFERENCES referencedtablename (referencedcolumnname);
```

---

## Add a UNIQUE Constraint to Existing Table
```sql
ALTER TABLE table_name
	ADD [CONSTRAINT constraint_name] UNIQUE (column_name);
```

---

## Add a CHECK Constraint to Existing Table
```sql
ALTER TABLE table_name
	ADD [CONSTRAINT constraint_name] CHECK (condition);
```

---

## Add a NOT NULL Constraint to Existing Table
```sql
ALTER TABLE table_name
	MODIFY (column_name [CONSTRAINT constraint_name] NOT NULL);
```

---

## Enabling Constraints
```sql
ALTER TABLE table_name
	DISABLE CONSTRAINT constraint_name;

ALTER TABLE table_name
	ENABLE CONSTRAINT constraint_name;
```

---

## Dropping Constraints
```sql
ALTER TABLE table_name
	DROP PRIMARY KEY | UNIQUE (column_name) | CONSTRAINT constraint_name;
```