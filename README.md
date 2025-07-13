# sql_50_interview_question

🔹 Basic SQL Interview Questions


1.What is SQL?

2.What are the different types of SQL statements?

3.What is the difference between DELETE, TRUNCATE, and DROP?

4.What is a primary key?

5.What is a foreign key?

6.What are constraints in SQL?

7.What is the difference between WHERE and HAVING?

8.What is the difference between CHAR and VARCHAR?

9.What is normalization? Explain its types.

10.What is denormalization?

🔹 SQL Queries (Hands-on)


11.Write a query to fetch all records from a table.

12.Write a query to fetch unique values from a column.

13.Write a SQL query to get the second highest salary.

14.Write a query to count the number of employees in each department.

15.Write a query to fetch employees whose names start with 'A'.

16.Write a query to fetch employees hired in the last 30 days.

17.Write a query to find NULL values in a column.

18.How do you update data in a table?

19.Write a query to delete duplicate records from a table.

20.Write a query to swap values between two columns.

🔹 Joins and Relationships


21.What are joins in SQL? Name all types.

22.What is the difference between INNER JOIN and LEFT JOIN?

23.What is a self join?

24.What is a cross join? Where is it used?

25.Write a query to join two tables and fetch matching records.

🔹 Indexes and Views


26.What is an index? What types are there?

27.How does indexing improve query performance?

28.What is a view in SQL? How is it different from a table?

29.Can you update a view in SQL?

30.What are materialized views?

🔹 Advanced SQL Concepts


31.What are subqueries? Explain with an example.

32.What is a correlated subquery?

33.What are window functions in SQL?

34.What is RANK() vs DENSE_RANK()?

35.What are GROUP BY and ORDER BY clauses?

🔹 Transactions and Locks


36.What is a transaction? What are its properties (ACID)?

37.What is the difference between COMMIT and ROLLBACK?

39.What is a savepoint in SQL?

40.What are locks in databases?

41.What is the difference between pessimistic and optimistic locking?

🔹 Stored Procedures, Triggers, and Functions


42.What is a stored procedure?

43.What is the difference between stored procedures and functions?

44.What is a trigger in SQL?

45.When would you use a trigger?

🔹 Data Types and Functions


46.What are common SQL data types?

47.What is the difference between NOW(), CURRENT_TIMESTAMP, and SYSDATE()?

48.What is the use of COALESCE()?

47.What is the difference between ISNULL() and NULLIF()?

🔹 Performance & Best Practices


49.How can you optimize a slow SQL query?

50.What are common reasons for poor SQL performance?



🔹 Part 1: SQL Basics (1–10)


1. What is SQL?
Answer:

SQL (Structured Query Language) is a standard language used to communicate with relational databases for storing, retrieving, and manipulating data.

3. Types of SQL Statements
   
Answer:

DDL (Data Definition Language): CREATE, ALTER, DROP

DML (Data Manipulation Language): INSERT, UPDATE, DELETE

DQL (Data Query Language): SELECT

TCL (Transaction Control Language): COMMIT, ROLLBACK, SAVEPOINT

DCL (Data Control Language): GRANT, REVOKE

3. Difference Between DELETE, TRUNCATE, and DROP

   
| Command  | Deletes Rows | Rollback Possible | Resets Identity | Deletes Structure |
| -------- | ------------ | ----------------- | --------------- | ----------------- |
| DELETE   | Yes          | Yes               | No              | No                |
| TRUNCATE | Yes (All)    | No                | Yes             | No                |
| DROP     | Yes (All)    | No                | N/A             | Yes               |

DELETE FROM employees WHERE id = 1;
TRUNCATE TABLE employees;
DROP TABLE employees;

4. What is a Primary Key?
Answer:
A primary key uniquely identifies each record in a table. It must be unique and not null.

CREATE TABLE employees (
  id SERIAL PRIMARY KEY,
  name TEXT
);

5. What is a Foreign Key?
Answer:
A foreign key links two tables together by referencing the primary key in another table.

CREATE TABLE departments (
  id SERIAL PRIMARY KEY,
  name TEXT
);

CREATE TABLE employees (
  id SERIAL PRIMARY KEY,
  name TEXT,
  dept_id INT,
  FOREIGN KEY (dept_id) REFERENCES departments(id)
);


6. What are Constraints in SQL?
Answer:
Constraints enforce rules on data in tables.

NOT NULL

UNIQUE

PRIMARY KEY

FOREIGN KEY

CHECK

DEFAULT

CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  age INT CHECK (age >= 18)
);


7. Difference Between WHERE and HAVING
WHERE filters rows before aggregation.
HAVING filters after aggregation.


-- WHERE filters individual rows
SELECT * FROM employees WHERE age > 30;

-- HAVING filters groups
SELECT dept_id, COUNT(*) 
FROM employees 
GROUP BY dept_id 
HAVING COUNT(*) > 5;

8. Difference Between CHAR and VARCHAR
CHAR(n): Fixed length, padded with spaces.

VARCHAR(n): Variable length, up to n characters.

CREATE TABLE example (
  fixed CHAR(10),
  variable VARCHAR(10)
);


9. What is Normalization?
Answer:
Normalization organizes data to reduce redundancy.

1NF: Atomic columns

2NF: Remove partial dependencies

3NF: Remove transitive dependencies



10. What is Denormalization?
Answer:
Denormalization is the process of combining tables to improve performance, at the cost of redundancy.

🔹 Part 2: SQL Queries (11–20)
11. Write a query to fetch all records from a table.
SELECT * FROM employees;

12. Write a query to fetch unique values from a column.

SELECT DISTINCT department FROM employees;

13. Write a SQL query to get the second highest salary.

SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

✅ Alternatively (PostgreSQL syntax):

SELECT salary
FROM employees
ORDER BY salary DESC
OFFSET 1 LIMIT 1;

14. Write a query to count the number of employees in each department.

SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;


15. Write a query to fetch employees whose names start with 'A'.

SELECT * FROM employees
WHERE name LIKE 'A%';


16. Write a query to fetch employees hired in the last 30 days.

SELECT * FROM employees
WHERE hire_date >= CURRENT_DATE - INTERVAL '30 days';


17. Write a query to find NULL values in a column.

SELECT * FROM employees
WHERE manager_id IS NULL;


18. How do you update data in a table?

UPDATE employees
SET salary = salary * 1.10
WHERE department = 'Sales';


19. Write a query to delete duplicate records from a table.
✅ Using CTE and ROW_NUMBER() (PostgreSQL):

WITH duplicates AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY name, email ORDER BY id) AS rn
  FROM employees
)
DELETE FROM employees
WHERE id IN (SELECT id FROM duplicates WHERE rn > 1);


20. Write a query to swap values between two columns.

UPDATE employees
SET column1 = column2,
    column2 = column1;

    
✅ In PostgreSQL:
UPDATE employees
SET column1 = column2,
    column2 = column1;

    
But for atomic swap (avoid same value being overwritten), use a temp:
UPDATE employees
SET column1 = column2,
    column2 = column1;

🔹 Part 3: Joins and Relationships (21–25)

21. What are joins in SQL? Name all types.
    
Answer:
Joins are used to combine rows from two or more tables based on a related column.

Types of Joins:

INNER JOIN: Only matching rows from both tables.

LEFT JOIN: All rows from the left table and matching from right.

RIGHT JOIN: All rows from the right table and matching from left.

FULL OUTER JOIN: All rows from both tables.

CROSS JOIN: Cartesian product (all combinations).

SELF JOIN: A table joined with itself.

22. Difference between INNER JOIN and LEFT JOIN


| Feature       | INNER JOIN         | LEFT JOIN                           |
| ------------- | ------------------ | ----------------------------------- |
| Returns       | Only matching rows | All from left + matching from right |
| Missing match | Not included       | NULLs for missing right-side rows   |


-- INNER JOIN
SELECT e.name, d.name AS dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;

-- LEFT JOIN
SELECT e.name, d.name AS dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;


23. What is a SELF JOIN?

Answer:
A self join is a regular join where a table is joined with itself.

SELECT a.name AS employee, b.name AS manager
FROM employees a
JOIN employees b ON a.manager_id = b.id;

24. What is a CROSS JOIN? Where is it used?
Answer:
A CROSS JOIN returns the Cartesian product of two tables.

Use case: Generating combinations (e.g. pairing sizes with colors).

SELECT * FROM colors CROSS JOIN sizes;

If colors has 3 rows and sizes has 2 → result has 6 rows.



25. Write a query to join two tables and fetch matching records.

SELECT e.name, d.name AS dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.id;


🔹 Part 4: Indexes and Views (26–30)

26. What is an index? What types are there?
Answer:
An index is a database object that improves the speed of data retrieval.

Types of indexes:

B-tree index (default in PostgreSQL)

Hash index

Unique index

Composite index

Partial index

GIN/GiST indexes (used for full-text search, arrays)

-- Creating a basic index
CREATE INDEX idx_name ON employees(name);

-- Creating a unique index
CREATE UNIQUE INDEX idx_email ON employees(email);

27. How does indexing improve query performance?
Answer:
Indexes allow the database to locate data without scanning the entire table. Think of it like a book’s index – it helps you find information quickly.

Without index:

SELECT * FROM employees WHERE email = 'abc@example.com';
→ Full table scan
With index on email → O(log n) search time


28. What is a view in SQL? How is it different from a table?

Answer:
A view is a virtual table based on a SQL query. It does not store data itself, only the query definition.


CREATE VIEW sales_summary AS
SELECT department, SUM(sales) AS total_sales
FROM employees
GROUP BY department;
✔️ Views are useful for:

Abstraction

Reuse of complex queries

Security (limiting column access)


29. Can you update a view in SQL?
Answer:

Yes, but only if:

The view is based on a single table

It doesn’t use aggregate functions or GROUP BY

No joins, subqueries, or DISTINCT

✅ Example:


CREATE VIEW emp_view AS
SELECT id, name FROM employees;

-- Updatable
UPDATE emp_view SET name = 'John' WHERE id = 2;

30. What are materialized views?
Answer:
A materialized view stores the query result physically, unlike normal views which are just saved queries. They can be refreshed manually or automatically.


CREATE MATERIALIZED VIEW sales_cache AS
SELECT department, SUM(sales)
FROM employees
GROUP BY department;

To refresh it:
REFRESH MATERIALIZED VIEW sales_cache;


🔹 Part 5: Advanced SQL Concepts (31–35)
31. What are subqueries? Explain with an example.
Answer:
A subquery is a query inside another SQL query. It can be used in SELECT, WHERE, or FROM.


-- Example: Get employees earning more than the average salary

SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);


32. What is a correlated subquery?
Answer:
A correlated subquery uses values from the outer query. It runs once for each row of the outer query.


-- Example: Find employees with the highest salary in their department
SELECT e1.name, e1.salary
FROM employees e1
WHERE salary = (
    SELECT MAX(e2.salary)
    FROM employees e2
    WHERE e1.department = e2.department
);


33. What are window functions in SQL?
Answer:
Window functions perform calculations across a set of rows related to the current row, without collapsing rows like GROUP BY.


SELECT name, department, salary,
       RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM employees;

📌 Common window functions:

RANK(), DENSE_RANK(), ROW_NUMBER()

SUM(), AVG(), COUNT() OVER (...)

LEAD(), LAG(), FIRST_VALUE()



34. What is RANK() vs DENSE_RANK()?



| Function       | Gap in Ranks | Example (3rd rank skipped) |
| -------------- | ------------ | -------------------------- |
| `RANK()`       | Yes          | 1, 2, 2, 4                 |
| `DENSE_RANK()` | No           | 1, 2, 2, 3                 |


SELECT name, salary,
       RANK() OVER (ORDER BY salary DESC) AS rank,
       DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
FROM employees;


35. What are GROUP BY and ORDER BY clauses?
GROUP BY: Groups rows with the same values.
ORDER BY: Sorts the result set.

-- Group by department and count employees

SELECT department, COUNT(*) AS emp_count
FROM employees
GROUP BY department;

-- Sort by salary descending

SELECT name, salary
FROM employees
ORDER BY salary DESC;


🔹 Part 6: Transactions and Locks (36–40)

36. What is a transaction? What are its ACID properties?
Answer:

A transaction is a group of SQL operations executed as a single unit. It must follow ACID properties:

A – Atomicity: All operations succeed or none do.

C – Consistency: Keeps database in a valid state.

I – Isolation: Transactions don’t interfere with each other.

D – Durability: Once committed, changes are permanent.


BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;


37. Difference between COMMIT and ROLLBACK

    

| Operation  | Meaning                                    | Effect                 |
| ---------- | ------------------------------------------ | ---------------------- |
| `COMMIT`   | Saves all changes made in the transaction  | Changes are permanent  |
| `ROLLBACK` | Undoes all changes made in the transaction | Reverts to last COMMIT |


BEGIN;

DELETE FROM employees WHERE id = 101;

-- Undo the deletion
ROLLBACK;


38. What is a SAVEPOINT in SQL?
Answer:
SAVEPOINT allows partial rollbacks within a transaction.


BEGIN;

UPDATE employees SET salary = salary * 1.1;
SAVEPOINT before_bonus;

UPDATE employees SET bonus = 1000;

-- Oops, mistake! Roll back only bonus update
ROLLBACK TO SAVEPOINT before_bonus;

COMMIT;


39. What are locks in databases?
Answer:
Locks are mechanisms to prevent concurrent users from modifying the same data at the same time.

Types:
Shared Lock (Read lock): Multiple reads allowed.
Exclusive Lock (Write lock): Only one writer, no other reads/writes.

Used automatically by the DB engine during transactions.



40. Difference between Pessimistic and Optimistic Locking

    
| Lock Type           | Behavior                                       | Use Case                   |
| ------------------- | ---------------------------------------------- | -------------------------- |
| Pessimistic Locking | Locks data before accessing                    | High-conflict environments |
| Optimistic Locking  | Allows access but checks for conflicts at save | Low-conflict scenarios     |


🔸 Pessimistic:


SELECT * FROM accounts WHERE id = 1 FOR UPDATE;


🔸 Optimistic (via version column):
UPDATE products
SET price = 100, version = version + 1
WHERE id = 1 AND version = 2;


🔹 Part 7: Stored Procedures, Triggers, and Functions (41–44)


41. What is a stored procedure?
Answer:
A stored procedure is a precompiled collection of one or more SQL statements that can be executed with a single call.

✅ Benefits:

Improves performance (compiled once)

Encourages reuse

Helps with security and abstraction


-- PostgreSQL stored procedure


CREATE OR REPLACE PROCEDURE give_bonus(emp_id INT, bonus_amount NUMERIC)
LANGUAGE plpgsql
AS $$
BEGIN
  UPDATE employees SET bonus = bonus + bonus_amount
  WHERE id = emp_id;
END;
$$;

-- Call it
CALL give_bonus(101, 500);


42. Difference between stored procedures and functions

    
    
| Feature           | Stored Procedure             | Function                    |
| ----------------- | ---------------------------- | --------------------------- |
| Returns a value?  | No (optional OUT parameters) | Yes (must return a value)   |
| Called with       | `CALL`                       | `SELECT function_name(...)` |
| Used in queries?  | No                           | Yes                         |
| Can use `RETURN`? | Only for exiting procedure   | Yes (to return result)      |


✅ Example function:

CREATE OR REPLACE FUNCTION get_salary(emp_id INT)
RETURNS NUMERIC AS $$
BEGIN
  RETURN (SELECT salary FROM employees WHERE id = emp_id);
END;
$$ LANGUAGE plpgsql;

-- Use:
SELECT get_salary(101);



43. What is a trigger in SQL?
Answer:
A trigger automatically performs an action in response to an event on a table (INSERT, UPDATE, DELETE).

✅ Example use cases:

    Auditing
  
    Enforcing business rules

    Auto-updating fields


44. When would you use a trigger?
Answer:
Triggers are used when you need automatic and consistent enforcement of rules or actions, such as:

   Automatically updating a timestamp column

   Logging changes to a table

   Restricting certain operations


-- Example: Update `updated_at` on row change

CREATE OR REPLACE FUNCTION update_modified_time()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_time_trigger
BEFORE UPDATE ON employees
FOR EACH ROW
EXECUTE FUNCTION update_modified_time();



🔹 Part 8: Data Types and Functions (45–48)


45. What are common SQL data types?
Answer:
Here are commonly used PostgreSQL data types:



| Category   | Data Types                                               |
| ---------- | -------------------------------------------------------- |
| Numeric    | `INT`, `BIGINT`, `DECIMAL`, `NUMERIC`, `FLOAT`, `SERIAL` |
| Character  | `CHAR(n)`, `VARCHAR(n)`, `TEXT`                          |
| Date/Time  | `DATE`, `TIME`, `TIMESTAMP`, `INTERVAL`                  |
| Boolean    | `BOOLEAN` (`TRUE` / `FALSE`)                             |
| Binary     | `BYTEA`                                                  |
| JSON/Array | `JSON`, `JSONB`, `ARRAY`                                 |

CREATE TABLE sample (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  price NUMERIC(10, 2),
  created_at TIMESTAMP DEFAULT NOW(),
  active BOOLEAN
);


46. Difference between NOW(), CURRENT_TIMESTAMP, and SYSDATE()



| Function            | Description                        |
| ------------------- | ---------------------------------- |
| `NOW()`             | Returns current timestamp          |
| `CURRENT_TIMESTAMP` | ANSI standard, same as `NOW()`     |
| `SYSDATE()` (MySQL) | Returns time of function execution |

✅ PostgreSQL:


SELECT NOW();                 -- 2025-07-13 21:10:45
SELECT CURRENT_TIMESTAMP;     -- 2025-07-13 21:10:45

✅ MySQL:

SELECT SYSDATE();             -- Slightly different behavior: time of execution



47. What is the use of COALESCE()?
Answer:
COALESCE() returns the first non-null value in the argument list. It's useful for handling NULLs.

SELECT name, COALESCE(bonus, 0) AS safe_bonus
FROM employees;


➡️ If bonus is NULL, it returns 0.

48. Difference between ISNULL() and NULLIF()


| Function       | Use Case                               |
| -------------- | -------------------------------------- |
| `ISNULL(x)`    | (In SQL Server) returns 1 if x is NULL |
| `NULLIF(x, y)` | Returns NULL if x = y; else x          |


✅ PostgreSQL example:


-- NULLIF: Prevent division by zero

SELECT salary / NULLIF(bonus, 0) FROM employees;


➡️ If bonus = 0, NULLIF makes it NULL and avoids divide-by-zero error.

✅ Shall I continue with Part 9: Performance & Best Practices (Questions 49–50)?



🔹 Part 9: Performance & Best Practices (49–50)


49. How can you optimize a slow SQL query?

Answer:

Here are key techniques to improve SQL performance:

✅ Use Indexes Wisely

    Create indexes on columns used in WHERE, JOIN, and ORDER BY.

✅ **Avoid SELECT ***

    Fetch only the required columns.


  SELECT name, salary FROM employees;  -- ✅ Good
  SELECT * FROM employees;             -- ❌ Bad


✅ Use EXPLAIN ANALYZE to Debug

In PostgreSQL:


   EXPLAIN ANALYZE SELECT * FROM employees WHERE salary > 50000;

✅ Use LIMIT/OFFSET with large data


   SELECT * FROM employees LIMIT 10 OFFSET 100;
✅ Avoid functions on indexed columns in WHERE


-- ❌ Slower (can't use index)
WHERE UPPER(name) = 'JOHN'

-- ✅ Faster
WHERE name = 'John'


✅ Avoid correlated subqueries when possible


50. What are common reasons for poor SQL performance?
Answer:
Common causes include:

     Missing Indexes
    
    *Poor query structure (e.g., using SELECT )
    
    Too many joins or subqueries
    
    Not analyzing/explaining query plans
    
    Stale statistics or bloated tables
    
    Large datasets without pagination
    
    Using OR instead of UNION ALL (in some cases)

✅ Example bad query:
      SELECT * FROM orders WHERE status = 'shipped' OR status = 'delivered';
      
✅ Optimized alternative:
      SELECT * FROM orders WHERE status IN ('shipped', 'delivered');






