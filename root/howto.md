---
layout: default
title: HowTo
---

- [Jekyll Step by Step](https://jekyllrb.com/docs/step-by-step/01-setup/)
- {{ page.title }}
- {{ page.url }}

# How to make site
- install and make site
  - gem install bundler jekyll
  - jekyll new my-awesome-site
  - cd my-awesome-site
- change site
  - mkdir myawesome-site/root
  - cd myawesome-site/root
  - vi index.html

```
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Home</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

- build and run
  - jekyll build
    - _site directory has all contents from build.
  - jekyll serve
- run browser
  - http://localhost:4000
    - you can see the result!!!

# syntax
## Variable
- basically all variables exist in page. so we can get variable as ```page.variable```.
```
{ { page.title } }
```

## Tags
- Tags create the logic and control flow for templates. They are denoted by curly braces and percent signs: { % ...  % }. For example:
- Outputs the sidebar if page.show_sidebar is true. You can learn more about the tags available to Jekyll here.

```
{ % if page.show_sidebar % }
  <div class="sidebar">
    sidebar content
  </div>
{ % endif % }
```

## Filters
- Filters change the output of a Liquid object. They are used within an output and are separated by a |
```
{ { "hi" | capitalize } }
{ { "Hello World!" | downcase } }
```

## Front Matter
- Front matter is a snippet of YAML which sits between two triple-dashed lines at the top of a file. 
- jekyll build  compiles with the following syntax.

```
---
my_number: 5
---
{ { page.my_number } }
```

## Layouts
- Using a layout is a much better choice. Layouts are templates that wrap around your content. They live in a directory called _layouts.
- Create your first layout at _layouts/default.html with the following content:

```
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{ { page.title } }</title>
  </head>
  <body>
    { { content } }
  </body>
</html>
```

## Markdown
- Create about.md in the root.
- after running jekyll build , you can find web in http://localhost:4000/root/about  .
  - about.md ->  about.html
- when file has ```layout: default``` ,  they use _layout/default.html .
  - The following contents is in ```{ { content } }``` .

```
---
layout: default
title: About
---
<h1>{ { "Hello World!" | downcase } }</h1>
```

## include
- The include tag allows you to include content from another file stored in an _includes folder. 
- Includes are useful for having a single source for source code that repeats around the site or for improving the readability.
- _includes/navigation.html

```
<nav>
  <a href="/">Home</a>
  <a href="/about.html">About</a>
</nav>
```


- _layout/default.html

```
<body>
  { % include navigation.html % }
  { { content  { }
</body>
```

### Current page highlightingPermalink
- _includes/navigation.html needs to know the URL of the page it’s inserted into so it can add styling. Jekyll has useful variables available one of which is page.url

```
<nav>
  <a href="/" { % if page.url == "/" % } style="color: red;"{ % endif % }>
    Home
  </a>
  <a href="/about.html" { % if page.url == "/about.html" % } style="color: red;"{ % endif % }>
    About
  </a>
</nav>
```

## Data Files
- Jekyll supports loading data from YAML, JSON, and CSV files located in a _data directory.
- Create a data file for the navigation at _data/navigation.yml with the following:

```
- name: Charles
  link: /root
- name: RootAbout
  link: /root/about.html
```


- Jekyll makes this data file available to you at site.data.navigation. 
- Instead of outputting each link in _includes/navigation.html, now you can iterate over the data file instead:

```
<nav>
  { % for item in site.data.navigation % }
    <a href="{ { item.link } }" { % if page.url == item.link % }style="color: red;"{ % endif % }>
      { { item.name } }
    </a>
  { % endfor % }
</nav>
```


## Assets
- Jekyll sites often use this structure to keep assets organized:

```
.
├── assets
|   ├── css
|   ├── images
|   └── js
...
```


### Sass
- Sass is a fantastic extension to CSS baked right into Jekyll.
- First create a Sass file at /assets/css/styles.scss with the following content:

```
---
---
@import "main";
```


- The empty front matter at the top tells Jekyll it needs to process the file. The @import "main" tells Sass to look for a file called main.scss in the sass directory (_sass/ by default).
- Create _sass/main.scss with the following content:

```
.current {
  color: green;
}
```


- Open _layouts/default.html and add the stylesheet to the <head>:

```
<link rel="stylesheet" href="/assets/css/styles.css">
```


# Blogging (Posts)
## Posts
- Blog posts live in a folder called _posts. The filename for posts have a special format: the publish date, then a title, followed by an extension.
- Create your first post at ```_posts/2018-08-20-bananas.md``` with the following content:

```
---
layout: post
author: jill
---
A banana is an edible fruit – botanically a berry – produced by several kinds
of large herbaceous flowering plants in the genus Musa.

In some countries, bananas used for cooking may be called "plantains",
distinguishing them from dessert bananas. The fruit is variable in size, color,
and firmness, but is usually elongated and curved, with soft flesh rich in
starch covered with a rind, which may be green, yellow, red, purple, or brown
when ripe.
```


## Layout
- The post layout doesn’t exist so you’ll need to create it at ```_layouts/post.html``` with the following content:

```
---
layout: default
---
<h1>{ { page.title } }</h1>
<p>{ { page.date | date_to_string } } - { { page.author } }</p>

{ { content } }
```



## List Posts
- Jekyll makes posts available at site.posts.
- Create blog.html in your root (/blog.html) with the following content:

```
---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul>
  { % for post in site.posts % }
    <li>
      <h2><a href="{ { post.url } }">{ { post.title } }</a></h2>
      <p>{ { post.excerpt } }</p>
    </li>
  { % endfor % }
</ul>
```

- code
  - ```post.url``` is automatically set by Jekyll to the output path of the post
  - ```post.title``` is pulled from the post filename and can be overridden by setting title in front matter
  - ```post.excerpt``` is the first paragraph of content by default


- add Blog into _data/navigation.yml

```
- name: Blog
  link: /blog.html
```


### More posts
- make md files

