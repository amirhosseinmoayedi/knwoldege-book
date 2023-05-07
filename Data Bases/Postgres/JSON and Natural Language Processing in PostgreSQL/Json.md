---
title: Json
updated: 2022-06-30 14:42:24Z
created: 2022-06-30 07:57:59Z
latitude: 51.50735090
longitude: -0.12775830
altitude: 0.0000
---

Serialization keyword comes from the old days which data was sent throw wire with serialized bits of data.

Each programming language has ways of representing the two core types of collections.

| Language | Linear Structure | Key / Value Structure |
| --- | --- | --- |
| Python | list() \[1,2,3\] | dict() {'a':1, 'b': 2} |
| JavaScript | Array \[1,2,3\] | Object {'a':1, 'b': 2} |
| PHP | Array array(1,2,3) | Array array('a' => 1, 'b' => 1) |
| Java | ArrayList | HashMap |

In order to move structured data between applications, we need a "language independent" syntax to move the data. If for example, we want to send a dictionary from Python to PHP we would take the following steps:

1.  Within Python, we would convert the dictionary to this "independent format" ([serialization](https://en.wikipedia.org/wiki/Serialization)) and write it to a file.
2.  Within PHP we would read the file and convert it to an associative array de-serialization).

****Another term for serialization and deserialization is [marshalling](https://en.wikipedia.org/wiki/Marshalling_%28computer_science%29) and [unmarshalling](https://en.wikipedia.org/wiki/Unmarshalling).****

A long time ago.... We used XML as this "format to move data structures between various languages":

```xml
<array>
    <entry>
       <key>a</key>
       <value>1</value>
    <entry>
    <entry>
       <key>b</key>
       <value>2</value>
    <entry>
</array>
```

XML (like HTML) is a good syntax to represent documents, but it is not a natural syntax to represent lists or dictionaries. We have been using XML as a way to represent structured data for interchange since the 1990's. Before that we had serialization formats like [ASN.1](https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One) fsince the mid-1980s. And formats like Comma-Separated Values (CSV) work for linear structures but not so much for key value structures.

Around 2000, we started seeing the need to move structured data between code written in JavaScript in browsers (front-end) and code running on the servers (back-end). Initially the format of choice was XML resulting in a programming pattern called [AJAX](https://en.wikipedia.org/wiki/Ajax_%28programming%29) \- Asynchronous JavaScript And XML. Many programming already had libraries to read and write XML syntax so it was an obvious place to start. And in the browser, XML looked a lot like HTML so it seemed to make sense there as well.

The problem was that the structures we used in programs (list and key/value) were pretty inelegant when expressed in XML, makeing the XML hard to read and a good bit of effort to convert.

## JSON - JavaScript Object Notation

Given the shortcomings of XML to represent linear and key/value structures, as more and more applications, started to transfer data between JavaScript on the browser and the databases on the back-end, Douglas Crockford noticed that the syntax for JavaScript constants might be a good serialization format. In particular, JavaScript already understood the format natively:

&lt;script type="text/javascript"&gt;
who = {
"name": "Chuck",
"age": 29,
"college": true,
"offices" : \[ "3350DMC", "3437NQ" \],
"skills" : { "fortran": 10, "C": 10,
"C++": 5, "python" : 7 }
};
console.log(who);
&lt;/script&gt;

It turned out to be easier to add libraries to the back-end languages like Python, PHP, and Java to convert their data structures to JSON than to use XML to serialize data because the back-end languages were already good at XML. The reason was really because XML did a bad job of representing linear or key/value structures that are widely used across all languages.

To help advance adoption in an industry that was (at the time) obsessed with XML, Douglas Crockford wrote a simple specification for "JSON", and put it up at [www.json.org](https://www.json.org/) and programmers started to use it in their software development.

In order to make parsing and generating JSON simpler, JSON required all of the keys of key value pairs be surrounded by double quotes.

For those familiar with Python, JSON looks almost exactly like nested Python list and dictionary constants. And while Python was not so popular in 2001, now almost 20 years later, with Python and JavaScript emerging as the most widely used languages, it makes reading JSON pretty natural for those skilled in either language.

JSON has quickly become the dominant way to store and transfer data structures between programs. JSON is sent across networks, stored on files, and stored in databases. As JavaScript became an emerging server language with the development of the [NodeJS](https://nodejs.org/en/) web server and JSON specific databases like [MongoDB](https://www.mongodb.com/) were developed, JSON is now used for all but a few data serialization use cases. For those document-oriented use cases like [Microsoft Office XML formats](https://en.wikipedia.org/wiki/Microsoft_Office_XML_formats), XML is still the superior solution.

Database systems like Oracle, SQLServer, PostgreSQL, and MySQL have been adding native JSON columns to suport document-style storage in traditional relational databases.

* * *

## JSON in Python

In this section we will do a quick introduction of the [JSON library in Python](https://docs.python.org/3/library/json.html).

Using JSON in Python is very simple because JSON maps perfectly onto lists and dictionaries.

The [json.dumps()](https://docs.python.org/3/library/json.html#json.dumps) library takes a python object and serializes it into JSON.

https://www.pg4e.com/code/json1.py

```python
import json

data = {}
data['name'] = 'Chuck'
data['phone'] = {}
data['phone']['type'] = 'intl';
data['phone']['number'] = '+1 734 303 4456';
data['email'] = {}
data['email']['hide'] = 'yes'

# Serialize
print(json.dumps(data, indent=4))
```

Produces the following output:

```python
{
    "name": "Chuck",
    "phone": {
        "type": "intl",
        "number": "+1 734 303 4456"
    },
    "email": {
        "hide": "yes"
    }
}
```

The [json.loads()](https://docs.python.org/3/library/json.html#json.loads) takes a string containing valid JSON and deserializes it into a python dictionary or list as appropriate.

https://www.pg4e.com/code/json2.py

```python
import json 
data = '''
{"name" :"Chuck",
 "phone" : {
  	"type" : "intl", "number" : "+1 734 303 4456" 
  	},
 "email" : {
  	"hide" : "yes" 
  	}
  }
'''
```

info = json.loads(data) print('Name:', info\["name"\]) print('Hide:', info\["email"\]\["hide"\])

This code executes as follows:

> Name: Chuck
> Hide: yes

This also works with Python lists as well:

- Serializing a list - https://www.pg4e.com/code/json3.py
- Deserializing a list - https://www.pg4e.com/code/json4.py

Before we move on, here is a simple example of de-serializing XML in Python similar to **json2.py** above:

https://www.pg4e.com/code/xml1.py

```python
import xml.etree.ElementTree as ET 
data = ''' 
    <person>
         <name>Chuck</name>
         <phone type="intl"> +1 734 303 4456 </phone>
         <email hide="yes" />
     </person>
'''
tree = ET.fromstring(data)
print('Name:', tree.find('name').text)
print('Attr:', tree.find('email').get('hide'))
```

Because XML is a tree based approach (neither a list nor a dictionary) we have to use find **find()** function to query the tree, figure out its structure and *hand transform* the data tree into our lists and/or dictionaries. This is the impedance mismatch between the "shape" of XML and the "shape" of data structures inside programs that is mentioned by Douglas Crockford in his interview above.

Again, it is importaint to point out that XML is a better than JSON when representing things like hierarchical documents. Also XML is a bit more verbose and as such a bit more self-documenting as long as the XML tags have reasonable names.

* * *

[06-JSON.sql](../../../../_resources/06-JSON.sql)

Now we *finally* get to talk about JSONB support in PostgreSQL. Like many of things with PostgreSQL, the lead up / background is more complex to understand than the support within PostgreSQL.

In this section, we will be going back to our music data except we will now be using [JSON data](https://www.pg4e.com/code/library.jstxt). If you are interested, you can see the [Python code](https://www.pg4e.com/code/librarytojson.py) which is used to convert the original [XML data](https://www.pg4e.com/code/Library.xml) is converted to JSON format.

> {"name": "Another One Bites The Dust", "artist": "Queen", "album": "Greatest Hits", "count": 55, "rating": 100, "length": 217103}
> {"name": "Beauty School Dropout", "artist": "Various", "album": "Grease", "count": 48, "rating": 100, "length": 239960}
> {"name": "Circles", "artist": "Bryan Lee", "album": "Blues Is", "count": 54, "rating": 60, "length": 355369}

Separately, we will go over a detailed walkthrough of [SQL statements](https://www.pg4e.com/lectures/06-JSON.sql) using JSONB, so we will just show some of the highlights here.

We will create a table and import the above JSON file into the table as follows:

```psql
CREATE TABLE IF NOT EXISTS jtrack (id SERIAL, body JSONB);
\copy jtrack (body) FROM 'library.jstxt' WITH CSV QUOTE E'\x01' DELIMITER E'\x02';
```

The **\\copy** command above is somewhat inelegant but it got our data in with a single command. In the [next section](https://www.pg4e.com/lectures/06-JSON.php#swapi) of these notes, we will insert our JSON data using Python which gives us a lot more flexibility.

When using JSONB it is important to know the types of data and use the cast (::) operator where appropriate. You can extract field data from the JSON using queries that use the "retrieve field and convert from jsonb to text" operator (->>).

```sql
SELECT (body->>'count')::int FROM jtrack WHERE body->>'name' = 'Summer Nights';
```

You can query JSONB fields by comparing them to other JSONB documents or document fragments using the contains (@>) operator.

```sql
SELECT (body->>'count')::int FROM jtrack WHERE body @> '{"name": "Summer Nights"}';
```

You can check to see if a JSONB document contains a key:

```sql
SELECT COUNT(*) FROM jtrack WHERE body ? 'favorite';
```

You can use JSONB expressions most anywhere you can use a column in your SQL, making sure to cast the results where appropriate.

```sql
SELECT body->>'name' AS name FROM jtrack ORDER BY (body->>'count')::int DESC;
```

> `->` returns json (or jsonb) and `->>` returns `text`

### Indexes

Part of the benefit of using JSONB is the way you can easily add indexes to the whole column or portions of the column using BTREE, HASH, Gin and other types of PostgresSQL indexes:

```sql
CREATE INDEX jtrack_btree ON jtrack USING BTREE ((body->>'name'));
CREATE INDEX jtrack_gin ON jtrack USING gin (body);
CREATE INDEX jtrack_gin_path_ops ON jtrack USING gin (body jsonb_path_ops);
```

We will look at the kinds of **WHERE** clauses that make use of the various indexes.

> if we use ->> we use text index B-tree, ? and @> uses gin index.

**jsonb\_path\_ops** index on key, value pairs and super fast, it's bigger and slower to create.