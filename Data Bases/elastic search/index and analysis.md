---
title: 'index and analysis '
updated: 2021-11-25 11:08:04Z
created: 2021-11-23 15:56:52Z
latitude: 51.29930000
longitude: 9.49100000
altitude: 0.0000
---

Node :. A computer running a service.

An i<ins>ndex may be splited to multiple part</ins> which called ==**shards**== which is to be used into multiple nodes each shardes.

Each shardes may be **primary** or **replica.**

<ins>Nodes uses replicas and primary shards to be also backup of files too.</ins>(it will keep a primary shard of an index in a node of it in another node so if a node went down we does not lose data)

Each node knows what other nodes have so if it didn't have that data it will **rerouted** to the other nodes that have it.

Each change on index or shards **will be made to primary then to replicas** ==even if the replica exists on the node that will reroute the request.==

For get request it will balance the request , and will show the results even from replicas.

Shards consist of **segments** and segments is a **inverted index**.

**Analysis is action of prepareing data for search, Analysis is process of converting text to tokens and terms, With tokens inverted indexes will be created and they will be move to memory buffer and when buffer is full they will be committed to segments.**

**Analysis Will tokenize the sentence and split them base on white space and will remove stop words like: the , in , and etc, Will lowercase them ,Will get the root of word like swim for swimming( called steming), and will also index synonyms so if you have words like thin and skinny, one of them will be indexed.**

==These are called filters==

So analyzer have two step:

1.  tokenize
2.  filter

Analizers will be applied to fields.

We can create custom analyzer.

Analizers will be applied to query too.

**The right way to store data into an index is to first specify the mapping and settings.**

For creating index we make a put request with just name of index .

```
PUT /chicks/
```

if we don't specify the number of shard it will be  **5 by default** .

**Standard** analyzer will just split text base on white space and lowercase it.

Numeric values do not need analyzer.

If we add field that is not on the mapping elastic search **will add it**, we can make it more restrict.

For production we want elastic not to update the mapping.

```
PUT /chicks
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  },
  "mappings": {
    "properties": {
      "name":{
        "type": "text",
        "analyzer": "standard"
      },
      "age": {
        "type": "integer"
      }
    }
  }
}
```

restriction rules can be applied after mapping config or when creating index.

![photo_2021-11-23_19-39-50.jpg](../../../_resources/photo_2021-11-23_19-39-50.jpg)

```
PUT my-index-000001
{
  "mappings": {
    "dynamic": false, 
    "properties": {
      "user": { 
        "properties": {
          "name": {
            "type": "text"
          },
          "social_networks": {
            "dynamic": true, 
            "properties": {}
          }
        }
      }
    }
  }
}
```

We can test analyzers with requesting to _analyze.

```
POST _analyze
{
  "analyzer": "whitespace",
  "text":     "The quick brown fox."
}
```

bulk indexing:

```
POST /vehicles/_bulk
{ "index": {}}
{ "price" : 10000, "color" : "white", "make" : "honda", "sold" : "2016-10-28", "condition": "okay"}
{ "index": {}}
{ "price" : 20000, "color" : "white", "make" : "honda", "sold" : "2016-11-05", "condition": "new" }
```