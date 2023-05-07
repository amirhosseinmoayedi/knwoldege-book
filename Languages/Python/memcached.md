---
title: memcached
updated: 2021-07-24 11:44:08Z
created: 2021-07-24 11:29:33Z
author: Amir Hossein Moayedi
---

memcached is like redis for chaching, python it self got data caching and offers some other  method but memcached is used when data is distributed and we have duplicate of our services.

*memcached offer something like dictionary in memory.*

*Cache invalidation*, which defines when to remove the cache because it is out of sync with the current data, is also something that your application will have to handle

FallBack:

The FallbackClient queries the old cache passed to its  constructor, respecting the order. In this case, the new cache server  will always be queried first, and in case of a cache miss, the old one  will be queried—avoiding a possible return-trip to the primary source of  data.

If any key is set, it will only be set to the new cache. After some time, the old cache can be decommissioned and the FallbackClient can be replaced directed with the new_cache client.

CAS:

The gets method returns the value, just like the get method, but it also returns a *CAS value*.

What is in this value is not relevant, but it is used for the next method cas call. This method is equivalent to the set operation, except that it fails if the value has changed since the gets operation. In case of success, the loop is broken. Otherwise, the operation is restarted from the beginning.

to use the memchached:
    1. install it on os (apt install memcached)
    2. install the python library for it

```
from pymemcache.client import base

client = base.Client(('localhost', 11211))

client.set('some_key', 'some value')

print(client.get('some_key'))
```