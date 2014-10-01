---
layout: post
title: "The Functional side of Ruby"
date: 2013-09-28 11:08
comments: true
categories: [Ruby, Functional]
---

Many people come into Ruby from a C-based language background, and are
quick to use only what they really feel comfortable with that has a
direct parallel in their language of choice. Doing so, you miss out on
all types of wonderful features of Ruby, and in this post we'll cover a
few of them.

<!-- more -->

## Functional?

![XKCD 1270](http://imgs.xkcd.com/comics/functional.png)

If you're like me, you've heard this word thrown around more than
anything, and never really defined. Everyone sings praises of this great
new renaissance of programming, but no one seems to know what it even
is.

In its simplest terms, functional programming is a program based on
functions. Functions return values, mutation is a naughty word, and the
law of the land is no side effects. 

Well that sounds all well and good, but how exactly can you program if
all variables are in their final state? That seems rather
counterintuitive at best, and confoundedly stupid at worst. So why then?

## First Class Citizens

In languages that support functional programming style, all functionas
are first class citizens. This means that they can be passed themselves
as arguments the same as any other value, because by their definition
they return a value.

With Ruby, every function returns a value, whether implicitly or
explicitly. Let's see what we mean here:

``` ruby First Class Functions
def bob
  "my name is Bob!"
end

def hello(message)
  puts "Hello, #{message}"
end

hello bob 
  # => Hello, my name is Bob!
```

We just passed a function as a value! This opens up a lot of interesting
possibilities, which brings us to our next point.

## Anonymous Functions

Anonymous functions are functions without a name. This may sound
strangely foreign, but if you've ever touched javascript you might
recognize this pattern:

``` javascript Anonymous Functions in Javascript
var square = function (x){
  alert(x*x);
}
```

You may notice that we just set a variable equal to a function, or more
correctly that we just named an anonymous function. So where did this
come from? Let's take a look at the same thing in Scheme:

``` scheme Anonymous Functions in Scheme
(define square (lambda (x) (* x x)))
```

This type of pattern is extremely common in LISP like languages, which
is why some readers are going to start noticing some striking
similarities to Ruby at this point. Let's give this one more try in
Ruby:

``` ruby Anonymous Functions in Ruby
square = lambda(x){ x * x }
square = ->(x){ x * x } # Ruby 1.9+ Syntax
```

Blocks are essentially anonymous functions that are called on the fly to
operate on enumerator values, and discarded. Blocks can also be saved if
need be, which brings us to

## Why Bother?

What benefits does it really bring? Is it even worth it? In short, the
authors (probably biased) opinion is yes. The key reason to this is
idempotence.

You see, idempotence is a complicated word that essentially means that
no matter how many times you run a function, given the same input it
will *always* return the same output.

The benefit of this is that you don't have to worry about a mystical
black box and ordering scheme, as well as necessary blood sacrifices in
order to get a unit test to pass. You know for a fact that a function
will return the same every single time. That, my friends, will save you
a great many nightmares down the road.

The great thing about idempotence is it translates almost directly into
thread safe methods that will not do unusual things to your values if
written correctly. Functional languages thrive in multi-threaded
and distributed environments, just look at Erlang.

Erlang was a language invented by Sony Ericsson in order to manage their
massive phone distributions. They came across a hairy question, how do
we update our phone network **and** ensure no down time? Enter Erlang
with its hot-swappable modules that could be changed out in production.
Functional languages and techniques can give you that type of power.

## The Good and the Bad

So what constitutes good practice and bad practice? Let's dive into a
few examples shall we?

In string manipulation, modifying the original string will yield some
very nasty side effects very quickly.

``` ruby String Manipulation
str = 'Hello, ' + str # BAD, we just mutated the variable! Running
multiple times would be BAD news.

"Hello, #{str}" # GOOD, no mutation, just a return value
```

Bang (!) methods should be used extremely rarely, as they modify the
sender. Instead, return the results to a new array.

``` ruby Square an Array
ary = [1,2,3,4,5]

ary.map!{ |i| i * i } # BAD, mutated array

new_ary = ary.map{ |i| i * i } # GOOD
```

This would be more amusing if I hadn't done it before when I started.
Read up on the Enumerable module, as it will save you immeasurable
amounts of time in the long run.

``` ruby Select from an Array
ary = %w(hartnell troughton pertwee baker davison baker mccoy mcgann
eccleston tennant smith capaldi)

new_ary = []
ary.each{ |name| new_ary << name.length if name.length > 5 } # BAD

new_ary = ary.select{ |name| name.length > 5 } # GOOD
```

Again with the things I wish I had never done. Iterators like this are
definitely not needed in a language like Ruby where practically
everything is an object. Again, learning the methods will save you a
lot.

``` ruby Count Number of Records Processed
ary = %w(hartnell troughton pertwee baker davison baker mccoy mcgann
eccleston tennant smith capaldi)

i = 0
new_ary = ary.select{ |name| i++ if name.length > 5; name.length > 5 } # BAD

new_ary = ary.select{ |name| name.length > 5 }
new_ary.count # GOOD

```

The amount of time that you will save by simply reading over the
Enumerable module, and learning the commands map, reduce, and select
will be astounding. All of which originated from a LISP like language.

## In the Wild

So, this is a fairly short writeup on the subject, and I will definitely
cover it in more detail later on, but you should have a decent idea of
what to look for.

The thing to take away from this is that if used properly, unit tests
and making sure things behave as they should becomes exponentially
easier. Some of the hardest tasks in programming merely require a
different perspective.
