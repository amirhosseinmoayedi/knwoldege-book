---
title: Clean Code & Refactoring
updated: 2021-10-11 10:25:33Z
created: 2021-10-03 17:38:00Z
latitude: 35.72700000
longitude: 51.33360000
altitude: 0.0000
---

Clean Code:

- well tested
- well documented
- simple

clean code is less but not complex, less code mean less error

Zen of python :

```python
import this
```

PEP: python enhancement proposal

Dry 3 rule:

1.  if one implemented
2.  if two it may be slightly different
3.  if three code should be improved

Refactoring rules:

if the code is hard to understand you should put some comment if it did't work you should refactor it

if refactor fix the bug you should do it , and try to create test for it for insurance

linters: tools that check the style of your code

anti patterns: are pythonic code that are no good

for removing the anti patterns:

- separating specific from general
- keep it simple
- moving feature around
- splitting and merging functions and classes
- relocate where the data is stored

splitting the functions and methods have this benefits:

1.  easier to document
2.  easier to understand
3.  easier to test

it's not always good to split the functions or classes, some time it's a benefit to have it together

we can make things simple by just putting them in variables like conditions.

for small classes we can use named tuple which is count as micro classes

try you functions or methods to not call functions or methods from other classes

Design patterns provide two:

1.  template for oop
2.  Refactoring

Its always good to have design pattern in mind to have better and cleaner code

Sometimes we can split conditioning that have a certain logic into classes that inherit from abstract one

We can do remove conditioning occasionally with null object pattern