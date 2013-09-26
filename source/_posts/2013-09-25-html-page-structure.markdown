---
layout: post
title: "HTML: Page Structure"
date: 2013-09-25 20:26
comments: true
categories: [HTML, Web]
---

Now that we’ve covered the basics of HTML it’s time to start getting to
some real implementation.

<!-- more -->

Note that this is an updated version of the 2011 post:
http://www.blog.baweaver.com/post/8769994134/practical-html-page-structure

## So What is HTML for? Really?

HTML is meant to be a skeleton on which CSS is laid, marking up areas of
text and content so that it can be cleanly divided into sections. This
makes HTML an extremely modular language in conjunction with CSS.

I will warn you, always start with your content first. Never start
coding CSS until your HTML skeleton is finished and ready. If your site
cannot stand on its own in a logical order as just HTML then you have
not done a proper job in arranging the elements.

## What do I use to achieve this?

There are four main tags that are used to section off content in HTML,
the *div* tag and the *span* tag for content, and the *ol* and *ul* tags
for lists. 

With HTML5 we introduced many more tags that are more correct for
content, and should be preferred. *section*, *aside*, *header*,
*footer*, *nav* and *article* are far more semantically sound than using
multiple *div* tags as was the way with older HTML versions

So what’s the difference?

### Div Tags

A div tag is what is referred to as a block element. A block element is
rendered to line-break after a tag is closed. This means elements in div
tags will logically stack on top of eachother as if they were blocks.

### Span Tags

A span tag is an element who displays inline. This means that it will
render elements one after another until it reaches the end of a page,
hence being in line.

### Header

The Header of your site, for things such as your banner, and your
navigation

### Nav

The Navigation of your site, typically a list will be the contents, and
we will cover those in a few seconds.

### Footer

The Footer of your site, best for lonks, contact information,
and copywrites.

### Section

Literally a section of text, such as your articles section of your
site.

### Aside

Used for content that is partially related to the matter at hand, such
as a side quote for an article.

### Article

A syndicated section of text such as a Blog Post.

### Lists

Elements that are naturally ordered belong in an ordered list element,
with each list item being denoted by an *li* tag as such:

``` html Ordered List
<ol>
  <li>Item</li>
  <li>Item</li>
  <li>Item</li>
</ol>
```

Elements not naturally ordered would go into an Unordered List:

``` html Unordered List
<ul>
  <li>Item</li>
  <li>Item</li>
  <li>Item</li>
</ul>
```

#### Why Lists?

Lists are extremely helpful for things such as menus, especially when
you use an *a* tag inside them. With some styling you can get them to
display inline, remove the bullet points, and move them all about.

## A Basic Layout

A basic layout using these involves two more attribute that is directly
tied to CSS: id and class. An id is a unique marker whereas a class can
be used multiple times. These are later used as hooks for CSS styling.

An id is used to define areas that only occur once, such as a main menu,
a header, a column, or otherwise. A class is used to define an item that
occurs multiple times, like an image you want on a certain side of your
text.

Here’s a basic example of a layout using these tags:

``` html A Basic Page
<html>

<head>
  <title>My Site's Name</title>
</head>
 
<body>
 
<header>
 
<h1>Header text</h1>
 
<nav>
  <ul>
    <li><a href="#">menu item</a></li>
    <li><a href="#">menu item</a></li>
    <li><a href="#">menu item</a></li>
  </ul>
</nav>
 
</header>
 
<section id="mainContent">
  
  <article>
    <h2>Title</h2>
 
    <p>Text</p>
  </article>

</section>
 
<section id="sideContent">
 
  <article>
    <h3>Other Title</h3>
   
    <p>Text</p>
  </article>
 
</section>
 
<footer>
  <p>Copywrite info and other such</p> 
</footer>
 
</body>

</html>
```

## What to do from here

We have ourselves a basic skeleton made in HTML using what we know so
far, the next step along the road is to start learning css to make this
into a basic site.
