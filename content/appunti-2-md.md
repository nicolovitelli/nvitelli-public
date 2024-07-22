# 001
```sql
create package pack1 as
	pragma deprecate (pack1);
	procedure my_procedure;
end;
>> Package PACK1 compiled

create package pack2 as
	my_variable number := 1;
	pragma deprecate(my_variable, 'unused variable');
end;
>> Package PACK2 compiled
```

[Oracle Documentation - DEPRECATE Pragma](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/DEPRECATE-pragma.html)

# 002
- A is false:
> The ACCESSIBLE BY clause restricts access to a unit or subprogram by other units.
> This clause can be used by the following statements:
> - ALTER TYPE
> - CREATE FUNCTION
> - CREATE PROCEDURE
> - CREATE PACKAGE
> - CREATE TYPE
> - CREATE TYPE BODY

```sql
create function funct1 return boolean
    accessible by (package pack1)
is
begin
    return true;
end;
>> Function FUNCT1 compiled
```

- B is true:
```sql
create function funct6 return number
is
begin
    null;
end;
> Function FUNCT6 compiled

declare
    my_var number := 0;
begin
    my_var := funct6;
end;
> ORA-06503: PL/SQL: Function returned without value
```

- C is false:
```sql
create or replace procedure p_test (my_var in date)
is
begin
    null;
end;
> Procedure P_TEST compiled

create or replace function f_test (my_var in date) return date
is
begin
    return my_var;
end;
> Function F_TEST compiled

declare
    my_var_param date;
begin
    my_var_param := sysdate;
    p_test(my_var_param);
    my_var_param := f_test(my_var_param);
end;
> PL/SQL procedure successfully completed.
```

- D is false:
```sql
create or replace procedure p_test (my_var in date)
is
begin
    null;
end;
> Procedure P_TEST compiled

create or replace function f_test (my_var in date) return date
is
begin
    return my_var;
end;
> Function F_TEST compiled

select p_test()
from dual;
> ORA-00904: "P_TEST": invalid identifier

select f_test(sysdate)
from dual;
> F_TEST(SYSDATE)       
> --------------------- 
> 7/20/2024, 5:17:37 PM 
```

- E is true:
```sql
create procedure p_test_1
is
begin
    return 1+1;
end;
> PLS-00372: In a procedure, RETURN statement cannot contain an expression
```

- F is true:
```sql
create or replace function f_test return number
is
begin
    null;
    return;
end;
> PLS-00503: RETURN <value> statement required for this return from function
```