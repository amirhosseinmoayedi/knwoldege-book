---
title: Pickle
updated: 2021-08-19 16:09:16Z
created: 2021-08-19 16:03:42Z
author: Amir Hossein Moayedi
---

Pickle in Python is primarily used in [serializing and deserializing a Python object structure](https://wiki.python.org/moin/UsingPickle).  In other words, it’s the process of converting a Python object into a  byte stream to store it in a file/database, maintain program state across sessions or transport data over the network. The pickled byte stream can be used to re-create the original object hierarchy by unpickling the stream. This whole process is similar to object serialization in Java or .Net.

When a byte stream is unpickled, the pickle module creates an instance of the original object first and then populates the instance with the correct data. To achieve this, the byte stream contains only the data specific to the original object instance. But having just the data alone may not be sufficient. To successfully unpickle the object,  the pickled byte stream contains instructions to the unpickler to reconstruct the original object structure along with instruction operands, which help in populating the object structure.

According to the pickle module documentation, the following types can be pickled:

- None, true, and false
- Integers, long integers, floating-point numbers, complex numbers
- Normal and Unicode strings
- Tuples, lists, sets, and dictionaries containing only picklable objects
- Functions defined at the top level of a module
- Built-in functions defined at the top level of a module
- Classes that are defined at the top level of a module

Pickle allows different objects to declare how they should be pickled using the __reduce__ method. Whenever an object is pickled, the  __reduce__ method defined by it gets called. This method returns either a  string, which may represent the name of a Python global, or a tuple describing how to reconstruct this object when unpickling.

Generally, the tuple consists of two arguments:

- A callable (which in most cases would be the name of the class to call)
- Arguments to be passed to the above callable

The pickle library will pickle each component of the tuple separately and will call the callable on the provided arguments to construct the new object during the process of unpickling.