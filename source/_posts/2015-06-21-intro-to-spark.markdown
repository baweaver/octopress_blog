---
layout: post
title: "Intro to Spark"
date: 2015-06-21 18:13:24 -0700
comments: true
categories: [Functional Programming, Scala, Python, Spark]
---

Assuming you've read the first article on [Functional Programming in Scala and Python](http://baweaver.com/blog/2015/06/20/a-functional-programming-primer-for-spark/), you should be ready to sink your teeth into a few practical Spark problems

<!-- more -->

## Getting Spark

The first step to running Spark is to get a standalone instance to play with on our machines.

Go to the Spark homepage: https://spark.apache.org/downloads.html

We'll be using version ``1.4.0``. Select that version from releases, and select Pre-built for Hadoop 2.6 and later (unless you currently have another Hadoop / HDFS instance at a different version.)

Go ahead and download / unpack that into the directory of your choice, and ``cd`` into it.

## Getting our wordlist

We'll be using an [english wordlist from SIL](http://www-01.sil.org/linguistics/wordlists/english/) for the following exercises. Make sure to save ``wordsEn.txt`` somewhere where you can load it later.

## Spark REPL

The last tutorial mentioned the concept of a REPL as a way to play with code interactively. Handy enough, Spark implemented its own REPL over Scala and Python (and not Java.)

For Scala that would be ``bin/spark-shell``

For Python, it's ``bin/pyspark``

You should see something like this (snipped for length):

```text spark-repl-scala
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 1.3.1
      /_/

Using Scala version 2.10.4 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_31)
Type in expressions to have them evaluated.
Type :help for more information.
```

or this:
```text spark-repl-python
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 1.3.1
      /_/

Using Python version 2.7.5 (default, Mar  9 2014 22:15:05)
SparkContext available as sc, HiveContext available as sqlContext.
```

There will be a considerable amount of other debugging and logging statements than that, but for the point of this those will do as things to look for.

## Spark Context

In the Spark shell, we're given the entirety of the Spark library as ``sc`` to interact with. We can use that to load in our text file:

```scala scala-wordlist
scala> val wordList = sc.textFile("/Users/lemur/dev/wordlist/wordsEn.txt")
// ...debugger output
```

```python python-wordlist
>>> wordList = sc.textFile("/Users/lemur/dev/wordlist/wordsEn.txt")
# ...debugger output
```

**WARNING** - Remember last time when I mentioned Spark was Lazy? If you type that path in wrong, it's not going to tell you anything until you try and run commands on it. This is the same for a lot of functions in Spark, you won't know it's broken until you run it.

Now we have our files loaded into memory to do some experimentation with as RDDs (Resilient Distributed Datasets), Spark's abstraction for distributed data.

Let's try a basic one to start, how many lines are in the file? (I'm going to be trimming output so we don't fill the page with debugger info)

```scala scala-wordlist-count
scala> wordList.count()
// ...debugger output
res0: Long = 109583
```


```python python-wordlist-count
>>> wordList.count()
# ...debugger output
109583
```

With that you've just run a Spark job. Simple as that, and not much different than how you'd interact with anything else.

## Starts with

Now, since this is a dictionary, each word is in there once. That makes a wordcount a bit pointless, so instead let's get a list of what letters they start with:

```scala starts-with-scala
scala> wordList.filter(_ != "").map(word => (word(0), 1)).reduceByKey(_+_).foreach(println)

(w,2714)
(s,12108)
(e,4494)
(a,6541)
(k,964)
(i,4382)
(y,370)
(u,3312)
(o,2966)
(q,577)
(g,3594)
(d,6694)
(z,265)
(m,5806)
(c,10324)
(p,8448)
(x,79)
(t,5530)
(b,6280)
(h,3920)
(n,2475)
(f,4701)
(j,1046)
(v,1825)
(r,6804)
(l,3363)
```

```python starts-with-python
letterCounts = wordList \
  .filter(lambda w: w != "") \
  .map(lambda w: (w[0], 1)) \
  .reduceByKey(lambda a,b: a + b) \
  .collect() # Force the result to run

>>> for count in letterCounts:
...   print count
...
(u'a', 6541)
(u'c', 10324)
(u'e', 4494)
(u'g', 3594)
(u'i', 4382)
(u'k', 964)
(u'm', 5806)
(u'o', 2966)
(u'q', 577)
(u's', 12108)
(u'u', 3312)
(u'w', 2714)
(u'y', 370)
(u'b', 6280)
(u'd', 6694)
(u'f', 4701)
(u'h', 3920)
(u'j', 1046)
(u'l', 3363)
(u'n', 2475)
(u'p', 8448)
(u'r', 6804)
(u't', 5530)
(u'v', 1825)
(u'x', 79)
(u'z', 265)
```

## Spark SQL

On occasion we'll have the niceties of structured data such as JSON, and Spark has just the way to deal with it using Spark SQL.

**WARNING** - Spark guide has been quoted as saying:

> Note that the file that is offered as a json file is not a typical JSON file. Each line must contain a separate, self-contained valid JSON object. As a consequence, a regular multi-line JSON file will most often fail.

...and it will crash if you pass it actually valid JSON. If any reader knows the reasoning behind this particularly confounding piece of work, I'd love to know.

We'll be using fake people data: [https://gist.githubusercontent.com/baweaver/b6460bb96feff1faeb78/raw/4c9b46be165725d041ff47bdc042c6a4880c1877/people.json](people.json) (right click to save)

Let's go ahead and load it up using the ``sqlContext``:

```scala scala-sql-load
scala> val people = sqlContext.jsonFile("/Users/lemur/dev/wordlist/people.json")
people: org.apache.spark.sql.DataFrame = [_id: string, address: string, age: bigint, balance: double, company: string, email: string, eyeColor: string, gender: string, guid: string, index: bigint, isActive: boolean, latitude: double, longitude: double, name: string, phone: string, picture: string, registered: string]

// Make SURE to register it as a table
scala> people.registerTempTable("people")
```

```python python-sql-load
>>> people = sqlContext.jsonFile("/Users/lemur/dev/wordlist/people.json")

>>> people
DataFrame[_id: string, address: string, age: bigint, balance: double, company: string, email: string, eyeColor: string, gender: string, guid: string, index: bigint, isActive: boolean, latitude: double, longitude: double, name: string, phone: string, picture: string, registered: string]

# Make SURE to register it as a table
>>> people.registerTempTable("people")
```

Let's start with something fairly basic on the SQL, getting the index of people who are inactive with a balance greater than $2000:

```scala scala-sql-basic
// Note I'm calling on SQL Context here
scala> sqlContext.sql("""
     |   SELECT index
     |   FROM people
     |   WHERE isActive == false AND
     |         balance > 2000.00
     | """).count()

res1: Long = 75
```

```python python-sql-basic
>>> sqlContext.sql("""
...   SELECT index
...   FROM people
...   WHERE isActive == false AND
...         balance > 2000.00
... """).count()

75
```

Triple quotes are a life saver when making larger SQL-like strings.

Like SQL, you can join, count, group, and various other operations all in a big data context. It's a shame it won't play nicely with actual JSON, but the features are handy nonetheless.

[Further reading](https://spark.apache.org/docs/1.4.0/sql-programming-guide.html#starting-point-sqlcontext)

## Spark MLLib - Statistics

Spark even comes with its own Machine Learning libraries, but for the sake of brevity we're only going to look into some of the basic statistical options. Later tutorials will address this in some depth.

We'll be looking into the column stats of our wordList from earlier:

```scala scala-statistics-basics
// Make SURE to import it
scala> import org.apache.spark.mllib.stat.Statistics
scala> import org.apache.spark.mllib.linalg.Vectors

scala> val wordList = sc.textFile("/Users/lemur/dev/wordlist/wordsEn.txt")

scala> val wordLengths = wordList.map(w => Vectors.dense(w.length))
wordLengths: org.apache.spark.rdd.RDD[org.apache.spark.mllib.linalg.Vector] = MapPartitionsRDD[6] at map at <console>:32

scala> val summaryStatistics = Statistics.colStats(wordLengths)
summaryStatistics: org.apache.spark.mllib.stat.MultivariateStatisticalSummary = org.apache.spark.mllib.stat.MultivariateOnlineSummarizer@4377e40a

// Let's take a look inside shall we?
scala> summaryStatistics.mean
res22: org.apache.spark.mllib.linalg.Vector = [8.533905806557591]

scala> summaryStatistics.max
res23: org.apache.spark.mllib.linalg.Vector = [28.0]

scala> summaryStatistics.min
res24: org.apache.spark.mllib.linalg.Vector = [0.0]

scala> summaryStatistics.variance
res25: org.apache.spark.mllib.linalg.Vector = [6.448337984119102]
```

```python python-statistics-basics
# Make SURE to import it
>>> from pyspark.mllib.stat import Statistics

>>> wordList = sc.textFile("/Users/lemur/dev/wordlist/wordsEn.txt")

# Python will take a standard list in
>>> wordLengths = wordList.map(lambda w: [len(w)])

>>> summaryStatistics = Statistics.colStats(wordLengths)

# Let's take a look inside shall we?
>>> summaryStatistics.mean()
array([ 8.53390581])

>>> summaryStatistics.max()
array([ 28.])

>>> summaryStatistics.min()
array([ 0.])

>>> summaryStatistics.variance()
array([ 6.44833798])
```

[Further reading](https://spark.apache.org/docs/1.4.0/mllib-statistics.html)

## Wrapping Up

We've taken a cursory look at some of the features and basic operations of Spark. Here's the question though, what do you as readers want to know more about? Vote on Strawpoll to let me know: http://strawpoll.me/4701594

Think of it as a choose your own adventure of sorts. I'll be writing about all of the above in more detail, but in the order you want to see it happen.
