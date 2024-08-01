---
title: Advanced Concepts
---

## Arithmetic Operations on Dates
**Rules**
- Operations between DATE value and Binary Operators
	- \+ Addition: adds days to a DATE value; DATE value must be at the left-side.
	- \- Subtraction: subtracts days to a DATE value; DATE value must be at the left-side.
	- \* Multiplication: NOT ALLOWED
	- / Division: NOT ALLOWED
- Operations between two DATE values
	- \+ Addition: NOT ALLOWED
	- \- Subtraction: Return number of days between the two Dates
	- \* Multiplication: NOT ALLOWED
	- / Division: NOT ALLOWED

Supposing SYSDATE is 28-NOV-22:
- SYSDATE - 1 → 27-NOV-22
- 1 - SYSDATE → ORA-00932: inconsistent datatypes
- SYSDATE + 1 → 29-NOV-22
- 1 + SYSDATE → 29-NOV-2022
- SYSDATE * 1 → ORA-00932: inconsistent datatypes
- 1 * SYSDATE → ORA-00932: inconsistent datatypes
- SYSDATE / 1 → ORA-00932: inconsistent datatypes
- 1 / SYSDATE → ORA-00932: inconsistent datatypes
- SYDATE - TO_DATE('27-NOV-2022') → 1
	- Calculates number of days between two dates.

**Sources**
- [Oracle Documentation - Datetime/Interval Arithmetic](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/Data-Types.html#GUID-E405BBC7-DA9A-4DF2-9F22-E60CB9EC0705)

---

## RRRR vs. YYYY
- **YYYY**: first two digits will always be current century. (99 → 2099)
- **RRRR**: first two digits depend on range:
	- years in range 00 to 49 are assumed to be in current century (21 → 2021)
	- years in range 50 to 99 are assumed to be in the previous century (99 → 1999)

There's no difference when working with 4-digits years.
"2022" will always be 2022 no matter if you're using YYYY or RRRR as format mask.

---

## NLS_DATE_FORMAT
NLS_DATE_FORMAT specifies the default date format to use with the TO_CHAR and TO_DATE functions.\
The default value of this parameter is determined by NLS_TERRITORY.

The value of this parameter can be any valid date format mask, and the value must be surrounded by double quotation marks. For example:
```sql
NLS_DATE_FORMAT = "MM/DD/YYYY"
```

**Current Date Mask**
```sql
SELECT VALUE
FROM NLS_SESSION_PARAMETERS
WHERE PARAMETER = 'NLS_DATE_FORMAT';
```

**Sources**
- [Oracle Documentation - NLS_DATE_FORMAT](https://docs.oracle.com/en/database/oracle/oracle-database/21/refrn/NLS_DATE_FORMAT.html)