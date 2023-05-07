---
title: Test Types
updated: 2021-05-28 13:47:46Z
created: 2021-05-24 18:05:59Z
author: Amir Hossein Moayedi
---

list of some type of test in python :

- doc test
- unit test
    - nose
    - nose 2
    - py test
- integrated test

note: nose, nose 2, py test are frameworks for testing.

doc test: is the simplest way to test method or function in python, but they will make the code hard to read

is part of python library

```
def add(a, b):
    """
    >>> add(2, 3)
    5
    >>> add(-3, 4)
    1
    """
    return a + b
```

assert: assert is for testing the returned value of method or functions

```
def add(a,b):
    return a + b

assert add(2, 4) == 6, 'it will return true if condition meet else false and raise assertion error'

```

unit test: is part of Python standard library that uses assert, it got many functions and will be written in another file

the name of unit test functions  and file must start with test, and for running it we need __name__ == '__main__':

```
import unittest

class TestStringMethods(unittest.TestCase):

    def test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

    def test_isupper(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    def test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

if __name__ == '__main__':
    unittest.main()
```

[list of unittest](https://docs.python.org/3/library/unittest.html)
***we can also run unittest this way ***:
`python -m unittest unit_test_file`
line below can find the all the unit test and run it
`python -m unittest discover`

Fixture(Something securely fixed in place): a method for not repeating code that will be run after each test

|     |     |
| --- | --- |
| special method name | description |
| setup | A method called to prepare the test fixture. This is called immediately before calling the test method |
| tearDown | A method called immediately after the test method has been called and the result recorded |

Nose(not important): a framework that needs to be installed, it also works with python default unit test

to run:
`nosetest`

pytest:  a framework for testing that's also compatible with unit tests.
fixtures in pytest:

we create a function and add a decorator to it and give the created function to all methods.

```
from pytest import fixture

class PYTest:
    @fixture
    def setup(self):
        self.x = 3
        self.y = 2

    def test_add(self, setup):
        assert add(3, 4) == 7
        assert add(-3, -4) == -7
        assert add(2, 0) == 2
```

for assertion

```
with pytest.raisers(exception):
    function
```

coverage:    a tool to see what to test

```
coverage run file_name
coverage  report  # to see the result of run
coverage report --show-missing # to see missing code to test
coverage HTML # export report as HTML
```

***we can combine coverage with pytest ***

```
pytest --cov=name_fo_file
pytest --cov=name_of_file --cov-report=html # to see the result in HTML format
```