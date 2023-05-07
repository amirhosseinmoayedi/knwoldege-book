---
title: Advance Python Extra
updated: 2021-03-04 09:39:12Z
created: 2021-03-04 09:04:17Z
author: Amir Hossein Moayedi
---

virtual env
we can use 2 thing for this :
1 - python venv
crating env: python3 -m venv name
activate env : source /bin/activate
deactivate : deactivate
2 -**pipenv**
creating env: pipenv shell
activate env :   pipenv shell
deactivate :  exit
note: **pipenv**  is not installed on default  we use pip to install it.
`pip install pipenv`

* * *

args and kwargs

*args is for when we don't know **how many argument** we recived and the type is **tuple**.

```
def test(*args):
    for i in args:
        print i
```

**kwargs is for when we don't know how many **keyword** we will use and the type is **dict**.

```
def test(**kwargs):
    for i in kwargs.values:
        print i
```

* * *

magic or special method

__str__ : we use return with it to if object of a class is called what to return(print) and it will just print string , so  when we call it it would print what we returned not location of it on memory.

__repr__: it goals is to return a string to** recreate an object easily **

static and class method
*normal method are attribute method.*

**class method**: can be called without an instance and no instance parameters or attribute won't be used in it just class attribute,they get cls instead of self.

```
@classmethode
def class_methode(cls):
```

**static method**: it will be run any way, they don't take any argument.
*it dos't have any info about class or instance attributes.*

```
@staticmethode
def static_methode():
```

class method are called **factory** too we can define** class method for creating instance.**

```
type = ['paper back']
 @classmethode
 def paperback(cls, name):
     return cls(name, cls.type[0])
```

class composition
**more used than inheritance**

*in this way we create an object (instance) of class using instance of other classes .*

like we create 2 instance of class book and create a book shelf that contain them.

* * *

type hinting
specify the output and argument type of function or method.
`def test(var :str) -> bool:`
or in class composition we use this:
`def __init__(self, book: list[Book]):`
*it will just accept a list of object of book*

* * *

__name__ is a global variable that differ **based on file name**

__main__ is a file that we run and any other file that were imported __name__ would be their filename.

__init__.py  tells the python this is a python package and when we create module and want to import from it it sometimes needed

you can use ( ) in front of return to return multi line

```
return ( something in
        multi line)
```

* * *

relative import

*when we import a module our main code will be stopped and the code that we called is gonna run*

for using relative import (importing files that are in our current folder)
we need to put that files in a folder or we cant do it .
relative import:
`from * import *`
** it's better to use full path for not running to error.**

* * *

**error handling**
for raise we can use python errors or our custom error
try: it try to run a code
except: if it couldn't run it will do something
else: it would run if try was success

finally: don't care if  the try or except happens, the code after it would be ran anyway

**creating a custom error **
we raise it manually

can we define class in it name and it would inherit from  python exception or errors

then we can pass and use it
`raise TypeError('something')`

* * *

first class functions

we give parameters of an **function** to another or use it to call** different function**.

decorators
**decorators** can be without @ ,with defining a function in other
***are kinda functions that add extra functionality to a functions or method***

**@ if it's in top of a function it prevent function from being created before calling the function that is  in front of @.**

is kinda decorator we use lot : functool_wraper