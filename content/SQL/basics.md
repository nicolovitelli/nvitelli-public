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