---
layout: post
title: "The Impersonal Interview"
date: 2015-03-22 20:21:38 -0700
comments: true
categories: [philosophy, interviewing]
---

It's been said [time](http://erniemiller.org/2013/09/19/interviews-are-broken/) [and](https://www.linkedin.com/pulse/20140625202040-56760691-your-technical-interview-is-broken) [time](https://medium.com/backchannel/the-way-we-hire-is-all-wrong-3e19e2051f3e) again, our technical interview process is broken. Lately, it's become fashionable to attack the interview process, but little seems to be done in regards to it. This is my opinion on the matter.

<!-- more -->

## IQ and GPA are Irrelevant

Google famously came out saying that GPA was [worthless](http://dailycaller.com/2013/06/20/google-executive-gpa-test-scores-worthless-for-hiring/). Trick problems and brain teasers were doing little to no good in revealing good engineers.

In an industry where it's borderline impossible to establish reliable metrics to programmers skills, is it any wonder that interviews backfire? Despite this, we're trying to use one metric in particular to measure our coders, and it's doing a great deal of harm to the industry: memorization.

## Standardized Testing in Schools

When confronted with the idea of standardized testing, teachers I've spoken to have been quick to say that they are a poor measure of students. Some kids just don't learn by memorization and test like that, potentially brilliant young people are barred because they can't memorize a fact sheet. Why should they?

What bothered me in school was that I was expected to memorize a bunch of information that was literally inches from me, either via internet or the text book. Sure, I could memorize the quadratic formula, but what would be the point? Memory is faulty, but being able to automate or make reference of something is true value.

## We're testing rote memorization

If this is failing so badly in our schools, why are we applying the same principles to coding interviews?

It's not uncommon to have pre-interview filters ask manual page questions, system internals, or things in general that would take no more than a second for someone to find in reference. Instead, we expect them to have an instant answer to these questions when rarely do coders actually have such information memorized.

The amount of false-negatives here is staggering, especially for cross-job interviews such as a developer seeking devops jobs or vice-versa. Of course a developer won't have systems knowledge memorized, and likewise an administrator probably won't have the entire Skiena's book of algorithms memorized. Does this make them incapable of the job? Hardly.

We're not studying for a test. We're trying to show that we have what it takes to innovate, to build. Memory is an abhorrent measure of aptitude. If GPA is already pegged for this, we should be doing away with rote memorization in much the same manner.

## A Cache System

An engineer's true power lies not in memorization. If anything, I would argue that it's a severe weakness to have an engineer who insists on only memorizing large sums of information. It's inefficient. Much like a computer, only information that is immediately relevant should be cached. That's why we have references and man pages, we've relegated the information to a metaphorical hard drive for later recovery when it's needed.

Here are a few examples:

* Instead of memorizing a system, dictate it into reference.
* Instead of memorizing a deployment process, automate it.
* Instead of memorizing esoteric language behaviors, write hooks in your editor and VCS to catch them
* Instead of memorizing how to manually get system uptime and kernel information, write a tool to fetch it and return relevant information

The list goes on.

An experienced engineer has a lot of knowledge to draw on from past jobs. Chances are they've probably forgotten more than a more junior engineer has claimed to have memorized. Does this make them less valuable? Hardly. They recognize that it's sometimes necessary not to attempt to know everything up front.

Now granted that an experienced engineer is going to be far more effective in finding the correct references and information. It makes a world of difference and can really speak to the skill level of a person:

<blockquote class="twitter-tweet" lang="en"><p>the older I get, the more convinced I am that the key qualities of a senior engineer are research skills and leveraging past experience <a href="https://twitter.com/hashtag/fb?src=hash">#fb</a></p>&mdash; Scott Francis ن (@darkuncle) <a href="https://twitter.com/darkuncle/status/581509216893960192">March 27, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Who would you rather have working for you? Someone who memorizes your entire deployment process to a T, or someone who automates the entire thing so anyone can do it with a click? I would argue that the latter is exponentially more valuable to a team.

## Hit By a Bus Effect

The weakness in memorization is that if only one person becomes a bastion of all knowledge, you're going to get in trouble quickly. If they take a vacation and the entire team falls apart, there's a problem. Information is meant to be shared and made easily accessible.

Memorization has the horrid side-effect of blinding your team from bad documentation and process. By building tools, you won't have to explain to the poor new hire the hundreds of caveats of even starting to develop your application and nonsensical process that was allowed to grow over time.

## But Here We Are

Yet given this, there's a perverse obsession with reciting algorithms from the book, quoting man pages, and all forms of memory-backed questions. It's a double standard. We interview on the metric of memory, yet any sane coder will go into a panic attack given a completely memory based employee without a fierce knack for automation and tooling.

## Then what's a better way?

Do away with pre-interviews, all they do is filter out potentially great people with knowledge they may not have in immediate memory. Instead, ask them what they've built, what makes them tick, what has them up plugging away. You'll learn far more from getting someone's story than asking them five quick questions.

Avoid anything based in reciting man-pages and algorithm books. Instead, seek to either pair with the person or have them demonstrate on a small project. Dig through one of their already built projects, do something practical. Whatever tool they have available to them as a developer should be fair game. If you really want to be bold, have them bring their own laptops in to see how they work.

The point is to learn if this person can contribute to your team, not chant Dijkstra's Algorithm and write out a Quick Sort. If you're not going to be working on it daily, it does not belong in an interview.

## Parting Thoughts

"Everybody is a genius. But if you judge a fish by its ability to climb a tree, it will live its whole life believing that it is stupid." - Albert Einstein
