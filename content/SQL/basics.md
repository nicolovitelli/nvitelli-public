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
SELECT DISTINCT *column_name*
FROM *table_name*;

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