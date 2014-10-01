---
layout: post
title: "Izzy Hackery"
date: 2014-09-30 22:43:48 -0700
comments: true
categories: [Ruby, Monkeypatch, Hackery, Streamable, Pipeable, Izzy] 
---

In which I explain the gem Izzy

<!-- more -->

In the time I've been off of writing on here, I've had a bit of a stint of gem creation. We're going to cover a number of them in the coming week.

Some may say that monkeypatching is inherently evil, but I would tend to disagree.  An RPG serves a very tactical purpose when used correctly, but often times it can have rather unfortunate results in the hands of the untrained. Such is monkeypatching, something that should be viewed in a pragmatic sense rather than one of dogmatic vitriol. With that, let's take a look:

## Izzy

Izzy got popular right after a [Ruby Weekly post mentioned it](http://rubyweekly.com/issues/180) as a method of mitigating long conditionals. I made it for the express purpose of simplifying multiple conditionals on the same object into something more succinct.

Going off of what's in the README as far as order, we'll take a look into some of the inspiration and workings of each method.

### Matchers

Matchers are methods that are checked against any of the attributes of an Object that includes Izzy. Let's say we have an instance of me made in a Person class implementing Izzy:

```ruby
brandon = Person.new('brandon', 24, 'm')
```

Now it gets really tiresome to do something like this while trying to validate against this object:

```ruby
brandon.age > 18 && brandon.name =~ /^br/ && brandon.gender == 'm'
```

It seems repetitive and downright unnecessary to specify the object multiple times. Rails has a tendency to use hashes to create, query, and update object, so why not add some of that type of magic to validations?

```ruby
brandon.matches_all? name: /^br/, age: -> a { a > 18 }, gender: 'm'
```

To me that's far more succinct. So how do we make something like this in Ruby? Let's take a look at the source:

```ruby
def matches_all?(matchers = {})
  matchers.all? &matcher_check(:all?)
end

def matches_any?(matchers = {})
  matchers.any? &matcher_check(:any?)
end

def matches_none?(matchers = {})
  matchers.none? &matcher_check(:any?)
end

private

def matcher_check(type = :all?)
  -> matcher { 
    m, val = *matcher
    values = val.is_a?(Array) ? val : Array[val]
    values.send(type) { |v| v === self.send(m) }
  }
end
```

The first thing you may notice is that the body of the block is abstracted into a private matcher_check. This is to abstract the logic for reuse on the two other matcher types.

The fun thing about this is because it's in a method, we can send another argument to it. The value is then pulled into the block, or closure if you prefer. In this case, we're sending what to check the values against dynamically. Notice that we only use all on the ``matches_all?`` method.

Let's step through this piece by piece with only using the check ``brandon.matches_all? name: /^br/``:

```ruby
# Call matches_all? on brandon:
matchers = {name: /^br/}

matchers.all? &matcher_check(:all?)

# matcher_check
type = :all?

# Hash gets exploded into the method and the value to check against
m, val = [:name, /^br/]

# Since we're able to check against multiple conditions using an array, we want to make sure we have one to work with:
values = Array[/^br/]

# We then check the values array with :all?, or :any? in the case of any and none checks.
values.send(:all?) ...

# which will use === to check it against the actual value:
/^br/ === self.send(:name)
```

So why does ``===`` work there you might wonder. It's overridden very frequently for classes, notably for Regex (matches), Range (includes), and Proc (call). Most of the time this is bad practice not to use the longhand versions, but in this case it affords us a great deal of flexibility not to worry about how it's evaluated as long as it does a proper match.

This is actually one of the most powerful features in the case statement, which uses ``===`` for its ``when`` clauses. Notice that Proc.call is the same as Proc.===, meaning you can throw lambda and friends into the mix for even more powerful checks.

### Boolean Matchers

Boolean matchers were the original method, again using the abstracted block:

```ruby
def all_of?(*methods)
  methods.all? &method_check
end

def any_of?(*methods)
  methods.any? &method_check
end

def none_of?(*methods)
  methods.none? &method_check
end

private

def method_check
  -> m { self.send(m) }
end
```

This one is far simpler in that all it does is call a list of methods on an object, checking their truthfulness. If we had some methods defined on person to check legal status, or various other simple checks, this would come in handy:

```ruby
brandon.all_of? :legal?, :older_than_21?, :male?
```

### Enumerable Module

Because sometimes it's nice to have a bit of that Rails feel in regular Ruby. These methods use the ``matches_all?`` method in conjunction with ``select``, ``reject``, and ``find`` to provide some Rails like shorthand:

```ruby
def select_where(matchers = {})
  self.select { |s| s.matches_all? matchers }
end

def reject_where(matchers = {})
  self.reject { |s| s.matches_all? matchers }
end

def find_where(matchers = {})
  self.find { |s| s.matches_all? matchers }
end
```

We're not always in Rails, and one of my favorite features are the ActiveRecord ``where`` and ``find`` methods. Composing the two functions allows us to do that quite nicely.

## Final Notes

Combining multiple small functions into something larger is one of the cornerstones of functional programming known as composition, and something well worth looking into. Not every gem has to be a monolithic beast that can tame the worlds problems. Sometimes you only need to do the simple things well and build up from there.

Next up we'll look into [Streamable](https://github.com/baweaver/streamable), [Pipeable](https://github.com/baweaver/pipeable), [@banister's Funkify Library](https://github.com/banister/funkify), and hacking Piping functionality onto Ruby.
