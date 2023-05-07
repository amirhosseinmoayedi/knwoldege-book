---
title: Notes
updated: 2021-12-06 06:47:50Z
created: 2021-11-24 10:07:05Z
latitude: 35.69800000
longitude: 51.41150000
altitude: 0.0000
---

About soft and hard deleting : soft delete data that might be used later but do not soft delete all the data.

if deleted entries are too much their table must be moved.

use the DataTimeField instaed of boolean field which are nullable , so if it is False it's null and if true there is time in it, the size might increase but data is more.

django choices are model form level and not database level.

example of using choices:

```
class UserRole(model.TextChoices):
    NORMAL = 'normal', 'Normal'
    ADMIN = 'admin', 'Admin'
    ARCHIVED = 'archived', 'Archived'
    

class User(models.Model):
    role = model.CharField(choices=UserRole.choices)
    
    class Meta:
    constraints = [
    	models.CheckConstraint(
    		name=
    	)
    ]
```

audit your model with fields like :

created\_at, modified\_at, deleted\_at, deleted\_by,

in time fields use default=timezone.now() because it's database level and auto\_now\_add is django level.

the default behavior of django is auto commit to change it use atomic.

```
from django.db import transacction


def post_something(request, *args):
    ...
    with transaction.atomic():
        some query 
        
    ...
```

query set in orm are atomic.