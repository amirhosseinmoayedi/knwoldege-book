---
title: Notes from documentation
updated: 2021-08-03 07:18:56Z
created: 2021-08-01 17:44:58Z
author: Amir Hossein Moayedi
---

Url will be compiled after a run to regex to work faster.

if the form we submit will change the database method should be POST

we can integrate Apache with WSGI of Django in two ways:
1. Memory embed: Loads the project in memory not very secure but fast
2. Deamon: create an independent process in the background it's safer

we can make each Django app into a python package, [how to](https://docs.djangoproject.com/en/3.2/intro/reusable-apps/).

Model Verbos name: If the verbose name isn’t given, Django will automatically create it using the field’s attribute name, ***converting underscores to spaces***.

In a relational field, in Django, it's better to give the name of another model in a quote than importing them.

Generally, ManyToManyField instances should go in the object that’s going to be edited on a form.

For creating an object in intermidatry table we use through_defaults to give it extra info it's need.(many to many)

`beatles.members.create(name="George Harrison", through_defaults={'date_joined': date(1960, 8, 1)})`

We can create ***our own Custom field*** in Django.

In overriding the save and delete :

- If you forget to call the superclass method, the default behavior won’t happen and the database won’t get touched.
- delete() method for an object is not necessarily called when deleting objects in bulk using a QuerySet or as a result of a cascading delete. To **ensure** customized delete logic **gets executed**, you can use pre_delete and/or post_delete signals.
- Unfortunately, there isn’t a workaround when creating or updating objects in bulk, since none of save(), pre_save, and post_save are called.

Inheriting from meta class abstract class
`class Meta(CommonInfo.Meta):`

Note:Due to the way Python inheritance works, if a child class inherits from multiple abstract base classes, only the Meta options from the first listed class will be inherited by default. To inherit Meta options from multiple abstract base classes, you must* explicitly declare the Meta inheritance*

when you are using ***related_name or related_query_name in an abstract base class*** (only), part of the value should contain '%(app_label)s' and '%(class)s'.

If you** don’t specify a related_name attribute for a field** in an abstract base class, the default reverse name will be the name of the child class followed by '_set'.

one to one field is a kind of multi-table inheritance (concrete inheritance).

proxy inheritance: is like abstract except non of the method or manager or meta would be inherited from the  proxy model.

note: we can index the field with db_index=True in the field or we can specify index = (fields) in the meta class.

*blow codes are not equal:*

```
model.objects.filter(c1).filter(c2)

# and

model.object.filter(c1 , c2)
```

the secound one is like OR.

we can't have multiple conditions in exclude() we should make a*** separate query*** for that.

Each QuerySet contains a cache to minimize database access.

to activate caching we should evaluate the query else the query will hit the database again.

can't update* relational field* in django we can't use .update() method.

By default the RelatedManager used for reverse relations is a subclass of the default manager for that model.

If you pickle a QuerySet, this will force all the results to be loaded into memory prior to pickling.

Annotate: Data annotation is **the process of labeling the data available in various formats like text, video or images**

aggregate expression: do a operation with value of a rows in a column

Note: it's better to use .desc() rather than negative sign ' - '