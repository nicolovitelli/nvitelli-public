# 001
- A is true:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY "Last name";
-- No data found
~~~

- B is true:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY 2, cust_id;
-- No data found
~~~

- C is false:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY CUST_NO;
-- ORA-00904: "CUST_NO": invalid identifier
~~~

- D is true:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY 2, 1;
-- No data found
~~~

- E is false:
~~~sql
SELECT cust_id, cust_last_name "Last name"
FROM sh.customers
WHERE country_id = 10
UNION
SELECT cust_id CUST_NO, cust_last_name
FROM sh.customers
WHERE country_id = 30
ORDER BY "CUST_NO";
-- ORA-00904: "CUST_NO": invalid identifier
~~~

# 002

- A is false:
~~~sql
select count(cust_id), cust_first_name
from sh.customers
where cust_gender = 'M'
group by cust_first_name
having count(1) > 60;
-- COUNT(CUST_ID) CUST_FIRST_NAME 
-- -------------- --------------- 
--             64 Antony          
--             63 Arnand          
--             63 Baldwin         
--             63 Bartholomew     
--             64 Baxter   
~~~

- B is true:
~~~sql
select count(cust_id), cust_first_name
from sh.customers
group by cust_first_name
having count(1) > 60;
-- COUNT(CUST_ID) CUST_FIRST_NAME 
-- -------------- --------------- 
--             63 Angela          
--             64 Antony          
--             63 Arnand          
--             65 Ashley          
--             64 August  
~~~

- C is false:
~~~sql
select count(cust_id) cnt, cust_first_name
from sh.customers
group by cust_first_name
having cnt > 60;
-- ORA-00904: "CNT": invalid identifier
~~~

- D is true and E is false?
Technically, the query processing order is:
- FROM
- JOINs
- GROUP BY
- HAVING
- WHERE
- SELECT
- ORDER BY
So HAVING clause is calculated before WHERE, therefore D should be false and E true.

Articles:
- [article 1](https://tipsfororacle.blogspot.com/2016/10/oracle-sql-query-order-of-operations.html)
- [article 2](https://logicalread.com/order-query-execution-oracle-12c-mc06/)