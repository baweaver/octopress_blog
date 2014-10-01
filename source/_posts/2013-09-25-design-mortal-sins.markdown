---
layout: post
title: "Design: Mortal Sins"
date: 2013-09-25 20:33
comments: true
categories: [Tutorial, Design, Web] 
---

So far we’ve covered quite a lot of ground in both coding and design,
but before we get on to real work there are some things that need to be
made clear. In Web Design there are certain practices that are absolute
taboo, we’re going to cover them and why they’re taboo in the first
place.

<!-- more -->

Note that this article is a revision of the 2011 article found here:
http://www.blog.baweaver.com/post/8815905313/practical-web-design-the-mortal-sins

## Bad Font Choice

One of the most common mistakes made, and one of the most damaging is
using poor fonts. If your content is king then you want to communicate
it to your user as clearly and effectively as possible. Font is crucial
to that point and a bad call can cost you some serious points.

The most notorious offenders of misuse are Comic Sans and Papyrus.

Comic Sans is a perfectly acceptable font, but the real issue lies in
its misuse. Comic Sans was designed to be used in Comics and Children
materials. So if this is the proper use, why do we find it posted in
Hospitals, Fortune 500 Companies, Schools, Professional Presentations,
and more? The problem here is misuse. While it seems like a fun font you
are far better off going with a time tested classic that reflects
professionalism instead of “fun”. A more appropriate font choice for web
would be verdana, georgia, tahoma, or times.

Papyrus on the other hand is a horrible font period. It has horrible
letter spacing, it is practically illegible at most all sizes, and is a
poorly made font that is used to make something look foreign. A more
appropriate font would be something such as Trajan, which is a standard
font on most computers.

Menus are one of the most critical elements of your site. Without a
navigation that is clear and readable immediately you will almost
certainly lose a majority of your visitors in an instant. It’s not about
personality, it’s about the user.

Your safest bet in web typography is to stick with the classics: Times
New Roman, Verdana, Georgia, Tahoma, and Arial. With CSS3 there are new
methods of implementing new fonts in a practical manner. I fully support
using this technology, but remember: with great power comes great
responsibility. Don’t go overkill on fonts just because you have a
massive library to play with.

@font-face and font kits are widely available, a quick google search
will turn up multiple resources.

http://www.fontsquirrel.com/fontface

One of my favorite resources, two of my personal favorite fonts are
Liberation and DejaVu.

## Tables for Design

I’ve seen this point made in multiple places, and I can assure you my
resistance to this is not due to some obscure elitism. I started
designing pages in tables quite a while back, and I have long since seen
the light of CSS based design.

Tables are meant for tabular data and were never meant to be a holder
for any form of design. More semantically correct code is available and
the differences in efficiency are absolutely astounding.
Tables have some severe weaknesses, ignoring their deprecation in
design. Each of these I have found through various personal experiences
in learning and research.

By recoding an old tables layout I reduced the size of the site by well
over 95%, not a small margin, several megabytes.

When I presented a site coded with CSS and Divs my clients were almost
immediately able to comprehend what was going on in the page. When I
would show anyone a tables layout they would sit there and stare forever
and have no idea what they were looking at. It is far cleaner and
substantially more readable.

Google and other bots absolutely detest long markup and useless tags
which tables abound in. A CSS site is much cleaner and easier to parse
for a bot, meaning you’re more likely to get relevant information out to
where it counts: The search engines.

It is infinitely more flexible than tables could hope to be. I have one
CSS file that controls the style for at least 5 pages whereas I would
have to completely recode a table site just to add a minor item. This is
wasted time on completely unnecessary tasks, I’ve saved weeks worth of
time by making the switch. Complete site overhauls no longer give me
insomnia and nightmares for months on end.

## Gradients and Patterns are a Privilege

They are not a right, the way some people abuse them should be a crime.
These are both background elements. The point of a background element is
to be subtle and to accent the foreground.

By having blaring gradients and patterns splattered all over your page
you’re detracting from the focus of your user, and you only have so long
before they move on. What would you rather them stare at, your product
or some tacky gradient/pattern that sticks out?

## Publisher is not a web design program

Microsoft publisher is not a design program, I don’t care what type of
insane ingrained reason you have. If you need more evidence of why this
is a bad thing, just look at the source code of a publisher page, it
makes tables look like nothing.

## W3 Compliance is a good thing

It’s a pain, I know it is, but make sure to validate your code through
the W3. This will save you tons of time in debugging. Standard compliant
code eliminates a majority of the bugs you will ever encounter in
design, a majority of the rest are bad measurements involving the box
model.

## Don’t bother with IE6

If you’re too young to know about IE6, be thankful. This standards hell
was a hurdle for web designers for years. Microsoft became complacent
after dominating the browser market for so long they quit updating for
an extended period of time. To their credit they finally owned up and
deprecated the browser.

## The Geocities Nightmare

Geocities was a popular hosting platform of the 90’s that was recently
shut down forever. Buried with it are several nightmares of the Web 1.0
era that should never surface again and see the light of day.
Fluorescent colors are forbidden in excess, they get attention in all
the wrong ways. Don’t blind your user with such gimmicks as this.
No matter how cool that music is that you found or your band made, DO
NOT set it to auto play when people visit. Some people like me are
already playing music, and that’s the fastest way to get me to close the
tab and never go back.

Animated GIFs are long since dead, like myspace, let’s leave them that
way.

Patchy background images are not cool, they don’t add personality, they
just drive a user insane. Be professional about things.

Comic sans is not to be used in web, period, even with cited instances
in earlier subjects.

The font property is a privilege, don’t abuse it. A clean and readable
font beats out a fun one every time, don’t do it.

Let’s make sure Geocities and its cesspool stay buried forever.

## Copying other Designers

Just because you think something looks cool does NOT mean you can steal
it from a designers site. Believe it or not there are legal actions to
face for this, it’s best just to get inspiration and take notes rather
than blatantly ripping parts off. Just like in school, even if you never
get caught it will haunt you later on.

## Playing Artist

Let’s get a point clear, design and art are two completely different
fields.

Design is composition and how elements are situated to provide the same
message to all users, it is a science.

Art is an abstract that can give several different meanings, it is a
creative pursuit.

If you are not a skilled artist do not attempt to create logos or fancy
headers, it will end poorly. You’re better off to hire an artist or get
a friend.

This isn’t to say you can’t learn, but don’t try and do something far
and above your head. Do what you know you can do. I’ve seen simple
headers based on gradients and basic font that look amazing. You don’t
need a massive amount of creativity to make a good design.

Remember: Design can be learned, art is a talent that is developed with
years of practice.

## Owning Photoshop or Dreamweaver does not make you a Pro

This is a sore point for several designers. Owning professional software
makes you a professional designer as much as owning a stethoscope makes
you a licensed neurosurgeon. Be respectful to professionals, they know
what they’re doing and you hire them for a reason.

## IDEs are a Bad Idea

Dreamweaver and its kin are a bad idea to use starting out with. They
have auto completion and code snippet functions designed for pros. In
the hands of a newbie this becomes a massive handicap, they become
reliant and never learn the code. What do you think happens when there’s
no more dreamweaver for them to use?

## Conclusion

Keep these things in mind as you start on your design sprees, the next tutorial will be over basic typography.
