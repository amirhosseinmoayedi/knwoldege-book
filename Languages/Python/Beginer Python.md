---
title: Beginer Python
updated: 2021-03-04 08:24:57Z
created: 2021-03-04 08:04:51Z
author: Amir Hossein Moayedi
---

**statements** : every command and variable and ...
**expression**: mix of value and variable and operators.
**indention**:4 space is common but can use any number of space

- ![78.jpg](../../../_resources/78.jpg)
- we got 3 type of function in python :1- **builtin** 2- **library** 3-** one that we made**.
- iteration:do something while things are in condition
- **break** can be used for 'for loop' too.
- letters are compared in their **ASCII** values:
    - "b" > "a" True "b" < "a" False
    - "a" > "A" True "a" < "A" False
- **Methode** is function that do something on instance or class.
- can use help in python console:
    - help(library.methodes) example :

`help(str.count)`

- this is old string formatting:

```
print('hello %s' %name)
print("salam %s khobi %i salet shode" %(name, sen))
```

- dir() for seeing hole class methodes
- list 1 + list 2 just return the sum of list's but list1.extend([list2]) saves the list 2 + 1 in list 1.
- del l1[2] for removing *third item*. l1.remove(2) for *removing value 2* from list.
- list.pop for showing last item then removing it.
- "delimiter".join(list/tuple name) for joining list members.
- == compare **values** is compare id number.list got id number **independent of their inner members**
- in  in the dictionary just check the **keys** not values.
- dict.get(key, the value if key not in dict) : used for not running into error when checking dictionaries
- **library** are some *program that written by someone else* and made them into a package.
- 1-standard library are library that are python defaults are do exist in it.(like float,str, and ...)(python core)
- 2-built in library exist in python and do not need importing.
- 3- libraries that can be imported

**pip** is python package manager for installing new module.
**ipython** is an extra python library with got it self interpreter.
"{0:.4f}".format(x) for showing number up to 4 decimal.

**handle**  is name that we give to a variable that it's used to  file to open , close , write and ...

\t is escape char for tab
**csv** (comma seperated value)
assert is a condition that stop program example: assert age > 12, "no no no "
`assert False, 'this will be printed always and give assertion error'`