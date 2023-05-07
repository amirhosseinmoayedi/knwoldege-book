---
title: 'Basics - M001 '
updated: 2022-02-24 12:56:58Z
created: 2022-02-17 09:12:53Z
latitude: 35.69800000
longitude: 51.41150000
altitude: 0.0000
---

**Mongo is no-sql Document base DB**, ==**Documents**== are stored into ==**collections**== and we can have several collections.

Document is in **Bson** to store data in ==**field-value**== format.

note: \*\*Redis is key-value. \*\*

\*\*Field : \*\*a unique identifier for a datapoint.

**Value**: data related to the identifier.

<img src="../../../_resources/5376d5cc629e37e16b7f61bca507fa29.png" alt="5376d5cc629e37e16b7f61bca507fa29.png" width="250" height="315" class="jop-noMdConv"><img src="../../../_resources/5698e4cae513a64f878a4a37e88bc18f.png" alt="5698e4cae513a64f878a4a37e88bc18f.png" width="527" height="199" class="jop-noMdConv">

<img src="../../../_resources/e02a77288b04e823483e6f82795000b4.png" alt="e02a77288b04e823483e6f82795000b4.png" width="459" height="240" class="jop-noMdConv"><img src="../../../_resources/2e42c28b09a5c315614570dfeb81176d.png" alt="2e42c28b09a5c315614570dfeb81176d.png" width="573" height="239" class="jop-noMdConv">

JSON is not memory efficent at all,and does not support all data type.

Mongo stores JSON in memory in BSON.

<img src="../../../_resources/8582f2477dd34e5ad41a6606b847616e.png" alt="8582f2477dd34e5ad41a6606b847616e.png" width="500" height="279" class="jop-noMdConv"><img src="../../../_resources/2d2af700806d1a61012e9033c6f82196.png" alt="2d2af700806d1a61012e9033c6f82196.png" width="602" height="301" class="jop-noMdConv">

commands to backup and restore data in BSON or JSON in mongo-shell:

- BSON:
    - mongorestore
    - mongodump
- JSON:
    - mongoimport : we also can use not only JSON data but CSV too.
    - mongoexport

for querying in mongo we use a json.

**name space** is concat of database name and collection name.  like: **costumers.address**

There is a default db in mongo wich does multiple thing , one is authentication and stores user and password of users, called ==**Admin**==.

```mql
# show the list of databases
show dbs
# selecting database 
use db_name
# listing collection of a db
show collections
# query in a collection
db.collection_name.find(valid json for querying)
# if we did not select the database 
db_name.collection_name.find(valid json for querying)
# for showing more of a list of documents in a collection
it # stands for iterate
# count the document in a find 
db.collection_name.find(valid json for querying).count()
# to see all the data 
db.collection_name.find()
# a more readble verions
db.collection_name.find().pretty()
```

<img src="../../../_resources/1ffc08e7365576e82d56c8c7149223b4.png" alt="1ffc08e7365576e82d56c8c7149223b4.png" width="431" height="249" class="jop-noMdConv">

Note: **Mongo shell** is fully functional java script interpreter.

<img src="../../../_resources/f006c84209f3a39d6abfc133dd50db24.png" alt="f006c84209f3a39d6abfc133dd50db24.png" width="381" height="183" class="jop-noMdConv"><img src="../../../_resources/34b6ce5ac10a88292e7df03e895cd601.png" alt="34b6ce5ac10a88292e7df03e895cd601.png" width="492" height="133" class="jop-noMdConv">

```mql
# to get familiar with shape of documents in a collection or get one result in a find query
db.collection_name.findOne()
# create document
db.collection_name.insert(document in json format)
# create multiple document at a time 
db.collection_name.insert([document in json format, document in json format])
# to disable default insert ordering 
db.inspections.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },
                       { "_id": 3, "test": 3 }],{ "ordered": false })
# updating a document
db.collection_name.updateOne({condition to filter},{"$inc": {"field to update": value_to_updated}) # inc is to increase value it's a numeric operator
# updating many document
db.collection_name.updateMnay()
# the operators can be applied into multiple fields at same time
db.zips.updateOne({ "zip": "12534" }, { "$set": { "population": 17630 } })
# adding an element to an array field 
db.grades.updateOne({ "student_id": 250, "class_id": 339 },
                    { "$push": { "scores": { "type": "extra credit",
                                             "score": 100 }
                                }
                     })
# delete one 
db.collection_name.deleteOne()
# delete many
db.collection_name.deleteMany()
# example 
db.inspections.deleteOne({ "test": 3 })
# drop collection
db.inspection.drop()
```

**Like elastic search indexes  mongo db collections can be created with just inserting data into them** .

when we insert multiple document the default behavior is to insert them in order we give we can change that by setting it to false, the default behavior will cause if a error occures the documents after the problamatic document not be inserted.

if a field does not exists and we update it , it will get created.

Note: **because the mongo will keep the duplicate data use One operators like deleteOne only with _id.**

**removing all collection from database will remove the database.**

<img src="../../../_resources/e44ec69adf6ed7e53a6f278092536f2b.png" alt="e44ec69adf6ed7e53a6f278092536f2b.png" width="760" height="575" class="jop-noMdConv">

MQL is like SQL for MongoDB.

<img src="../../../_resources/2a6d6f3f8313e0533ed983a5ab88cfa9.png" alt="2a6d6f3f8313e0533ed983a5ab88cfa9.png" width="442" height="328" class="jop-noMdConv"><img src="../../../_resources/520a65bd3cb15d07859cf7600c989ceb.png" alt="520a65bd3cb15d07859cf7600c989ceb.png" width="453" height="216" class="jop-noMdConv">

**we can use this opertators in find, delete, update**.

```mql
# using comparison operator in a query
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$eq": "Customer" }}).pretty()
```

<img src="../../../_resources/7cd19f63f875202d118c07ef6f3552ce.png" alt="7cd19f63f875202d118c07ef6f3552ce.png" width="440" height="284" class="jop-noMdConv">

```mql
# example of logic operators
db.routes.find({ "$and": [ { "$or" :[ { "dst_airport": "KZN" },
                                    { "src_airport": "KZN" }
                                  ] },
                          { "$or" :[ { "airplane": "CR2" },
                                     { "airplane": "A81" } ] }
                         ]}).pretty()
```

**==and== logic operator is present by default**. but in some situation we must add the and manually(best practise to add it).

<img src="../../../_resources/37b2445f86c66886b8125ea9918bcfa8.png" alt="37b2445f86c66886b8125ea9918bcfa8.png" width="619" height="190" class="jop-noMdConv"><img src="../../../_resources/8f4da47302f35387852bf1de9168009b.png" alt="8f4da47302f35387852bf1de9168009b.png" width="584" height="321" class="jop-noMdConv"><img src="../../../_resources/e473230984f976a2b4e582faab3a4cba.png" alt="e473230984f976a2b4e582faab3a4cba.png" width="454" height="358" class="jop-noMdConv"><img src="../../../_resources/0e81ac478f3572ec91726cdc103805cb.png" alt="0e81ac478f3572ec91726cdc103805cb.png" width="596" height="308" class="jop-noMdConv">

<img src="../../../_resources/b5a4ef6079e027db24fba2b8d2744632.png" alt="b5a4ef6079e027db24fba2b8d2744632.png" width="502" height="208" class="jop-noMdConv">

querying normal will search in array fields to in such way that what we specify is part of an array if wrap our codition in bracket it will look for eaxct match.

```mql
# ammen is array filed
# this will look into the array if shampoo exists in it  
{"ammen": "shampo"}
# this will look for an array with single index 
{"ammen": ["shampo"]}
# the order of array in this way matter
# use $all operator to search in way that order does not matter
# $size to limit the result of array
db.listingsAndReviews.find({ "amenities": {
                                  "$size": 20,
                                  "$all": [ "Internet", "Wifi",  "Kitchen",
                                           "Heating", "Family/kid friendly",
                                           "Washer", "Dryer", "Essentials",
                                           "Shampoo", "Hangers",
                                           "Hair dryer", "Iron",
                                           "Laptop friendly workspace" ]
                                         }
                            }).pretty()
```

we can add ==**projection**== to the query to only see the fields that are interested in.

<img src="../../../_resources/70a2856f353c97b80c366246b35f01c2.png" alt="70a2856f353c97b80c366246b35f01c2.png" width="567" height="251" class="jop-noMdConv"><img src="../../../_resources/8960a79cac1ed0bf8a72c8236e0d127f.png" alt="8960a79cac1ed0bf8a72c8236e0d127f.png" width="657" height="376" class="jop-noMdConv">

**we can show only fields that match a certain condition**.

```mql
# Find all documents where the student in class 431 received a grade higher than 85 for any type of assignment
db.grades.find({ "class_id": 431 },
               { "scores": { "$elemMatch": { "score": { "$gt": 85 } } }
             }).pretty()
```

**==$elemMatch==** can be used in query or project part. this is an array operator

<img src="../../../_resources/ef2b09b76aef604389874a10ba1fbece.png" alt="ef2b09b76aef604389874a10ba1fbece.png" width="622" height="178" class="jop-noMdConv"><img src="../../../_resources/4886b3bb42ec92fecfb83e336af314b0.png" alt="4886b3bb42ec92fecfb83e336af314b0.png" width="673" height="272" class="jop-noMdConv">

we can access the sub documents with dot(.). called **dot notation**.

MongoDB also has a regex operator for querying.

==**Aggregation frame work:**== an frame work to query and modify the data with more advance features.

aggregate command accept a an array which **order of elements matter** and data will be aggregate in order.

==**$match**== statement is to filter base on some conditions ( like elastic search).

==**$project**== statement will act like the projection and will filter the  the fields that are not included or excluded.

==**$group**== is like group by in sql.

we have $sum operator for the the field .

```mql
db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 } } }
                                ])
```

<img src="../../../_resources/384456175d395db75a9c2a52e0e104cc.png" alt="384456175d395db75a9c2a52e0e104cc.png" width="307" height="313" class="jop-noMdConv">

**sort** and **limit** are cursor methods like **pretty** and **count**.

a ==**cursor method**== is applied to the data that lives in cursor not the actual data.

<ins>the order of them does not matter.</ins>

<img src="../../../_resources/3d9343316d28725668562af62a535837.png" alt="3d9343316d28725668562af62a535837.png" width="507" height="249" class="jop-noMdConv">

```mql
db.zips.find().sort({ "pop": -1 }).limit(10)
```

we can crate **index** to increase the query speed.

if we don't create index it will go look throw data **sequentially**.

there is two type of index:

- **single**: only on single filed
- **compound**: on multiple field

```mql
db.trips.createIndex({ "birth year": 1 })
db.trips.createIndex({ "birth year": -1 })
db.trips.createIndex({ "start station id": 1, "birth year": 1 })
```

<img src="../../../_resources/8dac7bbb8afcbef199f05188b3152f7d.png" alt="8dac7bbb8afcbef199f05188b3152f7d.png" width="527" height="231" class="jop-noMdConv"><img src="../../../_resources/cd42f00790b4f480c81fd7f16da55d3f.png" alt="cd42f00790b4f480c81fd7f16da55d3f.png" width="497" height="311" class="jop-noMdConv"><img src="../../../_resources/0353201342f775bea9b97bfac93c60b9.png" alt="0353201342f775bea9b97bfac93c60b9.png" width="294" height="252" class="jop-noMdConv">

==**upsert**== is false by default.

```mql
db.iot.updateOne({ "sensor": r.sensor, "date": r.date,
                   "valcount": { "$lt": 48 } },
                         { "$push": { "readings": { "v": r.value, "t": r.time } },
                        "$inc": { "valcount": 1, "total": r.value } },
                 { "upsert": true })
```