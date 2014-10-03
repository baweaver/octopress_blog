---
layout: post
title: "The Frivolous Frontend Framework"
date: 2014-10-02 19:46:01 -0700
comments: true
categories: [Rails, Ruby, API, Frontend Framework, AngularJS, EmberJS, BackboneJS, jQuery, AJAX]
---

Many a hardened Rails programmer will swear by their ERB or HAML, shaking a fist at the sky decrying the acolytes from the land of Javascript for their frivolous frontend frameworks.

"Who needs them!" they say haughtily. "jQuery has sustained us perfectly fine, and our applications are not nearly large enough to warrant the extra overhead! Why would any of us use a frontend framework?"

Yet there are those of us, standing upon the hill, pilgrims from the unholy land of Ajax Callbacks and Asynchronous Updates, looking upon them with something akin to pity.

<!-- more -->

## The Short Version

If you're looking here for answers on whether you need a framework, chances are very high that you do. If you've found your way fumbling about AJAX one too many dark nights whilst imbibing strong drink, it's time to bite the bullet and make a jump to a better place.

## The (Poetically) Long Version

In the modern day web, dynamic never seems to be dynamic enough for some. Data needs to update live on the page, masses of components need to update and render as if in some practiced dance. You find yourself saying "Just one more patch hack and it should work again." How many nights has it been now? Two? It seems you've lost count. Anything akin to structure is a mad snarl of brambles waiting to take you should you misstep even one unit test.

You cry out in anguish as IE8 fails to render yet again. Surely there must be a better way, but the application is not yet large enough to warrant such an expenditure of effort! What you do are only a few AJAX calls to your APIs, the callbacks have only nested five levels by now. You'll switch when it gets worse, you think to yourself. Only, does anything ever happen in that most special circle of hell known as Technical Debt?

### What a piece of CRUD

Duplication, everywhere you see. The same basic operations of Create, Read, Update, and Delete. All of which done to the tune of the team who happened to be working on them that particular week. None quite work the same, and all attempts at consolidation and style guides have long since been laughed off as meaningless. The code is littered with edge cases, special hacks, and one time things with promises of removal and cleaning.

It's not that any of the implementations were particularly bad (except for Bobs, that was a mess, how is he still working here again?) They all make sense in their own particular ways, and their creators could speak at great length on their strengths in such a grandiose bravado. You nod vigorously, a great deal of sense is made here! ...but venture you further into the tribe of Neckbeard to here their prophet of the promise speak so eloquently of their path to righteousness. You knuckle your head as you bow out, some poor fool had brought up editors again.

It was quite a quandary, so many made sense but in such different ways. Which was right, who was to say, but then you saw it. The new hotness they had called it on HackerNews, singing its praises in bringing order to the chaos. Skeptically you listened in on the discussion, and rightly so. You seem to recall them saying something about Java and Perl dying again last week for the fifth time this year. Best to take them with a grain of salt. How many had claimed by now to have the one true way? AngularJS stood proud among the rest, EmberJS attracting its crowds as well, while still more frameworks begged attention.

Which one should I investigate? The answer is quite simply that they all have a point, and as to which one is not nearly as important as deciding upon migration before Jira comes to swallow your hopes and dreams.

### AngularJS

For the sake of this article, I'll cover things in the terms of Angular. I have a great deal of respect for Ember and the work they've done, but I can't speak nearly as much to its strengths. The purpose of this is far more to show how a front end framework can liberate you from the shackles of the oppressive Raw AJAX.

Organically grown frameworks very seldom work, and more often than not end up becoming a cluster of micro-frameworks that are incomprehensible to all but the most trained in their ways. Best hope there are no rouge buses to rob you of their knowledge. While in the first place it seems like a good idea to allow free reign to interpret ideas and build more creatively, you'll quickly learn that anything that can be considered a social issue to programmers is grounds for a war.

This is why we have style guides and procedures in place, to bring order. No more having to listen to a lengthy discussion on the merit and readability of two spaces versus four, the style guide had set it in stone months ago.

Much the same can be said for a Javascript Frontend Framework. While many would say they're frivolous things, they bring order to the wildness of javascript. If only for that reason I would take them into great consideration, but their effectiveness does not end there.

With Angular, the DOM feels almost a distant memory. Cleanly abstracted from you, table rows can be updated dynamically and the page manipulated with as little as a simple data binding. Search bars for data sets are within perhaps 50 characters at most. Things that would strike horror into a pure jQuery Programmers heart are now trivial, abstracted away to build upon to greater heights.

Now things are tied to directives and actions rather than nodes that may change by a simple accident. The page can be reasoned about as a whole rather than as segmented pieces cobbled together with selectors.

When someone asks me why a frontend framework, I would quite simply reply:

"Because the sense of unity and structure they provide far outweighs the price of its implementation"

Well, that, and because Angular behaves properly in IE8 as of current versions. That alone is worth its weight in gold.
