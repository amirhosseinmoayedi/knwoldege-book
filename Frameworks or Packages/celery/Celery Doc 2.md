---
title: Celery Doc 2
updated: 2022-02-01 10:48:35Z
created: 2021-10-20 07:38:10Z
latitude: 35.69800000
longitude: 51.41150000
altitude: 0.0000
---

**celery beat** is a scheduler; It kicks off tasks at regular intervals, that are then executed by available worker nodes in the cluster.

You have to ==ensure only a single scheduler is running for a schedule at a time,== otherwise you’d end up with duplicate tasks.

The periodic task schedules uses the UTC time zone by default, but you can change the time zone used using the [`timezone`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-timezone) setting.

```python
timezone = 'Europe/London'
```

==For Django users== the time zone specified in the `TIME_ZONE` setting will be used, or you can specify a custom time zone for Celery alone by using the [`timezone`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-timezone) setting.

To call a task periodically you have to ==add an entry to the beat schedule list==.

```python
from celery import Celery
from celery.schedules import crontab

app = Celery()

@app.on_after_configure.connect
def setup_periodic_tasks(sender, **kwargs):
    # Calls test('hello') every 10 seconds.
    sender.add_periodic_task(10.0, test.s('hello'), name='add every 10')

    # Calls test('world') every 30 seconds
    sender.add_periodic_task(30.0, test.s('world'), expires=10)

    # Executes every Monday morning at 7:30 a.m.
    sender.add_periodic_task(
        crontab(hour=7, minute=30, day_of_week=1),
        test.s('Happy Mondays!'),
    )

@app.task
def test(arg):
    print(arg)

@app.task
def add(x, y):
    z = x + y
    print(z)
```

**Note that [`on_after_configure`](https://docs.celeryproject.org/en/stable/reference/celery.html#celery.Celery.on_after_configure "celery.Celery.on_after_configure") is sent after the app is set up, so tasks outside the module where the app is declared (e.g. in a tasks.py file located by [`celery.Celery.autodiscover_tasks()`](https://docs.celeryproject.org/en/stable/reference/celery.html#celery.Celery.autodiscover_tasks "celery.Celery.autodiscover_tasks")) must use a later signal, such as [`on_after_finalize`](https://docs.celeryproject.org/en/stable/reference/celery.html#celery.Celery.on_after_finalize "celery.Celery.on_after_finalize").**

you can also define in other way:

## Crontab Scheduels

```python
from celery.schedules import crontab

app.conf.beat_schedule = {
    # Executes every Monday morning at 7:30 a.m.
    'add-every-monday-morning': {
        'task': 'tasks.add',
        'schedule': crontab(hour=7, minute=30, day_of_week=1),
        'args': (16, 16),
    },
}
```

[for more detail options in crontab.](https://docs.celeryproject.org/en/stable/userguide/periodic-tasks.html)

* * *

## BASIC

The simplest way to do routing is to use the [`task_create_missing_queues`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-task_create_missing_queues) setting (on by default).

With this setting on, a named queue that’s not already defined in [`task_queues`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-task_queues) will be created automatically. This makes it easy to perform simple routing tasks.

```python
task_routes = ([
    ('feed.tasks.*', {'queue': 'feeds'}),
    ('web.tasks.*', {'queue': 'web'}),
    (re.compile(r'(video|image)\.tasks\..*'), {'queue': 'media'}),
],)
# name of task or directory, {'queue': 'name of queue'}
```

then you should give this queues to workers

* * *

# brief settings

## Tasks

#### `task_annotations`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#task-annotations "Permalink to this headline")

This setting can be used to rewrite any task attribute from the configuration. The setting can be a dict, or a list of annotation objects that filter for tasks and return a map of attributes to change.

```python
task_annotations = {'*': {'rate_limit': '10/s'}}
```

#### `task_compression`

Default compression used for task messages. Can be `gzip`, `bzip2` (if available), or any custom compression schemes registered in the Kombu compression registry.

The default is to send uncompressed messages.

#### `task_track_started`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#task-track-started "Permalink to this headline")

Default: Disabled.

If `True` the task will report its status as ‘started’ when the task is executed by a worker. The default value is `False` as the normal behavior is to not report that level of granularity. Tasks are either pending, finished, or waiting to be retried. Having a ‘started’ state can be useful for when there are long running tasks and there’s a need to report what task is currently running.

#### `task_time_limit`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#task-time-limit "Permalink to this headline")

Default: No time limit.

Task hard time limit in seconds. The worker processing the task will be killed and replaced with a new one when this is exceeded.

#### `task_soft_time_limit`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#task-soft-time-limit "Permalink to this headline")

Default: No soft time limit.

Task soft time limit in seconds.

The [`SoftTimeLimitExceeded`](https://docs.celeryproject.org/en/stable/reference/celery.exceptions.html#celery.exceptions.SoftTimeLimitExceeded "celery.exceptions.SoftTimeLimitExceeded") exception will be raised when this is exceeded.

#### `task_acks_late`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#task-acks-late "Permalink to this headline")

Default: Disabled.

Late ack means the task messages will be acknowledged **after** the task has been executed, not *just before* (the default behavior).

#### `task_acks_on_failure_or_timeout`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#task-acks-on-failure-or-timeout "Permalink to this headline")

Default: Enabled

When enabled messages for all tasks will be acknowledged even if they fail or time out.

Configuring this setting only applies to tasks that are acknowledged **after** they have been executed and only if [`task_acks_late`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-task_acks_late) is enabled.

#### `task_default_rate_limit`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#task-default-rate-limit "Permalink to this headline")

Default: No rate limit.

The global default rate limit for tasks.

This value is used for tasks that doesn’t have a custom rate limit

## backend

#### `result_backend_always_retry`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#result-backend-always-retry "Permalink to this headline")

Default: `False`

If enable, backend will try to retry on the event of recoverable exceptions instead of propagating the exception. It will use an exponential backoff sleep time between 2 retries.

#### `result_backend_max_retries`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#result-backend-max-retries "Permalink to this headline")

Default: Inf

This is the maximum of retries in case of recoverable exceptions.

## rpc

#### `result_persistent`[](https://docs.celeryproject.org/en/stable/userguide/configuration.html#result-persistent "Permalink to this headline")

Default: Disabled by default (transient messages).

If set to `True`, result messages will be persistent. This means the messages won’t be lost after a broker restart.

* * *

# Monitoring

Celery command can be used for monitoring but there are lot's of command and it's hard

[commands guid](https://docs.celeryproject.org/en/stable/userguide/monitoring.html#id5)

==Flower== is a real-time web based monitor and administration tool for Celery. It’s under active development, but is already an essential tool. Being the recommended monitor for Celery, it obsoletes the Django-Admin monitor, `celerymon` and the `ncurses` based monitor.

```bash
# install it
pip install flower
# running it 
celery -A proj flower
celery -A proj flower --port=5555
celery flower --broker=amqp://guest:guest@localhost:5672//
```

## RabbitMQ[](https://docs.celeryproject.org/en/stable/userguide/monitoring.html#rabbitmq "Permalink to this headline")

To manage a Celery cluster it is important to know how **RabbitMQ can be monitored**.

RabbitMQ ships with the **==rabbitmqctl==** command, with this you can list queues, exchanges, bindings, queue lengths, the memory usage of each queue, as well as manage users, virtual hosts and their permissions.

an example:

```bash
rabbitmqctl list_queues name messages messages_ready
```

## Events[](https://docs.celeryproject.org/en/stable/userguide/monitoring.html#events "Permalink to this headline")

<ins>The worker has the ability to send a message whenever some event happens.</ins> These events are then captured by tools like Flower, and **celery events** to monitor the cluster.

* * *

# Security

## Broker:

By default, workers trust that the data they get from the broker **hasn’t been tampered with**.

The first line of defense should be to put a firewall in front of the broker, allowing only white-listed machines to access it.

Keep in mind that both firewall misconfiguration, and temporarily disabling the firewall, is common in the real world. Solid security policy includes monitoring of firewall equipment to detect if they’ve been disabled, be it accidentally or on purpose.

## Client

==Having the broker properly secured doesn’t matter if arbitrary messages can be sent through a client==

## Worker

Task running inside the worker have the same permission as the worker it self.

## Message Signing[](https://docs.celeryproject.org/en/stable/userguide/security.html#message-signing "Permalink to this headline")

Celery can use the [cryptography](https://pypi.python.org/pypi/cryptography/) library to ==sign message using Public-key cryptography==, where messages sent by ==clients== are **signed using a private key** and then later verified by the ==worker using a public certificate==.

Optimally certificates should be signed by an official [Certificate Authority](https://en.wikipedia.org/wiki/Certificate_authority), but they ==can also be self-signed.==

To enable this you should configure the [`task_serializer`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-task_serializer) setting to use the auth serializer. Enforcing the workers to only accept signed messages, you should set accept\_content to \[‘auth’\]. For additional signing of the event protocol, set event\_serializer to auth. Also required is configuring the paths used to locate private keys and certificates on the file-system: the [`security_key`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-security_key), [`security_certificate`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-security_certificate), and [`security_cert_store`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-security_cert_store) settings respectively. You can tweak the signing algorithm with [`security_digest`](https://docs.celeryproject.org/en/stable/userguide/configuration.html#std-setting-security_digest).

With these configured it’s also necessary to call the `celery.setup_security()` function. Note that this will also disable all insecure serializers so that the worker won’t accept messages with untrusted content types.

This is an example configuration using the auth serializer, with the private key and certificate files located in /etc/ssl.

```python
app = Celery() 
app.conf.update( 
    security_key='/etc/ssl/private/worker.key'
    security_certificate='/etc/ssl/certs/worker.pem'
     security_cert_store='/etc/ssl/certs/*.pem',
      security_digest='sha256',
       task_serializer='auth',
        event_serializer='auth',
         accept_content=['auth'] ) 
app.setup_security()
```

Note

While relative paths aren’t disallowed, using absolute paths is recommended for these files.

Also note that the auth serializer won’t encrypt the contents of a message, so if needed this will have to be enabled separately.

## Logs

Logs are usually the first place to look for evidence of security breaches, but they’re useless if they can be tampered with.

A good solution is to set up centralized logging with a dedicated logging server. Access to it should be restricted. In addition to having all of the logs in a single place, if configured correctly, it can make it harder for intruders to tamper with your logs.