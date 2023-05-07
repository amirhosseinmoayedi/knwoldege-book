#database #sql #postgres 
The `ALTER` command in SQL is <font color="#de7802">used to modify existing database objects</font> such as tables, columns, and constraints.
```sql
ALTER TABLE students
ADD age INT;
```

you <u>can read data from a file that contain sql commands</u>, the extension is `.sql`.

<font color="#76923c">TimeStamps</font> are used to state <font color="#76923c">when</font>, like for posts we use timestamp because the <font color="#76923c">time zone is different for some readers and when you say</font>.

The `now()` function in Postgres<u> returns the current date and time</u> <font color="#fac08f">according to the server's clock.</font>

```sql
CREATE TABLE employees (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT UNIQUE,
  hire_date DATE DEFAULT now()
);
```

```sql
select NOW() AT TIME ZONE 'UTC';
```
> **UTC** is<u> independent time zone</u> even thou it's for England.

we can see the list of time zones in postgres in table `pg_timezone_name`
```sql
SELECT * FROM pg_timezone_name;
```

### <font color="#31859b">Casting</font>
casting refers to <font color="#31859b">the process of converting a value of one data type to another data type</font>.

- <font color="#ffff00">Implicit casting</font> is done <u>automatically by PostgreSQL when it detects that a value needs to be converted</u> to a different data type. For example, if you try to add an integer and a decimal value, PostgreSQL will automatically convert the integer to a decimal type before performing the addition.
- <font color="#ffff00">Explicit casting</font>, on the other hand, is done using the `::` syntax to indicate the desired data type.

example **age is text**:
```sql
SELECT age::integer FROM my_table;
```

### <font color="#d99694">Intervall</font> 
you can use the `interval` data type<font color="#d99694"> to represent a duration of time</font>, such as hours, minutes, or days. You can<font color="#d99694"> also perform arithmetic operations</font> with intervals to add or subtract time from a date or timestamp value.
```sql
SELECT DATE '2022-05-03' + INTERVAL '1 week';
```

### <font color="#ffff00">Truncating</font>
removing some part from data or anything.
![[_resources/e408bff78c059b09dd10703b757ed55c.png]]

> <u>quries with same result might have different perfomance</u>. some times quries might scan the tables or rows of table is called <font color="#92d050">table scan</font> and it's slow.



<font color="#92d050">Table scan</font>, also known as<font color="#92d050"> full table scan</font>, is a <u>method of accessing data in a database table where the database engine reads every row of the table to find the data it needs</u>. can be prevented in ways like: **using `where` on column that is <u>unique</u> or has <u>index</u>**.

> **truncating is faster than casting**

`trunc()` is a function in PostgreSQL that is used to truncate a number to a specified number of decimal places.
```sql
SELECT trunc(3.14159, 2);
```


## <font color="#ffc000">Order of SQL Query</font>
1.  `FROM` clause: specifies the table or tables from which to select data.
2.  `JOIN` clause: joins two or more tables together based on a specified condition.
3.  `WHERE` clause: filters the rows based on a specified condition.
4.  `GROUP BY` clause: groups the rows based on one or more columns.
5.  `HAVING` clause: filters the groups based on a specified condition.
6.  `SELECT` clause: selects the columns to be returned in the result set.
7.  `DISTINCT` keyword: removes duplicate rows from the result set.
8.  `ORDER BY` clause: sorts the rows based on one or more columns.
9.  `LIMIT` or `TOP` clause: limits the number of rows returned in the result set.
![[_resources/fcb542145a85e690df0db04d4a64ec05.png]]

### <font color="#00b0f0">GROUP by</font>
the `GROUP BY` clause is used to <u>group rows in a table based on one or more columns</u>. The `GROUP BY` clause is <u>typically used in conjunction with aggregate functions</u>, such as `SUM`, `AVG`, `COUNT`, or `MAX`, to perform calculations on the grouped data.
```sql
SELECT department, AVG(salary) as average_salary
FROM employees
GROUP BY department;
```

### <font color="#366092">Data Replication</font>
<font color="#d83931">Vertical replication</font>, also known as <font color="#d83931">column-based replication</font>, is a database replication technique that <u>replicates individual columns of data between database servers</u>. This is in contrast to<font color="#ffc000"> horizontal replication</font>, which <font color="#ffc000">replicates entire rows of data between servers</font>.

> for performance<u> it's better to get the data that only you need</u>, like column that you need from a table.

### <font color="#76923c">subquery</font>
A subquery, also known as a <font color="#76923c">nested query</font>, is a query that is nested inside another query and <u>used to retrieve data that will be used in the outer query</u>.
```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

![[_resources/662ff20f17ad3f8420fa00ac882a312e.png]]
it works like this<u> the inner one will create a temp table</u> and t<u>he outer one will read from it</u>.


### <font color="#ffff00">Concurency</font>
Concurrency in PostgreSQL <u>refers to the ability of multiple users or processes to access and modify data in a database at the same time</u>.
![[_resources/062c99618957f5502559369083f7cd84.png]]

- <font color="#ffff00">Locking</font> is a mechanism that <u>prevents multiple users or processes from modifying the same data</u> at the same time. PostgreSQL supports **several types of locks**, including <u>row locks</u>,<u> table locks</u>, and <u>transaction-level locks</u>.

- <font color="#ffff00">Isolation</font> levels control <u>how changes made by one transaction are visible to other transactions</u>. 

![[_resources/91c16b885081e5e13a01af1f17570965.png]]

>  the locking process cost memory and time and the better the locking mechanism is the better the data base is.

multiple request recived and they would wait for the lock of previous to be done and then their transaction starts.

![[_resources/cc239b73e022016bdf6290770698a580.png]]
![[_resources/984f0e3d3376727a7d8525c3f3924cd1.png]]
`RETURN` statment will return the updated value or created value.

### <font color="#d99694">ON CONFLICT</font>
`ON CONFLICT` is a clause in SQL that is used to<u> handle conflicts</u> that may arise <u>when inserting or updating data in a table with a unique or primary key constraint</u>.
`ON CONFLICT` clause <u>specifies what action the database should take when a conflict occurs</u>.
```sql
INSERT INTO employees (id, name, department)
VALUES (123, 'John Doe', 'Marketing')
ON CONFLICT (id) DO UPDATE
SET department = EXCLUDED.department;
---
INSERT INTO employees (id, name, department)
VALUES (123, 'John Doe', 'Marketing')
ON CONFLICT (id) DO NOTHING;
```

### <font color="#548dd4">Transaction</font> 
transaction is a<u> sequence of one or more database operations that are treated as a single logical unit of work</u>.

Transactions are used to <font color="#548dd4">ensure data integrity and consistency</font> in the database, especially in multi-user or multi-process environments where multiple operations may be occurring simultaneously.

- `begin`  for starting a transaction 
- `commit` for committing it 
- **rollback** for reverting it
```sql
BEGIN; 

UPDATE accounts SET balance = balance - 100 WHERE account_id = 123;
INSERT INTO transactions (account_id, amount, type) VALUES (123, 100, 'debit');

UPDATE accounts SET balance = balance + 100 WHERE account_id = 456;
INSERT INTO transactions (account_id, amount, type) VALUES (456, 100, 'credit');

COMMIT;
```

When <u>encountering a lock during a transaction</u>, there are generally three approaches to handling the lock:
1. <font color="#ffc000"> Wait for the lock to be released</font>: This approach involves waiting for the lock to be released before proceeding with the transaction. This can be a good approach if the lock is expected to be released quickly and the transaction can afford to wait. However, if the lock is held for an extended period of time, this can cause the transaction to block other transactions and lead to performance issues.

2.  <font color="#ffc000">Retry the transaction</font>: This approach involves retrying the transaction after a short delay if a lock is encountered. This can be a good approach if the transaction is not critical and can be retried without causing any harm. However, if the transaction is critical or involves sensitive data, retrying the transaction multiple times can increase the risk of data corruption or inconsistency.

3.  <font color="#ffc000">Skip the locked part of the transaction</font>: This approach involves skipping the locked part of the transaction and proceeding with the rest of the transaction. This can be a good approach if the locked part of the transaction is not critical and can be safely skipped without causing any harm. However, if the locked part of the transaction is critical or involves sensitive data, skipping it can lead to data corruption or inconsistency.

- `FOR UPDATE` : used to <u>lock rows in a table when they are selected for update</u>, This is useful when multiple transactions need to update the same data
```sql
BEGIN;
SELECT * FROM accounts WHERE id = 123 FOR UPDATE;
UPDATE accounts SET balance = balance - 100 WHERE id = 123;
COMMIT;
```
 - `NO KEY UPDATE` is used to<u> prevent updates to the primary key or unique columns of a table</u>. This is useful when you want to prevent accidental updates to critical data.
```sql
UPDATE employees SET department = 'Marketing' WHERE id = 123
NO KEY UPDATE
RETURNING *;
```

### <font color="#366092">stored procedure</font>
stored procedure is a <font color="#d99694">precompiled and stored database program</font> that performs one or more database operations. Stored procedures are typically <font color="#d99694">used to encapsulate complex business logic or database operations that are frequently used by an application</font>.

```pgsql
CREATE OR REPLACE FUNCTION get_employee_count(dept_name text)
RETURNS integer AS
$$
DECLARE
  count integer;
BEGIN
  SELECT count(*) INTO count FROM employees WHERE department = dept_name;
  RETURN count;
END;
$$
LANGUAGE plpgsql;
```
`SELECT get_employee_count('Marketing');`


### <font color="#c3d69b">Trigger</font>
A trigger is a special type of database object that is associated<font color="#c3d69b"> with a table or view and automatically executes a specified set of actions in response</font> to a specific database event, such as an insert, update, or delete operation on the associated table or view.
```sql
CREATE TRIGGER update_last_modified
ON my_table
AFTER UPDATE
AS
BEGIN
  UPDATE my_table SET last_modified = GETDATE() WHERE id IN (SELECT id FROM inserted);
END;
```
The `BEGIN` and `END` statements define the body of the trigger, which in this case consists of an update statement that sets the `last_modified` column of the updated rows to the current date and time using the `GETDATE()` function.

`inserted` table, which is a special table that contains the rows that were inserted or updated by the current operation.