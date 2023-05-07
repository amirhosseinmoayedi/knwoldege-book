---
title: regex
updated: 2021-03-06 10:33:35Z
created: 2021-03-04 07:41:12Z
author: Amir Hossein Moayedi
---

regular expression:
regex are different in every language.

|     |     |
| --- | --- |
| [ ] | search everything that is like between this two |
| { } | search this number of stuff |
| ^   | start of line |
| [^ ] |  for negate |
| $   | end of line |
| \d  | digit |
| \w  | word |
| \s  | space |
| \t  | tab-space and ... |
| \n  | new line |
| .   | everything in one letter |
| "+"  double quote is not needed | at least one of before char |
| *   | every even nothing |
| ?   | lazy sign for regex to find less |
| ( ) | grouping |

examples:

```
r"\d {3}" : 3 digit
r"[^abc]": shoudln't have a or b or c
```

notes:
using it need re module.

```
re.search(regex, string)
re.findall(regex, string)
re.sub(regex, sub, string)  # would replace a substring of string
```