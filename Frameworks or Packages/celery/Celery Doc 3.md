---
title: Celery Doc 3
updated: 2021-10-21 10:15:19Z
created: 2021-10-21 09:53:14Z
latitude: 51.29930000
longitude: 9.49100000
altitude: 0.0000
---

## Debugging

[`celery.contrib.rdb`](https://docs.celeryproject.org/en/stable/reference/celery.contrib.rdb.html#module-celery.contrib.rdb "celery.contrib.rdb") is an extended version of [`pdb`](https://docs.python.org/dev/library/pdb.html#module-pdb "(in Python v3.11)") that enables remote debugging of processes that doesn’t have terminal access.

```python
from celery import task
from celery.contrib import rdb

@task()
def add(x, y):
    result = x + y
    rdb.set_trace()  # <- set break-point
    return result
```

Test

an Test Example:

```python
assert mul.delay(4, 4).get(timeout=10) == 16
```

## Signals

Signals use the same implementation as `django.core.dispatch`.

Signals in celery can be from task or worker or evntlet ...

this is docs for signals because the list is long :

[Document](https://docs.celeryproject.org/en/stable/userguide/signals.html)