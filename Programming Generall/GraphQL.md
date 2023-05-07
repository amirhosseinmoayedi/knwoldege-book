#api #general #graphql

## What is GraphQL
is an **open-source data query** and <font color="#92cddc">manipulation language for APIs</font>.

The main goal is Reading data from multiple source and mutate it with a single request.

## QueryLanguage
Query language (QL) refers to **any computer programming language that requests and retrieves data from database and information systems by sending queries**.

## The advantages over Rest:
* <font color="#95b3d7">No extra data and unused </font>, we get lots of extra data in rest
* <font color="#ffff00">No multiple request</font> for buidling a page or a task to be done
![[Pasted image 20230225132302.jpg]]
**GraphQL servers**<u> sit in between the client and the backend services</u>.

GraphQL supports <font color="#c3d69b">queries</font>, <font color="#92cddc">mutations</font> (<u>applying data modifications to resources</u>), and <font color="#fac08f">subscriptions</font> (<u>receiving notifications on schema modifications</u>).


The GraphQl is<u> heavily typed</u>, and it can be <u>nesting</u>.

Example of Quey:
```GraphQL
  query city(name: "Rotterdam") {
        name
        population
        lat
        lng
    }
```

The result is in [[Programming Generall/JSON]].

## GraphQL pros and cons 
|Pros|Cons|
|---|---|
|Flexibility | Complexity |
| Efficient Response | Typed System |
| No roundtrips | No Caching (POST only)
| Single endpoint | No standard errors |
||Expensive queries|

## <font color="#ffff00">subscription</font> 
**notifing the client when data is changed by server**.
## <font color="#ffff00">mutation</font> 
**changing the resource by by the client**.

