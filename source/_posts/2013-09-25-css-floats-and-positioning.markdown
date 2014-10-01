---
layout: post
title: "CSS: Floats and Positioning"
date: 2013-09-25 20:30
comments: true
categories: [Tutorial, CSS, Web] 
---

Now that we’ve covered  a majority of the basics we can finally get to
what makes a site layout come together, floats and positioning.

<!-- more -->

Note that this article is a revision of the 2011 post here:
http://www.blog.baweaver.com/post/8807627420/practical-css-floats-and-positioning

I would seriously consider looking into a Grid system after you
understand these concepts, as that will be a later post.

## Floats

Floats are a CSS property that allows an element to be taken out of
normal flow and be moved to a side instead. If two floats are next to
eachother and do not overflow into eachother they will appear beside
eachother. At the end of a floated segment the float needs to be cleared
as well to prevent elements like a footer from playing to the same
rules. Here’s an example of the CSS:

``` css Floats
#container{ width:1000px; }

#mainCol{ float: left; width: 700px; padding 10px; margin:15px; }

#sideCol{ float: right; width: 200px; padding:10px; margin:15px; }

.clear{ float: clear; }
```

Traditionally the clear class will be put on either an empty div or a
self closing &lt;br /&gt; tag.

I swear on the fact that you will be caught by the box model rule while
floating a layout more times than you will care to admit. When designing
a page, we need to take the width of the container and the real width of
all floated elements inside, otherwise you’re going to get an
interesting effect known as wrapping.

I cannot say this enough: measure and measure again. You’ll save hours
of headaches by doing the simple things right. Looking at the above
example we see that we have a total width of 1000.

1000  == (700 + 10 x 2 + 15 x 2) + (200 + 10 x 2 + 15 x 2)

Now why am I multiplying the 10 and 15 by 2? Remember that padding and
margin when set with one number apply to all sides, meaning left and
right. Another thing easily missed.

## Positioning
Positioning comes in three different types: absolute, relative, and
fixed. There is regrettably no real shorthand to setting coordinates for
these types.

### Absolute

Absolute positioning is an absolute location on the page specified by
coordinates:

``` css Absolute
#absolute{
  position: absolute;
  top: 25px;
  left: 15px;
}
```

### Relative

Relative positioning takes an element out of normal flow and moves it x
and y units from its original location:

``` css Relative
#relative{
  position: relative;
  top: -25px;
  right: 15px;
}
```

### Fixed

Fixed positioning fixes an element on the page and the element will stay
in that location even as the page scrolls down, changing dynamically:

``` css Fixed
#fixed{
  position: fixed;
  bottom: 50px;
  left: 100px;
}
```

## Tip
When using positioning, a nice little feature that’s been added in for
us is absolute positioning relative to the parent container. This means
that absolute positioning refers to the space within its containing
element. Nice eh?

To achieve this we simply have to tag position:relative; in the parent
element, nothing else. Just put a position: absolute; in the child and
it will be within the parameters of the parent container.

## Conclusion

That’s it for the basics of HTML and CSS, with this you should be able
to make basic site frames and have a basic site up.

Some will notice that you have no idea how to really work with this
structure and turn it into an amazing design. Fret not, because the next
tutorial series will focus on Web Design theory and where to go from
here!
