---
title: Advance Python
updated: 2021-03-04 09:04:44Z
created: 2021-03-04 08:22:21Z
author: Amir Hossein Moayedi
---

**polymorphism** : think it as a function that take d*iffrent data type*, built in examples:

```
len(list)
len("ali")
print("ali", 2)
```

**encapsulation** in **OOP** is for preventing class var from used *outside*:
we  do it with:
`__var = value  # private , it better to use _var said by python guid it self.`
instead of :
`var = value  # public`
.title() methods of number are the same as their normal.

**lambda function**: they are anonymous and can be used once

```
lamda a, b: a + b
lamda x: x * 2
```

note: *we can use compressed form of if and for in lambda.*
`lamda x: 'big' if x > 10 else "small"`

- usage example :

`dict.sort(key= lambda x: x[1])`

map (function , iterator) :a function that apply on each part of a list.

note**: it return map not a list** but *we can give to a another function that reads list and still do the same* but for **print** we should convert it to list first ,we can give it a predifiend function or a lambda.

filter (function,  iterator): a function that filter a list component base on expression.

`filter(lamda x: x % 2 == 0, list)`
note:**classic functions have memory and time problem **
**
**

**generators** : like classic function but uses yield and first time you call it it would return the first yeild and remember what next to return. (yeild is something like return)

for calling them we use them in iterator(like for or while):
`for i in firstn():`

note:the real difference is that **generators***unlike the classic function do not calculate and store every thing in memory or wait for function to end to show the result*.

**functional programming** : a language that every thing is a function in it.
every thing in python is a **class**.
note: we use procedural programming normally.

in class we got **two type of variable** one that is for everyone **class variable** and** instance attribute**.

classes got constructor method that are ran when we give a object to a class  to construct an instance of class.

note: in method we got self that **represent any object in class.** we can use any name instead of self too.

**class inheritance**: one class got every method and variable of another class and got something extra(like a method) too.

we can give a object of class extra variable : object.variable(that dosn't exist in class) = *

in class we got __init__ as object **constructor** to create an object we can also **delete** a object with  del object_name

for calling a class variable:  vlass.variable =  something

i*n inheritance if a child class got the same method name as the parent first the child method will run and then the parent.*

*choosing library is important.*
*
*
**web scraping** is proccess of gathering data from web and analysing it.
request library is for **requesting** data from web.

- uses HTTP method.
- bring HTTP response
- also get html code and ...
- there are free API's that we can use for our program
- **JSON** is very similar like dictionary in python.
- string default is UTF-8

**virtual envioment**: *is directory that contain all library ,python and pip  we need for our program to run independent of OS library python and pip version* .

using virt env: python3(or 2) -m directory_name_to_make  name_of_environment
for activing it :source  virtualenv_directory/bin/activate
deactive : deactivate
pip freez > file : for saving installed packages with version in file
pip install -r file : to install requierd packages from file

**machine learning**
giving machine a lot of info with result for it to lean via many method
scikit-learn is an ml library