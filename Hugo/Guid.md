#hugo 
<h2>Files and folders in Hugo</h2>
- <font color="#ffff00"> Config.toml</font> is for website configs.
- <font color="#ffff00">themes</font> are just for putting the theme in it and no more.
- <font color="#ffff00">static</font> are for css  and anything not a web page.
- <font color="#ffff00">content</font> is for markdown file which will be used for website rendering.
- <font color="#ffff00">public</font> is where generated site will be.
- <font color="#ffff00">archetypes</font> is where default md **template** exists.

after using **<font color="#ffc000">theme</font>** we should mention it in `config.toml` file.

You can manually create **<font color="#4bacc6">content files</font>** (for example as `content/<CATEGORY>/<FILE>.<FORMAT>`) and provide metadata in them, however you can use the `new` command to do a few things for you (like add title and date):
```bash
hugo new posts/my-first-post.md
```

Meta Data Example:
```text
---
title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true
---
```
<font color="#ffc000">Note</font>: **Drafts do not get deployed**

site config is the `config.toml` that located in root of website.
Example: 
```toml
baseURL = "https://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "ananke"
```

<font color="#ffc000">Note</font>: Output of static website generation (`hugo -D`)  will be in `./public/` directory by default (`-d`/`--destination` flag to change it, or set `publishdir` in the config file).

we can have *subdirectory* and it will be shown like a category for manging our **content** directory.

for **<font color="#92d050"> linking contents</font>**( Pages ) togheter it's like linking in obsidan.
``` Markdown
See the [Linked Page](/path_in_content_directory)
```

for linking **<font color="#ffc000">images</font>** we first put them in <font color="#00b0f0">static</font> directory, and them we link to it like other contents and we give path to it like it is located in content directory because hugo will move items from static directory to content directory after compile.

There are two types of <font color="#00b0f0">pages</font> in hugo:
1. <font color="#ffff00">Singles</font>
2. <font color="#ffff00">List</font> : list of links to singles, but it can have content like the singles.

in `layouts/_default` we have  the **default layout** for the pages we make.

in `layouts/partials` we have the part of pages that will be **reused** like  the footer for our website.

hugo has **variables** that each mean something and they are used in layouts, like `Content` mean the content we create in our `.md` files and where it should be.

if we have `index.html` on our layout directory it will <font color="#00b050">override</font> the **main page** of the theme. 

**partials** have scriptings inside the template, there is somthing called **shortcodes** which are like partials but in files.