---
layout: post
title: "A Functional Programming Primer for Spark"
date: 2015-06-20 19:48:31 -0700
comments: true
categories: [Functional Programming, Scala, Python, Spark]
---

There's a lot of hype around Spark and Big Data in general, especially around the concepts of Functional Programming. Problem is, Functional Programming is a tall order for a standard Java programmer.

The goal of this post is to get you up to speed in the very basics of Functional Programming as they'll later relate to Spark. I'll be primarily covering Scala with Python alternate versions.

<!-- more -->

## What about Java 8?

I do not intend to cover Java in this tutorial or any other. MapReduce is a concept based in Functional Programming, and you would be doing yourself a great disservice by trying to shoehorn Java into that role, including Java 8.

You might wonder how bad it could possibly be, perhaps I'm just biased. I would direct you to look at the [Hadoop word count example](http://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html#Example:_WordCount_v2.0) and see the horrors of allowing Java patterns and card carrying GoF members to pretend they're programming functionally:

```java hadoop-wordcount-example
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.Counter;
import org.apache.hadoop.util.GenericOptionsParser;
import org.apache.hadoop.util.StringUtils;

public class WordCount2 {

  public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    static enum CountersEnum { INPUT_WORDS }

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    private boolean caseSensitive;
    private Set<String> patternsToSkip = new HashSet<String>();

    private Configuration conf;
    private BufferedReader fis;

    @Override
    public void setup(Context context) throws IOException,
        InterruptedException {
      conf = context.getConfiguration();
      caseSensitive = conf.getBoolean("wordcount.case.sensitive", true);
      if (conf.getBoolean("wordcount.skip.patterns", true)) {
        URI[] patternsURIs = Job.getInstance(conf).getCacheFiles();
        for (URI patternsURI : patternsURIs) {
          Path patternsPath = new Path(patternsURI.getPath());
          String patternsFileName = patternsPath.getName().toString();
          parseSkipFile(patternsFileName);
        }
      }
    }

    private void parseSkipFile(String fileName) {
      try {
        fis = new BufferedReader(new FileReader(fileName));
        String pattern = null;
        while ((pattern = fis.readLine()) != null) {
          patternsToSkip.add(pattern);
        }
      } catch (IOException ioe) {
        System.err.println("Caught exception while parsing the cached file '"
            + StringUtils.stringifyException(ioe));
      }
    }

    @Override
    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      String line = (caseSensitive) ?
          value.toString() : value.toString().toLowerCase();
      for (String pattern : patternsToSkip) {
        line = line.replaceAll(pattern, "");
      }
      StringTokenizer itr = new StringTokenizer(line);
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        Counter counter = context.getCounter(CountersEnum.class.getName(),
            CountersEnum.INPUT_WORDS.toString());
        counter.increment(1);
      }
    }
  }

  public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    GenericOptionsParser optionParser = new GenericOptionsParser(conf, args);
    String[] remainingArgs = optionParser.getRemainingArgs();
    if (!(remainingArgs.length != 2 | | remainingArgs.length != 4)) {
      System.err.println("Usage: wordcount <in> <out> [-skip skipPatternFile]");
      System.exit(2);
    }
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount2.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);

    List<String> otherArgs = new ArrayList<String>();
    for (int i=0; i < remainingArgs.length; ++i) {
      if ("-skip".equals(remainingArgs[i])) {
        job.addCacheFile(new Path(remainingArgs[++i]).toUri());
        job.getConfiguration().setBoolean("wordcount.skip.patterns", true);
      } else {
        otherArgs.add(remainingArgs[i]);
      }
    }
    FileInputFormat.addInputPath(job, new Path(otherArgs.get(0)));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs.get(1)));

    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}
```

That's just a word count example, imagine the headaches of anything remotely complex in that paradigm and you'll do the same as I did and swear off Hadoop.

Are there workarounds for it? Yes. You can also put a nice coat of paint on an old rusty car. It'll look nicer but it's not fooling anyone what's under the hood.

## Spark can do better

Remember that word count example? Scala and Spark do it substantially better:

```scala spark-wordcount-example https://spark.apache.org/examples.html
val textFile = spark.textFile("hdfs://...")
val counts = textFile.flatMap(line => line.split(" "))
                 .map(word => (word, 1))
                 .reduceByKey(_ + _)
counts.saveAsTextFile("hdfs://...")
```

Even Python leaves it in the dust:

```python python-wordcount-example https://spark.apache.org/examples.html
text_file = spark.textFile("hdfs://...")
counts = text_file.flatMap(lambda line: line.split(" ")) \
             .map(lambda word: (word, 1)) \
             .reduceByKey(lambda a, b: a + b)
counts.saveAsTextFile("hdfs://...")
```

You don't have to spend five seconds scrolling to get through that one. The point is that by defining a mapreduce task in terms of functions, we only need to tell Spark what actions it's taking on the data. We'll get to what all this means later.

## Why Scala over Python?

I advocate the usage of Scala in general for Big Data problems over Python. The reasoning is that Scala is a Statically typed language, surprisingly moreso than even Java (we'll cover that in a moment.) Spark was also written in Scala, meaning its DSL is going to be very familiar if you're any grade of Scala programmer.

Python is a great language, don't get me wrong, but it's not fully functional. You'll see why that's a big deal in a moment here. That, and I don't like typing ``lambda`` all the time.

## Basics of Functional Programming

So what is Functional Programming, besides the most thrown around concept in modern days? Quite simply it's a program built up from Functions instead of Objects.

A little bit more into it, Functional Programming embraces a few interesting ideals:

* The REPL - That's Read Evaluate Print Loop, a program for running code inline much like a Unix Shell
* Mutation is forbidden - All variables are final
* Functional purity - If you pass ``A`` into a function, you're always getting ``B`` back
* Nil is dead - Null pointer exceptions begone!
* Programs are composed of functions - Think writing a program on terms of verbs instead of nouns
* Functions are first class citizens - You can pass functions as arguments, and even return them
* Laziness is useful - Functions and values that don't evaluate until they're called

We'll be getting into each of those and why they're relevant to Spark here in a moment.

## Before we get too far

You're going to want to get Scala or Python installed so we can get at their REPLs. When you have them installed, drop into your terminal shell and type in either ``scala`` or ``python`` to drop into a REPL. Give it a swing real quick:

```scala scala-repl
Welcome to Scala version 2.11.5 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_31).
Type in expressions to have them evaluated.
Type :help for more information.

scala> 5 + 5
res0: Int = 10
```

```python python-repl
Python 2.7.5 (default, Mar  9 2014, 22:15:05)
[GCC 4.2.1 Compatible Apple LLVM 5.0 (clang-500.0.68)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 5 + 5
10
```

In terms of Functional Programming, and later Spark, the REPL will quickly become your best friend.

## A Function

So what's a function? Let's give the REPL a whirl:

```scala scala-basic-function
scala> def add2(x:Int) = x + 2
add2: (x: Int)Int

scala> add2(3)
res1: Int = 5
```

Fair warning that Python wants you to put a blank line before it assumes you're done typing

```python python-basic-function
>>> def add2(x): return x + 2
...
>>> add2(3)
5
```

Notice something interesting about Scala there? There's no need for a return. It's implied that the last statement in a function is the return value. You'll also notice that the Scala REPL guessed that we're going to return an Integer as well, as per the method signature.

Now what's in a method signature? It's a contract, a guarantee of a return type. This brings us to our next concept

## Goodbye to Nil

Now remember when I said that Scala was more statically typed than Java? Try to give ``add2`` a ``nil`` and see what happens in both languages:

```scala scala-none-function
scala> add2(None)
<console>:9: error: type mismatch;
 found   : None.type
 required: Int
              add2(None)
```

```python python-none-function
>>> add2(None)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 1, in add2
TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
```

But that's not ``nil``! That's something called ``None``!

So what's the difference? Put briefly, Scala and Python both do not have a concept of ``nil`` which is a very very good thing for us.

Unless we explicitly tell Scala it can take a ``None``, it will always throw a type error. So how do we define something that might take a value or might not? That's what we have ``Option`` for:

```scala
scala> def add2Maybe(x:Option[Int]) = x.getOrElse(0) + 2
add2Maybe: (x: Option[Int])Int

scala> add2Maybe(Some(2))
res6: Int = 4

scala> add2Maybe(None)
res7: Int = 2
```

Python does not have this concept, it only replaces ``nil`` with ``None``.

## Higher Order Functions and Map

One of the most powerful concepts in Functional Programming is the ability to pass functions as arguments. By doing this we're afforded a great deal of flexibility in defining abstract interfaces for basic operations.

**WARNING** Python users, normally you're going to want to use List Comprehensions for this type of thing. Since you're going to be applying this to Spark, it's necessary to know these types of functions.

Take ``map`` for instance, a function that applies a function to a list. This will be confusing for first timers in this territory, so stick with me for a bit here:

```scala scala-map
scala> List(1,2,3,4).map { i => i * 2 }
```

```python python-map
>>> map(lambda x: x * 2, [1,2,3,4])
```

Now what do you suppose those two do? We're passing in a function that takes an argument ``x`` and returns ``x * 2``. We're applying that function to each element in the list, so let's step through this in Scala:

```scala
// val is short for value, or an immutable variable
scala> val myList = List(1,2,3,4)
myList: List[Int] = List(1, 2, 3, 4)

scala> myList.map { i => i * 2 }

// First iteration:  i is 1, returns 2
// Second iteration: i is 2, returns 4
// Third iteration:  i is 3, returns 6
// Fourth iteration: i is 4, returns 8
// ...and now we have a new list returned:
res8: List[Int] = List(2, 4, 6, 8)

// You remember I said immutable? What's myList right now?
scala> myList
res9: List[Int] = List(1, 2, 3, 4)
```

So not only did we double each element in the list, but our original list is untouched. Given this, we can transform ``myList`` however we want and it'll never change the value of it. Now if we want that result for something, we can always create a new ``val`` to save it.

An aside, Scala is very good about trying to simplify things when it can:

```scala scala-short-map
scala> List(1,2,3,4) map (_ * 2)
res10: List[Int] = List(2, 4, 6, 8)
```

We don't really need the dot there, and whenever something only takes one parameter Scala will be more than happy to take an underscore to shorten it up for us. While this may seem obscuring to some, it's a very common pattern in Scala. Best to understand what it's doing because no amount of rudimentary googling is going to turn that up without some fidgeting, but such are operator and syntactic sugar searches.

## Higher Order Functions - Filter

The next function on our list is filter, which takes a function and applies it to each element of a list looking for elements where the result is ``true``:

```scala scala-filter
scala> List(1,2,3,4) filter (_ > 2)
res10: List[Int] = List(3, 4)
```

```python python-filter
>>> filter(lambda x: x > 2, [1,2,3,4])
[3, 4]
```

## Higher Order Functions - Reduce

This one is going to be a bit trickier, as it takes a function with two arguments: an accumulator and a value. It reduces a list of elements into one element. Now why would you want such a function? Think of something such as a sum. I'll be using longhand here as this is one of the harder first functions to really understand:

```scala scala-reduce
scala> List(1,2,3,4).reduce { (accumulator, i) => accumulator + i }
res11: Int = 10
```

```python python-reduce
>>> reduce(lambda accumulator, i: accumulator + i, [1,2,3,4])
10
```

But how did that work? Let's step through the logic here in Scala:

```scala scala-reduce-explained
scala> List(1,2,3,4).reduce { (accumulator, i) => accumulator + i }

// In our first iteration, the accumulator is either set to a default value,
// or the head element of the list is used. In this case it's 1

// First iteration - accumulator: 1, i: 2 => 3

// This function returns 3, which is passed in as the next value of the
// accumulator:

// Second iteration - accumulator: 3, i: 3 => 6
// Third iteration  - accumulator: 6, i: 4 => 10

// Now we're out of elements, so reduce returns the accumulator as the result:
res11: Int = 10
```

Naturally there's a shorthand for this:
```scala scala-reduce-shorthand
scala> List(1,2,3,4).reduce(_+_)
res12: Int = 10
```

An astute reader will notice that we used two underscores here. Scala binds arguments in succession to the underscore, making for a bit more confusion in searching.

## Higher Order Functions - Closures

One of the really nifty things about Functions in languages like Scala is that they capture their local environment when they're defined. What do I mean by that? Let's take a look:

```scala scala-closure
scala> def adder(x:Int) = (y:Int) => x + y
adder: (x: Int)Int => Int

scala> val add3 = adder(3)
add3: Int => Int = <function1>

scala> add3(5)
res14: Int = 8
```

So where did it get 3 from? It remembered the value, or in functional terms it closed over the value when it was defined.

How is this handy? Let's use map and pass it our function:

```scala scala-closure-map
scala> List(1,2,3,4).map(add3)
res15: List[Int] = List(4, 5, 6, 7)
```

Let's take it one step further though and just use adder. After all, we might need a bit more flexibility there:

```scala scala-closure-map-dynamic
scala> List(1,2,3,4).map(adder(5))
res17: List[Int] = List(6, 7, 8, 9)
```

So now we have a function which returns a function that gets used by map. To put it another way, let's have at filter:

```scala scala-closure-filter
scala> def divisibleBy(y:Int) = (x:Int) => x % y == 0
divisibleBy: (y: Int)Int => Boolean

scala> List(1,2,3,4).filter(divisibleBy(2))
res18: List[Int] = List(2, 4)
```

## Laziness can be good

You might wonder when a concept like a lazy value might be handy. Let's say you need an infinite list, or stream, from which you have no clue how many elements you'll either need or get from it.

How about an infinite stream of Fibonacci numbers, straight from the Scala source code:

```scala scala-lazy
scala> val fibs: Stream[BigInt] =
  BigInt(0) #:: BigInt(1) #:: fibs.zip(fibs.tail).map { n => n._1 + n._2 }
fibs: Stream[BigInt] = Stream(0, ?)

scala> fibs.take(10).foreach(println)
0
1
1
2
3
5
8
13
21
34
```

That's a lot of code to digest, but it gives us an infinite stream of Fibonacci numbers. Let's break it apart a bit:

```scala scala-lazy-explained
// We're creating a new value that's a stream of BigInts
val fibs: Stream[BigInt] =
  // Where the first value is zero, lazily concatenated with
  BigInt(0) #::
  // The second value, which is one, lazily concatenated with
  BigInt(1) #::
  // A function that takes the current fibonnaci numbers,
  // zips them with their tail, and adds those pairs together
  fibs.zip(fibs.tail).map { n => n._1 + n._2 }

// What are zip and tail?
scala> val zipList = List(1,2,3,4)
zipList: List[Int] = List(1, 2, 3, 4)

// Remember head? It gets the first element of our list
scala> zipList.head
res22: Int = 1

// Tail just gets the rest
scala> zipList.tail
res23: List[Int] = List(2, 3, 4)

// It takes two lists and zips them together into tuple pairs
scala> zipList.zip(zipList.tail)
res24: List[(Int, Int)] = List((1,2), (2,3), (3,4))

// Now about that map function:
// map { n => n._1 + n._2 }
//
// That just adds the two elements of the tuple together. In Scala, _n is the
// nth element of the list, non-zero indexed
```

## How is this relevant?

Now that we have all these components, let's take another look at that wordcount example for Spark:

```scala spark-wordcount-example https://spark.apache.org/examples.html
val textFile = spark.textFile("hdfs://...")
val counts = textFile.flatMap(line => line.split(" "))
                 .map(word => (word, 1))
                 .reduceByKey(_ + _)
counts.saveAsTextFile("hdfs://...")
```

Some of those look familiar? Let's dissect it a bit:

```scala spark-wordcount-example-explained https://spark.apache.org/examples.html
// We're reading our document from HDFS, and storing it in textFile
val textFile = spark.textFile("hdfs://...")

// Now we're defining our pipeline - By the way, this is lazy
val counts =
  textFile
    // Flat map is very similar to map, except it flattens the results after it
    // gets them (see flatmap below)
    //
    // What we're doing here is splitting each line by whitespace, and then
    // flattening into one stream of words to go through
    .flatMap(line => line.split(" "))
    // Then we're mapping all those words into a tuple, we'll see why in a
    // second
    .map(word => (word, 1))
    // Reduce by key takes all similar keys and reduces the values with a
    // function, in this case a sum
    .reduceByKey(_ + _)

// Why the tuple and reduce by key? Normally you'd use a groupBy operator here,
// but that does not parallelize cleanly.
//
// What we do to compensate here is make tuples so that we can send specific
// words to different partitions to be reduced

// NOW the pipeline gets called, as we want a value out of it. In this case it
// saves a new text file
counts.saveAsTextFile("hdfs://...")

// Flat Map
scala> List(List(1,2,3), List(2,3,4)).map(list => list.map(adder(2)))
res27: List[List[Int]] = List(List(3, 4, 5), List(4, 5, 6))

scala> List(List(1,2,3), List(2,3,4)).flatMap(list => list.map(adder(2)))
res28: List[Int] = List(3, 4, 5, 4, 5, 6)
```

Now there's a lot more to Spark than this, but now you've got a grounding by which you can build on. Next we'll be looking more into Spark specifically.
