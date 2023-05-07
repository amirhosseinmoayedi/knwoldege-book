---
title: Selenium
updated: 2021-03-05 10:46:29Z
created: 2021-03-05 10:35:37Z
author: Amir Hossein Moayedi
---

some websites are dynamics and need interaction for accessing data on web so we cant get data with just a simple get request, we need ***selenium*** .

it used for website **testing** as well.

some cases of use in web scraping: **infinite scroll**, **next page button** , and ...

SELENIUM is a set of tools and libraries.it's open source got  4 part:
         IDE: an setup and playback kinda tools.(it's a addon)
         RC:a tool for creating web ui test

         Web Driver : an API to connect  to a browser and it's like RC but more addvanced

         Grid : allow to run test on diffrent machine in same time
Json : **JavaScript Object Notation**

![109.jpg](../../../_resources/109.jpg)
element's :***every thing in a web page such as link, text and ...***
html elements are also called ***nodes***.

dynamic elements: element's that id, name or their attribute are not fixed and will be changed base on session and database changes.

element locating methods in selenium :

**by name , by id (best) , link text , by partial link text(regex) , css selecto**

**r ,tag name , class name, xpath(language for locating nodes in xml documnet), find element ,find elements**

![111.jpg](../../../_resources/111.jpg)

because each command of selenium is going throw ***json*** via ***http*** it take a wile and better to extract data with **beautiful soup**.

we got two methods of waiting in selenium : implicit and explicit.
first one is by time and second one are by condition.
before scraping from a web read their robots.txt if they allow scraping or not
it's best to load page without images with chrome or firefox options
use disk chache for browser it speed it up.