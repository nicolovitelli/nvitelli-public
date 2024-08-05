---
title: Constraints
---

## UNIQUE Constraint
A UNIQUE constraint prohibits multiple rows from having the same value in the same column or combination of columns but allows some values to be null.

More than one column can be part of a UNIQUE Constraint (*Composite Unique Key*); That means the combination of the specified columns will be unique.\
A Composite Unique Key can only be specified out of line.

**Syntax**
```sql
CREATE TABLE table_name(
	column_name datatype UNIQUE
);
```

**Syntax - Composite Unique Key**
```sql
CREATE TABLE table_name(
	column_name_1 datatype,
	column_name_2 datatype,
	[CONSTRAINT constraint_name] UNIQUE (column_name_1, column_name_2)
);
```

**Notes about UNIQUE**
- When defining a UNIQUE Constraint on one or more columns, Oracle automatically creates an index on the column(s).
- To improve queries performance, it is recommended to create an UNIQUE INDEX instead of a UNIQUE Constraint.
- Despite the UNIQUE constraint allowing multiple NULL values, if only column of a Composite Unique Key has a NULL value, then the other column must have a different value. See example below:
```sql
CREATE TABLE t (t1 NUMBER, t2 NUMBER, UNIQUE (t1,t2));
INSERT INTO t VALUES (1, NULL);
-- 1 row inserted.
INSERT INTO t VALUES (1, NULL);
-- ORA-00001: unique constraint (SYS_C0046702) violated
```

**Restrictions**
- UNIQUE Constraint cannot be created on the following column datatypes:
	- `LOB`
	- `LONG`
	- `LONG RAW`
	- `VARRAY`
	- `NESTED TABLE`
	- `OBJECT`
	- `REF`
	- `TIMESTAMP WITH TIME ZONE`
	- user-defined type
- A Primary Key column cannot be part of a UNIQUE Constraint.

**Sources**
- [Oracle Documentation - Unique Constraints](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/constraint.html)

---

## NOT NULL Constraint
A NOT NULL constraint prohibits a column from containing nulls.

**Syntax**
```sql
CREATE TABLE table_name(
	column_name_1 datatype [NULL | NOT NULL]
	[, column_name_n datatype]
);
```

**Notes about NOT NULL**
- A NOT NULL Constraint must be defined inline. 
- It is possible to specify NULL to explicitly permit a column to contain NULLs.

**Restrictions**
- You cannot specify NOT NULL in a View Constraint. 

**Sources**
- [Oracle Documentation - NOT NULL Constraints](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/constraint.html)