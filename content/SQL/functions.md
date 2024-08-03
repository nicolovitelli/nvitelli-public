---
title: Functions
---

## Character Functions
Character functions operate on values of dataype CHAR or VARCHAR2.

**List of Character Functions covered**
- CONCAT
- INITCAP
- LOWER
- LPAD
- LTRIM
- REPLACE
- RPAD
- RTRIM
- SUBSTR
- TRIM
- UPPER
- INSTR
- LENGTH

---

## CONCAT
The CONCAT (Single-Row) Function concatenates two strings and returns the combined string.

The two strings do not need to be the same data type.

**Syntax**
```sql
CONCAT(string1, string2)
```
- *string1*: the first string to concatenate
- *string2*: the second string to concatenate

**Sources**
- [Oracle Documentation - CONCAT](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/CONCAT.html)

---

## INITCAP
The INITCAP (Single-Row) Function sets the first character in each word to uppercase and the rest to lowercase.

**Syntax**
```sql
INITCAP(string1)
```
- *string1*: the string argument whose first character in each word will be converted to uppercase and all remaining characters converted to lowercase.

**Notes**
- Special characters and numbers are unaffected by this function.

**Examples**
- 'hello everyone!' → 'Hello Everyone!'
- 1234567890 → 1234567890
- TO_DATE('20-NOV-2022') → '20-Nov-2022'

**Sources**
- [Oracle Documentation - INITCAP](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/INITCAP.html)

---

## LOWER
The LOWER (Single-Row) Function converts all letters in the specified string to lowercase.

**Syntax**
```sql
LOWER(string1)
```
- *string1*: the string to convert to lowercase.

**Notes**
- Special characters and numbers are unaffected by this function.

**Examples**
- 'hello EVERYONE!' → 'hello everyone!'
- 1234567890 → 1234567890
- TO_DATE('20-NOV-2022') → '20-nov-2022'

**Sources**
- [Oracle Documentation - LOWER](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/LOWER.html)

---

## UPPER
The UPPER (Single-Row) Function converts all letters in the specified string to uppercase.

**Syntax**
```sql
UPPER(string1)
```
- *string1*: the string to convert to uppercase

**Notes**
- Special characters and numbers are unaffected by this function.

**Examples**
- 'hello EVERYONE!' → 'HELLO EVERYONE!'
- 1234567890 → 1234567890
- TO_DATE('20-NOV-2022') → '20-NOV-2022'

**Sources**
- [Oracle Documentation - UPPER](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/UPPER.html)

---

## LPAD & RPAD
The LPAD & RPAD (Single-Row) Functions pad the left/right-side of a string with a specific set of characters.

**Syntax**
```sql
LPAD(string1, padded_length, [, pad_string])
RPAD(string1, padded_length, [, pad_string])
```
- *string1*: the string to pad characters to (the left/right-hand side)
- *padded_length*: the number of characters to return
- *pad_string* the string that will be padded to the left/right-hand side of *string1*
	- optional value; default is blank space

**Notes**
- If *string1* is NULL, the function returns NULL.
- If the *padded_length* is smaller than the original string, the function will truncate the string to the size of *padded_length*.

**Examples**
- LPAD('Hello!', 10, 'a') → aaaaHello!
- RPAD('Hello!', 10, 'a') → Hello!aaaa

**Sources**
- [Oracle Documentation - LPAD](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/LPAD.html)
- [Oracle Documentation - RPAD](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/RPAD.html)

---

## LTRIM & RTRIM
The LTRIM & RTRIM (Single-Row) Functions remove all specified characters from the left/right-hand side of a string.

**Syntax**
```sql
LTRIM(string1, [, trim_string])
RPAD(string1, [, trim_string])
```
- *string1*: the string to trim the characters from the left/right-hand side
- *trim_string*: the string that will be removed from the left/right-hand side of *string1*
	- optional value; default is blank space

**Examples**
- LTRIM('Hello!','H') → ello!
- RTRIM('Hello!', '!') → Hello
- LTRIM('Hello!', 'h') → Hello!

**Sources**
- [Oracle Documentation - LTRIM](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/LTRIM.html)
- [Oracle Documentation - RTRIM](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/RTRIM.html)

---

## TRIM
The TRIM (Single-Row) Function removes all specified characters either from the beginning or the end of a string.

**Syntax**
```sql
TRIM([[LEADING | TRAILING | BOTH] trim_character FROM] string1)
```
- LEADING: the function will remove *trim_character* from the front of *string1*
- TRAILING: the function will remove *trim_character* from the end of *string1*
- BOTH: the function will remove *trim_character* from the front and end of *string1*
- *trim_character*: the character that will be removed from *string1*
	- optional value; default is blank space
	- this argument cannot contain more than 1 character
- *string1*: the string to trim

**Examples**
- TRIM(LEADING 'H' FROM 'Hello!') → ello!
- TRIM(TRAILING 'o' FROM 'Hello') → Hell
- TRIM('!' FROM 'Hello!') → Hello 

**Sources**
- [Oracle Documentation - TRIM](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/TRIM.html)

---

## REPLACE
The REPLACE (Single-Row) Function replaces a sequence of characters in a string with another set of characters.

**Syntax**
```sql
REPLACE(string1, string_to_replace [, replacement_string])
```
- *string1*: the string to replace a sequence of characters with another set of characters.
- *string_to_replace*: the string that will be searched for in *string1*.
- *replacement_string*: all occurrences of string_to_replace will be replaced with *replacement_string* in *string1*.
	- if omitted, the function removes all occurrences of *string_to_replace*

**Examples**
- REPLACE('123Hello123', '123') → Hello
- REPLACE('123Hello123', '456') → 456Hello456
- REPLACE('0123456789', '0') → 123456789

**Sources**
- [Oracle Documentation - REPLACE](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/REPLACE.html)

---

## LENGTH
The LENGTH (Single-Row) Function returns the length of the specified string.

**Syntax**
```sql
LENGTH(string1)
```
- *string1*: the string to return the length for.

**Examples**
- LENGTH('Hello World!') → 12
- LENGTH(1234567890) → 10
- LENGTH(TO_DATE('20-NOV-2022')) → 9
N.B. LENGTH does **not** count dashes in DATE values.

**Sources**
- [Oracle Documentation - LENGTH](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/LENGTH.html)

---

## SUBSTR
The SUBSTR (Single-Row) Function allows to extract a substring from a string.

**Syntax**
```sql
SUBSTR(string. start_position [, length])
```
- *string*: the source string.
- *start_position*: the starting position for extraction. First position is always 1.
- *length*: number of characters to extract.

**Notes**
- If *string* is SYSDATE, Oracle will execute an implicit data type conversion.
- If *start_position* is 0, SUBSTR treats *start_position* as 1.
- If *start_position* is negative, SUBSTR start from the end of the string and count backwards.
- If *length* is omitted, SUBSTR will return the entire string.
- If *length* is negative, SUBSTR returns a NULL value.

**Sources**
- [Oracle Documentation - SUBSTR](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/SUBSTR.html)

---

## INSTR
The INSTR (Single-Row) Function returns the location of a substring in a string.

**Syntax**
```sql
INSTR(string, substring [, start_position [, nth_appearance]])
```
- *string*: the string to search
- *substring*: substring to search for in *string*
- *start_position*: position in *string* where the search will start
	- default is 1
- *nth_appearance*: the nth appearance of *substring*
	- default is 1

**Notes**
- If either *string* or *substring* is NULL, INSTR returns NULL.
- If *substring* is not found in *string*, INSTR returns 0.
- If *start_position* negative, the INSTR function counts backwards.

**Examples**
```sql
SELECT INSTR('ciaoc', 'c', 1, 2)
FROM dual;
-- 5
```

**Sources**
- [Oracle Documentation - INSTR](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/INSTR.html)

---

## LEAST
The LEAST (Single-Row) Function returns the smallest value in a list of expressions.

**Syntax**
```sql
LEAST(expr1 [, expr2, ... expr_n])
```
- *expr1*: the first expression to evaluate whether it is the smallest.
- *expr2*: additional expressions that are to be evaluated.

**Notes**
- If either *expr1* or *expr2* is NULL, LEAST returns NULL.

**Examples**
- LEAST('hello', 'HELLO', 'z') → HELLO
- LEAST(NULL, 'HELLO', 'hELLO', 'zzz') → NULL
- LEAST(8, 56, 99, 5, NULL) → NULL

**Sources**
- [Oracle Documentation - LEAST](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/LEAST.html)

---

## Numeric Functions
The Numeric Functions take a numeric input as an expression and return numeric values.\
The return type for most of the Numeric Functions is NUMBER.

**List of Numeric Functions covered**
- CEIL
- FLOOR
- MOD
- POWER
- REMAINDER
- ROUND
- TRUNC

---

## CEIL
The CEIL (Single-Row) Function returns the smallest integer value that is greater than or equal to a number.

**Syntax**
```sql
CEIL(number)
```
- *number*: the value used to find the smallest integer value.

**Examples**
- CEIL(21.65) → 21
- CEIL(21.21) → 22
- CEIL(21) → 21
- CEIL(-21.65) → -21

**Sources**
- [Oracle Documentation - CEIL](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/CEIL.html)

---

## FLOOR
The FLOOR (Single-Row) Function returns the largest integer value that is equal to or less than a number.

**Syntax**
```sql
FLOOR(number)
```
- *number*: the value used to determine the largest integer value that is equal to or less than a number.

**Examples**
- FLOOR(21.65) → 21
- FLOOR(21.21) → 21
- FLOOR(21) → 21
- FLOOR(-21.65) → -22

**Sources**
- [Oracle Documentation - FLOOR](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/FLOOR.html)

---

## MOD
The MOD (Single-Row) Function returns the remainder of m divided by n.

The difference with the REMAINDER function is that REMAINDER uses ROUND in its calculation, and MOD uses the FLOOR function.

**Syntax**
```sql
MOD(m, n)
```
- *m*: the numeric value used in the calculation.
- *n*: the numeric value used in the calculation.
	- MOD returns *m* if *n* is 0.

**Examples**
- MOD(15, 4) → 3
- MOD(15, 0) → 0
- MOD(11.6, 2) → 1.6

**Sources**
- [Oracle Documentation - MOD](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/MOD.html)

---

## POWER
The POWER (Single-Row) Function returns m raised to the nth power.

**Syntax**
```sql
POWER(m, n)
```
- *m*: the base used in the calculation.
	- if *m* s negative, then *n* must be an integer.
- *n*: the exponent used in the calculation.

**Examples**
- POWER(3, 2) → 9
- POWER(5, 3) → 125
- POWER(-5, 3) → -125

**Sources**
- [Oracle Documentation - POWER](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/POWER.html)

---

## REMAINDER
The REMAINDER (Single-Row) Function returns the remainder of m divided by n.

The difference with the MOD function is that REMAINDER uses ROUND in its calculation, and MOD uses the FLOOR function.

The REMAINDER is calculated as follows:\
m - (n * X)\
where X is the integer nearest n / n

**Syntax**
```sql
REMAINDER(m, n)
```
- *m*: a numeric value used in the calculation.
- *n*: a numeric value used in the calculation.

**Examples**
- REMAINDER(15, 6) → 3
- REMAINDER(15, 5) → 0
- REMAINDER(15, 4) → -1

**Sources**
- [Oracle Documentation - REMAINDER](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/REMAINDER.html)

---

## ROUND (number)
The ROUND (Single-Row) Function returns a number rounded to a certain number of decimal places.

**Syntax**
```sql
ROUND(number [, decimal_places])
```
- *number*: the number to round.
- *decimal_places*: the number of decimal places rounded to.
	- if omitted, the ROUND function will round the number to 0 decimal places
	- ff negative, rounds a number one digit to the left of the decimal point

**Example**
```sql
SELECT ROUND(152.3435, -1)
FROM dual;
-- 150
```

**Sources**
- [Oracle Documentation - ROUND (number)](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/ROUND-number.html)

---

## TRUNC (number)
The TRUNC (Single-Row) Function returns a number truncated to a certain number of decimal places.

**Syntax**
```sql
TRUNC(number [, decimal_places])
```
- *number*: the number to truncate
- *decimal_places*: the number of decimal places to truncate to
	- if omitted, the TRUNC function will truncate the number to 0 decimal places
	- if negative, replaces digits to the left of the decimal point with 0

**Example**
```sql
SELECT TRUNC(152.3435, -1)
FROM dual;
-- 150
```

**Sources**
- [Oracle Documentation - TRUNC (number)](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/TRUNC-number.html)

---

## Date Functions
Character functions can be used to manipulate temporal values.

**List of Date Functions covered**
- ADD_MONTHS
- CURRENT_DATE
- CURRENT_TIMESTAMP
- DBTIMEZONE
- LAST_DAY
- LOCALTIMESTAMP
- MONTHS_BETWEEN
- NEXT_DAY
- ROUND
- SESSIONTIMEZONE
- SYSDATE
- SYSTIMESTAMP
- TRUNC

---

## ADD_MONTHS
The ADD_MONTHS (Single-Row) Function returns a date with a specified number of months added.

**Syntax**
```sql
ADD_MONTHS(date1, number_months)
```
- *date1*: the starting date (before the *n* months have been added)
- *number_months*: number of months to add to *date1*

**Sources**
- [Oracle Documentation - ADD_MONTHS](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/ADD_MONTHS.html)

---

## CURRENT_DATE
CURRENT_DATE returns the current date in the session time zone, in a value in the Gregorian calendar of data type DATE.

**Example**
```sql
SELECT CURRENT_DATE
FROM DUAL;
-- 06-DEC-2022
```

**Sources**
- [Oracle Documentation - CURRENT_DATE](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/CURRENT_DATE.html)

---

## CURRENT_TIMESTAMP
CURRENT_TIMESTAMP returns the current date and time in the session time zone, in a value of data type TIMESTAMP WITH TIME ZONE. The time zone offset reflects the current local time of the SQL session.

**Example**
```sql
SELECT CURRENT_TIMESTAMP
FROM DUAL;
-- 06-DEC-22 03.20.35.553971000 PM EUROPE/BERLIN
```

**Sources**
- [Oracle Documentation - CURRENT_TIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/CURRENT_TIMESTAMP.html)

---

## DBTIMEZONE
DBTIMEZONE returns the value of the database time zone.

**Example**
```sql
SELECT DBTIMEZONE
FROM DUAL;
-- +00:00
```

**Sources**
- [Oracle Documentation - DBTIMEZONE](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/DBTIMEZONE.html)

---

## LAST_DAY
The LAST_DAY (Single-Row) Function returns the last day of the month based on a date value.

**Syntax**
```sql
LAST_DAY(date)
```
- *date*: the date value to use to calculate the last day of the month

**Examples**
- LAST_DAY(TO_DATE('01-DEC-2022')) → 31-DEC-2022
- LAST_DAY(TO_DATE('31-DEC-2022')) → 31-DEC-2022

**Sources**
- [Oracle Documentation - LAST_DAY](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/LAST_DAY.html)

---

## LOCALTIMESTAMP
LOCALTIMESTAMP returns the current date and time in the session time zone in a value of data type TIMESTAMP.

**Example**
```sql
SELECT LOCALTIMESTAMP
FROM DUAL;
-- 06-DEC-22 03.20.04.446137000 PM
```

**Sources**
- [Oracle Documentation - LOCALTIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/LOCALTIMESTAMP.html)

---

## CURRENT_DATE vs. CURRENT_TIMESTAMP vs. LOCALTIMESTAMP
![CURRENT_DATE vs. CURRENT_TIMESTAMP vs. LOCALTIMESTAMP](https://i.imgur.com/UWZEQr7.png)

---

## MONTHS_BETWEEN
The MONTHS_BETWEEN (Single-Row) Function returns the number of months between date1 and date2.

**Syntax**
```sql
MONTHS_BETWEEN(date1, date2)
```
- *date1*: the first date used to calculate the number of months between.
- *date2*: the second date used to calculate the number of months between.

**Examples**
- MONTHS_BETWEEN(TO_DATE('01-DEC-2022'), TO_DATE('01-JAN-2023')) → -1
- MONTHS_BETWEEN(TO_DATE('01-DEC-2022'), TO_DATE('01-NOV-2022')) → 1
- MONTHS_BETWEEN(TO_DATE('01-DEC-2022'), TO_DATE('31-DEC-2022')) → 0.96

**Sources**
- [Oracle Documentation - MONTHS_BETWEEN](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/MONTHS_BETWEEN.html)

---

## NEXT_DAY
The NEXT_DAY (Single-Row) Function returns the first weekday that is greater than a date.

**Syntax**
```sql
NEXT_DAY(date, weekday)
```
- *date*: a date value used to find the next weekday.
- *weekday*: the day of the week that you wish to return.
	- must be one of the following:
		- SUNDAY or SUN
		- MONDAY or MON
		- TUESDAY or TUE
		- WEDNESDAY or WED
		- THURSDAY or THU
		- FRIDAY or FRI
		- SATURDAY or SAT

**Examples**\
*Considering 6 December 2022 was a Tuesday:*
- NEXT_DAY(TO_DATE('06-DEC-2022'), 'Sunday') → 11-DEC-2022
- NEXT_DAY(TO_DATE('06-DEC-2022'), 'Tuesday') → 13-DEC-2022

**Sources**
- [Oracle Documentation - NEXT_DAY](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/NEXT_DAY.html)

---

## ROUND (date)
The ROUND (Single-Row) Function returns a date rounded to a specific unit of measure.

**Syntax**
```sql
ROUND(date [, format])
```
- *date*: the date to round.
- *format*: the unit of measure to apply for rounding.
	- if omitted, the function will round to the nearest day
	- must be one of the following values: [(complete list)](https://prnt.sc/eusrvY7iTSvr)

**Examples**
- ROUND(TO_DATE('01-NOV-2022'), 'YEAR') → 01-JAN-2023
- ROUND(TO_DATE('16-NOV-2022'), 'MONTH') → 01-DEC-2022
- ROUND(TO_DATE('16-NOV-2022')) → 16-NOV-2022

**Sources**
- [Oracle Documentation - ROUND (date)](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/ROUND-date.html)

---

## SESSIONTIMEZONE
SESSIONTIMEZONE returns the time zone of the current session.

**Example**
```sql
SELECT SESSIONTIMEZONE
FROM DUAL;
-- Europe/Berlin
```

**Sources**
- [Oracle Documentation - SESSIONTIMEZONE](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/SESSIONTIMEZONE.html)

---

## SYSDATE
SYSDATE returns the current date and time set for the operating system on which the database server resides.

The data type of the returned value is DATE, and the format returned depends on the value of the NLS_DATE_FORMAT initialization parameter.

**Example**
```sql
SELECT SYSDATE
FROM DUAL;
-- 06-DEC-2022
```

**Sources**
- [Oracle Documentation - SYSDATE](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/SYSDATE.html)

---

## SYSTIMESTAMP
SYSTIMESTAMP returns the system date, including fractional seconds and time zone, of the system on which the database resides.

The return type is TIMESTAMP WITH TIME ZONE.

**Example**
```sql
SELECT SYSTIMESTAMP
FROM DUAL;
-- 06-DEC-2022 03.41.91.693169000 PM +01:00
```

**Sources**
- [Oracle Documentation - SYSTIMESTAMP](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/SYSTIMESTAMP.html)

---

## INTERVAL
The INTERVAL datatype allows you to store periods of time.

There are two types of INTERVAL:
- **INTERVAL YEAR TO MONTH**: stores intervals using year and month;
- **INTERVAL DAY TO SECOND**: stores intervals using days, hours, and seconds including fractional seconds.

**Sources**
- [Oracle Documentation - Interval Expressions](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/Interval-Expressions.html)
- [Oracle Documentation - Interval Literals](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/Literals.html)

---

## INTERVAL YEAR TO MONTH
The INTERVAL YEAR TO MONTH data type allows you to store a period of time using the YEAR and MONTH fields.

**Syntax**
```sql
INTERVAL YEAR [(year_precision)] TO MONTH
```
- *year_precision*: number of digits in the YEAR field (*ranges from 0 to 9*).

**Literals Syntax**
```sql
INTERVAL 'year[-month]' leading(precision) TO trailing
```
- *leading* and *trailing* can only be YEAR or MONTH.
- If *leading* is YEAR and *trailing* is MONTH, then the MONTH field ranges from 0 to 11.
- The *trailing* field must be less than the *leading* field.
- *precision* represents the number of digits in the *leading* field. (ranges from 0 to 9; default is 2).

**Literals Examples**
- INTERVAL '120-3' YEAR(3) TO MONTH → 120 Years and 3 Months.
- INTERVAL '105' YEAR(3) → 105 Years.
- INTERVAL '500' MONTH(3) → 500 Months.
- INTERVAL '9' YEAR → 9 Years.
- INTERVAL '180' YEAR → Invalid; must specify the precision because it's greater than default's value.

**Sources**
- [Oracle Documentation - INTERVAL YEAR TO MONTH](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/Literals.html)

---

## INTERVAL DAY TO SECOND
The INTERVAL DAY TO SECOND stores a period of time in terms of days, hours, minutes, and seconds.

**Syntax**
```sql
INTERVAL DAY [(day_precision)] TO SECOND [(fractional_seconds_precision)]
```
- *day_precision*: number of digits in the DAY field (*ranges from 0 to 9*).
- *fractional_seconds_precision*: number of digits in the fractional part of the SECOND field (*ranges from 0 to 9*).

**Literals Syntax**
```sql
INTERVAL leading(leading_precision) TO trailing(fractional_seconds_precision)
```

**Literals Examples**
- INTERVAL '11 10:09:08.555' DAY TO SECOND(3) → 11 Days, 10 Hours, 9 Minutes, 8 Seconds and 555 Thousandths of a Second.
- INTERVAL '11 10:09' DAY TO MINUTE → 11 Days, 10 Hours and 9 Minutes.
- INTERVAL '100 10' DAY(3) TO HOUR → 100 Days and 10 Hours.
- INTERVAL '999' DAY(3) → 999 Days.
- INTERVAL '8' HOUR → 8 Hours.
- INTERVAL '09:30' HOUR TO MINUTE → 9 Hours and 30 Minutes.

**Sources**
- [Oracle Documentation - INTERVAL DAY TO SECOND](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/Literals.html)

---

## TRUNC (date)
The TRUNC (Single-Row) Function returns a date truncated to a specific unit of measure.

**Syntax**
```sql
TRUNC(date [, format])
```
- *date*: the date to truncate.
- *format*: the unit of measure to apply for truncating.
	- if omitted, the function will truncate the date to the day value, so that any hours, minutes, or seconds will be truncated off
	- Must be one of the following values: [(complete list)](https://prnt.sc/sUPMMLZAJivS)

**Oracle Live SQL**
- [TRUNC (date)](https://livesql.oracle.com/apex/livesql/s/bbwa5b00l6a1p8bxh838e1iut)

**Sources**
- [Oracle Documentation - TRUNC (date)](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/TRUNC-date.html)