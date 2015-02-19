---
layout: post
title: Parameterized JUnit 4
---

A friend of mine's been working to introduce [Cucumber](http://cukes.info/) into their automation stack. As we've worked, I got my first taste of parameterizing tests. While Cucumber certainly does it better and cleaner, here's what I've cobbled together in an attempt to try it with JUnit (4).

At a high level, you need a few things:

1. A way to get the parameters. (Static method)
2. A way to know which one you're running. (Constructor/Annotations)
3. One or more tests.

But here's a breakdown on how to actually implement this strategy.

## Run with

At the top of this class you'll need `@RunWith` annotation, like so:

{% gist icbat/6babaf32f4a634633dba %}

All this does is instruct JUnit to run with the special [Parameterized runner](http://junit.org/apidocs/org/junit/runners/Parameterized.html). This will handle piping data to each of the test instances.

## Get the data

{% gist icbat/c59e8cce2e1492ccd459 %}

There's a few key points here. You'll need that Annotation, you'll also need to make this method `static` and ensure it returns a `Iterable<Object[]>`. That's really where the constraints end, though. Feel free to read this in from a file, etc, as long as it can be done statically.

The static bit is important. The way the Runner works, it will run the method annotated as `@Parameters` in a similar way to `@BeforeClass`, then create a new instance of this test class. 

The name argument to the `@Parameters` annotation highlights a few of the placeholders granted by the runner. Each of the paremeters will get mapped to a number, commonly referenced by their position in the inner array. In this example, at one point the name would be `4 : 4 squares to 16`. It's not actual test output, just a little something to keep track of which iteration this is. If you leave out this argument, you'll instead get `testCase[{index}]`, which is rarely what you want to convey.

## Pass it to the instance

The Parameterized runner will make an instance of your class, and try to pass its knowledge on in one of two ways. You can use a constructor with the right number of arguments, like so: 

{% gist icbat/03111d04e95806054d81 %}

You can also use a more explicit, if simplified, syntax:

{% gist icbat/0595b503174f5462733f %}

Annotating the fields is more concise and clear, but doesn't allow you to process the inputs in anyway like the constructor approach.

## Test!

Run any test you want! You've got data in your fields. It's worth noting that each `@Test` will be run for each input, so it's usually best to segregate the parameterized tests away from the static ones.

## Everything put together

{% gist icbat/9788fec45c29f1fde15c %}

## Some extras

A few extra things I've learned, with sources provided where possible:

### Exact arguments only!

I thought I could be cute and make optional parameters using the `Object...` structure, as in:

{% gist icbat/b68a9e71ad75607095d9 %}

Alas, you'll get an exception here. Probably for the best.

### As many as you need!

You aren't limited to 2 parameters! Most tutorials and documentation out there aren't explicit about this, but [this tutorial](http://www.mkyong.com/unittest/junit-4-tutorial-6-parameterized-test/) shows us we can do more. When I first started working with this, I naively assumed I could only have two, and set about crafting a horrible string-parser to represent more data.

### Objects, not primitives

My naivety could also have been circumvented by realizing the parameters object is in fact an array of Objects! You can send whatever you want here, which wasn't immediately clear to me. Far better to pass in a real input than to try to construct your own.

### Naming tests for Sonar/sanity

One problem with the `@Parameters(name = "...")` bit above is that it really only works well in this contrived example. If you're using objects as one parameter, your `toString()` method is suddenly a necessity. If you have too many, your test runners, be it in an IDE or on a tool like Sonar, start getting cruft-filled.

A cute trick to alleviate this is to add a String parameter to each case whose only purpose is to be the name of the case. I've wound up with code more akin to:

{% gist icbat/e3cf3d1ca639e6249ab1 %}
