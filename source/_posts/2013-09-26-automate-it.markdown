---
layout: post
title: "Automate it"
date: 2013-09-26 21:51
comments: true
categories: [Automation, Unix, Programming]
---

The better programmer is not the one who flies across the keyboard,
generating hundreds of lines of code, but the one who has but a few
strokes that do the same work in half the effort.

<!-- more -->

## When to Automate

  *  Did you use it more than twice in a day? Automate it.
  *  Does it take more than five keystrokes? Alias it.
  *  Are you repeating yourself? Automate it.
  *  Did you just wonder if you should automate it? Do it.

Automation seems to be a scary concept for some, a black magic that many
try and avoid because they already know all of their commands and
appreciate their vanilla editors.

## But it takes too much time!

![XKCD 974](http://imgs.xkcd.com/comics/the_general_problem.png)

In some cases, yes, you are spending far more time automating something
than actually getting it done. Then again, really, how far and inbetween
are those cases that you can justify it all away with just that? XKCD,
as always, has our backs on timing it out:

![XKCD 1205](http://imgs.xkcd.com/comics/is_it_worth_the_time.png)

## Shells

If you're in a Unix environment, your shell should be your best friend.
Know your way around a command prompt well enough and you've already
made some serious headway in reducing the amount of time it takes to do
something!

I would seriously suggest taking a look into
[ZSH](http://mikegrouchy.com/blog/2012/01/zsh-is-your-friend.html) and its' extension
[Oh My ZSH!](https://github.com/robbyrussell/oh-my-zsh) as they alone will save you a lot of time in a shell
prompt.

## Aliases

Now let's take a look at a few of the aliases I frequent:

``` sh Aliases

alias v="vim"            # Shorten Vim
alias vrc="vim ~/.vimrc" # Edit my Vimrc
alias vc="vim ."         # Open the current directory in Vim

alias vzpf="vim ~/.zprofile"  # Edit my zprofile
alias zsrc="source ~/.zprofile" # Reload my zprofile
```

These, of course, being pulled from my [Special Sauce
Repository](https://github.com/baweaver/special-sauce).

So what rule of thumb do I use when adding new aliases? If it takes more
than five keystrokes to do, I alias it. Digging into my .zprofile will
show you a most_used command which I have to keep me honest about how
much I use commands.

The amount of time I save from just that adds up quickly as I type many
of those commands several hundred times a day. Adding an alias, and
sourcing my .zprofile takes me all of five seconds to do.

## It adds up

So what, we have a few niceties and aliases around. We may save five
minutes a day with a basic set. Perhaps, but the more you alias and the
more you start to chip away at your daily repetition, the more you will
realize that you're quickly outpacing your normal speeds.

## Be Lazy

Really. Be lazy. Hate to repeat yourself so much that adding an alias is
a natural twitch. Hate doing things by hand so much that you crack open
your editor and start scripting it out! 

Learn the keyboard shortcuts, and stop touching that mouse. If you're
really hardcore on it, learn Vim or Emacs and get to town on Macros and
keybindings.

## Automate it

When in doubt, automate it and document it. Sharing is caring, and many
people are quite kind as to [post their zprofiles](https://github.com/search?q=zprofile&ref=cmdform&type=Code),
so take a peek and learn.
