---
title: CELERY Doc 1
updated: 2022-02-01 10:32:51Z
created: 2021-10-19 05:55:23Z
latitude: 51.29930000
longitude: 9.49100000
altitude: 0.0000
---

Redis and rabbitmq is very like and both can be used as backend and broker.

The first thing you need is a Celery instance. We call this the Celery application or just app for short. As this instance is used as the entry-point for everything you want to do in Celery, like creating tasks and managing workers.

```python
app = Celery('tasks', broker='pyamqp://guest@localhost//')
```

The first argument to Celery is the ==name of the current module==. This is only needed so that names can be automatically generated when the tasks are defined in the main module.

The second ==argument is the broker keyword argument,== specifying the URL of the message broker you want to use. Here using RabbitMQ (also the default option).

To call our task you can use the ***delay()*** method.

This is a handy shortcut to the **apply_async()** method that gives greater control of the task execution.

Calling a task returns an **AsyncResult** instance. This can be used to check the state of the task, wait for the task to finish, or get its return value

If you want to keep track of the tasks’ states, Celery needs to store or send the states somewhere:

```python
app = Celery('tasks', backend='rpc://', broker='pyamqp://')
```

The ***ready()*** method returns whether the task has finished processing or not:

```python
result.get(timeout=1)
```

==The default configuration should be good enough for most use cases,==

When you send a task message in Celery, that message <ins>won’t contain any source code</ins>, but only the name of the task you want to execute. This works similarly to how host names work on the internet: every worker maintains a mapping of task names to their actual functions, called the **task registry.**

Creating a Celery instance will only do the following:

- Create a logical clock instance, used for events.
- Create the task registry.
- Set itself as the current app (but not if the set\_as\_current argument was disabled)
- Call the app.on_init() callback (does nothing by default).

The application instance is ==lazy==.

To create a ==custom task class you should inherit from the neutral base class==:

```python
from celery import Task
class DebugTask(Task):
     def call(self, *args, kwargs):
      	print('TASK STARTING: {0.name (http://0.name/)}[{0.request.id (http://0.request.id/)}]'.format(self)) 			 
         	return self.run(*args, kwargs)
```

A task is a class that can be created out of any **callable**. It performs dual roles in that it defines both **what happens when a task is called** (sends a message), and **what happens when a worker receives that message**.

==Every task class has a unique name==, and this name is referenced in messages so the **worker can find the right function to execute**.

<ins>the function won’t cause unintended effects e**ven if called multiple times with the same arguments**.</ins>

Note that the worker will acknowledge the message if the child process executing the task is terminated (either by the task calling [`sys.exit()`](https://docs.python.org/dev/library/sys.html#sys.exit "(in Python v3.11)"), or by signal) even when [`acks_late`](https://docs.celeryproject.org/en/stable/userguide/tasks.html#Task.acks_late "Task.acks_late") is enabled. This behavior is intentional as…

1.  We don’t want to rerun tasks that forces the kernel to send a `SIGSEGV` (segmentation fault) or similar signals to the process.
    
2.  We assume that a system administrator deliberately killing the task does not want it to automatically restart.
    
3.  A task that allocates too much memory is in danger of triggering the kernel OOM killer, the same may happen again.
    
4.  A task that always fails when redelivered may cause a high-frequency message loop taking down the system.
    

I==f you really want a task to be redelivered in these scenarios you should consider enabling the [`task_reject_on_worker_lost`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-task_reject_on_worker_lost) setting.==

u can easily create a task from any callable by using the [`task()`](https://docs.celeryproject.org/en/stable/reference/celery.html#celery.Celery.task "celery.Celery.task") decorator,There are also many [options](https://docs.celeryproject.org/en/stable/userguide/tasks.html#task-options) that can be set for the task.

```python
@app.task(serializer='json')
```

**Multiple decorators**:

When using multiple decorators in combination with the task decorator you must make sure that the task decorator is applied ==last==

### Task inheritance

```python
import celery

class MyTask(celery.Task):

    def on_failure(self, exc, task_id, args, kwargs, einfo):
        print('{0!r} failed: {1!r}'.format(task_id, exc))

@app.task(base=MyTask)
def add(x, y):
    raise KeyError()
```

<ins>best practice is to use absolute import.</ins>

* * *

app.Task.**request**: contains information and state related to the currently executing task.

The ==bind== argument means that the function will be a “bound method” so that **you can access attributes and methods on the task type instance**.

***app.Task.retry()*** can be used to re-execute the task.

The ***app.Task.retry()*** call will raise an exception <ins>so any code after the retry won’t be reached.</ins> This is the Retry exception, it isn’t handled as an error but rather as a semi-predicate to signify to the worker that the task is to be retried, so that it can store the correct state when a result backend is enabled.

**This is normal operation and always happens unless the throw argument to retry is set to False.**

Sometimes you just want to retry a task whenever a particular **exception** is raised.

Fortunately, you can tell Celery to automatically retry a task using ***autoretry_for*** argument in the task() decorator.

**Task.default\_retry\_delay**

<ins>**Default time in seconds before a retry of the task should be executed**</ins>. Can be either int or float. Default is a three minute delay.

**Task.rate_limit**

Set the rate limit for this task type (limits the number of tasks that can be run in a given time frame). <ins>Tasks will still complete when a rate limit is in effect, but it may take some time before it’s allowed to start.</ins>

**Task.compression**

A ==string identifying the default compression scheme to use==.

**Task.backend**

The result store backend to use for this task.

**Task.track_started**

If True the task will report its status as “started” when the task is executed by a worker. The default value is False as the normal behavior is to **not report that level of granularity**. Tasks are either **pending**, **finished**, or **waiting to be retried**. Having a “started” status can be useful for when there are long running tasks and there’s a need to report what task is currently running.

* * *

==No backend works well for every use case. You should read about the strengths and weaknesses of each backend, and choose the most appropriate for your needs.==

**RPC Result Backend** (RabbitMQ/QPid)

The RPC result backend (rpc://) is special as **it doesn’t actually store the states,** but rather **sends them as messages**. This is an important difference as it means that a **result can only be retrieved once**, and only by the client that initiated the task. ==Two different processes can’t wait for the same result.==

**Even with that limitation, it is an excellent choice if you need to receive state changes in real-time. Using messaging means the client doesn’t have to poll for new states.**

==The messages are transient (non-persistent) by default, so the results will disappear if the broker restarts. You can configure the result backend to send persistent messages using the result_persistent setting.==

## Built-in States

PENDING

Task is waiting for execution or unknown. Any task id that’s not known is implied to be in the pending state.

STARTED

Task has been started. Not reported by default, to enable please see app.Task.track_started.

SUCCESS

Task has been successfully executed.

**meta-data**

**result contains the return value of the task.**

ready :Yes

FAILURE

Task execution resulted in failure.

meta-data

result contains the exception occurred, and traceback contains the backtrace of the stack at the point when the exception was raised.

RETRY

Task is being retried.

meta-data

result contains the exception that caused the retry, and traceback **contains the backtrace of the stack at the point when the exceptions was raised.**

REVOKED

Task has been revoked.

If you don’t care about the results of a task, be sure to set the ignore_result option, as storing results wastes time and resources.

* * *

# Performance and Strategies

## Granularity

**The task granularity is the amount of computation needed by each subtask.** ==In general it is better to split the problem up into many small tasks rather than have a few long running tasks.==

**With smaller tasks you can process more tasks in parallel** and the won’t run long enough to **block the worker from processing other waiting tasks**.

## Data locality

**The worker processing the task should be as close to the data as possible**. **The best would be to have a copy in memory.**

**The easiest way to share data between workers is to use a ==distributed cache system==, like memcached.**

## State

**Since Celery is a distributed system, you can’t know which process, or on what machine the task will be executed**. You can’t even know if the task will run in a timely manner.

==Another gotcha is Django model objects. They shouldn’t be passed on as arguments to tasks==. ==**It’s almost always better to re-fetch the object from the database when the task is running instead, as using old data may lead to race conditions.**==

## Database transactions

Let’s have a look at another example:

```python
from django.db import transaction
 from django.http import HttpResponseRedirect
 
@transaction.atomic
def create_article(request):
    article = Article.objects.create()
    expand_abbreviations.delay(article.pk (http://article.pk/)) 
    return HttpResponseRedirect('/articles/')
```

This is a Django view creating an article object in the database, then passing the primary key to a task. ==It uses the transaction.atomic decorator, that will commit the transaction when the view returns, or roll back if the view raises an exception.==

There’s a race condition if the task starts executing before the transaction has been committed; The database object doesn’t exist yet!

**The solution is to use the on_commit callback to launch your Celery task once all transactions have been committed successfully.**

```python
from django.db.transaction import on_commit

def create_article(request)
    article = Article.objects.create()
    on_commit(lambda: expand_abbreviations.delay(article.pk (http://article.pk/)))
```

* * *

The API for calling task defines a standard set of execution options, as well as three methods:

```python
apply_async(args[, kwargs[, …]])
```

**Sends a task message**.

```python
delay(*args, kwargs)
```

**Shortcut to send a task message, but doesn’t support execution options**.

```python
calling (call)
```

**Applying an object supporting the calling API (e.g., add(2, 2)) means that the task will not be executed by a worker, but in the current process instead (a message won’t be sent).**

### Quick Cheat Sheet

```python
T.delay(arg, kwarg=value)

# Star arguments shortcut to .apply_async. (.delay(*args, kwargs) calls .apply_async(args, kwargs)).

T.apply_async((arg,), {'kwarg': value})

T.apply_async(countdown=10)

# executes in 10 seconds from now.

T.apply_async(eta=now + timedelta(seconds=10))

# executes in 10 seconds from now, specified using eta

T.apply_async(countdown=60, expires=120)

# executes in one minute from now, but expires after 2 minutes.

T.apply_async(expires=now + timedelta(days=2))

# expires in 2 days, set using datetime.
```

## Linking (callbacks/errbacks)

**Celery supports linking tasks together so that one task follows another. The callback task will be applied with the result of the parent task as a partial argument**

```python
add.apply_async((2, 2), link=add.s(16))
# add.s is another method
```

T==he expires argument defines an optional expiry time, either as seconds after task publish, or a specific date and time using datetime==

==**Data transferred between clients and workers needs to be serialized,**== so every message in Celery has a **content_type** header that describes the serialization method used to encode it. The default serializer is ***JSON***.

Celery can **<ins>route tasks to different queues</ins>**.Simple routing (name &lt;-&gt; name) is accomplished using the queue option:

```python
add.apply_async(queue='priority.high')
```

You can then assign workers to the priority.high queue by using the workers **-Q** argument:

```bash
celery -A proj worker -l INFO -Q celery,priority.high
```

You can start multiple workers on the same machine, but be sure to name each individual worker by specifying a node name with the --**hostname** argument.

The hostname argument can expand the following variables:

|     |     |
| --- | --- |
| %h  | Hostname, including domain name. |
| %n  | Hostname only. |
| %d  | Domain name only. |

```bash
celery -A conf worker -l info -n worker1@%h
```

**If the worker won’t shutdown after considerate time, for being stuck in an infinite-loop or similar, you can use the KILL signal to force terminate the worker: but be aware that currently executing tasks will be lost (i.e., unless the tasks have the ==acks_late== option set).**

==By default multiprocessing is used to perform concurrent execution of tasks==, but you can also use Eventlet. The number of worker processes/threads can be changed using the ***--concurrency*** **argument and defaults to the number of CPUs available on the machine**.

You can get a list of queues that a worker consumes from by using the active_queues control command:

```bash
celery -A proj inspect active_queues
```

## Queues

**A worker instance can consume from any number of queues. By default it will consume from all queues defined in the task_queues setting** (==that if not specified falls back to the default queue named celery==).

You can specify what queues to consume from at start-up, by giving a comma separated list of queues to the -Q option:

```bash
celery -A proj worker -l INFO -Q foo,bar,baz
```

If the queue name is defined in ==task_queues== it will use that configuration, but if it’s not defined in the list of queues Celery will automatically generate a new queue for you (depending on the ***task\_create\_missing_queues*** option).

You can also tell the worker to start and stop consuming from a queue at run-time using the remote control commands ***add_consumer*** and ***cancel_consumer***.