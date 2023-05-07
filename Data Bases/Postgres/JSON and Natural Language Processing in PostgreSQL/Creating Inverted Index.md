---
title: Creating Inverted Index
updated: 2022-05-19 11:44:29Z
created: 2022-05-12 10:07:36Z
latitude: 51.53330994
longitude: 4.45920992
altitude: 0.0000
---

## By Hand

We can split long text columns into space-delimited words using PostgreSQL's split-like function called **string\_to\_array()**. And then we can use the PostgresSQL **unnest()** function to turn the resulting array into separate rows.

<img src="../../../../_resources/3183c5e6873c8bf0d3cf5b08a1630617.png" alt="3183c5e6873c8bf0d3cf5b08a1630617.png" width="455" height="359" class="jop-noMdConv">

```sql
string_to_array('Hello world', ' ');
```

> string\_to\_array
> \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
> {Hello,world}

```sql
unnest(string_to_array('Hello world', ' '));
```

> unnest
> \-\-\-\-\-\-\-\-
> Hello
> world

After that, it is just a few **SELECT DISTINCT** statements and we can create and use an inverted index.

```sql
CREATE TABLE docs (id SERIAL, doc TEXT, PRIMARY KEY(id));
INSERT INTO docs (doc) VALUES
('This is SQL and Python and other fun teaching stuff'),
('More people should learn SQL from UMSI'),
('UMSI also teaches Python and also SQL');

CREATE TABLE docs_gin (
  keyword TEXT,
  doc_id INTEGER REFERENCES docs(id) ON DELETE CASCADE
);
```

The proccess is too much ;)))). for the proccess file is attached.

[05-FullText.sql](../../../../_resources/05-FullText.sql)

* * *

- Generalized Inverse Index (GIN)
- Generalized Search Tree (GiST)

*GIN indexes are the preferred text search index type.* Advantages: exact matches, efficient on lookup/search. Disadvantages: can be costly when inserting or updating data because every new word is inserted somewhere in the index and can get large. Like the B-Tree, the GIN is the usual "go-to" inverted index and GiST is used in more special cases. The previous example was a rough approximation of a GIN index.

Hashing is used to reduce the size of and cost to update the GiST. *A GiST index is lossy, meaning that the index might produce false matches, and it is necessary to check the actual table row to eliminate such false matches. (PostgreSQL does this automatically when needed.)* The indexes have equivalent functionality and will return the same rows - but there may be performance / storage tradeoffs between GIN and GiST.

Both GIN and GiST want to know something about the type of array data it will be indexing and the kinds of operations that we will be using in **WHERE** clauses.

When selecting an inverted index keep in mind if the querying is more than inserting or not , in this situation if it is we should pick gist.

there is a technique that we first move our data and then build index, duo to the cost of creating index.

* * *

To take advantage of the "naruralness" of natural language, we need to ignore words that convey no meaning and consistently reduce variations of words with equivalent meanings down to a single "stem word".

<img src="../../../../_resources/07f820519cd28f7964b66fcb04f2c0f8.png" alt="07f820519cd28f7964b66fcb04f2c0f8.png" width="489" height="386" class="jop-noMdConv">

* * *

```sql
SELECT cfgname FROM pg_ts_config;
```

**COALESCE** is a function in database that returns the first not null.

```sql
SELECT COALESCE(NULL, NULL, 'sql');
```

* * *

PostgreSQL provides some functions that turn a text document/string into an "array" with stemming, stop words, and other language-oriented features.

**ts_vector()** returns a list of words that represent the document. **ts_query()** returns a list of words with operators to representaions various logical combinations of words much like [Google's Advanced Search](https://www.google.com/advanced_search).

in ts_vector the distance between the words matter if we have multiple words, vector is multi dimensional arrays.

ts_query will do stemming and delete stop words and so on.

```
SELECT to_tsvector('english', 'UMSI also teaches Python and also SQL');
```

> ```
>  to_tsvector 
> ```
> 
> \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
> 'also':2,6 'python':4 'sql':7 'teach':3 'umsi':1

```
SELECT to_tsquery('english', 'teaching');
```

> to_tsquery
> \-\-\-\-\-\-\-\-\-\-\-\-
> 'teach'

In a **WHERE** clause we use the **@@** operator to ask is a **ts_query** matches a **ts_vector**.

```
SELECT to_tsquery('english', 'teaching') @@
  to_tsvector('english', 'UMSI also teaches Python and also SQL');
```

* * *

As you might expect, letting PostgreSQL do all the work is the easy part. Stop words and stems are all handled in the "ts_" functions. And the GIN knows what operations you will be using automatically when you pass in a **ts_vector**.

```sql
CREATE TABLE docs (id SERIAL, doc TEXT, PRIMARY KEY(id));

CREATE INDEX gin1 ON docs USING gin(to_tsvector('english', doc));
INSERT INTO docs (doc) VALUES 
('This is SQL and Python and other fun teaching stuff'),
('More people should learn SQL from UMSI'),
('UMSI also teaches Python and also SQL');

SELECT id, doc FROM docs WHERE to_tsquery('english', 'learn') @@ to_tsvector('english', doc);
```

> id | doc
> ----+----------------------------------------
> 2 | More people should learn SQL from UMSI

```sql
EXPLAIN SELECT id, doc FROM docs WHERE
    to_tsquery('english', 'learn') @@ to_tsvector('english', doc);
```

> ```
>  QUERY PLAN 
> ```
> 
> \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
> Bitmap Heap Scan on docs (cost=12.05..23.02 rows=6 width=36)
> Recheck Cond: ('''learn'''::tsquery @@ to_tsvector('english'::regconfig, doc))
> -\> Bitmap Index Scan on gin1 (cost=0.00..12.05 rows=6 width=0)
> Index Cond: ('''learn'''::tsquery @@ to_tsvector('english'::regconfig, doc))

* * *

You can ask PostgreSQL the different index / **WHERE** clause operator combinations it supports. There are quite a few and they can change from one PostgreSQL version to another.

```sql
SELECT am.amname AS index_method, opc.opcname AS opclass_name
FROM pg_am am, pg_opclass opc
WHERE opc.opcmethod = am.oid
ORDER BY index_method, opclass_name;
```

> index\_method | opclass\_name
> --------------+------------------------
> brin | abstime\_minmax\_ops
> brin | bit\_minmax\_ops
> brin | box\_inclusion\_ops
> ...
> brin | uuid\_minmax\_ops
> brin | varbit\_minmax\_ops
> btree | abstime_ops
> btree | array_ops
> ...
> btree | varbit_ops
> btree | varchar_ops
> btree | varchar\_pattern\_ops
> gin | array_ops
> gin | jsonb_ops
> gin | jsonb\_path\_ops
> gin | tsvector_ops
> gist | box_ops
> gist | circle_ops
> ...
> gist | tsvector_ops
> hash | abstime_ops
> hash | aclitem_ops
> hash | array_ops
> ...
> hash | varchar\_pattern\_ops
> hash | xid_ops
> spgist | box_ops
> spgist | inet_ops
> spgist | kd\_point\_ops
> spgist | poly_ops
> spgist | quad\_point\_ops
> spgist | range_ops
> (134 rows)