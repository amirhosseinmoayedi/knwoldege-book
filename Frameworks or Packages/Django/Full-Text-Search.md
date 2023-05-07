---
title: Full-Text-Search
updated: 2021-12-06 10:27:26Z
created: 2021-12-06 07:11:51Z
latitude: 35.69800000
longitude: 51.41150000
altitude: 0.0000
---

**Full text search supports index,sorting and relavancy, fuzzy search for misspelling, accent/ language support , weighting, categorizing and highlighting.**

if your project is not big you better use full text search of postgres, if it is big use elastic search.

you can see the SQL query with .==query==() method.

```python
Book.objects.filter(title='amir').query()

SELECT "book_book"."id", "book_book"."title", "book_book"."authors" FROM "book_book" WHERE UPPER("book_book"."title"::text) LIKE UPPER(%harry%)
```

**contain and i contain uses LIKE in sql query.**

analyze the query:

```python
Book.objects.filter(title='amir').explain(analyze=True)

Seq Scan on book_book  (cost=0.00..306.90 rows=18 width=69) (actual time=0.020..18.886 rows=43 loops=1)
  Filter: (upper((title)::text) ~~ '%HARRY%'::text)
  Rows Removed by Filter: 11084
Planning Time: 2.074 ms
Execution Time: 18.947 ms
```

for using postgres advance features we should this to our installed app:

```
django.contrib.postgres
```

__search :

```python
Book.objects.filter(title__search='Harry')
```

1.  creates a **to_tsvector** in the database from the title.
2.  creates a **plainto_tsquery** from search term .

note: tsvector is list of lexemes.

tokenizing will do exactly like analyzer in elastic search.

![Screenshot from 2021-12-06 12-26-43.png](../../../_resources/Screenshot from 2021-12-06 12-26-43.png)

**in django \_\_serach will use plainto\_tsquery.**

**plainto_tsquery:**

- text is parsed and normmilized
- escape all chars and put& between words
- cant recognize bool operator, weight label, prefix match.

**to_tsqury**:

- offer more features than plainto_tsquery
- input, weight can be attached to each lexeme
- single-quoted phrases

**Search vector**: allow us to search multiple fields .

```
Book.objects.annotate(search=SearchVector('title', 'authors'), ).filter(search=q)
```

**Search Rank**: ranking base of relevancy

```python
vector = SearchVector('title')
query = SearchQuery(q)
results = Book.objects.annotate(rank=SearchRank(vector, query)).order_by('-rank')
```

**weights**: to decleare that some parts are more important than the other.

![99c6aaf3db0cae8a2186f329ecac19af.png](../../../_resources/99c6aaf3db0cae8a2186f329ecac19af.png)

```python
vector = SearchVector('title', weight='B') + SearchVector('authors', weight='A')
```

**cover density means that the query terms must be similar to shown result at some point.**

**Trigram Search**: groups of three consecutive characters taken from a string, measure of similarity of two string by counting the number of trigrams they share.

for using this we need to install **pg_trgm** to the database.

```python
results = Book.objects.annotate(similarity=TrigramSimilarity('title', q), ).filter(
                similarity__gte=0.1).order_by('-similarity')
```

trigram distance is used to create did you mean option.(oposite of similarity)

```python
results = Book.objects.annotate(distance=TrigramDistance('title', q), ).filter(
                distance__lte=0.5).order_by('distance')
```

search headline : for highliting 

```
vector = SearchVector('title')
query = SearchQuery(q)
results = Book.objects.annotate(search=vector, headline=SearchHeadline('authors', query)).filter(search=query)
```

**we can install extension in postgres via django.**