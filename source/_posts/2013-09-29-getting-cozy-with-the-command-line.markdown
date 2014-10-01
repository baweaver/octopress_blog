---
layout: post
title: "Getting Cozy with the Command Line"
date: 2013-09-29 00:50
comments: true
categories: [zsh, command line, automation]
---

To some, the command line is a truly frightening beast. To be fair, when
I began, it really was. Who in the world would ever want to sit around
in a prompt when there are such beautiful visual editors out there? It
seems so counterintuitive that no one should ever want to go that way.

Yet here we are. The great bearded ones hammering away in their prompts,
invoking vim wizardry, emacs enigmas, and unix hackery. What makes them
so cozy?

<!-- more -->

## It Will Hurt

When I was getting started into technology, a good friend and mentor of
mine recommended I install OpenBSD. I installed it, and the first words
out of my mouth were 'Where's the GUI!?'

I'd never been closer to throwing a computer out a window than trying to
figure that thing out. It was horrible, I was slow, and nothing made
sense. I kept projecting my expectations for an OS onto it, proclaiming
loudly how worthless it was and why it was so stupid.

After some coaxing, my friend told me to wait it out, read a few books
on the subject, and bear through it. If there was a single moment that
changed everything I'd ever known on technology, this would have been
it.

The best advice I can give to a newbie to the great prompt is that man
pages are your friend, google is an infinite purveyor of knowledge, and
amazon holds within it great archives of literature waiting to be
discovered. Read, and find a basic Linux administration guide such as
[Linux Administration - A Beginner's
Guide](http://www.amazon.com/Linux-Administration-Beginners-Guide-Soyinka/dp/0071767584/).

The commands you are going to want to know inside and out are Awk, Sed,
Grep, and Find. They will make your experiences with log files and other
text files far more enjoyable.

## ZSH

The single best thing you can do for your prompt is to install ZSH, and
shortly thereafter Oh My ZSH. Any knowledge you have of BASH will be
quickly transferrable, as it will all already work in ZSH.

ZSH offers quite a few little niceties that will speed up your work flow
immensely. 

* Command Correction - Type in the wrong command? It'll ask you what you
  meant.
* Tab Completion - Press Tab in an empty directory and you get a list of
  all files to cycle through, and as you type it will start a fuzzy search.
* Git Integration - cd into a Git Repo, and it will tell you your
  branch, and give you a number of aliases to shorten git work.
* Shared History - Command in the wrong shell? Not a problem with shared
  history

There are so many more things that I could discuss on ZSH, but there are
[plenty](http://mikegrouchy.com/blog/2012/01/zsh-is-your-friend.html)
[of](http://www.slideshare.net/jaguardesignstudio/why-zsh-is-cooler-than-your-shell-16194692) [reasons](http://fendrich.se/blog/2012/09/28/no/).

## Aliases

As I mentioned in an earlier post, aliases are your friend. Anything I
type more than once that's greater than five characters will get an
alias. Combine with ZSH features such as global and suffix and you can get some crazy commands
going fast.

## VIM

Vim was probably the biggest learning curve I had when switching to a
command prompt based layout, and also by far the most rewarding when I
really got it. Heck, I'm writing this post in Vim.

The biggest advantage to it is that your hands **never** have to leave
the keyboard. The mouse has become an enemy to productivity to me, and I
refuse to touch it when programming. Learning shortcuts and how to use
vim properly has sped up my programming substantially.

Combined with any number of the [Great Master Tim Pope's
Plugins](https://github.com/tpope) and
VIM will be a match for much of any editor out there.

The real question to ask on matters of efficiency is this: When was the
last time you watched someone in Sublime or Textmate programming and
thought 'Wow!' ? Go watch a Vim guru fly, and you'll swear you just
witnessed black magic.

## EMACS

I can't mention Vim without mentioning Emacs, lest I invoke a Holy War.
Emacs is a beast all its own, and the only apt description of it would
be an Operating System pretending to be a Text Editor. Seriously, IRC
and a Music player? It undoubtably has some substantial power, but
ultimately it clashed with my desire for a streamlined workflow.

Perhaps I'll come back to this after I start back into LISP and go into
Emacs, but for now it's not my thing.

## TMUX

TMUX is a Terminal Multiplexer. But simply, it allows you to have
multiple panes open in a single terminal window. Combine that with the
ability to save your sessions, and make templates for new sections and
it will quickly become valuable.

Admittedly I have not had as much of a chance as I would have liked to
to experiment with it, but it is definitely worth a look.

## But Why?

I switched to an almost completely terminal based workflow for one
reason in the end: efficiency. I'm notoriously irratable with repetition
of anything, and anything that allows me to remove repetition is worth
the effort.

That, and it is always nice to have a new programmer accuse you of black magic
hackery after seeing you do anything.
