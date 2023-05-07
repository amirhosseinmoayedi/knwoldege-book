---
title: Beautiful Soup
updated: 2021-03-05 10:58:08Z
created: 2021-03-05 10:47:06Z
author: Amir Hossein Moayedi
---

**beautiful soup is an library for extracting data from xml and html.**
output is default UTF-8
the are 3 different html parser :
1)python default
2) lxml
3) html5 lib
installation :
`pip install beautiful soup4`

`pip install lxml `
for html parsing (it works even with broken html code)
`pip install request`
request is library for getting data from web like sending HTTP get request.

`from bs4 import BeautifulSoup`

`var = BeautifulSoup(what we want to scrape, parser)  # parser in this case is lxml`

`var.find(tag to search)  # it searches for first one`

`var.find_all(tag to search )  #for all`
note : we can add more specific tag item for more filtering

note: for telling python about html tags to search we should it's better to add ' _ ' after tag name like:

`var.find_all(div, class_= "something" )`
for seeing the result we can use iteration(for).

```
for course in course_cards:
         course.h5.text  # for accessing text in h5 tag
```

for getting link :var is that part we already extracted in this case
`var.a[href]`
----------------------------------------------------------------------
for finding element in web use browser inspector
request.get()  getting data from web normally response
request.get().text   for just getting web code content

like : .span for getting span inside the the tag we searched.
note: .tag also works in beautiful soup and not just request.