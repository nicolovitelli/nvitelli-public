# 001
- you cannot use DEPRECATE Pragma on Anonymous Blocks, Trigger Bodies or Database Links.
```plsql
create package pack1 as
	pragma deprecate (pack1);
	procedure my_procedure;
end;
-- Package PACK1 compiled

create package pack2 as
	my_variable number := 1;
	pragma deprecate(my_variable, 'unused variable');
end;
-- Package PACK2 compiled
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

```plsql
create function funct1 return boolean
    accessible by (package pack1)
is
begin
    return true;
end;
-- Function FUNCT1 compiled
```

- B is true:
```plsql
create function funct6 return number
is
begin
    null;
end;
-- Function FUNCT6 compiled

declare
    my_var number := 0;
begin
    my_var := funct6;
end;
-- ORA-06503: PL/SQL: Function returned without value
```

- C is false:
```plsql
create or replace procedure p_test (my_var in date)
is
begin
    null;
end;
-- Procedure P_TEST compiled

create or replace function f_test (my_var in date) return date
is
begin
    return my_var;
end;
-- Function F_TEST compiled

declare
    my_var_param date;
begin
    my_var_param := sysdate;
    p_test(my_var_param);
    my_var_param := f_test(my_var_param);
end;
-- PL/SQL procedure successfully completed.
```

- D is false:
```plsql
create or replace procedure p_test (my_var in date)
is
begin
    null;
end;
-- Procedure P_TEST compiled

create or replace function f_test (my_var in date) return date
is
begin
    return my_var;
end;
-- Function F_TEST compiled

select p_test()
from dual;
-- ORA-00904: "P_TEST": invalid identifier

select f_test(sysdate)
from dual;
-- F_TEST(SYSDATE)       
-- --------------------- 
-- 7/20/2024, 5:17:37 PM 
```

- E is true:
```plsql
create procedure p_test_1
is
begin
    return 1+1;
end;
-- PLS-00372: In a procedure, RETURN statement cannot contain an expression
```

- F is true:
```plsql
create or replace function f_test return number
is
begin
    null;
    return;
end;
-- PLS-00503: RETURN <value> statement required for this return from function
```

# 003

# 004

- A is false:
> The PARALLEL_ENABLE clause can appear in the following SQL statements:
> - CREATE FUNCTION Statement
> - CREATE PACKAGE Statement
> - CREATE TYPE BODY Statement

[Oracle Documentation - PARALLEL_ENABLE Clause](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/PARALLEL_ENABLE-clause.html)

- B is true:
> NOCOPY - Requests that the compiler **pass the corresponding actual parameter by reference instead of value** [...]

[Oracle Documentation - Formal Parameter Declaration](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/formal-parameter-declaration.html)

- C is false:
> **Do not specify DETERMINISTIC for a function whose result depends on the state of session variables** or schema objects, because results might vary across invocations.

[Oracle Documentation - DETERMINISTIC Clause](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/DETERMINISTIC-clause.html)

- D is false (*see answer B*)

- E is true:
> The **deterministic option marks a function that returns predictable results** and has no side effects.

[Oracle Documentation - DETERMINISTIC Clause](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/DETERMINISTIC-clause.html)

- F is false:
> Restriction on parallel_enable_clause
> You **cannot specify the parallel_enable_clause for a nested function** or SQL macro.

[Oracle Documentation - PARALLEL_ENABLE Clause](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/PARALLEL_ENABLE-clause.html)

- G is true:
> PARALLEL_ENABLE Clause - Enables the function for parallel execution, making it safe for use in concurrent sessions of parallel DML evaluations.
> Indicates that **the function can run from a parallel execution server of a parallel query operation**.

[Oracle Documentation - PARALLEL_ENABLE Clause](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/PARALLEL_ENABLE-clause.html)

# 005

- A is true:
> The **data type of index can be either a string type** (VARCHAR2, VARCHAR, STRING, or LONG) **or PLS_INTEGER**.
```plsql
declare
    type t1 is table of varchar2(100) index by pls_integer;
    type t2 is table of varchar2(100) index by varchar2(3);
begin
    null;
end;
-- PL/SQL procedure successfully completed.
```

- B is false (*see answer A*)
- C is true: you cannot declare an Associative Array in a classic SQL statement.
- D and E are false:
> **With the CREATE TYPE statement**, you can create nested table and VARRAY types, **but not associative arrays**. In a PL/SQL block or package, you can define all three collection types.

[Oracle Documentation - CREATE TYPE Statement](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/CREATE-TYPE-statement.html)

# 006

- A is false:
```plsql
create or replace package my_package is
    function my_function return number;
end;
-- Package MY_PACKAGE compiled

create or replace package body my_package is
    function my_function return number is
    begin
        return 1;
    end;
end;
-- Package Body MY_PACKAGE compiled

create or replace package my_new_package is
    function my_new_function return number;
end;
-- Package MY_NEW_PACKAGE compiled

create or replace package body my_new_package is
    function my_new_function return number is
        my_number number := my_package.my_function();
    begin
        return my_number;
    end;
end;
-- Package Body MY_NEW_PACKAGE compiled
```

- B is true:
```plsql
create procedure p1 (v1 in out number) is
begin
    null;
end;
```

- C is true:
```plsql
create or replace function f1 return number
is
begin
    null;
end;
-- Function F1 compiled

declare
    my_number number := f1();
begin
    null;
end;
-- ORA-06503: PL/SQL: Function returned without value
```

- D is false:
```plsql
create or replace package my_pack is
    my_var number := 1;
end;
-- Package MY_PACK compiled

declare
    my_new_var number := my_pack.my_var;
begin
    dbms_output.put_line(my_new_var);
end;
-- 1
```

- E is true:
> **Each subprogram is compiled and stored in executable form, which can be invoked repeatedly. Because stored subprograms run in the database server**, a single invocation over the network can start a large job. This division of work reduces network traffic and improves response times. Stored subprograms are cached and shared among users, which lowers memory requirements and invocation overhead.

[Oracle Documentation - PL/SQL Subprograms](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/plsql-subprograms.html)

- F is false:
> Block - It has an **optional declarative part**, a required executable part, and an optional exception-handling part. Declarations are local to the block and cease to exist when the block completes execution. Blocks can be nested.
```plsql
begin
    dbms_output.put_line('hello world');
end;
-- hello world
```

[Oracle Documentation - Block](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/block.html)

- G is false: a function's parameters are optional
```plsql
create or replace function f1 return number
is begin
    null;
end;
-- Function F1 compiled
```

# 007
Only the answer A is correct:
```plsql
create or replace procedure price_divide(p_id number, p_val number) is
    v_price number;
begin
    select prod_list_price into v_price from sh.products where prod_id = p_id;
    begin
        dbms_output.put_line(v_price/p_val);
        exception when zero_divide then
             dbms_output.put_line('error in inner block');
    end;
    exception when zero_divide then
        dbms_output.put_line('error in outer block');
end;
-- Procedure PRICE_DIVIDE compiled

begin
    price_divide(30,0);
    exception when zero_divide then
        dbms_output.put_line('error in calling block');
end;
-- error in inner block
```

# 008

# 009

- A is true:
```plsql
create or replace procedure calc_price is
    price number := 0;
begin
    declare
        price number;
    begin
        price := calc_price.price;
        dbms_output.put_line(price);
    end;
end;
-- Procedure CALC_PRICE compiled

begin
    calc_price;
end;
-- 0
```

- B is true:
```plsql
<<outer>>
declare
    price number := 0;
    procedure calc_price as
        begin
            dbms_output.put_line(price);
        end;
begin
    calc_price;
end;
-- 0
```

- C is false:
```plsql
<<outer>>
<<inner>>
declare
    price number := 0;
begin
    <<inner>>
    declare
        price number := null;
    begin
        price := inner.price;
        dbms_output.put_line(price);
    end;
end;
-- (no output)
```

- D is false:
```plsql
<<outer>>
<<inner>>
declare
    price number := 0;
begin
    <<inner>>
    declare
        price number := null;
    begin
        price := inner.price;
        dbms_output.put_line(price);
    end;
end;
-- (no output)
```

- E is false:
```plsql
<<outer>>
declare
    price number;
begin
    <<inner>>
    declare
        price number;
    begin
        price := 0;
    end;
    dbms_output.put_line(price);
end;
-- (no output)
```

# 010
- A is true:
```plsql
declare
    printer_name# number := 0;
begin
    dbms_output.put_line(printer_name#);
end;
-- 0
```

- B is false:
```plsql
declare
    1to7number number := 0;
begin
    dbms_output.put_line(1to7number);
end;
-- PLS-00103: Encountered the symbol "1" when expecting one of the following:
```

- C is false:
```plsql
declare
    yesterday's_date number := 0;
begin
    dbms_output.put_line(yesterday's_date);
end;
-- PLS-00103: Encountered the symbol "s_date number := 0;
```

- D is true:
```plsql
declare
    leap$year number := 0;
begin
    dbms_output.put_line(leap$year);
end;
-- 0
```

- E is true:
```plsql
declare
    Number_of_days_between_March_and_April number := 0;
begin
    dbms_output.put_line(Number_of_days_between_March_and_April);
end;
-- 0
```

- F is false:
```plsql
declare
   #printer_name number := 0;
begin
    dbms_output.put_line(#printer_name);
end;
-- PLS-00103: Encountered the symbol "#" when expecting one of the following:
```

- G is true:
```plsql
declare
   v_fname number := 0;
begin
    dbms_output.put_line(v_fname);
end;
-- 0
```

# 011

# 012
- A is true:
```plsql
declare
    v_price number :=44.99;
    v_pdt_name varchar2(15);
begin
    select prod_name into v_pdt_name
    from sh.products
    where prod_list_price = v_price;

    dbms_output.put_line('product name is :' ||v_pdt_name);

    exception when others then
        dbms_output.put_line('more than one row found');
end;
-- more than one row found
```

- B is false:
```plsql
declare
    v_price number :=44.99;
    v_pdt_name varchar2(15);
begin
    select prod_name into v_pdt_name
    from sh.products
    where prod_list_price = v_price;

    if sql%rowcount > 1 then
        dbms_output.put_line('more than one row found');
    else
        dbms_output.put_line('product name is :'||v_pdt_name);
    end if;
end;
-- ORA-01422: exact fetch returns more than requested number of rows
```

- C is false:
```plsql
declare
    v_price number :=44.99;
    v_pdt_name varchar2(15);
begin
    select prod_name into v_pdt_name
    from sh.products
    where prod_list_price = v_price;

    exception when too_many_rows then
        dbms_output.put_line('more than one row found');
    
    dbms_output.put_line('product name is :'||v_pdt_name);
end;
-- more than one row found
-- product name is :
```

- D is false:
```plsql
declare
    v_price number :=44.99;
    v_pdt_name varchar2(15);
begin
    select prod_name into v_pdt_name
    from sh.products
    where prod_list_price = v_price;

   if too_many_rows then
        dbms_output.put_line('more than one row found');
    else
        dbms_output.put_line('product name is :'||v_pdt_name);
    end if;
end;
-- PLS-00382: expression is of wrong type
```

- E is false:
```plsql
declare
    v_price number :=44.99;
    v_pdt_name varchar2(15);
begin
    select prod_name into v_pdt_name
    from sh.products
    where prod_list_price = v_price;

   exception when others then
        raise too_many_rows;
    dbms_output.put_line('product name is :'||v_pdt_name);
end;
-- ORA-01422: exact fetch returns more than requested number of rows
```

# 013
only the answer C is correct:
```plsql
declare
    v_salary number(10);
    v_commission number(10,2):=0;
    v_title employees.job_id%type;
begin
    select job_id,salary
    into v_title,v_salary
    from employees
    where employee_id=100;

    if v_salary<20000 and v_title='AD_PRES' then
        v_commission:=v_salary*0.1;
        elsif v_salary between 20000 and 40000 then
            v_commission:=v_salary*0.2;
        elsif v_salary > 4000 and v_title <> 'AD_PRES' then
            v_commission:=v_salary*0.3;
        else
            v_commission:=v_salary*0.05;
    end if;
    dbms_output.put_line(v_commission);
end;
-- 2500
```

# 014
the original procedure can't be executed:
```plsql
create or replace procedure pdt_report (p_pdt_price in out number) is
    cursor c_pdt(cur_price in out number) is
        select * from products where price>cur_price;
    v_pdt_name varchar2(20);
begin
    for r in c_pdt(p_pdt_price) loop
        v_pdt_name:=r.pdt_name;
        dbms_output.put_line(v_pdt_name);
    end loop;
end;
-- Procedure PDT_REPORT compiled

begin
    pdt_report(1);
end;
-- PLS-00363: expression '1' cannot be used as an assignment target
```

answers A-B are correct:
```plsql
create or replace procedure pdt_report (p_pdt_price in number) is
    cursor c_pdt(cur_price in number) is
        select * from products where price>cur_price;
    v_pdt_name varchar2(20);
begin
    for r in c_pdt(p_pdt_price) loop
        v_pdt_name:=r.pdt_name;
        dbms_output.put_line(v_pdt_name);
    end loop;
end;
-- Procedure PDT_REPORT compiled

begin
    pdt_report(1);
end;
-- PL/SQL procedure successfully completed.
```

# 015

- A is false:
```plsql
declare
    my_exception exception;
begin
    raise my_exception;
    exception when my_exception then
        dbms_output.put_line('testing my_exception');
end;
-- testing my_exception
```

- B is false:
> User-Defined Exceptions - **You must raise a user-defined exception explicitly**.

[Oracle Documentation - User-Defined Exceptions](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/plsql-error-handling.html)

- C is true:
```plsql
declare
    my_exception exception;
begin
    raise my_exception;
    exception when my_exception then
        dbms_output.put_line('testing my_exception');
end;
-- testing my_exception
```

- D is false:
> An internally defined exception does not have a name unless either PL/SQL gives it one or you give it one.

[Oracle Documentation - Internally Defined Exceptions](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/plsql-error-handling.html)

- E is true:
> Predefined exceptions are internally defined exceptions that have predefined names, which **PL/SQL declares globally in the package STANDARD**.

[Oracle Documentation - Predefined Exceptions](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/plsql-error-handling.html)

# 017

correct answer is C:
> ACCESSIBLE BY Hint Syntax: `ACCESSIBLE BY (accessor)`
> An entity named in an accessor is **not required to exist**.
```plsql
create or replace procedure protected_proc
    accessible by (calling_proc)
as
begin
    dbms_output.put_line('test1: protected_proc');
end;
-- Procedure PROTECTED_PROC compiled
```

# 018

correct answer is A:
```plsql
create table employees(emp_id number(10),first_name varchar2(30),last_name varchar2(30),salary number(8,2));
-- Table EMPLOYEES created.

insert into employees values (200,'john','smith',99999);
-- 1 row inserted.

declare
    first_name employees.first_name%type;
    v_name first_name%type;
    v_salary number(5);
    v_empid employees.emp_id%type := 200;
begin
    select salary,first_name
    into v_salary,first_name
    from employees
    where emp_id=v_empid;
end;
-- PL/SQL procedure successfully completed.
```
if the employee with EMP_ID=200 would have a salary higher than 99999, this would happen:
```plsql
create table employees(emp_id number(10),first_name varchar2(30),last_name varchar2(30),salary number(8,2));
-- Table EMPLOYEES created.

insert into employees values (200,'john','smith',100000);
-- 1 row inserted.

declare
    first_name employees.first_name%type;
    v_name first_name%type;
    v_salary number(5);
    v_empid employees.emp_id%type := 200;
begin
    select salary,first_name
    into v_salary,first_name
    from employees
    where emp_id=v_empid;
end;
-- ORA-06502: PL/SQL: numeric or value error: number precision too large
```

# 019

- A & B are correct:
    - you can't use a SQL function in a procedure's parameters (`upper(pdt_name_in)`)
    - DECODE can't be used outside a SQL statement
```plsql
create or replace procedure report(upper(pdt_name_in) in products.pdt_name%type) is
    current_price number:=155.55;
    new_price number(10,2) := round(current_price + (current_price*0.5));
    compare_value varchar2(20);
begin
    compare_value := decode(new_price,1001,'above 1000','below 1000');
    dbms_output.put_line(s.nextval|| ' ' || upper('new price') || ' ' || to_char(new_price));
    dbms_output.put_line(s.currval+1 || ' ' || upper('new price') || ' ' || new_price);
end;
-- (first error) PLS-00103: Encountered the symbol "(" when expecting one of the following: in out <an identifier> <a double-quoted delimited-identifier> table

-- (second error) PLS-00204: function or pseudo-column 'DECODE' may be used inside a SQL statement only
```

# 020

answers D,E,F are correct:
> You can create triggers for these events:
> **`AFTER STARTUP`**
> **`BEFORE SHUTDOWN`**
> `AFTER DB_ROLE_CHANGE`
> **`AFTER SERVERERROR`**
> `AFTER LOGON`
> `BEFORE LOGOFF`
> `AFTER SUSPEND`
> `AFTER CLONE`
> `BEFORE UNPLUG`
> `[BEFORE|AFTER] SET CONTAINER`

[Oracle Documentation - CREATE TRIGGER Statement](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/CREATE-TRIGGER-statement.html)