---
title: func tools
updated: 2021-07-25 10:10:29Z
created: 2021-07-25 10:04:57Z
author: Amir Hossein Moayedi
---

**Functools** module is for higher-order functions that work on other functions. It provides functions for working with other functions and callable objects to use or extend them without completely rewriting them. This module has two classes – **partial** and **partialmethod**.

some of the library functions:

**Total_ordering**

It is a class decorator that fills in the missing comparison methods (__lt__, __gt__, __eq__, __le__, __ge__). If a class is given which defines one or more comparison methods, “@total_ordering” automatically supplies the rest as per the given definitions. However, the class must define one of __lt__(), __le__(), __gt__(), or __ge__() and additionally, the class should supply an __eq__() method.

```
from functools import total_ordering

@total_ordering
class N:

	def __init__(self, value):
		self.value = value
	def __eq__(self, other):
		return self.value == other.value

	# Reverse the function of
	# '<' operator and accordingly
	# other rich comparison operators
	# due to total_ordering decorator
	def __lt__(self, other):
		return self.value > other.value

print('6 > 2 :', N(6)>N(2))
print('3 < 1 :', N(3)<N(1))
print('2 <= 7 :', N(2)<= N(7))
print('9 >= 10 :', N(9)>= N(10))
print('5 == 5 :', N(5)== N(5))
```

**Wraps**

It is a function decorator which applies update_wrapper() to the decorated function. It is equivalent to partial(update_wrapper, wrapped=wrapped, assigned=assigned, updated=updated).

```
from functools import wraps

def decorator(f):

	@wraps(f)
	def decorated(*args, **kwargs):
		"""Decorator's docstring"""
		return f(*args, **kwargs)

	print('Documentation of decorated :', decorated.__doc__)
	return decorated

@decorator
def f(x):

	"""f's Docstring"""
	return x

print('f name :', f.__name__)
print('Documentation of f :', f.__doc__)
```

**SingleDispatch**

It is a function decorator. It transforms a function into a generic function so that it can have different behaviors depending upon the type of its first argument. It is used for function overloading, the overloaded implementations are registered using the register() attribute of the

```
from functools import singledispatch

@singledispatch
def fun(s):

	print(s)

@fun.register(int)
def _(s):

	print(s * 2)

fun('GeeksforGeeks')
fun(10)
```