---
title: celery
updated: 2021-11-22 12:46:32Z
created: 2021-06-26 17:07:00Z
author: Amir Hossein Moayedi
---

celery needs a broker like rabbit-MQ or Redis or mango-DB and ...
celery is written in python.

in rabbit MQ :
we got channels with producers and consumers.
we got publisher and subscriber in Redis.

celery would stand between producer before channel and before consumer after channel and would manage the key and data and channel for us.

![gnome-shell-screenshot-S1MJ50.png](../../../_resources/gnome-shell-screenshot-S1MJ50.png)

we need to first create an instance of a class in celery, the instance is called application normally

each application is thread-safe, which means that celery applications can run together without a problem

```
from celery import Celery
from time import sleep

app = Celery('name of job normally the file name', broker= broker address in protocol:ip:port)

@app.task
some function
```

`celery worker -A(for the name of the module) name_of_moddule`

celery got log level (from debug to fatal)
to show the log level from a specified level to uppers:
`celery -A tasks worker --loglevel=info`

redis config

```
pip install -U "celery[redis]"
broker = redis://:password@hostname:port/db_number
```

for calling task in celery:

apply\_async  and delay, the difference between these are apply\_asynce got more options

count down: for executing after count downtime
expire: will wait until the expiration date if can't be executed

for running a task first we need to import it and then the name of the module then delay or apply_async

```
from task, import add
add.apply_async(args=[3, 4], countdown=10)
```

The flower is a graphical extension web-based for managing celery in a GUI.
for running it

```
pip install flower
flower -A module --port=5555
```

in celery retried and failed process must not be much

management commands in celery :
status: list of workers (node)
result: will return the task result
purge: will remove all the tasks or jobs
inspect: will show nods and their status
and ...

[management](https://docs.celeryproject.org/en/stable/userguide/monitoring.html)

for saving data we use RPC or Redis

first we setup the backend then we will call the module in a variable and receive it using get

we store the returned data in a variable
with .ready() we check if the result is returned or not

with .get() we get the returned value if it's ready we can use .get(timeout=2) to wait to get the value for 2 sec

```
result = add.delay(2, 3)
result.ready()
result.get()
```

Configs in celery:
celery got a lot of configs, we got 3 way to set the config
1-we add setting each in a line

```
app.conf.enable_utc = True
app.conf.timezone = 'asia/tehran'
```

2- we add setting in group

```
app.conf.update(
    enable_utc=True,
    timezone='asia/tehran',
    task_serializer='json'
)
```

3-we can store setting in a separate python file and add then add it
`app.config_from_object('conf')`
note: conf is the name of python file
in the file

```
enable_utc = True
timezone = 'asia/tehran'
task_serializer = 'json'
```