---
layout: post
title: "The World is a Functional Program"
date: 2015-02-20 22:01:46 -0800
comments: true
categories: [philosophy]
---

# Intro

What if the world was, in its entirety, a functional program? Through the discovery of mathematics and pure functions, we can derived the process in which lead to our world.

![We lost the documentation on quantum mechanics.  You'll have to decode the regexes yourself.](http://imgs.xkcd.com/comics/lisp.jpg)

<!-- more -->

This current variant is still in need of refinement, comments are appreciated as I clean up around the edges. Working on rewriting the current code segments and writing new ones in Lisp for added effect

# Divine Recursion

Take a simple recursive function, factorial:

```ruby
def factorial(n)
  n < 2 ? 1 : n * factorial(n-1)
end
```

We have established a base case in which a constant can be derived, numbers less than two will always return one. The flaw of current world views is that we assume creation to have occurred as a constant base case, and seek the answer there.

What really happened was something different entirely. While we quibble over how to find the base constants of our world, we miss a very crucial fact of inception: who called the function that started the divine recursion?

# Closures

In programming we have the concept of a closure, where a function closes over a value:

```ruby
closure_fn = -> {
  my_data = 'Foo'
  
  -> name {
    "Hello, #{name}. #{my_data}!"
  }
}.call

# This returns a function that we can now call:
closure_fn.call('Brandon') # => "Hello, Brandon. Foo!"
```

The function inside captures the state around itself, enclosing it, or rather creating a closure over it.

Our world is the result of a closure in which something defined our function with a set of constant values outside of our function, but inside of our scope of knowledge. This is how we derive logic, time, and the rules of the world in general. This begs the question though, how did it get called? A transcendence, much like a programmer that calls a function.

# Free Will and Branch Theory

When a recursive problem approaches a function that can go down many paths, its result is not necessarily known by the one who invoked it. Given certain conditionals, a branch may be permanently trimmed off the world tree as it approaches its absolute single-branch return value.

While the invoker may not know the result of the branch, they may be able to make certain guesses about the nature of its execution. With enough insight and knowledge of a program, you can predict the results of a tree. That makes it sound as if there's no such thing as free will and there's an inevitable predestination, but here's the brilliant part: it's not.

# Callbacks

Inside each execution loop of the world tree, a callback is invoked in which an external function can be reached. This can be considered much the equivalent of prayer, sacrifice, meditation, and other spiritual activities. Given that these callbacks do not prevent the execution of malicious code, bad things are able to happen as a result of misusing them (Oija boards, summonings, occult, falling from grace.)

Through this process of callbacks in the tree, the execution order is now unknown to even the invoker. Even at that, the knowledge of the inner workings of the program will still lend considerably more insight into the path of execution than will be known by the data (or person.)

In a way, it's a solved game. Much like a supercomputer playing chess, the end result was decided before the game even began. Free will is the result of the game itself being played in the interim around the fixed endpoints.

# Laziness, Currying, and Partial Application

Given the process of callbacks, the entire world tree is already effectively defined but the functions have not been called as all the data is not present.

```ruby
plus_one = -> x {
  -> y { x + y }
}.call(1)

plus_one # => anonymous function

plus_one.call(2) # => 3
```

Without all the data being present, a function will not be called. When provided with its last parameter, the value will be returned and a branch can be derived.

By currying our choices and states along the tree, we build up towards the execution of functions that will change the branch we're currently on. The world tree is lazy in nature, it will not execute branch changes until it has all the data necessary.

# Evil in the form of exceptions

The crux to giving the ability for callbacks and laziness in functions is that errors can and will be raised. Ones that are outside the influence of the invoker due to the nature of the function. Does this undermine the omnipotence of the invoker? No, as they had already provided rescue conditions throughout the application to save data from exceptions.

```ruby
def saved(function, data)
  function(data)
rescue
  outer_context(data)
end
```

# Monadic state

So then how do we reconcile with a young earth versus an old? We don't. Both are plausible at the same time with the presence of monadic state, a seed of sorts. By invoking the world tree with a set of predefined knowledge, time can be simulated, elongated, or generally distorted beyond the current rules of our world tree.

```ruby
def world_tree(state)
  some_execution_chain(state)
end

world_tree(logic: rules, entities: creations)
```

Much like a dream to us, we view something as always present, predefined. Perhaps our concept of time is warped by the seed data in such a way that we observe something beyond our functional world tree. As it recurses, it carries with it the state that could have easily been arbitrarily defined along the way.

# Evolution and Functional Composition

Evolution is also a result of seed data, but more thoroughly of functional composition:

```ruby
organism(cell(protein(x)))
```

Functions are composed upon one another such that the pattern that composes a monkey may well be an earlier variant of a human that has not had all of its functional chain called through. This is what leads to similarities of DNA, a monkey would merely be a human without the remaining functions between them.

We see evolution, but in reality it's the base functions that have been built from the ground up in order to create us and the creatures around us.

# The apocalypse and the return

You remember the presence of constants in the system? They were never meant to be the beginning, but the end of the chain. If the end is already known, and the beginning was made from seed data, the process in the middle is left largely to the result of execution.

At the end of our world tree function, and when certain branches are returned, our state is transferred into the closure above us, more commonly known as our heavens and hells. These returns will only happen when a function has called through an entire branch at the end of the tree, known as the apocalypse.

In the interim, we're stored in a state that's carried throughout the remainder of the world tree, in what would be called as Limbo.

Given that the function was invoked by an outside source, certain code may have been arbitrarily introduced in such a way to allow new state to manifest itself at certain points of the chain in ways that again defy our given rules. This can lead to such things as a virgin birth, resurrection, and even a return as the end condition itself.

The world is a functional program, and we are the data that flows through it.
