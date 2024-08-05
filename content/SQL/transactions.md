---
title: Transactions
---

A Transaction is a logical, atomic unit of work that contains one or more SQL statements

A Transaction groups SQL statements so that they are either all committed, which means they are applied to the database, or all rolled back, which means they are undone from the database.\
Oracle Database assigns every transaction a unique identifier called a transaction ID.

**ACID**\
All Oracle transactions obey the basic properties of a database transaction, known as ACID properties.\
ACID is an acronym for the following:

- **Atomicity**: All tasks of a Transaction are performed or none of them are. There are no partial Transactions.
For example, if a Transaction starts updating 100 rows, but the system fails after 20 updates, then the Database rolls back the changes to these 20 rows.
- **Consistency**: The Transaction takes the Database from one consistent state to another consistent state.
For example, in a banking Transaction that debits a savings account and credits a checking account, a failure must not cause the Database to credit only one account, which would lead to inconsistent data.
- **Isolation**: The effect of a Transaction is not visible to other Transactions until the Transaction is committed.
For example, one user updating the hr.employees table does not see the uncommitted changes to employees made concurrently by another user. Thus, it appears to users as if Transactions are executing serially.
- **Durability**: Changes made by committed Transactions are permanent. After a Transaction completes, the Database ensures through its recovery mechanisms that changes from the Transaction are not lost.

**Structure of a Transaction**
A Database Transaction consists of one or more statements. Specifically, a Transaction consists of one of the following:
- One or more DML Statements that together constitute an atomic change to the Database;
- One DDL Statement.

**Beginning of a Transaction**
A Transaction begins when the first executable SQL Statement is encountered.

A Transaction ID is unique to a Transaction and represents the undo segment number, slot, and sequence number.

**End of a Transaction**
A Transaction ends when any of the following actions occurs:
- A user issues a COMMIT or ROLLBACK statement without a SAVEPOINT clause.
- A user runs a DDL command such as CREATE, DROP, RENAME, or ALTER.
- A user exits normally from most Oracle Database utilities and tools, causing the current transaction to be implicitly committed. The commit behavior when a user disconnects is application-dependent and configurable.
- A client process terminates abnormally, causing the transaction to be implicitly rolled back using metadata stored in the transaction table and the undo segment.

**Sources**
- [Oracle Documentation - Transactions](https://docs.oracle.com/en/database/oracle/oracle-database/21/cncpt/transactions.html)

---

## ROLLBACK
A rollback of an uncommitted Transaction undoes any changes to data that have been performed by SQL Statements within the transaction.\
After a Transaction has been rolled back, the effects of the work done in the Transaction no longer exist.

In rolling back an entire Transaction, without referencing any savepoints, Oracle Database performs the following actions:
- Undoes all changes made by all the SQL Statements in the Transaction
- Releases all the locks of data held by the Transaction
- Erases all savepoints in the Transaction
- Ends the transaction

**Syntax**
```sql
ROLLBACK;
```

**Sources**
- [Oracle Documentation - ROLLBACK](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/ROLLBACK.html)

---

## COMMIT
A commit ends the current Transaction and makes permanent all changes performed in the Transaction.

When a Transaction commits, the following actions occur:
- Oracle Database releases locks held on rows and tables
- Oracle Database deletes savepoints.
- Oracle Database marks the transaction complete.

After a Transaction commits, users can view the changes.

**Syntax**
```sql
COMMIT;
```

**Sources**
- [Oracle Documentation - COMMIT](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/COMMIT.html)