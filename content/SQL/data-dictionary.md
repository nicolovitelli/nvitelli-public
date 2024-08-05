---
title: Data Dictionary
---

The Data Dictionary is a read-only set of tables that provides information about the database.
A data dictionary contains:
The definitions of all schema objects in the database (tables, views, indexes, clusters, synonyms, sequences, procedures, functions, packages, triggers, and so on)
How much space has been allocated for, and is currently used by, the schema objects
Default values for columns
Integrity constraint information
The names of Oracle users
Privileges and roles each user has been granted
Auditing information, such as who has accessed or updated various schema objects
Other general database information

The data dictionary is structured in tables and views, just like other database data.

---

## Prefixes of Data Dictionary Views
- `USER_` → Objects owned by the current user accessing the view.
- `ALL_` → Objects owned by any user in the Database to which the current user has privileges.
- `DBA_` → All objects in the Database.
- `V_$` (views) or `V$` (public synonyms) → Dynamic Pperformance Views, each of which has a public synonym counterpart. Stores information about the local database instance.
- `GV_$` (views) or `GV$` (public synonyms) → Global Dynamic Performance Views.

---

## Dynamic Performance Views

Dynamic Performance Views display information about current database activity in real time. They are maintaned automatically by the system and are available for querying with some limitations.

Dynamic Performance Views start with the prefix V_$ or their public synonym counterpart starts with V$.

**List of Dynamic Performance Views**
- `V$DATABASE`: Includes information about the Database itself, including the database name, the date created, etc.
- `V$INSTANCE`: Includes the instance name, host name, etc.
- `V$PARAMETER`: Current settings for system parameters such as NLS_LANGUAGE, NLS_DATE_LANGUAGE, NLS_CURRENCY, etc.
- `V$SESSION`: Current settings for each individual user session, showing active connections, login times, etc.
- `V$RESERVED_WORDS`: Current list of reserved words, including information indicating whether the keyword is always reserved, and if not, under what circumstances it is reserved.
- `V$OBJECT_USAGE`: Useful for monitoring the usage of INDEX objects
- `V$TIMEZONE_NAMES`: Includes two columns:
- `TZNAME`: time zone region
- `TZABBREV`: time zone abbreviation

**Sources**
- [Oracle Documentation - Overview of the Dynamic Performance Views](https://docs.oracle.com/en/database/oracle/oracle-database/21/cncpt/data-dictionary-and-dynamic-performance-views.html)

---

## Checking Privileges in the Data Dictionary
Privileges can be inspected using the following views:
- USER_SYS_PRIVS: System privileges granted to the current user
- USER_TAB_PRIVS: Granted privileges on objects for which the user is the owner, grantor or grantee.
- USER_ROLE_PRIVS: Roles granted to the current user.
- DBA_SYS_PRIVS: System privileges granted to users and roles.
- DBA_TAB_PRIVS: All grants on objects in the Database.
- DBA_ROLE_PRIVS: Role granted to users and roles.
- ROLE_SYS_PRIVS: System privileges granted to roles.
- ROLE_TAB_PRIVS: Table privileges granted to roles.
- SESSION_PRIVS: Session privileges that the user currenlty has set.

---

## USER_CATALOG
The USER_CATALOG view displays a summary listing of tables, views, synonym, and sequences owned by the user.\
There are two columns in USER_CATALOG:
- TABLE_TYPE: indicates the Database Object (SEQUENCE, TABLE, VIEW, etc.).
- TABLE_NAME: name of the Table.

**Sources**
- [Oracle Documentation - USER_CATALOG](https://docs.oracle.com/en/database/oracle/oracle-database/21/refrn/USER_CATALOG.html)

---

## USER_OBJECTS
The USER_OBJECTS view contain information about all objects owned by the user.

**Sources**
- [Oracle Documentation - USER_OBJECTS](https://docs.oracle.com/en/database/oracle/oracle-database/21/refrn/USER_OBJECTS.html)

---

## USER_SYNONYMS
The USER_SYNONYMS View describes the private synonyms (synonyms owned by the current user).\
Its columns (except for OWNER) are the same as those in ALL_SYNONYMS.

**Sources**
- [Oracle Documentation - USER_SYNONYMS](https://docs.oracle.com/en/database/oracle/oracle-database/21/refrn/USER_SYNONYMS.html)

---

## USER_TABLES
The USER_TABLES view shows detailed information about the Tables owned by current session account.

Some of the columns include:
- TABLE_NAME: Name of the Table.
- STATUS: Indicates whether the Table is currently valid and therefore available for use.
- ROW_MOVEMENT: Indicates whether ROW MOVEMENT has been enabled for the Table.
- AVG_ROW_LEN: Average length of the rows currently stored in the Table.

**Sources**
- [Oracle Documentation - USER_TABLES](https://docs.oracle.com/en/database/oracle/oracle-database/21/refrn/USER_TABLES.html)