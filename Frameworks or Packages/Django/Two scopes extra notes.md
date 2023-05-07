---
title: Two scopes extra notes
updated: 2021-08-02 10:54:28Z
created: 2021-08-01 18:01:46Z
author: Amir Hossein Moayedi
---

note: if we create concreate or multi-table inheritance the table of that model would be created and for getting fields every time there must be a database join between tables to get the fields.

how to use squshmigrations:
`python manage.py squashmigrations <appname> <squashfrom> <squashto>`

data redundancy: having duplicate data.

data integrity is the maintenance of, and the assurance of, data accuracy and consistency. means we check if data is changed or corrupted or not!

**Database normalization** is the process of structuring a database, usually a relational database, in accordance with a series of so-called normal forms in order to reduce data redundancy and improve data integrity.

**denormalized data** is the process of joining data or flatten the data which makes data redundant.

denormalized data take less space but because it needs a join of tables in the database it's got database overhead.

QuerySets are only evaluated in the following cases:
• The first time you iterate over them

• When you slice them, for instance, Post.objects.all()[:3:1]( only if specify the step)

• When you pickle or cache them
• When you call repr() or len() on them
• When you explicitly call list() on them
• When you test them in a statement, such as bool() , or , and , or if

A StreamingHttpResponse, on the other hand, is a response whose body is sent to the client in multiple pieces, or “chunks.”

note: One of the best use cases for streaming responses is to send large files, e.g. a large CSV file.