---
title: Basics
updated: 2022-02-01 12:38:00Z
created: 2021-11-21 16:26:45Z
latitude: 35.69800000
longitude: 51.41150000
altitude: 0.0000
completed?: no
---

Elastic is **document** based

Can create , delete , update, analyse, ==**search**==

elastic is Based on luciene which is a search engine by apache which was created In 90s.

Elastic search use **index** and **invert index** ( maps a key to document), Inverted index allow full text search.

Document is json based.

![photo_2021-11-21_20-03-10.jpg](../../../_resources/photo_2021-11-21_20-03-10.jpg)In elastic search 7.x the concept of ==**type**== were **removed.**

The insert functionality have special term in elasticsearch called **indexing.**

<ins>In older versions of elasticsearch type was to categories the documents.</ins>

![photo_2021-11-21_20-03-06.jpg](../../../_resources/photo_2021-11-21_20-03-06.jpg)

How to Index into elasticsearch:

```
PUT /{index}/{type}/{id}
{
    "field1": "value1",
    "field2": "value2",
    ...
}
```

**from 6 to later we can only have one type.**

```
PUT /vehicles/_create/123
{
  "make": "Honda",
  "color": "black"
}
```

response be like:

```
{
  "_index" : "vehicles",
  "_type" : "_doc",
  "_id" : "124",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}
```

the important fields are :

- name of **index**
- **id** of created document
- **version** which will be increamented each time we modify the object
- **result** will be deleted or created or any action we do
- **shard** will be number of document action made on

if we get the document we get a field called **found** which can be true or false , if document exists there will be field which called **_source** which will be our document.

we can specify the field that we want in **GET** request.

if we want to only check the particular Document exists we can use **HEAD** request.

==**Documents are imutable so updating it means reindexing.**==

if we want to update the Document we can use Post.

cant add field to document with Post.

```
POST /vehicles/_update/124/
{
  "doc":{ 
    "color" : "yellow"
  }
}
```

for deleting document and index:

```
DELETE /vehicles/_doc/124/
DELETE /vehicles/
```

<ins>when we delete a document it's not completly deleted, elastic mark them as deleted and will from time to time will wipe them all.</ins>

we can also get the index which will give us the general schema of index.

in index schema we have mapping section which is fields of index .

settings have data about the index.

in mapping :

- we got name of fields
- we got type of field like ==**long**== for numbers , ==**text**== for text(max is 256), have list but list data must be all the same type

<ins>you can see list of all indexes via kibana.</ins>

how to search:

```
GET /bussinece/_search
```

search will show how long it took, if it was timed out how many it found, and hits which is the found documents .

term query and match all:

```
GET /bussinece/_search
{
  "query": {
    "term": {
      "owner": {
        "value": "amir"
      }
    }
  }
}
GET /bussinece/_search
{
  "query": {
    "match_all": {}
    
  }
}
```

match all will bring back all the documents, term will do lookup.

with the ==**rench**== logo(which is after play button inside kibana devtools ) we can have the our request in curl or can be redirected to documentation.