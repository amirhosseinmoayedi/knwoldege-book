---
title: OS
updated: 2021-09-16 08:08:27Z
created: 2021-09-16 07:37:01Z
latitude: 55.66890000
longitude: 12.58720000
altitude: 0.0000
---

a python library for interacting with OS.

|     |     |
| --- | --- |
| Command | Usage |
| chdir | change directory |
| listdir | list the conntent in directory |
| mkdir | make single directory, not for creating nested directories |
| makedirs | making multiple or nested directories |
| rmdir | same but for removing |
| removedirs | same for removing |
| rename | renaming the file |
| stat | show the detail of file like modified time or size and etc |
| walk | generator that yeild three parameter. |

```python
for dirpath, dirnames, filenames in os.walk('home/'):
    pass
```

|     |     |
| --- | --- |
| environ | a dictionary that contain all environment variables |
| path | not callable it self and have methods it self |