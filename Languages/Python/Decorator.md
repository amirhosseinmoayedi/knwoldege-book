---
title: Decorator
updated: 2021-09-17 12:28:31Z
created: 2021-09-16 07:02:56Z
latitude: 35.69800000
longitude: 51.41150000
altitude: 0.0000
---

because **functions are objects** their self they can be passed as argument to other functions .

**inner function** are function that you define inside other functions, they have direct access to variables and names in enclosing functions.

the inner function have access to the function that contained it or **outer function(enclosing).**

usages:

1 - **encapsulation**: to hide or protect the inner function

2 - **high order functions**: functions that operate on other functions by taking them as arguments returning them or both.

3 - **closure** factory functions: python closures are these inner functions that are enclosed within the outer functions.

```python
def outer(name):
    def inner():
        print(name)
        
    return inner
    
myFunction = outer('Ali')
myfunction()
```

**note**: if you have class that has only init use closure instead of class , or you want to hide the data.

* * *

**decorator** is function that can be used to modify the **behavior** of function or class. with out modifying   the actual code.

like: @property, @classmethod, @staticmethod

```python
def outer(func):
    def inner():
        # do something before 
        func()
        # do something after 
    return inner
```

usages:

- debug
- cache
- logging
- timing

```python
def debug(func):
    def _debug(*args, **kwargs):
        result = func(*args, **kwargs)
        print(*args, **kwargs)
        return result
    return _debug
    
    
@debug
def add(a, b):
    return a + b
 
add(5, 6)
```

we import warps from func tools module to tell it that you are the inner function not the function that passed to you.

```python
from functools

def decorator(func):
    @wraps(func)
    def wrapper_decorator(*args, **kwrags):
        # do something 
        value = func(*args, **kwargs)
        # do something 
        return value
    return wrapper_decorator
```

if we use multiple decorator on something the order matters because the bottom one would be given to the top one.

for passing argument to decorator

```python
def repeat(num_time):
    def decorator_repeat(func):
        @functools.warps(func):
        def wrapper_repeat(*args, **kwargs):
            for _ in range(numb_times):
                value = func(*args, **kwargs)
            return value
        return wrapper_repeat
    return decorator_repeat
```

we can give attribute to functions , because functions are objects

```python
math.pi.hi = 'an attribute'
```

we can write decorator class based (not many usage)

```python
class DecoratorClass:

    def __init__(self, decorator_argument):
        """
        Instantiation of decorator_object, like decorator_object = DecoratorClass(arg)
        :param arg: Argument of decorator.
        """
        self.decorator_argument = decorator_argument

    def __call__(self, original_function):
        """
        Called when the decorator_object gets called, like decorator_closure = decorator_object(original_function).
        Returns actual decorator_closure ready to be executed.
        @decorator syntax executes this step automatically.
        """

        def wrapper(*args, **kwargs):
            # Adds logic before and after the original_function
            print('Logic before')
            result = original_function(*args, **kwargs) # function execution
            print('Logic after')
            print(f'Decorator_argument = {self.decorator_argument}')
        # Any argument you need is still accessible inside the wrapper

            return result
        return wrapper
```