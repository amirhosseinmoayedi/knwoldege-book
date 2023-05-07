---
title: Sqllite3
updated: 2021-03-15 13:00:16Z
created: 2021-03-05 10:59:47Z
author: Amir Hossein Moayedi
---

**it comes with python.**
it's like all other database (RDBMS) but simpler and liter .
like all it's need connector or handler .
`db = sqllite3.connect("dbname.db")`
we can also save temp db in memory with :
`db = sqllite3.connect(":memory:"`

and like other it need a cursor for executing commands and fetching data.(it's and API to database)

`cursor  = db.cursor()`

`cursor.excute('some sql commands')`
there are 5 data type in SQL Lite :
1-null 2 - integer 3-real(float) 4-text 5- blob(boolean)
in working with database commit the changes after executing the command :
`db.commit()`

after our job is done with database we better to close it , noting will happen but it's  better to be done.

`db.close()`
for receiving data from db  after executing a quarry.

```
cursor.fetchone()  # for first line of quarry
cursor.fetcall()  # for all the quarry
```

these command will return tuple.
for executing many commands

* * *

it would *create table if if and just if it dos't exist.*
`CREATE TABLE IF NOT EXISTS table_name (table structure)`

**there  is not limit** of how much connection we can  make to sqllite because *it's a file.*

each time we  do an **execute** it will do an **transaction**.

**cursor** in Sql Lite is different in cursor of data base it's *got additional functionality* , you can use it  in for after executing a command because it will load data one row each time. generally it need cursor for every thing but it would just use it in getting quarry from db.

so if you execute command from db it would create a temp cursor for just that command.

**
**

**Important: do not use *f string* for sql commands it will be used for * sql injection* attacks.**

**instead use this:**

```
def add_entry(desc: str, date: str):
    with db:
        cursor.execute(f'INSERT INTO entries VALUES (?, ?);', (desc, date))
```

you can **return cursor **because the cursor is for looping in data so you don't have to use fetch to get the data.

and also *db.execute will also return a cursor* .

```
cursor = db.execute(f'INSERT INTO entries VALUES (?, ?);', (desc, date))
return cursor
```

the value would be a** list of tuples or a tuple** .

***never never* use string formatting for sql commands om python.**
**
**

**case sense is turned off by default in sqllite so like will find  unexpected result.**