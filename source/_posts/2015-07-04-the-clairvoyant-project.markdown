---
layout: post
title: "The Clairvoyant Project"
date: 2015-07-04 21:48:54 -0700
comments: true
categories: [Clairvoyant, Ruby, Rails, Metaprogramming, Functional Programming, Logic Programming, Prolog, Lisp]
---

[The Clairvoyant project](https://github.com/baweaver/clairvoyant) is one of my more ambitious personal projects, with one "simple" goal in mind: _Your tests should be able to generate your application code_

This post will outline the beginnings of the madness that led to Clairvoyant as well as some of the details of how things are planned to be implemented.

<!-- more -->

## Code as Data

The LISPers among you will notice a very common theme throughout this post. That theme is quite simply that I'm taking a ruby file and treating it as data for an entirely different parser.

The DSL is already there, the data set, the question becomes what can we divine from what we have with reasonable certainty?

## Logic Languages

Along with inspirations from LISP, we're drawing pretty from Logical languages such as Prolog. A logic program is a statement of facts used to derive an answer to a question:

```prolog
{http://www.csse.monash.edu.au/~lloyd/tildeLogic/Prolog.toy/Examples/}
witch(X)  <= burns(X) and female(X).
burns(X)  <= wooden(X).
wooden(X) <= floats(X).
floats(X) <= sameweight(duck, X).

female(girl).          {by observation}
sameweight(duck,girl). {by experiment }

? witch(girl). {Now we ask it}
```

Now I don't pretend to be an expert in Prolog, or even really particularly any good at it. Given that, it still reminds me of something:

```ruby rspec-example
describe Witch do
  describe '#burns' do
    it 'burns' do
      expect(subject.burns).to eq(true)
    end
  end

  describe '#wooden' do
    it 'is made of wood' do
      expect(subject.wooden).to eq(true)
    end
  end

  # ...
end
```

So if RSPEC looks like it's already making assertions about the nature of our program, what happens if we treat it like a logic language?

## Repurposing a DSL

The thing about Ruby is it's incredibly flexible. The DSL from RSPEC can just as easily be hijacked and run in another compiler with this one simple trick (and I swear I won't clickbait):

```ruby
class MyParser
  def initialize(file_lines)
    @descriptions = []
    self.class_eval(file_lines)
  end
end
```

Now what have we done? We've evaluated the entirety of the loaded file in the context of our class. What happens if we redefine describe inside of there?

```ruby
def describe(description, &block)
  @descriptions << description
  block.call
end

# Now when it hits 'Witch', it returns a symbol of the name instead
def const_missing(name)
  name
end
```

We can capture the entirety of the ``describe`` blocks, or for that matter anything else we want. As long as the spec file isn't using ``::RSpec.describe`` we can hijack whatever we want. If it does, it just makes it mildly more annoying to reason about.

This is effectively the current state of Clairvoyant. You can take a _very basic_ spec file and generate a skeleton of it such that:

```ruby
describe Foo do
  describe '#bar' do
    it 'does something magical' do
      expect(subject.bar).to eq(5)
    end
  end
end
```

Will generate:

```ruby
class Foo
  # It does something magical
  #
  # @return [Integer]
  def bar
    # Code goes here later
  end
end
```

Granted that's not all that impressive at this point, but beyond this stage we're going to find some very interesting problems. This brings me to my next section.

## Theoreticals

Generating a skeleton is easy enough, and still very useful in its own right. Actually writing code from expectations and matchers that could be near infinite in number and complexity? That becomes a whole different story very quickly. These are theoretical musings of the future nature of Clairvoyant as I see it.

### The nature of the 'it' block

Past the description, we're defining what the logic of the program is. From here we can infer a good number of relevant details:

```ruby
it 'does this' do
  expect(method(a,b,c).last).to eq(5)
end
```

The description string of the method can be used for documentation and a nifty method description in some cases.

The actual expectation call tells us the name of the method, and potentially anything that we can call after it. In the above example we know that ``last`` can be called on the result of our method and the ``arity`` of the method _can_ be ``3``. We can also guess that the method is some form of Enumerable, dropping possible options for output substantially. Given that we only have an integer here, we can make a reasonable statement that the return value of method is ``Array[Integer]``

We have facts to work with here, and the more ``it`` methods we have inside of a ``describe``, the more we can divine from given facts of the method. Say another test called ``keys`` on our methods return, now we can reasonably guess it's a ``Hash`` or close derivative.

That's well and good, but a lot of ruby methods tend to be very conditional in nature. They could return different things dependent on a ``context``. Luckily we have just such a method we can hijack!

### Contextual contexts abound

Say we find our method doing strange things dependent on what the ``context`` is. We can possibly even derive a conditional from a well laid out ``context`` description:

```ruby
context 'When value is even' do
  # ...
end
```

We have a potentially bindable name in ``value``, and a proper context test to throw against with ``is even``. Of course at this point it's going to be a lot more difficult to glean this information and will be heavily dependent on the robustness of a tokenization library and parser.

This becomes substantially more difficult to reason about, because now we're trying to tell people how to write their tests instead of divining information from what's already there. That may be fine for new code but can be incredibly tedious to make behave properly.

### Expectational matchers

Matchers provide an even more interesting challenge, especially factoring in user defined options. We can make some reasonable assertions based on ``raise error`` to give some error handling for dynamic methods, but that again becomes dependent on ``context`` blocks being clear enough to grok.

### Meta-testing

Given that we have all of that figured out, the next fun part is proving whether or not what we did even works right. At this point we can run our generated code through the RSPEC again as the core team intended to see if we made it pass. If we did, great! That's the easy case if we've already gotten this far. If we haven't on the other hand it opens up a whole different can of worms.

### Meta-Meta-testing

So maybe it didn't pass. Hey! It gave us data back to use to further refine and polish our solutions. We can use that to (hopefully) get them to pass on the next round! At this point the failed tests would be ported back and we could do a few things at this point:

Fail the method and leave a comment for the user or Attempt to repair the method to make the test pass

The first would be far more practical if we've gotten this far, but hey, we're in theory land. Let's push our luck a bit more here.

### Meta-.*-testing

At this stage we would be throwing code back and forth until something works, a very brute force solution to hoping we hit the sweet spot. In something that could only be compared to the quandry of monkies writing Shakespear, we might squeeze just a bit more code out of there.

Though honestly, halting problem is just a euphemism for being dull, let's have more fun!

### S-Expression analysis

So we can get a hold of a lot of your application code as well right? Let's not limit that. Let's grab as much ruby code as we can stuff into memory and try and find patterns between their RSPEC code and application code. Machine learning and deep analysis can be applied to more acurately divine intended code based on community behaviors (though I will explicitly prune out you maniacs who use globals like candy.)

Throw it in a Spark cluster and let the thing roar. We're deep into AI land of making some very interesting code generation black magic happen, and probably well beyond anything that's been attempted up to this point.

I have no qualms saying this is well beyond me, but it sounds like a blast to try anyways.

## You're out of your mind

It wouldn't be the first time I've been told this, and certainly won't be the last. This is a personal project and a great deal of fun in learning Ruby internals along the way. Maybe one day this will be a fully functional project that can magically make your wildest dreams come true, or maybe not.

Really, when it gets down to it, that's the fun of it. The potentials here are limitless, the problem hard, and the code plentiful. That's the best type of problem to poke at. It'd be no fun if I knew entirely what I was doing.

Check out [Clairvoyant](https://github.com/baweaver/clairvoyant), leave me a comment, let me know what you think!
