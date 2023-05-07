---
title: 'Python '
updated: 2022-06-03 11:16:35Z
created: 2022-06-03 09:17:56Z
---

While we can do a lot of data importing, processing and exporting, some problems are most simply solved by writing a bit of Python. Python can communicate with databases very naturally. In a sense, you use all the SQL skills that you have been learning but construct the SQL statements as strings in Python and send them to your PostgreSQL server.

It is important to note that Python is just another client like psql or pgadmin. It makes a network connection to the database server using login credentials and sends SQL commands and receives results from the server.

In order to connect to

```python
import psycopg2 
# commands
```

If the **import** fails you need to install the library using a command like:

```bash
pip install psycopg2 # or pip3
```

Pip is only one of many ways to manage Python "dependencies" / "add ons" depending on your operating system / virtual environment / python installation pattern.

* * *

The sequence in Python to connect and log in to a PostgreSQL database is:

```python
import psycopg2

conn = psycopg2.connect(
 	host='35.123.23.37',
 	database='pg4e',
 	user='pg4e_user_42',
 	password='pg4e_pass_42',
 	connect_timeout=3
 	)
```

The connection makes ure that your account and password are correct and there is truly a PostgreSQL server running on the specified host. If you notice in the code for [simple.py](https://www.pg4e.com/code/simple.py), we store the actual secrets in a file called **hidden.py** and import them. You make your **hidden.py** file by copying [hidden-dist.py](https://www.pg4e.com/code/hidden-dist.py) and putting in your host, user, password, and database values.

Normally, we don't actually send SQL commands using the **connection**. For that we generally get a **cursor**. The cursor allows us to send an SQL command and then retrieve the results, perhaps in a loop. We can ue the cursor over and over in our program. You can think of the cursor as the equivalent of the "psql" command prompt but inside your Python program. You can have more than one cursor open at a time.

```python
cur = conn.cursor()
cur.execute('DROP TABLE IF EXISTS pythonfun CASCADE;')
#...
cur.execute('SELECT id, line FROM pythonfun WHERE id=5;')
row = cur.fetchone()
print('Found', row)
```

In the above example **cur** is just a commonly used variable name for a database cursor.

In the above example, when you send a **SELECT** using **cur.execute()** it does not retrieve the data. It primes the cursor to retrieve the data using methods like **fetchone()**.

Note: fetch one will return a single row, or None.
Note: `conn.commit() # Flush it all to the DB server`

* * *

The key benefit of a **GIN** / Natural Language index is to speed up the look up and retrieval of the rows selected in the **WHERE** clause. When we have a set of rows, what we do with those rows is **relatively inexpensive**.

Ranking of the "how well" a row (==ts_vector==) matches the query (==ts_query==) is something we compute directly from the data in the **ts_query** and the **ts_vector** in each row. We can use different fields in the ranking computation than the fields we use in the **WHERE** clause. The **WHERE** clause dominates the cost of a query as it decides how to gather the matching rows.

```sql
SELECT 
id, subject, sender, ts_rank(to_tsvector('english', body),
to_tsquery('english', 'personal & learning')) as ts_rank
FROM messages
WHERE to_tsquery('english', 'personal & learning') @@ to_tsvector('english', body)
ORDER BY ts_rank DESC;
```

> > id | subject | sender | ts_rank
> > ----+----------------------------+----------------------------+----------
> > 4 | re: lms/vle rants/comments | Michael.Feldstein@suny.edu | 0.282352
> > 5 | re: lms/vle rants/comments | john@caret.cam.ac.uk | 0.09149
> > 7 | re: lms/vle rants/comments | john@caret.cam.ac.uk | 0.09149

```sql
SELECT 
id, subject, sender, ts_rank_cd(to_tsvector('english', body),
to_tsquery('english', 'personal & learning')) as ts_rank
FROM messages
WHERE to_tsquery('english', 'personal & learning') @@ to_tsvector('english', body)
ORDER BY ts_rank DESC;
```

> > id | subject | sender | ts_rank
> > ----+----------------------------+----------------------------+-----------
> > 4 | re: lms/vle rants/comments | Michael.Feldstein@suny.edu | 0.130951
> > 5 | re: lms/vle rants/comments | john@caret.cam.ac.uk | 0.0218605
> > 7 | re: lms/vle rants/comments | john@caret.cam.ac.uk | 0.0218605

There are two at least two ranking functions **ts_rank** and **ts\_rank\_cd**. There is also the ability to weight different elements of a **ts_query** that influence how the relative ranking is computed.

ts query has a special syntaxs for querying. [postgres doc](https://www.postgresql.org/docs/current/textsearch-controls.html)

there are multiple ranking aloghorithm and they are base on relevency score.