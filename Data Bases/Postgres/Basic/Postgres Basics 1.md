#database #sql #postgres 

SQL is an <font color="#ffff00">Abstraction</font> to **tell the database what to do**.

**There are some databases in Postgres as default which to not mess with.

**Its better to not to work with db as a superuser**.(security concern)
name of super user in Postgres is `postgres`.

<font color="#fac08f">Schema</font>: is our <font color="#fac08f">contract with database to say that's what we need</font>.

you can connect to a db over server with `psql` because it's u**ses the network to connect to**, <u>even for local usage</u>.

### adding data to a table:
```sql
INSERT INTO table (name of fields) VALUES ( value to be inserted);
```

### delete a data from a table:
```sql
DELETE FROM table_name WHERE condidtion;
-- for all the table
DELETE FROM table_name;
```

### update:
```sql
UPDATE table_name SET field=value WHERE condition;
-- update the hole set
UPDATE table_name SET field=value;
```

### retreive:
```sql
-- * means every thing
SELECT * FROM table;
SELECT * FROM table WHERE condition;
```

### sorting:
```sql
SELECT * from table_name ORDER BY coulmn;
SELECT * from table_name ORDER BY coulmn DESC;
```

### condition for wild cards:
```sql
SELECT * FROM table_name WHERE coulmn LIKE some_regex;
```

`WHERE` <u>would create an index and compare that to the table</u> but `LIKE` <u>would do full table scan for finding rows because it can't create index</u>.

<font color="#548dd4">Index</font>: is like shortcut to the data, it's smaller than the value.

### limit/offset:
```sql
-- just show 2
SELECT * FROM table_name ORDER BY coulmn DESC LIMIT 2;
-- just show to but start from 1 not 0, because the default is 0
SELECT * FROM table_name ORDER BY coulmn DESC OFFSET 1 LIMIT 2;
```


`COUNT`( db function),Database <u> normally stores it so no operation is done</u>, but if <u>we add a condition</u> with `WHERE` then <u>it might do some operation</u>:
```sql
SELECT COUNT(*) FROM table_name;
```

Data types in Postgres:
![[_resources/28c18f830cfe8739ac29db04ed216158.png]]

-   `smallint`: 2-byte integer
-   `integer`: 4-byte integer
-   `bigint`: 8-byte integer
-   `numeric`: arbitrary precision decimal number
-   `real`: single precision floating-point number
-   `double precision`: double precision floating-point number
-   `boolean`: true or false
-   `char(n)`: fixed-length character string of length n
-   `varchar(n)`: variable-length character string of length up to n
-   `text`: variable-length character string of unlimited length
-   `date`: date (year, month, day)
-   `time`: time of day (no time zone)
-   `timestamp`: date and time (no time zone)
-   `timestamptz`: date and time with time zone
-   `interval`: time interval

`text` is no usually used with <font color="#e36c09">indexing</font> (`where`) and <font color="#e36c09">sorting</font> (`order by`).(it's super expensive)

### <font color="#92d050">character set sorting</font>
A character set is a <font color="#92d050">collection of characters that can be represented digitally</font>. <font color="#92d050">ASCII</font> is the <font color="#92d050">most common</font> character set and uses 7 or 8 bits (1 byte) to represent characters. However, some languages like Persian or Chinese <font color="#92d050">need more bits to represent their characters</font> due to the large number of characters in their writing systems. Sorting refers to arranging data based on specific criteria. Different character sets have different sorting rules, known as collation orders, that determine the order in which characters are sorted in a string. For example, ASCII sorts uppercase letters before lowercase letters, while other character sets may have a different order.


> we normally use `integer` which is **4 bit or 32 bite**.

> for <font color="#ffff00">scientific caculation</font> (not normal ones) it's better to use `Double`.

### <font color="#fac08f">Representing money</font>
In most database systems including PostgreSQL, the recommended data type for storing monetary values is `numeric` or `decimal`. This is because `numeric` is a <font color="#fac08f">fixed-point data type</font>, meaning it can <font color="#fac08f">represent exact decimal values without rounding errors</font>.

`timestamp` is<u> 64 bit</u>.
![[_resources/fd88cade78089d38453425e8608f7607.png]]

<font color="#31859b">Keys</font> are <font color="#31859b">used to relate tables</font>.

`SERIAL` is used to indicate that <font color="#ffff00">auto increment</font> value. usually used for `id`
```sql
CREATE TABLE transactions (
  id serial primary key,
  description varchar(50),
  amount numeric(10,2)
);
```

### <font color="#e36c09">primary key</font>
A primary key is a<font color="#e36c09"> special type of column in a table</font> that is <font color="#e36c09">used to uniquely identify each row</font> in the table. This is important <font color="#e36c09">for referencing the table from other tables or queries</font>. When you define a primary key on a table, the <font color="#e36c09">database system automatically creates an index for the primary key</font>. This index helps to speed up queries that reference the table by the primary key, making them faster and more efficient. In summary, a primary key is a way to ensure that each row in a table can be uniquely identified, and it also helps to improve the performance of queries that reference the table.

> `integer` <u>indexes are faster</u> than other types.

### <font color="#00b050">constraint</font>
 a constraint is <font color="#00b050">a rule that specifies the allowed values or actions for one or more columns in a table</font>.

| Constraint Type | Description |
| --- | --- |
| Primary key | A column or combination of columns that uniquely identifies each row in a table. Primary keys are used to enforce referential integrity and are often used as the basis for foreign keys in other tables. |
| Foreign key | A column or combination of columns that references the primary key in another table. Foreign keys are used to enforce referential integrity and ensure that data in related tables is consistent. |
| Unique | A column or combination of columns that must have unique values across all rows in a table. Unique constraints are similar to primary keys, but do not necessarily identify each row uniquely.(has index for value checking) |
| Default | A default value that is used for a column when a new row is inserted into the table, if no other value is provided. Default constraints are used to ensure that columns have a value, even if the user does not specify one. |
| Not null | A constraint that requires a column to have a value for each row in the table. Not null constraints are used to prevent null values from being inserted into a column. |

### <font color="#76923c">Auto increment</font>
Auto-increment is a feature in databases that <font color="#76923c">automatically generates a new, unique value for a column with each new row inserted into the table</font>. This is often used for<font color="#76923c"> primary key</font> columns to ensure that each row has a unique identifier without the need for manual input.
