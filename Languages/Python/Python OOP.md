---
title: Python OOP
updated: 2021-05-27 11:15:43Z
created: 2021-05-14 15:55:59Z
author: Amir Hossein Moayedi
---

Review
the complex data type is for coordination

        - i or j

`a = 2 + 3j`

`'sample_name'.isidentifier `
for checking if the name is valid for the python variable name or not.

like += we can use all other operations in this compact form.

```
x = 7
x //= 2
```

the operation of conversion data type is called casting.

method bin() for converting to binary.

bitwise operators

| Operator | Example | Meaning |
| --- | --- | --- |
| &   | a & b | Bitwise AND |
| \|  | a \| b | Bitwise OR |
| ^   | a ^ b | Bitwise XOR (exclusive OR) |
| ~   | ~a  | Bitwise NOT |
| <<  | a << n | Bitwise left shift |
| >>  | a >> n | Bitwise right shift |

```
>>> 128 * 2

256
>>> 128 << 1
256
```

conditional expression :
`m = a if a < b else b `

string formatting:

```
d = 2.3456767
a = '{: .2f}'.format(d)
```

f for float point
d for digit

slicing

```
a = [1,2,3,4,5,6]
print(a[::2])  # 2 is the step : 1 , 3 ,5
```

for transforming two list into dictionary as key value

```
a = [a, b]
b = [1, 2]
c = zip(a, b)
c = dict(c)
```

  sets in python got union and difference and intersection option

```
def example():
    ...
    return a, b
c , d = example()
```

in the example below b and c must be called for assigning value

```
def example(a, *, b, c):
    ...
example(1,b=2,c=3)
```

* * *

OOP
note: everything in python is class

|     |     |
| --- | --- |
| method | description |
| __dict__ |   method for showing object value in dictionary format. |
| __doc__ | method for showing class docstring |
| del | for deleting an object from class or deleting a property from an object |
| __init__ | object constructor in the class |

note: __init__ is standard  for creating a class object but we can define our method for assigning values

function isinstance for checking if an object is part of this class or not.

```
c1 = Cicle(3)
c1.area()
print(isinstance(c1, Cicle)) #True
```

note: for referring to class attribute we use cls normally but we can use self too.

variable in class are three different types(both for class and instance variables and even class methods):

    1. public
    2. protected
    3. private

note: there is no mechanism in python that fully prevent modification of values in python

```
a = a # public
_a = a # protected
__a = a # private
```

name mangling for calling a private variable :
`obj._calssname__a `

*there is no need for calling variable in the above form when we call it inside the class methods.*

magic methods (dunder methods):

|     |     |
| --- | --- |
| method | description |
| __str__ | return something as a string (for showing to the user) |
| __repr__ | like __str__  (for developing) |
| __len__ | for getting an object length |
| __getitem__ | for getting an item if the variable instance is list or dictionary |
| __setitem__ | for setting a value into a list or dictionary |
| __call__ | for adding call option to object(really not necessary)<br>we can define so it would do what we want when calling it like changing the instance attribute |
| __add__ | for defining the ability to sum this object with another thing (object attributes) |
| __gt__ | greater than  (comparison for objects) |
| __lt__ | lower than  (comparison for objects) |
| __eq__ | equal  (comparison for objects) |
| __set__ | for setting instance or class variable we don't need to call method to work |
| __get__ | for receiving a variable from the class of object |

```
def __getitem__(self, i):
    return self.lst[i]
def __setitem__(self, i, v):
    return self.lst[i] = v
def __add__(self, o):
    return self.a + o.a
def __set__(self, v):
    return self.v = v
```

note: all of the above methods must be written manually

note: for using comparison signs between  objects we need __gt__ and etc special methods

data descriptors :
setter and getter

```

# getter

@prperty
def example(self):
    return ...

# setter

@example.setter
def example(self,  v):
    self.v = v
```

inheritance:

```
class childe(parent):
    pass
```

for inheriting parent constructor:

```
def __init__(self, ...):
    super().__init__(...)
    or
    Parentclass.__init__(self, ...)
```

MRO: method resolution order (the order of inheritance)
`print(class.__mro__)`

multiple inheritance:

```
class grandchildren(‌‌c1, c3):
   B1.__init__(self, ...)
   B2.__init__(self, ...)

```

multi-level inheritance:

```
class A:
    ...
class B(A):
    ...
class C(B):
    def __init__(self, ...):
    super().__init__(...)
```

Dimond problem:

The "**diamond problem**" (sometimes referred to as the "Deadly Diamond of Death) is an ambiguity that arises when two classes B and C inherit from A, and class D inherits from both B and C. If there is a method in A that B and C have overridden, and D does not override it, then which version of the method does D inherit: that of B, or that of C?

***in python the order of inheritance in multiple inheritances is important.***
***
***

note: we can access protected instances from child class but not the private one (we can but it*** needs*** name mangling)

**calling the private method or instance doesn't need name mangling inside the same class.**

**
**
**decorator: **
**functions in python are also object**

**a function that receives a function as input and the output is also a function **

**
**

```
def dec(func):
    def wrapper():
        print('before')
        func()
        print('after')

    return wrapper

# for calling it

def f():
    print('hello')
a = dec(f)
a()

# or

@dec
def f():
    print('hello')
f()
```

instance method:
the normal method in classes
for accessing them we use objects
Class Method:
can't access self and can be called with or without an object
and it's class dependable
*the use case is for creating objects of the same class *

```
class ...:
    @classmethod
    def g(cls, a):
        pass
```

Static method:
the method in a class that can be called without an object
*self doesn't mean anything in static methods *

```
class ...:
    @staticmethod
    def func_sum(m, n):
        print(m + n)

A.func_sum(3 ,2)

# or

a = A(1)
a.func_sum(3, 2)
```

note: the difference between static and class method is that the class method can access class instances

@property for making a function that returns something called getter
@func.setter for changing it
@func.delleter for deleting inside

```
class Person:
    def __init__(self, name, family):
        self.name = name
        self.family = family
    @property
    def f(self)
        return name +" "+ family
    @f.setter
    def f(self, s):
        self.name = s
    @f.deleter
    def f(self):
        del self.name
```

Note:  instead of decorators we can apply them on functions like this

```
class C:
    def f(k):
        return ...
    f = classmethod(f)
```

functools is a python library for working with decorators

abstract method:
create an empty method for children to fill it

```
from abc import ABC, abstractmethod

class B(ABC)):
    def __init__(self, a):
    super().__init__()
    self.a = a

    @abstractmethod
    def f(self):
        pass

class D(B):
    def f(self):
        return self.a + a
```