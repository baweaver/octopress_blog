---
layout: post
title: "HTML: The Basics"
date: 2013-09-25 20:25
comments: true
categories: [Tutorial, HTML, Web]
---

Welcome to my lesson, HTML: The Basics. It is my goal to
explain HTML in the clearest and most concise manner without wasting
your time on complicated jargon and useless tags that have been long
since depricated.

<!-- more -->

It should be noted that this is a redux of my older post, and I have
gone through and updated everything to HTML5 Standards. The original
article was written in 2011, and is availible here:
http://www.blog.baweaver.com/post/8768400510/practical-html-the-basics

HTML should not take anyone more than a few hours to learn, but be well
aware it will take some time to master. In this series we will get to
the point of learning HTML.

## What is HTML?

HTML stands for Hyper Text Markup Language. Notice how is says markup
language, this means that HTML is not a programming language by any
means. What a markup language is is a means to semantically divide
content into logical sections and set forth a structure for a page. The
design and actual look of a page is covered in CSS (Cascading Style
Sheets.)

You can think of HTML as the structure of your building, the solid
pieces and framing on which CSS is used to turn this from just a
structure to a design.

## What Do I Need?

What you’re going to need to get started on your learning is a simple
copy of notepad or a similar program. When I was still using Windows I 
would prefer Notepad++ myself, which can be found here: 
http://notepad-plus-plus.org/

When I started into Unix Platforms, I acquired a taste for Vim, but this
is notably far and overkill for any newbie without some chops in it
already.

Macs have some nice tools as well such as Textmate and Sublime, but it
would be best to avoid relying on the code completion tools and
utilities as much as possible when learning.

## What Do I Need to Avoid?

I do not advocate the usage of such tools as Dreamweaver, Aptana, and
especially Frontpage to any starting learner. These programs develop
severe handicaps that will cripple you on the road to truly mastering
HTML. I know this from experience after relearning several times thanks
to bad habits.

Using a plain notepad like program teaches you to learn your code
properly and to memorize it as well. This is an invaluable strength as
you progress onwards.

Personally I would advocate for Vim, but it is most certainly an acquired
taste.

##Getting Started

The most basic page you can have contains the html tag:

``` html The HTML Tag
<html>Hello, World!</html>
```

## Saving as an HTML File

To save an HTML file, simply add the extension .html to your file after
selecting “Save As > all files” in notepad, or select the option from a
dropdown box in notepad++.

In HTML all tags have an opening and a closing tag, denoting an area
that is marked semantically as a part of it. The HTML tag marks the area
inside of it as HTML. There are exceptions to the closing tag, but these
will be covered later on.

## A Basic Page

Your most basic page will consist of a handful of tags, and it will look
like this:

``` html A Basic Page
<html>
<head>
  <title>My Site's Name</title>
</head>
 
<body>
  <h1>Title</h1>
  
  <p>My text here <a href="http://google.com">Google!</a></p>
  <img src="myimage.jpg" alt="my image" height="100px" width="100px" />
</body>
</html>
```

**&lt;head&gt;**: The head of your site, not to be confused with where a banner or
navigation should go. This is for your sites title, meta information,
scripts, and stylesheets.

**&lt;meta&gt;**: Meta is a tag that contains information about your site, such as
keywords, descriptions, and most importantly your content type.

**&lt;title&gt;**: The title tag contains the title of your site. This will
display in the top bar of your browser, or on the tab your site is
loaded under.

**&lt;body&gt;**: The body tag contains the entirety of your site that is
displayed to the user.

**&lt;h1&gt;**: A header tag. H1 is the largest of 6 tags, H1 to H6. These tags
are to be used not for making larger font, but for sectioning off
headers for news or other such posts on your site. Google keeps a keen
eye trained on H tags to show titles of posts and to get an idea of the
pages structure so be sure to use them well.

**&lt;p&gt;**: The paragraph tag. Every paragraph of your writing is contained in
a separate &lt;p&gt; tag. This contains basic text.

**&lt;a&gt;**: The anchor tag. This creates links. You’ll notice inside the tag
there is href. This is a tag attribute, designating a certain behavior
of the tag. In this case href shows where the text inside the &lt;a&gt; tag
links to.

**&lt;img&gt;**: The astute reader will recognize that this has no closing tag.
That is correct, it is among a group of tags that are self-closing. The
attributes for this tag are:
 
  * **src**: The source of the image.
  * **alt**: Text describing the image
  * **height/width**: The dimensions of the image. Can serve as a
    placeholder for broken images. 

## In Conclusion

Now this makes for a rather small page, but with just this you have a
substantial amount of base in HTML already covered. The rest is all
downhill from here.

In the next section we’ll be covering the &lt;div&gt; and &lt;span&gt; tags and
their usage as well as others to prepare for CSS. Newer versions of the
next tutorial include the HTML5 tags for section, article, aside, and
more.
