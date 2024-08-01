---
title: Use DDL to manage tables and their relationships
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
Use the CREATE GLOBAL TEMPORARY TABLE Statement to create a Global Temporary Table.\ 
The definition of a Global Temporary Table is visible to all sessions, but the data in a Global Temporary Table is visible only to the session that inserts the data into the table.

**Notes about Global Temporary Tables**
- DDL operations (except TRUNCATE) are allowed on an existing temporary table only if no session is currently bound to that temporary table. If you rollback a transaction, the data you entered is lost, although the table definition persists.
- Indexes can be created on global temporary tables. They are also temporary and the data in the index has the same session or transaction scope as the data in the underlying table.

**The ON COMMIT Clause**\
The ON COMMIT clause indicates if the data in the table is transaction-specific (the default) or session-specific, the implications of which are as follows:
- DELETE ROWS: The database truncates the table after each commit.
	- Rows will also be deleted after a failed DDL Statement.
	- N.B.: only certain failed DDL Statements will delete all rows.
- PRESERVE ROWS: The database truncates the table when you terminate the session.

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
- Only three DDL Statements are allowed: CREATE, DROP, and TRUNCATE. Other DDL Statements will return ORA-00942: table or view does not exist.
- You must be a User other than SYS to create Private Temporary Tables.

**The ON COMMIT Clause**\
The ON COMMIT clause indicates if the data in the table is transaction-specific (the default) or session-specific, the implications of which are as follows:
- DROP DEFINITION: All data in the table is lost, and the table is dropped at the end of transaction.
- PRESERVE DEFINITION: All data in the table is lost, and the table is dropped at the end of the session that created the table.

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