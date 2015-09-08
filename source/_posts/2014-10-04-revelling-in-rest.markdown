---
layout: post
title: "Reveling in REST"
date: 2014-10-04 20:03:43 -0700
comments: true
categories: [API, REST, Angular, Restangular, angular-data, http]
---

Services are always the last thing anyone mentions in conjunction with Angular, despite being the single most important part. It rather well kills the point of having a frontend framework if you can't even get data properly from your respective backend. Great! Now that we have that out of the way, let's dive right into how to implement a RESTful client in Angular to get you up on your data in high fashion!

...except if you've already tried, you noticed quite the disconcerting truth. Angular is Javascript, and like its kin it has as many implementations of REST as there are people who are capable of making one. A few might chuckle to themselves on [the fulfillment of the LISP curse](http://www.winestockwebdesign.com/Essays/Lisp_Curse.html), but fret not! There is hope yet, or at very least someone with enough patience to lay out a few options worth looking into so you don't have to.

<!-- more -->

We're going to cover some of the more popular options out there, some of their strengths, and where they're going to quickly become a thorn in your side. For those unaware of the LISP curse, it's quite simply that the language is so powerful that everything becomes a social issue. Javascript is very close to LISP in terms of expressiveness, and as a result suffers from a lot of the same effects. Hundreds of half baked implementations of what you want, with very few ever offering a full package beyond the all too common "It solved my problem fine" hack library.

## [$http](https://docs.angularjs.org/api/ng/service/$http)

The built in http methods, also known as rolling your own service.

### The Good

This is the low level of making a request. As long as it fits in the scheme of HTTP you can define it here. This gives you a lot of power to take care of those fine little details.

### The Bad

The problem with having that type of power is that very very rarely can anything be described as special enough in a RESTful framework to necessitate fine grained control. If it does, you're likely doing something very wrong and need to look at your implementation a bit more carefully.

### The Ugly

When I say low level, I mean it. You have to handle setting up every method for every type of request. The only way to really overcome this is to set up a base service and define common methods, but by the time you do that, you're already a great deal of the way to items further down the list.

If you see yourself there, it's time to take a sober look in the mirror and ask yourself if you really want to invent the next RESTful service handler in Angular. Nothing against that if you have something clever, but chances are you just want to get work done, or at least you're supposed to be getting it done.

Yes, it's easy. Yes, you could probably make something pretty spiffy. Yes, it would fit your needs like a glove. No, you probably don't have the time to maintain every little detail of it if you manage to make a mistake on it. Use something already out there unless you really need that level of granularity. The LISP curse needs no more help propagating itself into the Javascript world.

## [$resource](https://docs.angularjs.org/api/ngResource/service/$resource)

An abstraction beyond [$http](https://docs.angularjs.org/api/ng/service/$http) allowing you to define a lot of the methods at once.

### The Good

Unlike [$http](https://docs.angularjs.org/api/ng/service/$http) this allows you to define all of the resources in one swoop.

### The Bad

The Documentation is neigh unreadable and you'll spend plenty of time fumbling through blog posts and whatever books you can find to get a solid implementation of them

### The Ugly

It took a while for them to get to promises

## [RestAngular](https://github.com/mgonto/restangular)

Touted as solving a lot of the annoyances with [$resource](https://docs.angularjs.org/api/ngResource/service/$resource)

### The Good

You probably won't need to bother with making Services, you can just drop in Restangular and use it in your controllers. Everything from that point on is a Restangular object you can call through on, and they all return promises. Very handy to get a lot of code out of repetitive services.

Want to do something out of the usual? Restangular can do it. You get custom methods for sending new types of requests, and you can even define your own methods on it.

### The Bad

Hopefully you like lodash (I do), because it's a required dependency. This one is debatable, as I'm of the opinion that people should be using it more as is, but I digress.

What about relationships? You're going to end up with a lot more code there, especially on trying to get many to many relationships to behave in anything that resembles coherence.

The custom methods are nice, but you're going to very quickly see your controllers start looking like half-baked services. If you're finding yourself defining a ton of custom methods for unique methods, you'll find yourself going back towards services very quickly. Granted, that likely means you need to redesign systems on the backend, and of course there's nothing against abstracting Restangular into base controllers either.

### The Ugly

These objects can become heavyweight fast. All the Restangular methods are getting appended to the objects meaning you're passing around a lot more data. If you send a ``POST`` or ``PUT`` request to create or update something, you had better hope you're filtering paramaters.

You'll end up getting a mouthful of Restangular chaff on every object you're trying to send up, and the only way to get around this one is to run a cleaner on it. To me it seems like far too much work being done for far too little extra gain.

## [Angular Data](https://github.com/jmdobry/angular-data)

Eventually you get fed up with all of this and decide that there has to be a better way to manage all of this. If you've noticed a trend so far, it's that each successive recommendation is an abstraction on the last. Angular Data is the culmination of getting far too pissed off at implementing base services and other nonsense trying to get Angular to behave coherently.

### The Good

You can define relationships, have resources defined much in the same way as a Rails-like framework, and even bind data to the scope with very little extra code.

That means you get fun like ``hasMany``, ``hasOne``, and ``belongsTo``. No more needing to create extra methods to get at nested data, and no need to repetitively define relationships.

The author is extremely responsive to issues and is known to have feedback within the day. Many of the frustrations above were things that he'd cited as reasons for creating this framework.

### The Bad

I've yet to have found anything compelling against it to this point, except that you need to be very specific in telling it how your server responds to queries.

# So what wins?

Really it depends on how much horsepower you need to get tasks done, but as of now I would still put Restangular as the go-to for most occasions with angular-data being a very interesting up and coming framework.
