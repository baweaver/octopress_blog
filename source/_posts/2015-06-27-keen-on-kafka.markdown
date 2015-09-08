---
layout: post
title: "Keen on Kafka"
date: 2015-06-27 21:10:28 -0700
comments: true
published: false
categories: [Functional Programming, Scala, Python, Spark, Kafka]
---

Before we get into streaming with Spark, there's a tool that needs to be mentioned. Kafka is a 'high throughput distrubuted messaging system.' For our purposes, it's a very handy messaging system we can utilize as a pub-sub system in conjunction with our stream events, and a very necessary tool.

<!-- more -->

## What problem does it solve?

Have you ever tried to transfer data back and forth between your various service layers? Kafka.

In order to feed our stream, we're going to need a source of data we can poll from, and what better than a messaging service we can just subscribe to for updates? Better yet, we can write back to it to pass our observations back out to the real world as they happen.

## What are we covering?

As this is a tool necessary to get into Spark Streaming, I'll be covering it primarily this time around with another article on the way to cover details of Spark Streaming itself at a later date.

## Getting started

As with many of my tutorials, we're going to be playing with our new toys:

http://kafka.apache.org/downloads.html

For the sake of this round, I'm using the Kafka binary written for Scala 2.11, version ``0.8.2.1``


