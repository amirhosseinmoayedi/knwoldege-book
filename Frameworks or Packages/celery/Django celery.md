---
title: Django celery
updated: 2021-07-10 05:45:00Z
created: 2021-07-07 10:01:33Z
author: Amir Hossein Moayedi
---

if we want to use 4.0 or a later version of celery we need Django 1.8 +

for config the celery we can set them in env or we can put them in the project setting.

if we put them in the project setting:

```

# CELERY

CELERY_BROKER_URL = 'redis://:@localhost:6379/1'
CELERY_RESULT_BACKEND = 'redis://:@localhost:6379/0'
```

the CELERY_ in the begging is for the celery setting to be able to find them and distinguish between celery and other configs

.
after that, we create a file named anything(normally celery)
and :

```
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'celery_django.settings')

celery_app = Celery('Basic')
```

with os, we set the default celery setting to Django.
`celery_app.config_from_object('django.conf:settings', namespace='CELERY')`

we will read config from *Django setting* in this way, we use namespace to tell that what are you looking for in this situation is 'CELERY'

we add an auto-discover task to the instance, it will automatically find all the module named tasks in Django apps

`celery_app.autodiscover_tasks()`

after that, we will add this to __init__ of the Django project
`from .celery import celery_app`

for creating a task in an app after we created a file named task
we can import task or shared_task
task: if we have a single celery app config
shared task: for multi config (better to use this)

in a tasks.py :

```
from celery import shared_task

# @task

@shared_task
def add(x, y):
    return x + y
```

for running the worker first w got to the root of the project:
`celery -A app_name_that_config_exists_in_it worker ...`
in running the worker we use the app name instead of the module

we can call the celery task that we created in API or normal views they will return an instance that has a unique id

*note: we cant give a query set to a celery task *
*
*
periodic tasks: are tasks that will run on a certain schedule.

in Django, there is a package that will make this job simple named* Django celery beat* (will give a an admin interface).

`pip install django-celery-beat`

there is 5 table in admin:
    1. clocked
    2. crontabs
    3. intervals
    4. periodic tasks
    5. solar events
the periodic task will specify the task and the rest is time setting.

after installing the package and migrate, we need a worker then we run the celery beat with this command:

`celery  -A celery_django beat --loglevel=info --scheduler django_celery_beat.schedulers:DatabaseScheduler`

changing the schedule does not need a worker or beat restart.

note: we never run the celery and the beat in real-world manually we need a supervisor for that.

A supervisor is a tool that will run the tasks and workers in background as deamon

supervisor setup:

```
1. install supervisor
        sudo apt-get install supervisor

2. all supervisor processes goes here
        /etc/supervisor/conf.d

3. create project's celery configuration file for supervisor
        touch /etc/supervisor/conf.d/project_name.conf

4. write supervisor configuration:
        nano /etc/supervisor/conf.d/project_name.conf

        [program:project_name]
        user=user
        directory=/var/www/myproject/src/ # project directory

        command=/var/www/myproject/bin/celery -A myproject worker -l info # first is the virtual environment then the command to run

        numprocs=1 # number of worker
        autostart=true
        autorestart=true
        stdout_logfile=/var/log/myproject/celery.log
        stderr_logfile=/var/log/myproject/celery.err.log"

5. create log files
        touch /var/log/myproject/celery.log
        touch /var/log/myproject/celery.err.log

6. update supervisor configuration
        supervisorctl reread
        supervisorctl update

7. done
        supervisorctl {status|start|stop|restart} project_name
```