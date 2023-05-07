#database #sql #postgres 

### <font color="#92cddc">Repeating</font>
Postgres <u>has functions for generating  random data</u>.
- `Repeat()`. This function returns a string that repeats a given input string a specified number of times.
```sql
SELECT REPEAT('a', 5);
```
- `generate_series()`. This function returns a set of values that can be used to generate sequences of numbers or dates.
```sql
SELECT * FROM generate_series(1, 10);
SELECT generate_series('2022-01-01'::date, '2022-01-31'::date, '1 day') AS dates;
-- generate_series(start_value, end_value)
```
- `random()`. This function returns a random value between 0 and 1.
```sql
SELECT random() * 100;
```

### <font color="#c3d69b">Like</font>
-   `LIKE`: A <u>pattern matching operator</u> in PostgreSQL that allows you to match strings using simple wildcard characters such as `%` and `_`. It is<u> case-sensitive</u>.
-   `ILIKE`: Similar to `LIKE`, but <u>case-insensitive</u>.
-   `NOT LIKE`: A negation of the `LIKE` operator. It selects rows where the string does not match the pattern.
-   `NOT ILIKE`: A negation of the `ILIKE` operator. It selects rows where the string does not match the pattern, ignoring case.

```sql
-- Find all rows where the name starts with "Jen"
SELECT *
FROM users
WHERE name LIKE 'Jen%';

```
### <font color="#95b3d7">Similar TO</font>
`SIMILAR TO` is a pattern matching operator in PostgreSQL that allows you to match strings using regular expressions. It is similar to the `LIKE` operator, but it provides more powerful pattern matching capabilities.
works by comparing a string to a pattern, which is specified using a regular expression.
```sql
SELECT *
FROM users
WHERE name SIMILAR TO 'Jen%';
```

### <font color="#e36c09">WHEN</font>
`WHEN` is a <font color="#e36c09">conditional statement in SQL</font> that is used in conjunction with `CASE` expressions. It allows you to specify one or more conditions that, when true, will return a specific result.
```sql
SELECT name,
       CASE
          WHEN age < 18 THEN 'Child'
          WHEN age >= 18 AND age < 65 THEN 'Adult'
          ELSE 'Senior'
       END AS age_group
FROM users;
```

### <font color="#b2a2c7">Text Functions</font>
-   `upper`: Converts a string to <u>uppercase</u>.
-   `lower`: Converts a string to <u>lowercase</u>.
-   `right`: **Extracts a specified number of characters from the right** side of a string.
-   `left`: Extracts a specified number of characters **from the left side** of a string.
-   `strpos`: **Finds the position of a substring** within a string.
-   `substr`: <u>Extracts a substring from a string</u>, starting at a specified position and optionally with a specified length.
-   `split_part`: <u>Splits a string into an array of substrings</u> using a specified delimiter and returns a specified element from the array.
-   `translate`: <u>Replaces each character in a string that matches a specified set of characters</u> with a corresponding replacement character.
```sql
-- Convert a string to uppercase
SELECT upper('hello world');

-- Convert a string to lowercase
SELECT lower('Hello World');

-- Extract the last 3 characters from a string
SELECT right('hello world', 3);

-- Extract the first 5 characters from a string
SELECT left('hello world', 5);

-- Find the position of the substring 'world' within a string
SELECT strpos('hello world', 'world');

-- Extract a substring from a string, starting at position 7 and with length 5
SELECT substr('hello world', 7, 5);

-- Split a string into an array using the delimiter '-' and return the second element
SELECT split_part('one-two-three', '-', 2);

-- Replace all occurrences of 'e' with 'a' in a string
SELECT translate('hello world', 'e', 'a');
```

### <font color="#548dd4">EXPLAIN ANALYZE</font>
used to <u>analyze the performance of a query</u> and to see <u>how</u> the <u>database planner has optimized the query</u>.
![[_resources/3dca911e45e020de4921ec4901fa2cc1.png]]


<font color="#ffff00">UTF-8</font> is <font color="#ffff00">compression for UNICODE</font>. the lenght of all char is not 32 it's different between 8 to 32.
![[_resources/4627065081869950916b9cfbef2b2339.png]]
![[_resources/d9718edcebf36dc42300180611c15f01.png]]

### <font color="#00b050">Hash</font> function
 a hash function is a function that takes an input value (such as a string, integer, or date) and returns a fixed-size hash value.
```sql
 SELECT md5('hello world');
```
![[_resources/282611146b620d0aeee948b62dd39842.png]]
![[_resources/623208eb8553b3f8eae4177cee6bd0c6.png]] 

> `UNIQUE` <u>constraint is a b-tree which does not allow duplicate</u> but normal b-tree does .


![[_resources/e4cafcc5d580e47109631c9ce347d719.png]]
we can create an index using hashes. 


### <font color="#de7802">Concatination</font>
Concatenation is the <u>process of combining two or more strings into a single string</u>.
In PostgreSQL, you can concatenate strings using the `||` operator or the `concat()` function.
```sql
SELECT 'hello' || 'world';
SELECT concat('hello', ' ', 'world');
```

### <font color="#d83931">Regex</font>
<font color="#d83931">pattern-matching language used to search and manipulate text data</font>.
<font color="#ffff00">greedy</font> means it <u>will select the longest match</u>.
![[_resources/8fc7bd75ec9d6d2f6a6ea85944d97188.png]]![[_resources/25099c16c069b72b024a51251c7060d7.png]]
```sql
SELECT *
FROM users
WHERE email ~* '^johndoe\d+@example.com$';
```


