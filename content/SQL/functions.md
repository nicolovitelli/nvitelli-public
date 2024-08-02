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