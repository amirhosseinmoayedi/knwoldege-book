---
title: Query
updated: 2021-11-25 11:31:45Z
created: 2021-11-23 16:19:39Z
latitude: 51.29930000
longitude: 9.49100000
altitude: 0.0000
---

**we can have object inside each of a document** so a field can have properties which is connected to that field.

elasticsearch uses DSL.

DSL: Domain Specific Language

DSL  is in json.

elastic will go into the indexes and find the occurence of term and the more is found on a document the more relavance it is for elsaticsearch.(**relevancy score**)

search components are:

- query
- filter

can be combined.

**match_all** will bring all document back.

**match** will query on a field.

**exists** check if the field exists or not.

for multi match or any multi query field :

```
GET courses/_search
{
  "query": {
    "bool": { 
    "must":[
    {"match": {"name": "computer"}},
    {"match": {"room": "c12"}}
    ]
    }
  }
}
```

Use **must_not** and inside it the condition to negetive the query (exclude some result).

We use **should** to increase the **score of relevancy** (should mean better to have).

**Minimum\_should\_match** equals to a number to specify at least the number conditions in must be true in should part.

We can use **multi match** query for searching over multiple fields.

**Match_phrase** for matching a phrase. So it's for long text.

For searching partial tokens in phrase we can use **match\_phrase\_prefix.**

We can use **range** to search between number or dates of fields like students age between 20 to 40.

Usage of filters is like query we wrap it in bool.

And we use match and etc in it exatly like query but Filter does not do relevancy scoring.

If you don't need relevancy score always use filter instead of query.

You can control the relevancy score by boosting a filed by carrot(^).

```
"match": {"name^2":"ali"}
```

example of query:

```
GET cource/_search/
{
    "query":{
        "bool":{
        "must":[
            "match":{"name":"ali"} 
        ],
        "must_not": ...
        "should":
        "minimum_should_match":1
        }
    }
}
```

`usage of filter `

```
GET cources/_search
{
    "query":{
        "bool":{
            "filter": {
            }
        }
    }
}
```

by default the elastic will bring top ten back, we can change it 

by setting **size**  parameter.

we can set the **from** parameter for offsetting.

we can use **sort** to sort base on field.

```
GET /vehicles/_search
{
  "size": 5,
  "from": 5, 
  "query": {
    "match_all": {}
  },
  "sort": [
  	{"price": {"order": "desc"}}
  ]
}
```

aggregation is used for anaylize the data, it's done through summerization, aggregation can be done to grouping too .

count

```
GET /vehicles/_count
{
  "query": {
    "match": {
      "condition": "new"
    }
  }
}
```

counting and avg example:

```
GET /vehicles/_search
{
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      },
      "aggs": {
        "avg_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  }
  
}
```

we use make.keyword because the keyword means text.

we can combine the query, filter and aggregation.