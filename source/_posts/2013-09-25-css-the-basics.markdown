---
layout: post
title: "CSS: The Basics"
date: 2013-09-25 20:27
comments: true
categories: [CSS, Web] 
---


CSS is an acronym for Cascading Style Sheet. Note how this is a style
sheet, and like HTML, it is not a programming language.

<!-- more -->

Note that this is a revision of the 2011 post here:
http://www.blog.baweaver.com/post/8771165277/practical-css-the-basics

## How do I CSS?

There are multiple ways to implement CSS, inline, in the head, and
external. The prior two are highly frowned upon, an external style sheet
is recommended for all tasks.

An external stylesheet can be used on multiple HTML pages at once,
meaning you can completely overhaul your page with only changing one
file. Now that’s some power.

A stylesheet is linked in the head as such:

``` html CSS Include
<link type="text/css" rel="stylesheet" href="style.css"/>
```

## The Basics

A CSS file is completely different than HTML in terms of structure, but
still refers to HTML elements by tag, id, and class. I’ve left comments
above sections to explain what they are:

``` css Stylesheet
/* Style on everything: Font color to black */
* {color:#000000;}
 
/* Style on a tag: Font color to Green */
div { color: #00FF00; }
 
/* Style on an ID: Background to black font to white */
#header { background: #000000; color:#FFFFFF; }

/* Specific tag with ID */
div#header{ background: #000000; color:#FFFFFF; }
 
/* Style on a Class: font to red */
.red { color: #FF0000; }

/* Specific tag with Class */
div.red{ color:#FF0000; }
 
/* Style on a Descendant: font to blue */
div h1 { color:#0000FF; }
 
/* Psuedo elements involve action by a user */
 
/* Style on psuedo-selectors, links: font to yellow */
a:link{ color:#FFFF00; }
a:hover{ color:#FFFF00; }
a:visited{ color:#FFFF00; }
```

## What it Means

CSS selectors can be defined as above, with more specific ones defined
by the w3.

Anything contained in the brackets is applied to elements that they are
describing, but so far all we have are basic color modifications.

At this point I have left out a lot of design elements on purpose to
prevent new learners from jumping ahead too quickly and forgetting the
basics. Design will be covered in later tutorials.

In the next tutorial we’ll cover Font manipulation.
