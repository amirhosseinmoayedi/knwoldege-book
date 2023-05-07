---
title: Natural Language
updated: 2022-05-12 10:07:29Z
created: 2022-05-12 09:24:31Z
latitude: 51.53330994
longitude: 4.45920992
altitude: 0.0000
---

If we use some rows or data in Database very often, ==Database will keep them in memory as cache for faster operation==.

Rows can vary quite a bit in terms of length.

```sql
CREATE TABLE messages (
id SERIAL, -- 4 bytes
email TEXT, -- 10-20 bytes 
sent_at TIMESTAMPTZ, -- 8 bytes
subject TEXT, -- 10-100 bytes
headers TEXT, -- 500-1000 bytes
 body TEXT -- 50-2000 bytes -- 600-2500 bytes
  );
```

Since modifying data is so important to databases, we don't pack store one row after another in a file. We arrange the file into blocks (default 8K) and pack the rows into blocks leaving some free space to make inserts updates, or deletes possible without needing to rewrite a large file to move things up or down.

database rows are stored in block (8k blocks) **from back to the front**.

<img src="../../../../_resources/41037436d49611e3e37e0b51d28b3bcf.png" alt="41037436d49611e3e37e0b51d28b3bcf.png" width="690" height="365">

PostgreSQL Organizes Rows into Blocks

- We read an entire block into memory (i.e. not just one row)
- Easy to compute the start of a block within a file for random access
- There are the unit of caching in memory
- They are (often) the unit of locking when we think we are locking a row

What is the Best Block Size?

- Blocks that are small waste free space / fragmentation
- Large blocks take more memory in cache be cached for a given memory size
- Large blocks longer to read and write to/from SSD

if add data to a row we it will *shift the data* in the block to make space, then it **will rewrite the hole block**.(this happens in memory)

better to keep block small(4k ) in SSD , but not too small because it will cause **fragmentation**.

we can **change the block size**( but for most cases default is the best).

If we have a table that contains 1GB (125,000 blocks) of data, a sequential scan from a fast SSD takes about 2 seconds while with careful optimization, reading a random block can be fast as 1/50000th of a second. Some SSD drives can read as many as 32 different random blocks in a single read request. If the block is already cached in memory it is even faster. ==Sequential scans are very bad.==

* * *

**Indexes**: in short are hint(pointer) to map data we look for and **the block it is stored in**. 

Assume each row in the **users** table is about 1K, we could save a lot of time if somehow we had a hint about which row was in which block.

> email              | block
> -------------------+------
> anthony@umich.edu  | 20175
> csev@umich.edu     | 14242
> colleen@umich.edu  | 21456   

```sql
SELECT name FROM users WHERE email='csev@umich.edu';
SELECT name FROM users WHERE email='colleen@umich.edu';
SELECT name FROM users WHERE email='anthony@umich.edu';
```

Our index would be about **30 bytes per row** which is much smaller than the actual row data. *We store index data in 8K blocks as well* \- as indexes grow in size we need to find was to avoid reading the entire index to look up one key. We need an index to the index. For **string logical keys, a B-Tree index is a good, general solution**. B-Trees keep the keys in sorted order by reorganizing the tree as keys are inserted.

There are two Type of indexes:

1.  Forward
2.  Inverted

PostgreSQL Index Types:

- B-Tree - The default for many applications - automatically balanced as it grows
- BRIN - Block Range Index - Smaller / faster if data is mostly sorted ( it's like B-Tree)
- Hash - Quick lookup of long key strings
- GIN - Generalized Inverted Indexes - multiple values in a column
- GiST - Generalized Search Tree (hash based)
- SP-GiST - Space Partitioned Generalized Search Tree

a **B-tree** is a self-balancing [tree data structure](https://en.wikipedia.org/wiki/Tree_data_structure "Tree data structure") that maintains sorted data and allows searches, sequential access, insertions, and deletions in [logarithmic time](https://en.wikipedia.org/wiki/Logarithmic_time "Logarithmic time").( O log-n) 

For a B-Tree index the depth must be minimum to act faster. B-Tree is good for **number**, **date** and some other data type.(BRIN is the same).

B-Tree:

<img src="../../../../_resources/7f871a105c447f0257a583b6c12057c4.png" alt="7f871a105c447f0257a583b6c12057c4.png" width="636" height="177">

the number point to a block in memory.

example : if add 4 to this index, the 4 goes between 2 and 5 but other index must be moved to other blocks , 6 must go before 9.

**cost of creating index is cheap**.

### Hash can collide this mean two key have same hash. 

BRIN Index:

is sequentional and each block if index point to block on disk, some times to block cover a number like 10 and  we should get multiple block but it is still a lot cheaper in term of time and operation.

if you want to create index and it's exists in two block range, and both are full you create a new block and get some range from two block that this index may fit in but can't because it's full.

<img src="../../../../_resources/cf2f39e7127f6ae2011268f287e2bd96.png" alt="cf2f39e7127f6ae2011268f287e2bd96.png" width="1113" height="71">

It is not a perfect metaphor but in general there are two categories of indexes:

- **Forward indexes** \- You give the index a logical key and it tells you where to find the row that contains the key. (B-Tree, BRIN, Hash)
- **Inverse indexes** \- You give the index a string (query) and the index gives you a list of *all* the rows that match the query. (GIN, GiST), **Generally used for text search**.

The metaphor is not perfect - because B-tree indexes are stored in sorted order, if you give a B-Tree the prefix of a logical key, it can give you a set of rows...

The most typical use case for an **inverse index** is to quickly search text documents wit one or a few words.