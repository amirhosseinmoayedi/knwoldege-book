#index #database #performance #data_structure #postgres
## What is <font color="#ffff00">Index</font>:
Indexing makes columns <font color="#ffff00">faster to query by creating pointers to where data is stored within a database</font>.

An<u> index is a structure that holds the field the index is sorting and a pointer from each record to their corresponding record in the original table</u> where the data is actually stored.

>*Do not use indexing until it's needed and don't index multi-column do it one by one to see the result.*

**Each time the table changes** the **index must change** and <u>while it's being updated index can't be used</u> so be careful with the overhead of the index.
> <font color="#fbd5b5">postgres</font> have an feature that can use index while updating it

A row of an index:
![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20190812183525/Structure-of-an-Index-in-Database.jpg)

### List of some index types
-   **B-Tree**
-   **GIN**
-   **GiST**
-   **Hash**


## <font color="#fac08f">B-Tree</font>
It's a <font color="#fac08f">tree that each node can have more than two leaf</font>, I<u>t's not binary tree</u>, the depth is less than binary tree and the distance of each node to root is the same.

everything in postgres is stored in a something called <font color="#ffc000">page</font> **(8kb)**, in each page we have three thing for <font color="#fac08f">B-Tree</font>:
1. <font color="#ffff00">pointer</font>: pointer to the value
2. <font color="#ffff00">high key</font> (max value): used <u>to determine if this index page should be used or move to next node</u>
3. <font color="#ffff00">items</font> : that is hashmap like, **key is the indexed value** and **value of the key is the pointer in the real table**. ( if it's a node it's point to it's leafes)

### <font color="#e36c09">Full index</font>:
	for all the values in the columns 
```sql
CREATE INDEX [index_name] ON [table_name](column_name_list)

examples:
// single column
CREATE INDEX user_phone_idx ON users(phone);
// for multiple columns
CREATE INDEX user_full_name_idx ON users(first_name, last_name);
```

> in indexing of **multiple column** we should <u>order the index base on mostly to leastly used columns</u>.

### <font color="#e36c09">Partial Index</font>: 
	for some of thee values in columns, it's used to keep the index smaller and by doing so it's faster 
```sql
CREATE INDEX [index_name] ON [table_name](columns_name_list) where [condition];
example:
CREATE INDEX active_users_partial_idx ON users(is_active) where is_active = 1; 
```

### <font color="#e36c09">Unique Index</font>:
	index to not unique value, it can also have partial

```sql
CREATE UNIQUE INDEX [index_name] ON [table_name](column_name_list)
example: 
CREATE UNIQUE INDEX user_email_unique_idx ON users(email);
```

## <font color="#31859b">GIN </font>(General Inverted Index)
It's for [[Programming Generall/JSON]]  and <font color="#92d050">tsvectors</font> ( used for <font color="#92d050">fulltext search</font> ):
```sql
CREATE INDEX [index_name] ON [table_name] USING GIN (column)
CREATE INDEX article_fulltext_gin_idx ON article USING GIN(content);
```

It's will parse the text and make a array of it, each item in array is an <font color="#92d050">entry</font> which is unique and if it's repeated somewhere else it will use the previous entry, each entry point to leaf where the actual text is and that leaf leads to the value.

![[Pasted image 20230228214309.png]]

## <font color="#d99694">GiST</font>
a type of index that <font color="#b7dde8">can support any kind of data and lookup</font>.
It is an **extensible data structure** that allows users to develop indices over their specific data types <u>A GiST index is also lossy</u>, meaning that it **may produce false matches** that need to be checked with the actual table row.

> Postgres have the optimized function and index structure for any data type that is support so no need to implment it. 
```sql
CREATE INDEX [index_name] ON [table_name] USING GIST (column);
CREATE INDEX article_aaa_gist_idx ON article USING GIST(aaa);
```

## <font color="#548dd4">Hash</font>
Turn everything to a <font color="#548dd4">32-bit int</font> , used to search for exact matches, used for string mostly.
![[Pasted image 20230228215952.png]]
```sql
CREATE INDEX [index_name] ON [table_name] USING HASH (column);
CREATE INDEX article_aaa_gist_idx ON article USING HASH(aaa);
```