---
layout: post
title: "CSS: Box Model"
date: 2013-09-25 20:29
comments: true
categories: [CSS, Web] 
---

The Box model, an odd term isn’t it? It is used to describe the
combination of various elements that control the spacing of elements on
your page.

<!-- more -->

Note that this is a revision of the 2011 post found here:
http://www.blog.baweaver.com/post/8788929119/practical-css-the-box-model

## What comprises the Box Model?

The Box Model is made up of CSS that includes width, height, padding,
borders, and margin.

Think of a box, you have your content inside of it. Right outside your
content is padding to keep it away from the edge of the package, or
rather the border in this case. The space surrounding the box that isn’t
occupied by other boxes is the margin. 

The largest gotcha that I have ever seen are people setting the width of
an element and the Box Model properties of it, and wondering why it’s
too wide or causes breaks in elements. This is because the true width of
an element is calculated by adding: width + border + margin. This is a
crucial point that will save you loads of headaches later on.

## Padding vs Margin

So why would you use padding instead of margin or vice versa?
Padding is still inside of the box, meaning that it carries with it
background colors that can be on the box.

Margin, as said before, is the space outside of the box. The border will
render between these two.

## Height and Width

Height and width are not required elements but are extremely helpful in
layout out a framework for your site. A key point to remember is that
there are also min-height and min-width attributes that can fix certain
glitches in internet explorer.

Remember the earlier gotcha, actual height and width are a combination
of all elements added to them, not just what you set them to. A few
pixels can completely destroy a design.

## Implementation and Shorthand

Again, as with the font tag, we have been graced with the miracle of
shorthand. We’ll start with padding:

``` css Padding
/* All in One */
padding: 5px;
 
/* Top/Bottom and Left/Right */
padding: 5px 10px;
 
/* All defined seperately clockwise (up-right-bottom-left)*/
padding: 1px 2px 3px 4px;
 
/* Individual */
padding-top: 3px;
padding-right: 4px;
padding-bottom: 5px;
padding-left: 6px;
```

Now to borders, I would suggest avoiding longhand for sides as it is
completely overkill:

``` css Borders
/* All in One */
border: 2px solid #000;
 
/* Individual sides: Weight - Type - Color */
border-top: 2px outset #000;
border-right: 2px grooved #000;
border-bottom: 2px double #000;
border-left: 2px dashed #000;
/* Also ridge*/
```

Margin:

``` css Margins
/* All in One */
margin: 5px;
 
/* Top/Bottom and Left/Right */
margin: 5px 10px;
 
/* All defined seperately clockwise (up-right-bottom-left)*/
margin: 1px 2px 3px 4px;
 
/* Individual */
margin-top: 3px;
margin-right: 4px;
margin-bottom: 5px;
margin-left: 6px;
```

Now width and height can be set with the same units as font, with the
same rules applying, same with the min versions:

``` css Width and Height
width: 100px;
height: 100px;
 
min-height: 80px;
min-width: 80px;
```

## Conclusion

Now I can’t state enough how much you need to remember true width and
height for the next section. Write down every measurement and make sure
that they are exact, it will most certainly haunt you if you don’t.

Next in the series are Floats and Positioning
